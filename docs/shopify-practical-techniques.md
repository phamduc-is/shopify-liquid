# 💡 Shopify Theme Development — Sổ Tay Kỹ Thuật Thực Tế & Tips Tối Ưu (Best Practices)

Tài liệu này lưu trữ các kỹ thuật lập trình Liquid, tối ưu hiệu năng (Performance), giao diện (UI/UX) và chuẩn SEO được áp dụng trực tiếp trong các dự án Shopify Theme chuyên nghiệp.

---

## 📌 MỤC LỤC KỸ THUẬT

1. [Kỹ thuật 1: Aspect Ratio Box & Chống Giật Lag CLS (Cumulative Layout Shift)](#1-kỹ-thuật-1-aspect-ratio-box--chống-giật-lag-cls)
2. [Kỹ thuật 2: Responsive Images (srcset & sizes) Tối Ưu Băng Thông Mobile](#2-kỹ-thuật-2-responsive-images-srcset--sizes-tối-ưu-băng-thông-mobile)
3. [Kỹ thuật 3: Isolated Scope Component & Thẻ Render Lặp Tối Ưu (`render for as`)](#3-kỹ-thuật-3-isolated-scope-component--thẻ-render-lặp-tối-ưu-render-for-as)
4. [Kỹ thuật 4: Filter Chaining Làm Sạch SEO Meta Description](#4-kỹ-thuật-4-filter-chaining-làm-sạch-seo-meta-description)
5. [Kỹ thuật 5: Fallback Mock Data Khi Cửa Hàng Chưa Có Sản Phẩm](#5-kỹ-thuật-5-fallback-mock-data-khi-cửa-hàng-chưa-có-sản-phẩm)
6. [Kỹ thuật 6: `Handle` — Định Danh URL Của Resource & Cách Test Nhanh 1 Trang](#6-kỹ-thuật-6-handle--định-danh-url-của-resource--cách-test-nhanh-1-trang)
7. [Kỹ thuật 7: `<shopify-account>` — Custom Element Có Sẵn Cho Khu Vực Tài Khoản](#7-kỹ-thuật-7-shopify-account--custom-element-có-sẵn-cho-khu-vực-tài-khoản)
8. [Kỹ thuật 8: Section Rendering API — Section Phụ Thuộc Data Nội Bộ Của Shopify](#8-kỹ-thuật-8-section-rendering-api--section-phụ-thuộc-data-nội-bộ-của-shopify)

---

## 1. Kỹ thuật 1: Aspect Ratio Box & Chống Giật Lag CLS

### 🚨 Vấn đề thực tế (Problem):
Khi trang web đang tải, ảnh sản phẩm chưa về kịp. Trình duyệt không biết ảnh cao bao nhiêu nên để chiều cao bằng `0px`. Khi ảnh tải xong, khung ảnh đột ngột phồng to ra làm toàn bộ chữ và nội dung bên dưới bị đẩy xuống mạnh. Hiện tượng này gọi là **CLS (Cumulative Layout Shift)** làm điểm Google Speed Insights bị giảm nghiêm trọng.

### 💡 Giải pháp (Solution):
Sử dụng công thức tính % tỷ lệ khung hình ảnh trong Liquid:
$$\text{Ratio (\%)} = \left( \frac{\text{Height}}{\text{Width}} \right) \times 100$$

Gán `padding-top: {{ ratio }}%` cho container bọc ngoài và đặt ảnh ở dạng `position: absolute; top: 0; left: 0; width: 100%; height: 100%; object-fit: cover;`.

### 💻 Ví dụ Code Thực Tế:

```liquid
{% comment %} 1. Tính toán Aspect Ratio trong khối liquid {% endcomment %}
{%- liquid
  assign ratio = 100.0
  if product.featured_image and product.featured_image.width > 0
    assign ratio = product.featured_image.height | divided_by: product.featured_image.width | times: 100.0
  endif
-%}

{% comment %} 2. Áp dụng ratio vào style padding-top của Container {% endcomment %}
<div 
  class="product-card__image-wrapper" 
  style="padding-top: {{ ratio }}%; position: relative; overflow: hidden; background: #f9f9f9;"
>
  {%- if product.featured_image -%}
    <img
      src="{{ product.featured_image | image_url: width: 400 }}"
      alt="{{ product.featured_image.alt | escape }}"
      width="{{ product.featured_image.width }}"
      height="{{ product.featured_image.height }}"
      style="position: absolute; top: 0; left: 0; width: 100%; height: 100%; object-fit: cover;"
    >
  {%- endif -%}
</div>
```

---

## 2. Kỹ thuật 2: Responsive Images (`srcset` & `sizes`) Tối Ưu Băng Thông Mobile

### 🚨 Vấn đề thực tế (Problem):
Nếu load 1 tấm ảnh dung lượng lớn (1200px) cho điện thoại di động (màn hình 375px), khách hàng sẽ tốn nhiều dung lượng 4G và trang web bị tải chậm. Nếu chỉ dùng ảnh nhỏ (300px), hiển thị trên máy tính 4K sẽ bị mờ vỡ nét.

### 💡 Giải pháp (Solution):
Kết hợp filter `image_url: width: N` của Shopify CDN với thuộc tính `srcset` và `sizes` của HTML5.

### 💻 Ví dụ Code Thực Tế:

```liquid
<img
  src="{{ product.featured_image | image_url: width: 400 }}"
  srcset="
    {{ product.featured_image | image_url: width: 200 }} 200w,
    {{ product.featured_image | image_url: width: 400 }} 400w,
    {{ product.featured_image | image_url: width: 600 }} 600w,
    {{ product.featured_image | image_url: width: 800 }} 800w
  "
  sizes="(max-width: 768px) 50vw, (max-width: 1024px) 33vw, 25vw"
  alt="{{ product.featured_image.alt | escape }}"
  loading="lazy"
  width="{{ product.featured_image.width }}"
  height="{{ product.featured_image.height }}"
  style="position: absolute; top: 0; left: 0; width: 100%; height: 100%; object-fit: cover;"
>
```

#### 🔍 Giải thích thông số:
- **`200w`, `400w`, `600w`**: Khai báo cho trình duyệt biết kích thước thực tế của từng file ảnh do Shopify CDN cắt ra.
- **`sizes`**: Thỏa thuận với trình duyệt:
  - Nếu màn hình $\le 768\text{px}$ (Mobile): Ảnh chiếm $50\%$ chiều rộng màn hình (`50vw`). Trình duyệt tự chọn tải file `200w` hoặc `400w`.
  - Nếu màn hình $\le 1024\text{px}$ (Tablet): Ảnh chiếm $33\%$ chiều rộng màn hình (`33vw`).
  - Màn hình Desktop: Ảnh chiếm $25\%$ chiều rộng màn hình (`25vw`). Trình duyệt chọn tải file `600w` hoặc `800w`.

---

## 3. Kỹ thuật 3: Isolated Scope Component & Thẻ Render Lặp Tối Ưu (`render for as`)

### 🚨 Vấn đề thực tế (Problem):
Khi nhúng một partial template nhiều lần bằng thẻ `include`, các biến rác từ file cha bị lọt vào làm đụng độ tên biến và giảm tốc độ render. Khi lặp `for` lồng thẻ `render`, hệ thống tốn tài nguyên gọi đi gọi lại.

### 💡 Giải pháp (Solution):
Sử dụng thẻ `render` với cơ chế **Isolated Scope (Scope cô lập)** và cú pháp lặp siêu tốc `render 'snippet' for array as item`.

### 💻 Ví dụ Code Thực Tế:

#### Cấu trúc Snippet `snippets/product-card.liquid`:
```liquid
{% doc %}
  @file snippets/product-card.liquid
  @description Component hiển thị thẻ sản phẩm chuẩn OS 2.0
  @param product {Object} Object sản phẩm truyền vào
  @param show_vendor {Boolean} Cho phép hiển thị nhà sản xuất
{% enddoc %}

<div class="product-card">
  <h3>{{ product.title }}</h3>
  {% if show_vendor %}<p>{{ product.vendor }}</p>{% endif %}
</div>
```

#### Gọi render danh sách tại `layout/theme.liquid` hoặc Collection Page:
```liquid
{% comment %} Tối ưu hiệu năng gấp 5 lần so với vòng lặp for thông thường {% endcomment %}
<div class="product-grid">
  {% render 'product-card' for collections.all.products as product, show_vendor: true %}
</div>
```

---

## 4. Kỹ thuật 4: Filter Chaining Làm Sạch SEO Meta Description

### 🚨 Vấn đề thực tế (Problem):
Nội dung mô tả sản phẩm/bài viết nhập trong Admin thường chứa các thẻ HTML (`<p>`, `<span>`) hoặc các ký tự ngoặc kép (`"`). Nếu chèn trực tiếp vào thẻ `<meta name="description" content="...">`, HTML của trang web sẽ bị vỡ khung và Google Search không thể đọc được.

### 💡 Giải pháp (Solution):
Nối chuỗi filter `default` chọn nguồn dữ liệu linh hoạt, sau đó cho chạy qua chuỗi filter làm sạch: `strip_html` -> `truncate: 160` -> `escape`.

### 💻 Ví dụ Code Thực Tế:

```liquid
<meta
  name="description"
  content="{{ page_description | default: shop.description | default: 'Cửa hàng thời trang cao cấp' | strip_html | truncate: 160 | escape }}"
>
```

#### 🔍 Thứ tự chạy của dữ liệu:
1. `page_description`: Lấy mô tả trang hiện tại.
2. `default: shop.description`: Nếu rỗng, tự lấy mô tả chung của cửa hàng.
3. `default: 'Cửa hàng thời trang cao cấp'`: Nếu cả 2 đều rỗng, lấy câu mặc định.
4. `strip_html`: Xóa bỏ toàn bộ thẻ HTML.
5. `truncate: 160`: Cắt chuẩn 160 ký tự theo quy định SEO của Google.
6. `escape`: Mã hóa ký tự đặc biệt như `"` thành `&quot;` để không vỡ thẻ `<meta>`.

---

## 5. Kỹ thuật 5: Fallback Mock Data Khi Cửa Hàng Chưa Có Sản Phẩm

### 🚨 Vấn đề thực tế (Problem):
Khi khách hàng mới cài đặt Theme hoặc khi cửa hàng chưa có dữ liệu sản phẩm thật, trang web sẽ bị trống trơn làm giao diện xấu và khó xem trước layout.

### 💡 Giải pháp (Solution):
Sử dụng phép kiểm tra `collections.all.products.size == 0` kết hợp vòng lặp giả lập `(1..4)` và filter `placeholder_svg_tag`.

### 💻 Ví dụ Code Thực Tế:

```liquid
{% if collections.all.products.size == 0 %}
  <div class="product-grid">
    {% for i in (1..4) %}
      {% capture placeholder %}product-{{ i }}{% endcapture %}
      <div class="product-card product-card--mock">
        {{ placeholder | placeholder_svg_tag: 'placeholder-svg' }}
        
        {% case i %}
          {% when 1 %}
            <span class="badge badge--sale">Sale</span>
          {% when 2 %}
            <span class="badge badge--soldout">Hết hàng</span>
          {% when 3 %}
            <span class="badge badge--new">Mới</span>
        {% endcase %}

        <h3>Sản phẩm mẫu {{ i }}</h3>
        <p class="price">$29.99</p>
      </div>
    {% endfor %}
  </div>
{% else %}
  <div class="product-grid">
    {% render 'product-card' for collections.all.products as product %}
  </div>
{% endif %}
```

---

## 6. Kỹ thuật 6: `Handle` — Định Danh URL Của Resource & Cách Test Nhanh 1 Trang

### 🚨 Vấn đề thực tế (Problem):
Khi mới học Liquid, rất dễ nhầm giữa **tên hiển thị** (title) và **định danh dùng trong URL** của 1 collection/product/page. Muốn mở thử `sections/collection.liquid` trên trình duyệt nhưng không biết gõ URL nào, hoặc gọi nhầm `{% render 'collection' %}` (chỉ dùng được cho snippet) thay vì cách đúng để xem 1 section phụ thuộc object động như `collection`.

### 💡 Giải pháp (Solution):
**`handle`** là chuỗi định danh duy nhất, dạng slug (chữ thường, không dấu, cách nhau bằng `-`), được Shopify **tự sinh ra** từ title khi tạo resource (collection, product, page, blog, article, menu...). Handle chính là phần xuất hiện trong URL, khác với `title` (dùng để hiển thị cho người dùng đọc).

| Field | Ý nghĩa | Ví dụ |
|---|---|---|
| `collection.title` | Tên hiển thị | `Áo Thun Nam` |
| `collection.handle` | Định danh trong URL | `ao-thun-nam` |
| URL thực tế | `title` + `handle` kết hợp qua route | `/collections/ao-thun-nam` |

Mỗi loại resource có route riêng gắn với handle của nó:
```liquid
/collections/{{ collection.handle }}
/products/{{ product.handle }}
/pages/{{ page.handle }}
/blogs/{{ blog.handle }}/{{ article.handle }}
```

Shopify luôn có sẵn 1 collection đặc biệt chứa **toàn bộ sản phẩm** với handle cố định là `all` — dùng để test nhanh khi chưa tạo collection riêng nào:
```
/collections/all
```

### 💻 Ví dụ Code Thực Tế:

```liquid
{% comment %} Lấy handle để tự build link, thay vì hard-code URL {% endcomment %}
<a href="/collections/{{ collection.handle }}">{{ collection.title }}</a>

{% comment %} Cách chuẩn hơn — dùng sẵn field .url đã build route đúng {% endcomment %}
<a href="{{ collection.url }}">{{ collection.title }}</a>
```

#### 🔍 Lưu ý quan trọng khi kiểm thử section phụ thuộc object động:
- `{% render %}` **chỉ render được snippet** (`/snippets`). Muốn nhúng section thủ công phải dùng `{% section 'ten-section' %}`.
- Nhưng 1 section như `collection.liquid` phụ thuộc global object `collection` — object này **chỉ tự động có giá trị khi bạn đang đứng đúng route `/collections/<handle>`**. Nhúng section đó vào `layout/theme.liquid` (chạy trên mọi trang) sẽ khiến `collection` = `nil` ở các trang khác, gây lỗi hoặc không hiển thị gì.
- → Cách đúng để test: **truy cập trực tiếp URL route tương ứng** (`/collections/all` hoặc `/collections/<handle-thật>`) trên bản preview, không cần render thủ công.

---

## 7. Kỹ thuật 7: `<shopify-account>` — Custom Element Có Sẵn Cho Khu Vực Tài Khoản

### 🚨 Vấn đề thực tế (Problem):
Icon tài khoản trên header cần xử lý 2 trạng thái hoàn toàn khác nhau: khách **chưa đăng nhập** (hiện dropdown "Đăng nhập/Đăng ký") và khách **đã đăng nhập** (hiện dropdown "Đơn hàng của tôi/Đăng xuất"). Tự viết Liquid + JS xử lý toàn bộ luồng này (session, dropdown, đóng khi click ra ngoài...) tốn nhiều công sức và dễ sai sót bảo mật.

### 💡 Giải pháp (Solution):
Dùng **Custom Element** (Web Component) `<shopify-account>` — Shopify tự cung cấp sẵn qua script nạp trong `{{ content_for_header }}` (Ngày 7). Đây **không phải thẻ HTML chuẩn**, trình duyệt không tự hiểu — script của Shopify sẽ "nâng cấp" thẻ này thành UI tương tác thật, tự xử lý toàn bộ logic trạng thái đăng nhập.

### 💻 Ví dụ Code Thực Tế:

```liquid
{% if shop.customer_accounts_enabled %}
  <shopify-account menu="{{ section.settings.customer_account_menu }}">
    {{ 'icon-account.svg' | inline_asset_content }}
  </shopify-account>
{% endif %}
```

#### 🔍 Bóc tách từng phần:
- **`shop.customer_accounts_enabled`**: property của Global Object `shop` — kiểm tra merchant có bật tính năng tài khoản khách hàng hay không (có thể tắt hẳn trong Admin).
- **`menu="{{ section.settings.customer_account_menu }}"`**: truyền handle của 1 `link_list` setting để component biết hiển thị menu nào trong dropdown.
- **`inline_asset_content`**: filter nhúng thẳng **nội dung SVG** vào HTML (khác `asset_url` chỉ ra link tới file) — cho phép CSS chỉnh `fill`/màu icon trực tiếp bằng `svg { fill: ... }`, điều mà `<img src="...svg">` không làm được.

#### ⚠️ Lưu ý khi dùng:
`<shopify-account>` chỉ hoạt động đúng khi được đặt bên trong 1 trang có nạp đầy đủ `content_for_header` (tức mọi trang dùng `layout/theme.liquid` bình thường) — không tự viết logic phân biệt đăng nhập/chưa đăng nhập bằng `{% if customer %}` thủ công nữa, để component tự xử lý.

---

## 8. Kỹ thuật 8: Section Rendering API — Section Phụ Thuộc Data Nội Bộ Của Shopify

### 🚨 Vấn đề thực tế (Problem):
Section "You might also like" (related products) cần danh sách **thay đổi theo từng sản phẩm** đang xem. Ba cách làm quen tay đều sai:

| Cách làm | Vì sao sai |
|---|---|
| Hardcode danh sách trong section settings | Settings thuộc **template**, không thuộc product → cả 500 sản phẩm hiện đúng 4 cái giống nhau |
| Lấy `product.collections.first.products` | Chỉ là "cùng collection", không phản ánh hành vi mua hàng |
| Đọc `product.related_products` trong Liquid | **Không tồn tại** — Liquid không có field nào chứa data này |

Data gợi ý nằm trong **recommendation index nội bộ của Shopify**, không gắn trên product object. Ở lượt render thường, object `recommendations` luôn rỗng (`performed? == false`).

### 💡 Giải pháp (Solution):
**Section Rendering API** — render section **2 lượt**:
1. **Lượt 1** (server, từ `product.json`): `recommendations.performed == false` → section cố tình in ra vỏ rỗng, kèm URL trong `data-*`.
2. **Lượt 2** (browser, JS fetch): gọi endpoint `/{locale}/recommendations/products` với param `section_id` → Shopify **render lại chính section đó**, lần này `performed == true`, rồi JS thay nội dung vào.

Điểm mấu chốt là param **`section_id`**: có nó thì Shopify trả về **HTML đã render sẵn** từ chính file `.liquid` của mình; không có nó thì trả **JSON** — và mình sẽ phải viết lại toàn bộ markup product card bằng JavaScript (markup tồn tại 2 bản, sửa design phải sửa 2 chỗ).

### 💻 Ví dụ Code Thực Tế:

```liquid
{%- comment -%}
  Liquid không có toán tử `+=`, nên nối chuỗi bằng `assign x = x | append: ...`.
  Trong {% liquid %} mỗi dòng là 1 tag riêng — không xuống dòng giữa chuỗi filter.
{%- endcomment -%}
{%- liquid
  assign recommendations_url = routes.product_recommendations_url | append: '?section_id=' | append: section.id
  assign recommendations_url = recommendations_url | append: '&product_id=' | append: product.id
  assign recommendations_url = recommendations_url | append: '&limit=' | append: section.settings.products_to_show
  assign recommendations_url = recommendations_url | append: '&intent=' | append: section.settings.intent
-%}

<related-products class="related-products" data-recommendations-url="{{ recommendations_url }}">
  {%- if recommendations.performed and recommendations.products_count > 0 -%}
    <h2>{{ section.settings.heading }}</h2>
    <ul class="related-products__grid">
      {%- for recommended_product in recommendations.products -%}
        <li>{% render 'product-card', product: recommended_product %}</li>
      {%- endfor -%}
    </ul>
  {%- endif -%}
</related-products>
```

```js
class RelatedProducts extends HTMLElement {
  // connectedCallback tự chạy mỗi lần node vào DOM -> Theme Editor render lại
  // section là tự init, KHÔNG cần listener 'shopify:section:load'.
  connectedCallback() {
    if (!this.dataset.recommendationsUrl || this.querySelector('.related-products__grid')) return;

    // Section nằm cuối trang: chỉ fetch khi user scroll gần tới.
    this.observer = new IntersectionObserver(
      (entries, observer) => {
        if (!entries[0].isIntersecting) return;
        observer.unobserve(this);
        this.loadRecommendations();
      },
      { rootMargin: '0px 0px 400px 0px' }
    );
    this.observer.observe(this);
  }

  disconnectedCallback() {
    this.observer?.unobserve(this);
  }

  async loadRecommendations() {
    const response = await fetch(this.dataset.recommendationsUrl);
    if (!response.ok) return;

    // DOMParser tạo document riêng -> custom element trong đó KHÔNG được
    // upgrade -> connectedCallback không chạy -> không fetch đệ quy.
    const html = await response.text();
    const fresh = new DOMParser().parseFromString(html, 'text/html').querySelector('related-products');
    if (!fresh || !fresh.querySelector('.related-products__grid')) return;

    // innerHTML chứ không outerHTML: outerHTML xoá luôn data-attribute và
    // shopify_attributes của section, làm hỏng Theme Editor.
    this.innerHTML = fresh.innerHTML;
  }
}

if (!customElements.get('related-products')) {
  customElements.define('related-products', RelatedProducts);
}
```

#### 🔍 Bóc tách 4 param trong URL:

| Param | Trả lời câu hỏi | Thiếu nó thì sao |
|---|---|---|
| `section_id` | Render **section nào**? | Shopify trả **JSON** thay vì HTML |
| `product_id` | Gợi ý cho **sản phẩm nào**? | Endpoint là URL độc lập, không biết đang ở trang product nào → không trả gì |
| `limit` | Lấy **bao nhiêu** sản phẩm? | Mặc định 5, không khớp setting (tối đa 10) |
| `intent` | **Loại** gợi ý nào? | Mặc định `related` |

#### 🔍 Cơ chế `intent` — 2 nguồn, 1 endpoint:

| | `intent=related` | `intent=complementary` |
|---|---|---|
| Ai quyết định danh sách | Thuật toán Shopify | Merchant nhập tay trong app **Search & Discovery** |
| Tính lại theo thời gian | ✅ đơn hàng mới → kết quả đổi | ❌ cố định đến khi merchant sửa |
| Cần cài app | ❌ | ✅ |
| Code theme cần đổi | Không gì cả — chỉ đổi 1 setting | |

> Search & Discovery **không phải** server riêng để "trỏ vào" — nó chỉ là UI cho merchant nhập liệu; data được Shopify lưu rồi phục vụ lại qua **cùng endpoint đó**.

#### 🔍 Thuật toán `related` dựa trên gì (theo docs Shopify):

| Tiêu chí | Nội dung | Khi nào dùng |
|---|---|---|
| **Purchase history** | Sản phẩm từng được mua **cùng nhau** trong 1 order | Mọi store |
| **Product description** | Sản phẩm có **mô tả tương tự** | Mọi store |
| **Related collections** | Collection mà product hiện tại cũng thuộc, **loại trừ** handle `all` và `frontpage` | Chỉ khi 2 tiêu chí trên không có data |

Ngược lại với trực giác: thuật toán **không dùng** `tags`, `product_type`, hay `title`. Nhưng **`description` có ảnh hưởng thật** — mô tả copy-paste giống nhau sẽ làm gợi ý sai lệch.

#### ⚠️ Lưu ý khi dùng:

- **Section trống trên dev store là ĐÚNG, không phải bug.** Docs ghi rõ: không tính đơn hàng import từ platform khác; loại trừ sản phẩm hết hàng, giá = 0, gift card, và sản phẩm đang trong cart của khách. Dev store chưa có đơn hàng thật → mất tiêu chí mạnh nhất; sản phẩm chỉ thuộc `all`/`frontpage` → fallback cũng bị loại.
- **Không can thiệp được đầu vào thuật toán**, nhưng **lọc được đầu ra**: *"You can't customize the recommendation algorithm to exclude specific products. However, you can choose which of the returned products to show with JavaScript."*
- **Vị trí đặt section theo khuyến nghị docs**: `related` → cuối trang (section "You might also like"); `complementary` → **gần đầu trang**, trong khu product information cạnh ảnh sản phẩm (nên làm block trong product section, không phải section riêng ở cuối). Đây là lý do Dawn tách làm 2 chỗ.
- **Giới hạn 4 sản phẩm** dù API trả tối đa 10 — độ liên quan giảm dần theo thứ tự, sản phẩm thứ 10 gần như vô nghĩa.
- **Custom element thắng `querySelectorAll` + `shopify:section:load`**: `connectedCallback` tự chạy khi node vào DOM nên Theme Editor render lại section là tự init. Cả một lớp bug "listener chết sau khi section reload" biến mất, không phải nhớ gắn listener cho từng section.

#### 🗺️ Mental model — phân loại section theo nguồn data:

| Loại | Nguồn data | Ví dụ | Test được ngay trên dev store? |
|---|---|---|---|
| **Presentation** | Setting của section | slideshow, testimonials, logo-list, footer | ✅ luôn thấy |
| **Store data** | Object Liquid có sẵn | featured-collection (`collections[...]`), product | ✅ nếu có sản phẩm |
| **Platform-computed** | API riêng, 2 lượt render | **related-products** | ❌ cần đơn hàng thật hoặc app |

Phần lớn section thuộc 2 loại đầu — code xong là thấy kết quả. Loại thứ 3 viết đúng 100% vẫn có thể ra section trống.
