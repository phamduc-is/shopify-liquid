# 📝 Test kiến thức — Nhóm 6: Setting Types (Section Schema)

> Làm hết phần Câu hỏi trước, KHÔNG kéo xuống xem Đáp án sớm.

---

## 📋 Câu hỏi

### Trắc nghiệm

**Câu 1.** Setting sau đây trả về kiểu dữ liệu gì khi dùng trong Liquid?
```json
{ "type": "text", "id": "heading", "label": "Tiêu đề" }
```
- A. Number
- B. String
- C. Boolean
- D. Object (cần filter mới lấy được text)

**Câu 2.** `textarea` khác `text` ở điểm nào?
- A. `textarea` trả về Array các dòng, `text` trả về String
- B. `textarea` cho merchant nhập nhiều dòng văn bản (multi-line), nhưng vẫn trả về **string thuần** (không phải HTML), khác với `text` chỉ nhập 1 dòng
- C. `textarea` bắt buộc phải có `options`, `text` thì không
- D. Không có khác biệt, hai type là alias của nhau

**Câu 3.** Cho setting:
```json
{ "type": "richtext", "id": "description", "label": "Mô tả" }
```
Khi render `{{ section.settings.description }}` ngoài storefront, giá trị trả về có đặc điểm gì?
- A. String thuần, phải tự thêm thẻ `<p>` mới hiển thị đúng định dạng
- B. String đã có sẵn các thẻ HTML (VD `<p>`, `<strong>`, `<em>`) do Theme Editor sinh ra từ rich text editor, có thể render thẳng mà không cần filter
- C. Object chứa các field `body`, `format` giống `image_picker`
- D. Trả về mảng các đoạn văn (paragraph array)

**Câu 4.** Setting `select` sau đây trả về giá trị gì khi merchant chọn "Nhỏ"?
```json
{
  "type": "select",
  "id": "size",
  "label": "Kích cỡ",
  "options": [
    { "value": "small", "label": "Nhỏ" },
    { "value": "medium", "label": "Vừa" },
    { "value": "large", "label": "Lớn" }
  ]
}
```
- A. `"Nhỏ"` — trả về `label`
- B. `"small"` — trả về `value` của option đã chọn
- C. `0` — trả về index của option trong mảng `options`
- D. Object `{ value: "small", label: "Nhỏ" }` đầy đủ

**Câu 5.** `radio` và `select` giống nhau ở điểm cốt lõi nào?
- A. Cả hai đều bắt buộc có field `options: [{value,label}]` và đều trả về `value` đã chọn — chỉ khác nhau ở giao diện hiển thị (radio button vs dropdown) trong Theme Editor
- B. `radio` trả về Boolean, `select` trả về String
- C. `radio` cho phép chọn nhiều giá trị cùng lúc, `select` chỉ chọn 1
- D. `select` cần `min`/`max`, `radio` thì không

**Câu 6.** Setting `checkbox` trả về kiểu dữ liệu gì, và cách dùng phổ biến nhất trong Liquid là gì?
- A. String `"true"`/`"false"`, dùng trực tiếp trong `{{ }}`
- B. Boolean `true`/`false`, thường dùng trong `{% if section.settings.show_badge %}` để bật/tắt hiển thị 1 phần UI
- C. Number `1`/`0`
- D. Object chứa `checked` và `label`

**Câu 7.** Cho setting:
```json
{
  "type": "range",
  "id": "padding",
  "label": "Khoảng đệm",
  "min": 0,
  "max": 100,
  "step": 5,
  "unit": "px",
  "default": 20
}
```
Field nào trong số này quyết định **bước nhảy** mỗi lần merchant kéo thanh trượt, và giá trị trả về trong Liquid là kiểu gì?
- A. `unit` quyết định bước nhảy; trả về String có kèm đơn vị VD `"20px"`
- B. `step` quyết định bước nhảy (ở đây mỗi lần kéo tăng/giảm 5); trả về **Number thuần** (VD `20`), `unit` chỉ là nhãn hiển thị trong UI, KHÔNG tự động gắn vào giá trị trả về
- C. `max` quyết định bước nhảy; trả về Object `{value, unit}`
- D. `min` quyết định bước nhảy; trả về Boolean

