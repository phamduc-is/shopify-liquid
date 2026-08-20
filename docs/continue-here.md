# 📍 Continue Here — Trạng thái lúc dừng (chờ review)

> Ghi lại lúc dừng phiên làm việc, để pull về máy khác là tiếp được ngay.
> Nội dung cũ của file lấy lại bằng: `git show 0a98732:docs/continue-here.md`

---

## ⏸️ Việc đang TẠM DỪNG: section `product-tabs`

Đã build xong code, `theme check` sạch (31 files, 0 offenses), nhưng **chưa test trên browser** và **chưa quyết định giữ hay bỏ**. Không tiếp tục cho tới khi review.

### File đã tạo/sửa cho section này

| File | Trạng thái |
|---|---|
| `sections/product-tabs.liquid` | mới, 466 dòng — markup 3 tab + custom element + schema |
| `src/scss/components/_product-tabs.scss` | mới, 342 dòng |
| `assets/icon-filter.svg` | mới (theme chưa có icon filter) |
| `src/scss/main.scss` | thêm `@use 'components/product-tabs'` |
| `locales/en.default.json` | thêm 21 string `sections.product_tabs.*` |
| `locales/en.default.schema.json` | thêm `general.product_tabs`, `general.faq`, 6 label, `options.default_tab.*`, `info.write_review_url` |
| `templates/product.json` | chèn vào order: `main → product-tabs → related-products` + 3 FAQ block mặc định |

> ⚠️ Section **đang active trong `templates/product.json`** nên vẫn render trên trang product. Muốn ẩn tạm thì xoá `"product-tabs"` khỏi mảng `order` (giữ nguyên khối `sections`), hoặc xoá section trong Theme Editor.

### 3 tab, 3 cơ chế data — có chủ đích

| Tab | Nguồn data | Lý do chọn |
|---|---|---|
| Product Details | metafield `custom.product_details` (rich text) | Không dùng lại `product.description` vì cột product info bên trên đã render mô tả ngắn đó → tránh trùng nội dung |
| Rating & Reviews | metafield `custom.reviews` (`list.metaobject_reference` → metaobject `product_review`) + block `@app` | Cần per-product. Theme block/settings thuộc `templates/product.json` nên mọi sản phẩm sẽ dùng chung 1 danh sách review → sai bản chất |
| FAQs | Theme block `faq`, dùng `<details>` native | Ở đây block **là đúng** — FAQ thường giống nhau cho mọi sản phẩm |

Dùng **classic section blocks** (`{% for block in section.blocks %}` + `where: 'type', ...`) chứ không phải `{% content_for 'blocks' %}` như các section khác — vì cần lọc theo type để đặt FAQ block vào tab 3 và app block vào tab 2.

### Filter / sort / load more

Mỗi review card mang data để JS đọc trực tiếp qua `dataset`, không parse text DOM:

```liquid
<li class="product-tabs__review" data-rating="5" data-date="2023-08-14">
```

| Control | Cơ chế |
|---|---|
| Filter | `data-rating` — All ratings / 5★→1★, khớp chính xác |
| Sort | `data-date` (`YYYY-MM-DD` nên so sánh chuỗi ra đúng thứ tự) cho Latest/Oldest; `data-rating` cho Highest/Lowest rated |
| Load More | Reveal theo batch, default 6/lần (setting `reviews_per_page`) |
| Count "(N)" | JS ghi số vào `textContent`; dấu ngoặc để ở CSS `::before`/`::after` nên JS không phải ghép chuỗi |

Đổi filter → reset về batch đầu, không giữ số đã "load more" cũ.

Nút "Write a Review" render đủ theo design nhưng chỉ là link tới URL merchant tự set (setting `write_review_url`) — Shopify **không có form review native**.

### 🔧 Cần setup trong Admin để test tab Reviews

Theme **không tạo được** metaobject definition, phải làm tay 1 lần:

**1.** Content → Metaobjects → Add definition, type handle **`product_review`**:

| Field key | Type |
|---|---|
| `author` | Single line text |
| `rating` | Integer (1–5) |
| `body` | Multi-line text |
| `review_date` | Date |
| `verified` | True or false |

**2.** Settings → Custom data → Products → Add definition:
- `custom.reviews` — type **List of metaobjects** → Product review
- `custom.product_details` — type **Rich text**

**3.** Tạo vài metaobject entry rồi gán vào product qua metafield `custom.reviews`.

Spec này cũng nằm trong khối `{% comment %}` đầu file `sections/product-tabs.liquid`.

### ❓ Chưa verify

- Tab bar trên browser thật: đổi tab, keyboard nav (`←` `→` `Home` `End`), `aria-selected`.
- Accordion FAQ trên mobile.
- Reviews grid + filter/sort/load more (cần data metaobject mới test được).
- Section render đúng vị trí giữa product info và "You might also like".

---

## ✅ Section `related-products` — đã chốt phương án

Đã đi qua 2 lần rewrite, **kết luận: giữ pattern Recommendations API** (giống Dawn). Ghi lại để không lặp lại tranh luận:

| Phương án | Kết luận |
|---|---|
| Setting `product_list` cho merchant chọn | ❌ **Sai bản chất** — settings thuộc template nên mọi product hiện chung 1 danh sách |
| `product.collections.first.products` | ❌ Chỉ là "cùng collection", không phản ánh hành vi mua |
| Recommendations API 2 lượt render | ✅ **Đang dùng** — đường duy nhất đọc được data từ Search & Discovery |

