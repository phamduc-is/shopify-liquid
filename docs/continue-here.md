# 📍 Continue Here — Trạng thái hiện tại & Bài tập đang làm

> File này để lưu context lúc dừng lại, pull về máy khác là làm tiếp được ngay không cần hỏi lại từ đầu.

---

## 🗺️ Tiến độ chung

- **`my-first-theme/`** — đã hoàn thành đầy đủ **Ngày 1–12** (lý thuyết + bài tập theo roadmap). Toàn bộ kiến thức đã tổng hợp trong [docs/shopify-liquid-summary.md](shopify-liquid-summary.md) (mục lục Cmd+F ở đầu file). Bài tập dở dang `sections/header.liquid` (Ngày 13, Bước 1 — HTML tĩnh dropdown) ở theme này **coi như dừng lại, không tiếp tục nữa** — đã quyết định chuyển hẳn Ngày 13 trở đi sang project mới bên dưới.
- **`ecommerce-theme/`** — theme Shopify **độc lập, mới tinh** (chạy `shopify theme init`, chưa custom gì — mới ở dạng skeleton mặc định, 39 file, `theme check` sạch). Đây là nơi tiếp tục học **Ngày 13 trở đi**, đồng thời là project thật: **clone lại thiết kế Figma** bên dưới.

---

## 🎨 Dự án đang làm: Clone Figma "SHOP.CO" E-commerce Template

**Link Figma**: https://www.figma.com/design/AXFvzD9Zu9A2xkNwOItGWL/E-commerce-Website-Template--Freebie---Community-?node-id=0-1

> ⚠️ Agent không tự mở được link Figma — cần cung cấp lại screenshot nếu agent cần xem lại thiết kế ở máy khác.

### Các trang cần build (đã phân tích từ ảnh Figma)

| Trang | Thành phần chính |
|---|---|
| **Homepage** | Header (logo + nav + search + cart), Hero "Find Clothes That Matches Your Style" + banner brand logos, "New Arrivals"/"Top Selling" (product grid + rating), "Browse by Dress Style" (grid ảnh categories), "Our Happy Customers" (testimonial), Newsletter banner đen, Footer |
| **Product Detail Page** | Gallery ảnh (thumbnail dọc), giá + rating + review count, chọn màu/size, quantity + Add to cart, tab Reviews (rating breakdown + list), "You might also like" |
| **Category Page** | Sidebar Filters (dropdown Casual, Price slider, Colors, Size, Dress Style), product grid + pagination |
| **Cart** | Danh sách item (ảnh + size/color + số lượng), Order Summary (subtotal/discount/delivery/total), promo code input |

### Component dùng chung — PHẢI build 1 lần, tái sử dụng (đã nhận diện ở Bước 0)

| Component | Xuất hiện ở |
|---|---|
| **Product Card** (ảnh + tên + rating sao + giá + giá gạch) | Homepage (New Arrivals, Top Selling), Category Page, Product Detail (You might also like) |
| **Rating sao** (★★★★☆) | Product Card, trang Review |
| **Badge giảm giá %** | Product Card |
| **Newsletter banner đen** | Cuối mọi trang |
| **Footer** | Mọi trang |

---

## 📋 Quy trình đã thống nhất — làm theo đúng thứ tự này, KHÔNG nhảy cóc