**Câu 8.** Setting `color` trả về giá trị dạng nào?
- A. Object `{r, g, b}`
- B. String mã màu, VD `"#ff0000"` hoặc `"rgba(255,0,0,0.5)"`
- C. Number (mã hex dạng thập phân)
- D. Boolean `true` nếu có màu, `false` nếu không

**Câu 9.** Đây là bẫy hay gặp nhất khi dùng `image_picker`. Cho setting:
```json
{ "type": "image_picker", "id": "banner_image", "label": "Ảnh banner" }
```
Cách nào sau đây render đúng ảnh ra thẻ `<img>`?
- A. `<img src="{{ section.settings.banner_image }}">` — vì `image_picker` đã trả sẵn string URL
- B. `<img src="{{ section.settings.banner_image | image_url: width: 800 }}">` — vì `image_picker` trả về **image object**, bắt buộc qua filter `image_url` (thường kèm `width`) mới ra được URL dùng được
- C. `<img src="{{ section.settings.banner_image.src }}">` — truy cập trực tiếp field `.src` của object
- D. `<img src="{{ section.settings.banner_image | img_url }}">` — dùng filter `img_url` (không có `image_url`)

**Câu 10.** Setting kiểu `product` sau đây:
```json
{ "type": "product", "id": "featured_product", "label": "Sản phẩm nổi bật" }
```
`section.settings.featured_product` trả về gì, và muốn lấy tên sản phẩm thì viết Liquid thế nào?
- A. Trả về string là handle sản phẩm; lấy tên bằng `{{ section.settings.featured_product }}`
- B. Trả về **full product object** (giống object `product` lấy từ `{% for product in collection.products %}`); lấy tên bằng `{{ section.settings.featured_product.title }}`
- C. Trả về ID số của sản phẩm; phải dùng `{% assign p = all_products[section.settings.featured_product] %}` mới lấy được tên
- D. Trả về mảng chứa tất cả biến thể (variants) của sản phẩm

**Câu 11.** Setting `collection` hoạt động tương tự `product` nhưng cho collection. Đoạn Liquid nào lấy đúng tiêu đề của collection đã chọn?
```json
{ "type": "collection", "id": "featured_collection", "label": "Bộ sưu tập nổi bật" }
```
- A. `{{ section.settings.featured_collection }}` — object tự động in ra title khi để trong `{{ }}`, không cần field
- B. `{{ section.settings.featured_collection.title }}` — vì setting trả về full collection object, phải truy cập field `.title`
- C. `{{ collections[section.settings.featured_collection].title }}` — bắt buộc phải tra cứu qua `collections`
- D. `{{ section.settings.featured_collection.name }}` — collection object dùng field `name` chứ không phải `title`

**Câu 12.** Setting `page` trả về gì, và muốn hiển thị **nội dung HTML** của trang đó thì dùng field nào?
```json
{ "type": "page", "id": "about_page", "label": "Trang giới thiệu" }
```
- A. Trả về string là handle của page; dùng `pages[section.settings.about_page].content`
- B. Trả về full page object trực tiếp; dùng `{{ section.settings.about_page.content }}`
- C. Trả về full page object trực tiếp; dùng `{{ section.settings.about_page.body }}` (không có field `content`)
- D. Trả về ID number; phải `{% assign pg = pages[section.settings.about_page] %}` rồi mới truy cập `.content`

**Câu 13.** Setting `text_alignment` có gì đặc biệt so với `select` thông thường?
- A. Phải tự khai báo `options` với 3 giá trị left/center/right giống `select`
- B. Tự động có sẵn 3 lựa chọn `left`/`center`/`right` (hiển thị dạng nút căn lề trực quan trong Theme Editor) mà KHÔNG cần khai báo `options`, trả về string là 1 trong 3 giá trị đó
- C. Trả về Object `{align: "left"}` thay vì string thuần
- D. Chỉ dùng được trong `blocks[]`, không dùng được trong `settings[]` của section