Code hiện tại đã kế thừa 3 điểm từ Dawn:
1. **Custom element** `<related-products>` thay `querySelectorAll` + `shopify:section:load` — `connectedCallback` tự chạy khi node vào DOM.
2. **IntersectionObserver** `rootMargin: '0px 0px 400px 0px'` — chỉ fetch khi scroll gần tới.
3. **Setting `intent`** — `related` (thuật toán Shopify) / `complementary` (list merchant set trong Search & Discovery). Cùng 1 endpoint, chỉ khác 1 param.

Đã thêm `presets` (trước đó thiếu nên không thêm được qua "Add section").

> ⚠️ Section trống trên dev store là **ĐÚNG**, không phải bug: docs Shopify ghi rõ không tính đơn hàng import từ platform khác, và loại trừ sản phẩm hết hàng / giá 0 / gift card / đang trong cart. Dev store chưa có đơn hàng thật → mất tiêu chí mạnh nhất.

Bắt buộc có `display: block` trong SCSS cho cả `.related-products` và `.product-tabs` — custom element mặc định là `display: inline`.

---

## 📦 Trạng thái repo — 11 file CHƯA COMMIT

```
 M docs/shopify-practical-techniques.md
 M ecommerce-theme/assets/theme.css
 M ecommerce-theme/locales/en.default.json
 M ecommerce-theme/locales/en.default.schema.json
 M ecommerce-theme/src/scss/main.scss
 M ecommerce-theme/templates/product.json
?? ecommerce-theme/assets/icon-filter.svg
?? ecommerce-theme/sections/product-tabs.liquid
?? ecommerce-theme/sections/related-products.liquid
?? ecommerce-theme/src/scss/components/_product-tabs.scss
?? ecommerce-theme/src/scss/components/_related-products.scss
```

Commit cuối đã push: `0a98732` (docs: continue-here bug notes).

---

## 📚 Docs đã bổ sung

`docs/shopify-practical-techniques.md` — thêm **Kỹ thuật 8: Section Rendering API — Section Phụ Thuộc Data Nội Bộ Của Shopify** (~137 dòng, đã update mục lục).

Nội dung chính: cơ chế 2 lượt render, vì sao `section_id` là param quan trọng nhất (HTML vs JSON → markup 1 bản vs 2 bản), 3 tiêu chí của thuật toán `related` (purchase history / product description / related collections — **không** dùng tag, product_type, hay title), và mental model phân loại section theo nguồn data (Presentation / Store data / Platform-computed).

---

## ✅ Đã fix, KHÔNG cần làm lại

| Việc | Trạng thái |
|---|---|
| 8 bug slideshow (`z-index: -1`, `.slideshow__slide` không phải flex item thật, dots vô hình, ảnh dọc bị crop, nhiều `<h1>`, `loading: lazy` trên hero…) | ✅ đã xử lý hết |
| `testimonials.liquid` thiếu `shopify:section:load` | ✅ đã thêm listener |
| `header.liquid` thiếu `shopify:section:load` | ✅ đã thêm listener |
| `related-products.liquid` upload fail — xuống dòng giữa chuỗi filter trong `{% liquid %}` | ✅ đã tách thành nhiều lệnh `assign` |
| `templates/product.json` bị reject vì section file chưa có trên remote | ✅ đã `touch` để sync lại |

---

## 💡 Enhancement chưa làm (không phải bug)

**Kéo ngang bằng chuột cho `.testimonials__track`** — carousel chạy bằng CSS `scroll-snap` nên desktop đã có Shift+wheel, trackpad 2 ngón, và 2 nút prev/next. Riêng **click-kéo (grab)** thì browser không hỗ trợ native, cần JS Pointer Events.

Nếu làm, 2 điểm bắt buộc xử lý kẻo carousel giật/trễ:
1. Tạm set `scroll-snap-type: none` trong lúc kéo — snap `mandatory` sẽ liên tục kéo `scrollLeft` về điểm snap trong khi JS đang set thủ công. Bật lại ở `pointerup` để browser tự snap vào card gần nhất.
2. Tạm set `scroll-behavior: auto` — `_testimonials.scss` đang bật `smooth` cho nút prev/next, mỗi lần set `scrollLeft` sẽ khởi động 1 animation → chuột đi trước, track chạy theo sau.

Thêm ngưỡng ~5px trước khi coi là "đang kéo", để click vào link trong card không bị hiểu sai thành drag.

---

## 🗺️ Còn lại theo design SHOP.CO

| Trang | Trạng thái |
|---|---|
| **Homepage** | ✅ xong (header, hero slideshow, logo list, featured collection ×2, collections grid, testimonials, newsletter + footer) |
| **Product Detail** | ⏸️ main product + related-products xong; `product-tabs` đang tạm dừng chờ review |
| **Category Page** | ❌ chưa làm — sidebar filters (dropdown Casual, price slider, colors, size, dress style), product grid + pagination |
| **Cart** | ❌ chưa làm — list item (ảnh + size/color + quantity), Order Summary (subtotal/discount/delivery/total), promo code input |