1. ~~Bước 0 — Phân tích Figma tìm component dùng chung~~ ✅ Đã xong (bảng trên).
2. ~~Bước 1 — Thiết lập Design System~~ ✅ Đã xong — màu (9 màu: background/foreground/text_secondary/border/surface_secondary/sale_badge_bg+text/rating_star/success) + font (heading: Archivo Black, primary: Work Sans) đã vào `config/settings_schema.json`, sinh CSS variable trong `snippets/css-variables.liquid`, verify qua Theme Editor thật + `theme check` sạch (39 file, 0 lỗi). Tiện tay fix luôn 1 bug JSON có sẵn từ skeleton (trailing comma ở `locales/en.default.schema.json`).

   **🔄 Cập nhật thêm — đối chiếu lại với file Figma gốc qua API (mới xong):**
   - Đã dùng Figma Personal Access Token để gọi trực tiếp Figma REST API (agent trước đó không tự mở được link Figma dạng web), đọc toàn bộ 9 frame (Homepage, Category, Product Detail, Cart, Filters) và thống kê tần suất màu/font/spacing/radius thật trong file thiết kế.
   - Đối chiếu thì phát hiện phần lớn màu trong `settings_schema.json` đã khớp sẵn (rating_star, success, sale_badge_text, surface_secondary, background/foreground) — xác nhận theme đã được build đúng theo Figma. Lệch 2 chỗ, đã sửa theo Figma:
     - `border_color`: `#E8E8E8` → **`#D6DCE5`**
     - `button_corner_radius`: default `24` → **`62`** (dáng pill giống nút "Add to cart" trong Figma), phải nâng luôn `max` từ `40` → `100` để cho phép giá trị 62.
   - Có thử thêm font custom **Satoshi**/**Integral CF** (đúng font gốc trong Figma) qua `@font-face` trỏ tới file `.woff2` chưa upload — sau đó **quyết định bỏ hướng này** (bài demo tập trung Liquid, không cần font ngoài) và đổi `type_primary_font` sang **`poppins_n4`** (Poppins — font có sẵn trong thư viện Shopify, gần với phong cách Satoshi nhất; Poppins cũng có xuất hiện rải rác trong chính file Figma). `type_heading_font` giữ nguyên `archivo_black_n4` (đã đủ gần Integral CF).
   - Bổ sung vào `snippets/css-variables.liquid` các token Figma chưa có setting tương ứng: `--style-border-radius-full: 62px`, scale `--fs-*` (font-size), `--fw-*` (font-weight), `--space-*` (spacing 4→26px) — token tĩnh, không qua Theme Customizer.
   - **Bug vừa fix**: 1 khối comment Liquid `{% # ... %}` viết tràn nhiều dòng vật lý (chỉ mở/đóng tag ở dòng đầu/cuối) — tuy `shopify theme check` không báo lỗi nhưng để an toàn với editor/syntax highlighter, đã tách lại đúng chuẩn "1 dòng = 1 tag comment". Verify lại: `theme check` vẫn sạch (43 file, chỉ còn 2 warning `OrphanedSnippet` như đã biết).
   - File tham khảo rời (không nằm trong repo Shopify): `~/Documents/workspace/design-tokens.css` — bộ token CSS đầy đủ trích từ Figma, dùng để đối chiếu khi cần.
3. ~~Bước 2 — Dựng khung `layout/theme.liquid`~~ ✅ Đã xong — thứ tự đúng rule Ngày 2: `content_for_header` (đầu `<head>`, để theme CSS load sau ghi đè được style app) → `css-variables` → font preload → `critical.css` (preload) → `theme.css` (SCSS build ra, không preload) → `meta-tags`. Tiện thể setup xong tooling **SCSS** (`src/scss/` theo 7-1 pattern, `npm run build:css`/`watch:css`, output ra `assets/theme.css`) — xem chi tiết [ecommerce-theme/docs/project-structure.md](../ecommerce-theme/docs/project-structure.md).
4. ~~Bước 3 — Build trước các Snippet dùng chung~~ ✅ Đã xong — `snippets/product-card.liquid` (ảnh + tên + rating sao + giá + giá gạch + badge giảm giá %) và `snippets/rating-stars.liquid` (2 lớp sao `__track`/`__fill` overlay theo `fill_percent`), CSS riêng trong `src/scss/components/product-card.scss` + `rating-stars.scss`. `theme check` sạch (43 file, 0 lỗi thật) — 2 warning `OrphanedSnippet` (`product-card`, `rating-stars`) là bình thường ở giai đoạn này (xem mục bài học dưới), sẽ tự hết khi Bước 4 render snippet vào 1 section thật. 2 việc để-sau có chủ đích (KHÔNG phải sót, đã cân nhắc khi review):
   - `.rating-stars__track` và `.rating-stars__fill` đều `position: absolute` nên container `.rating-stars` không tự có kích thước từ 2 lớp sao (chỉ còn div "X/5" trong flow) → khi ráp vào Collection/Product Card thật sẽ set `width`/`height` cố định cho container; nếu lúc đó thấy sao đè lên chữ rating thì quay lại sửa ở `src/scss/components/_rating-stars.scss`.
   - Class `rating-stars_value` (1 gạch dưới) lệch BEM so với `__track`/`__fill` (2 gạch dưới) — gộp sửa chung 1 lượt dọn naming ở cuối project, không sửa riêng lẻ bây giờ.
5. **Bước 4 — Build từng trang theo đúng thứ tự roadmap: Header/Footer (13-14) → Homepage → Product Page (15-16) → Category (17) → Cart (18)** — **ĐANG LÀM, Header xong, tiếp tục sang Footer**. Mỗi section vẫn theo phương pháp đã luyện: **HTML tĩnh (dữ liệu giả) → thay dần bằng Liquid động (dùng kỹ thuật `render for as` — Ngày 6 — để loop `product-card` qua danh sách sản phẩm) → thêm `{% schema %}` cuối cùng, thêm dần từng setting một, test bằng Theme Editor sau mỗi lần thêm**.

   **✅ `sections/header.liquid` — ĐÃ XONG (tự tay làm trực tiếp trong IDE, agent chỉ verify):**
   - ✅ HTML tĩnh: announcement-bar + header (logo, nav, search, cart/account icon) — 4 icon SVG mới: `assets/icon-menu.svg`, `icon-search.svg`, `icon-close.svg`, `icon-chevron-down.svg`.
   - ✅ Utility `.container`/`.container-fullwidth` mới — `src/scss/base/_container.scss`, dùng chung cho mọi section sau này (không chỉ header): `max-width: calc(1240px + var(--page-margin) * 2)` để trừ `padding-inline` xong content vẫn đúng 1240px theo Figma (do `box-sizing: border-box` toàn theme).
   - ✅ Liquid động — phần bind vào object có sẵn (không cần schema): cart icon (`cart.item_count` + badge `.header__cart-count`), account icon (`routes.account_url`), search form (`<form action="{{ routes.search_url }}" method="get">` + `name="q"` + `value="{{ search.terms }}"`).
   - ✅ Nav — code loop `section.settings.menu.links` đã paste vào file thật (verify xong, dùng ở cả `.header__nav` desktop và `.mobile-nav__links` mobile).
   - ✅ `{% schema %}` cho header: setting `menu` (`link_list`, default `main-menu`) + 3 setting announcement-bar (`announcement_text`, `announcement_link_url`, `announcement_link_text`).
   - ✅ **Phát sinh thêm ngoài kế hoạch ban đầu** — hành vi tương tác (mobile nav + đóng announcement-bar), làm bằng `<dialog>` + `{% javascript %}` ngay trong `header.liquid`:
     - `.announcement-bar__close` → JS remove `.announcement-bar` khỏi DOM.
     - `.header__menu-toggle` (hamburger, hiện ở mobile) → mở `<dialog class="mobile-nav">` bằng `showModal()`; đóng bằng nút X hoặc click ra ngoài (`event.target === mobileNav`, kiểm tra click trúng chính `<dialog>` tức là click ra backdrop).
     - SCSS mới `src/scss/components/_mobile-nav.scss` (63 dòng, animation `transform` + `@starting-style`/`allow-discrete` cho hiệu ứng slide-in/out chuẩn `<dialog>` mới của trình duyệt) — đã `@use` vào `main.scss`, build ra `theme.css` sạch.
     - `.announcement-bar`/`.header` đổi thêm class `full-width` (utility grid có sẵn từ skeleton gốc ở `assets/critical.css`, không phải bug).
   - `theme check` sau cùng: vẫn sạch, 43 file, chỉ 2 warning `OrphanedSnippet` đã biết (không liên quan header).
   - ❌ `sections/footer.liquid` — **chưa bắt đầu, làm tiếp theo**.

### Lỗi/bài học đã rút ra, nhớ áp dụng tiếp
- Luôn chạy `shopify theme check` sau mỗi thay đổi — đã từng bắt được lỗi locale key sai (`cart.general.title` không tồn tại, phải dùng đúng `cart.title` khớp `en.default.json`).
- Đối chiếu `id` giữa nơi khai báo setting và nơi dùng biến CSS — dễ gõ nhầm (VD Ngày 11: `buttno_corner_radius` vs `button_corner_radius`).
- Các file trong theme (`my-first-theme` lẫn `ecommerce-theme`) đều có thể **xoá sạch viết lại từ đầu** khi thực hành — không cần giữ nguyên code skeleton gốc (agent đã ghi nhớ điều này trong memory, không cảnh báo khi thấy file bị ghi đè toàn bộ).
- Warning `OrphanedSnippet` của `theme check` xét theo **reachability xuyên chuỗi từ entry point thật** (layout → section/template → render), không phải "có ai render tên này không" theo kiểu grep phẳng. Nên nếu snippet A gọi `render` snippet B, nhưng A tự nó chưa được section/template nào gọi tới, thì B vẫn bị báo orphan luôn dù code có render B thật (case `product-card` → `rating-stars` ở Bước 3). Không phải bug, tự hết khi A được wire vào 1 section thật.
- `sections/header-group.json` / `footer-group.json` là JSON auto-generated có comment header `/* ... */` — Shopify tự strip trước khi parse nên hợp lệ, nhưng **vẫn phải tự soi trailing comma** vì `theme check` không luôn bắt được (đã gặp ở `header-group.json`, sửa xong).
- Chỉ dùng SCSS mixin khi pattern lặp lại ≥ 2 nơi (VD `respond-to` dùng chung breakpoint toàn theme) — pattern chỉ 1 nơi dùng (VD `flex-center` cho riêng announcement-bar) thì viết thẳng CSS, gọi qua mixin chỉ gây phải nhảy file đọc thêm không cần thiết.
- `box-sizing: border-box` (set global ở `critical.css`) làm `padding-inline` ăn vào `max-width` — muốn content bên trong đúng N px theo Figma thì `max-width: calc(Npx + var(--page-margin) * 2)`, không phải `max-width: Npx` trần.
- Phân loại "Liquid động": bind vào **object có sẵn** (`cart`, `routes`, `search`...) làm được ngay không cần schema; phần cần **setting merchant tự nhập** (menu, text tuỳ chỉnh) phải viết code Liquid trước (tạm rỗng) rồi mới thêm schema sau — đúng thứ tự đã thống nhất, không đảo ngược. Chi tiết pattern `<form method="get">` cho search (vì sao GET không phải POST, `name="q"` là key cố định) đã lưu trong `shopify-liquid-summary.md` (Ngày 13).
- Comment inline Liquid `{% # ... %}` nên viết **gọn trong 1 dòng** — viết tràn nhiều dòng vật lý (mở tag ở dòng đầu, đóng `%}` ở dòng cuối) tuy `shopify theme check` không bắt lỗi nhưng dễ gây hiểu nhầm/hiển thị sai ở syntax highlighter của editor. Nếu cần comment dài, tách thành nhiều tag `{% # %}` riêng, mỗi dòng một tag.
- Tương tác UI đơn giản (mở/đóng menu, dismiss banner) nên ưu tiên `<dialog>` native (`showModal()`/`close()`) thay vì tự quản lý `display`/class toggle bằng JS — có sẵn `::backdrop`, focus-trap, đóng bằng phím Esc, và animate được bằng CSS thuần qua `@starting-style` + `transition-behavior: allow-discrete` (không cần thư viện ngoài).

---

## ✅ Việc cần làm ngay khi tiếp tục
`sections/header.liquid` đã **xong hoàn toàn** — HTML tĩnh, Liquid động, `{% schema %}` (menu + announcement-bar), và cả phần tương tác mobile nav/JS phát sinh thêm. Design tokens (`config/settings_schema.json` + `snippets/css-variables.liquid`) cũng đã đối chiếu lại với Figma qua API, sạch `theme check`. **Việc tiếp theo**: làm `sections/footer.liquid` (Ngày 14) theo đúng quy trình 3 bước đã luyện (HTML tĩnh → Liquid động → schema thêm dần từng setting, test Theme Editor sau mỗi lần thêm). Snippet dùng chung (`product-card`, `rating-stars`) đã có sẵn từ Bước 3, chưa dùng tới — 2 warning `OrphanedSnippet` sẽ tự hết khi Homepage render chúng.

### Quy tắc làm việc mới (từ Bước 2 trở đi)
Agent **không tự sửa code trực tiếp vào file theme** nữa — chỉ **hướng dẫn từng bước** (giải thích cần sửa gì, ở đâu, tại sao), người dùng tự gõ code. Agent chỉ verify (theme check, đọc lại file) sau khi người dùng báo đã làm xong. Ngoại lệ: các file tracking tiến độ (`docs/continue-here.md`) và chạy lệnh CLI kiểm tra/pull vẫn agent tự làm được, không tính là "code vào theme".

---