**Câu 14.** Setting `header` có đặc điểm gì khác biệt căn bản so với các type còn lại (như `text`, `select`...)?
```json
{ "type": "header", "content": "Cài đặt hiển thị" }
```
- A. Vẫn cần `id` như mọi setting khác, chỉ khác là label hiển thị to hơn
- B. KHÔNG có (và không cần) field `id` — vì nó không lưu giá trị gì cả, chỉ là dòng tiêu đề/UI để nhóm các setting phía dưới cho dễ nhìn trong Theme Editor
- C. Trả về Boolean `true` mặc định, dùng để bật/tắt cả nhóm setting bên dưới
- D. Bắt buộc phải đứng cuối cùng trong mảng `settings[]`

**Câu 15.** Setting `paragraph` có vai trò gì?
```json
{ "type": "paragraph", "content": "Ảnh nên có tỉ lệ 16:9 để hiển thị đẹp nhất." }
```
- A. Giống `header`: không có `id`, không lưu giá trị, chỉ hiển thị 1 đoạn text hướng dẫn/ghi chú cho merchant trong Theme Editor
- B. Là alias của `richtext`, trả về string có HTML
- C. Bắt buộc phải đi kèm 1 setting `header` ngay phía trên
- D. Trả về string nội dung `content`, có thể dùng trong Liquid render ra storefront

**Câu 16.** Sự khác biệt cốt lõi giữa `settings.xxx` và `section.settings.xxx` là gì?
- A. Cú pháp khác nhau nhưng bản chất là một, Shopify tự động đồng bộ hai bên
- B. `settings.xxx` đọc từ `config/settings_schema.json` — áp dụng **toàn site** (global theme settings); `section.settings.xxx` đọc từ `{% schema %}` của **1 section instance cụ thể** — chỉ có hiệu lực trong section đó
- C. `settings.xxx` chỉ dùng được trong file `.json`, `section.settings.xxx` chỉ dùng được trong file `.liquid`
- D. `settings.xxx` dùng cho block, `section.settings.xxx` dùng cho section

**Câu 17.** Nếu 2 section khác nhau trong cùng theme đều có 1 setting cùng tên `id: "heading_color"` khai trong `{% schema %}` riêng của mỗi section, thì:
- A. Cả 2 section dùng chung 1 giá trị vì cùng `id`, đổi màu ở section A sẽ đổi luôn màu ở section B
- B. Mỗi section có giá trị `section.settings.heading_color` **độc lập, không liên quan gì nhau** — vì mỗi section instance có config setting riêng, dù `id` trùng tên
- C. Shopify sẽ báo lỗi build vì trùng `id` giữa 2 section
- D. Giá trị sẽ tự động lấy từ `config/settings_schema.json` để đảm bảo nhất quán

---

### Bài tập viết JSON schema / Liquid

**Bài 1.** Viết đúng 1 object setting kiểu `range` cho phép merchant chỉnh "Độ bo góc" (border radius) của nút bấm, với các yêu cầu: `id` là `button_radius`, giá trị từ `0` đến `20`, mỗi bước nhảy `2`, đơn vị hiển thị là `px`, giá trị mặc định `8`, label tiếng Việt là "Độ bo góc".

**Bài 2.** Viết đúng 1 object setting kiểu `select` cho phép merchant chọn "Vị trí hiển thị badge", `id` là `badge_position`, với đúng 3 `options`:
- value `top-left`, label "Trên trái"
- value `top-right`, label "Trên phải"
- value `bottom-right`, label "Dưới phải"

Giá trị mặc định (`default`) là `top-right`.

**Bài 3.** Viết đúng 1 object setting kiểu `image_picker` với `id` là `hero_image`, label "Ảnh hero". Sau đó viết đoạn Liquid dùng đúng filter cần thiết để hiển thị ảnh này ra thẻ `<img>` với chiều rộng 1200px (nhớ tránh bẫy đã học ở Câu 9).

**Bài 4.** Cho 3 setting sau cùng nằm trong `settings[]` của 1 section:
```json
{ "type": "header", "content": "Cài đặt Banner" },
{ "type": "text", "id": "banner_heading", "label": "Tiêu đề banner", "default": "Khuyến mãi lớn" },
{ "type": "checkbox", "id": "show_button", "label": "Hiển thị nút bấm", "default": true }
```
Viết đoạn Liquid render: hiển thị `banner_heading` trong thẻ `<h2>`, và chỉ hiển thị 1 thẻ `<a class="btn">Xem ngay</a>` nếu `show_button` là `true`. (Lưu ý: setting `header` không có `id` nên không đọc giá trị được, chỉ dùng cho Theme Editor.)

---

## 🔑 Đáp án & Giải thích

### Đáp án trắc nghiệm

**Câu 1:** B — `text` luôn trả về **String** thuần, không cần filter gì thêm để hiển thị.

**Câu 2:** B — `textarea` khác `text` ở chỗ cho phép nhập **nhiều dòng** (multi-line plain text), nhưng vẫn là string thuần, KHÔNG tự sinh thẻ HTML như `richtext`.

**Câu 3:** B — `richtext` là type duy nhất (cùng với các field tương tự) trả về string đã **có sẵn thẻ HTML** do Theme Editor's rich text editor sinh ra, nên có thể render thẳng bằng `{{ section.settings.xxx }}` mà không cần escape hay bọc thêm.

**Câu 4:** B — `select` (và `radio`) luôn trả về đúng field `value` của option đã chọn, KHÔNG phải `label` (label chỉ là text hiển thị cho merchant thấy trong Theme Editor).

**Câu 5:** A — `radio` và `select` giống hệt nhau về mặt dữ liệu: đều cần `options: [{value,label}]` và đều trả về `value`. Khác biệt DUY NHẤT là kiểu hiển thị UI trong Theme Editor (nút radio vs dropdown).

**Câu 6:** B — `checkbox` trả về đúng kiểu **Boolean** (`true`/`false`), dùng phổ biến nhất trong `{% if %}` để bật/tắt 1 khối hiển thị.

**Câu 7:** B — `step` quy định bước nhảy (ở đây là 5 đơn vị mỗi lần kéo). Giá trị Liquid trả về là **Number thuần** (VD `20`, không phải `"20px"`). `unit` (`px`) chỉ là nhãn hiển thị cạnh thanh trượt trong Theme Editor để merchant biết đơn vị, KHÔNG tự động gắn vào giá trị khi đọc trong Liquid — muốn có `"20px"` phải tự nối chuỗi, VD `{{ section.settings.padding }}px`.

**Câu 8:** B — `color` luôn trả về **string mã màu** (hex `#rrggbb` hoặc rgba), sẵn sàng dùng thẳng trong CSS như `style="color: {{ section.settings.text_color }}"`.

**Câu 9:** B — Đây là bẫy quan trọng nhất của `image_picker`: setting này trả về **image object** (không phải string URL), nên bắt buộc phải qua filter `image_url` (thường kèm `width`, có thể thêm `format`) mới lấy được URL dùng được cho `src`. Truy cập trực tiếp `.src` (đáp án C) hay in thẳng object ra `src=""` (đáp án A) đều sai — Shopify không có field `.src` công khai kiểu đó và filter đúng tên là `image_url`, không phải `img_url` (đáp án D, filter đó dùng cho asset khác/đã cũ).

**Câu 10:** B — `product` trả về **full product object**, y hệt object lấy được khi loop qua `collection.products`. Vì vậy truy cập tên sản phẩm dùng đúng field `.title`: `{{ section.settings.featured_product.title }}`. Không cần tra cứu qua `all_products` (đáp án C là thừa/sai vì `section.settings.featured_product` đã là object sẵn).

**Câu 11:** B — `collection` cũng trả về full object trực tiếp (không phải handle string), nên `.title` dùng thẳng là đúng. Không cần tra cứu qua biến `collections[...]` (đáp án C) vì setting đã tự trả về object rồi, không phải string handle.

**Câu 12:** B — `page` trả về full page object trực tiếp (giống `product`/`collection`), field chứa nội dung HTML của trang là `.content` — dùng `{{ section.settings.about_page.content }}` để render nội dung, đúng chuẩn Shopify (page object có field `content`, không phải `body`).

**Câu 13:** B — `text_alignment` là type "tiện dụng" đặc biệt: Shopify tự sinh sẵn 3 lựa chọn `left`/`center`/`right` với giao diện nút căn lề trực quan, merchant KHÔNG cần (và không thể) tự khai `options` cho type này. Giá trị trả về vẫn là string thường (`"left"`, `"center"`, hoặc `"right"`).

**Câu 14:** B — `header` không có `id` vì nó không lưu bất kỳ giá trị nào — nó chỉ là 1 dòng "tiêu đề nhóm" thuần UI, giúp Theme Editor phân nhóm trực quan các setting phía dưới nó, không ảnh hưởng gì tới dữ liệu hay Liquid render.

**Câu 15:** A — Giống hệt `header` về bản chất: `paragraph` không có `id`, không lưu giá trị, chỉ hiển thị 1 đoạn text ghi chú/hướng dẫn cho merchant xem trong Theme Editor (VD gợi ý kích thước ảnh nên dùng) — hoàn toàn không xuất hiện, không thể truy cập được trong Liquid.

**Câu 16:** B — `settings.xxx` lấy dữ liệu từ file cấu hình toàn cục `config/settings_schema.json`, có hiệu lực trên **toàn bộ theme/site**. `section.settings.xxx` lấy dữ liệu từ `{% schema %}` khai trong chính file `.liquid` của 1 section, chỉ có hiệu lực **trong phạm vi instance của section đó** — đổi giá trị ở section này không ảnh hưởng section khác.

**Câu 17:** B — Mỗi section instance có 1 bộ config setting hoàn toàn độc lập (lưu riêng trong file JSON template, theo đúng section instance đó). Dù 2 section khác nhau khai cùng `id: "heading_color"` trong schema riêng của chúng, giá trị đọc ra qua `section.settings.heading_color` ở mỗi section vẫn **độc lập tuyệt đối** — không hề dùng chung, không xung đột, và Shopify cũng không báo lỗi vì phạm vi (scope) của mỗi `id` chỉ gói gọn trong section chứa nó.

### Đáp án bài tập

**Bài 1:**
```json
{
  "type": "range",
  "id": "button_radius",
  "label": "Độ bo góc",
  "min": 0,
  "max": 20,
  "step": 2,
  "unit": "px",
  "default": 8
}
```

**Bài 2:**
```json
{
  "type": "select",
  "id": "badge_position",
  "label": "Vị trí hiển thị badge",
  "options": [
    { "value": "top-left", "label": "Trên trái" },
    { "value": "top-right", "label": "Trên phải" },
    { "value": "bottom-right", "label": "Dưới phải" }
  ],
  "default": "top-right"
}
```

**Bài 3:**
```json
{
  "type": "image_picker",
  "id": "hero_image",
  "label": "Ảnh hero"
}
```
```liquid
{% if section.settings.hero_image %}
  <img src="{{ section.settings.hero_image | image_url: width: 1200 }}" alt="{{ section.settings.hero_image.alt | default: 'Hero image' }}">
{% endif %}
```
👉 Giải thích: `hero_image` là **image object**, không phải string URL, nên bắt buộc qua filter `image_url: width: 1200` mới lấy được URL thật sự dùng được trong `src`. Nên kiểm tra `{% if section.settings.hero_image %}` trước vì merchant có thể chưa chọn ảnh nào.

**Bài 4:**
```liquid
<h2>{{ section.settings.banner_heading }}</h2>
{% if section.settings.show_button %}
  <a class="btn">Xem ngay</a>
{% endif %}
```
👉 Giải thích: `banner_heading` là `text` nên render thẳng string trong `<h2>`. `show_button` là `checkbox` trả về Boolean nên dùng trực tiếp trong `{% if %}` (không cần so sánh `== true`). Setting `header` ("Cài đặt Banner") không có `id`, không tồn tại trong `section.settings`, nên không xuất hiện trong đoạn Liquid render — nó chỉ có tác dụng nhóm hiển thị bên trong Theme Editor.
