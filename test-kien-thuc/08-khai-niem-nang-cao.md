# 📝 Test kiến thức — Nhóm 8: Khái niệm nâng cao

> Làm hết phần Câu hỏi trước, KHÔNG kéo xuống xem Đáp án sớm.

---

## 📋 Câu hỏi

### Trắc nghiệm

**Câu 1.** Một dev viết dòng sau để hiển thị chất liệu sản phẩm từ metafield:
```liquid
<p>Chất liệu: {{ product.metafields.custom.material }}</p>
```
Trên storefront, dòng này in ra rỗng hoặc in ra cả object thay vì text mong muốn. Nguyên nhân đúng nhất là gì?
- A. `product.metafields.custom.material` luôn `nil` với mọi sản phẩm, phải dùng field khác
- B. Metafield trả về **Object có type riêng**, phải truy cập qua `.value` (VD `{{ product.metafields.custom.material.value }}`) mới ra giá trị text thật
- C. Metafield chỉ đọc được từ `shop`, không đọc được từ `product`
- D. Phải dùng `{% render %}` thay vì `{{ }}` để in metafield

**Câu 2.** Merchant nhập metafield `product.metafields.custom.ingredients` với type `list.single_line_text_field`, có 3 giá trị: "Cotton", "Polyester", "Spandex". Dev muốn liệt kê từng thành phần ra 1 thẻ `<li>`. Cách viết nào đúng?
- A. `{{ product.metafields.custom.ingredients.value }}` — in thẳng ra vì nó là string
- B. `{% for item in product.metafields.custom.ingredients.value %}<li>{{ item }}</li>{% endfor %}` — vì type `list.*` trả về **array** trong `.value`
- C. `{{ product.metafields.custom.ingredients | first }}` — vì list metafield chỉ lấy được phần tử đầu
- D. `{% for item in product.metafields.custom.ingredients %}<li>{{ item.value }}</li>{% endfor %}` — lặp trực tiếp trên metafield, không qua `.value`

**Câu 3.** Merchant muốn tự thêm 1 thanh khuyến mãi (announcement bar) nằm xen giữa phía trên section header, mà không cần dev sửa code, đồng thời cũng muốn tự sắp xếp lại thứ tự các section này. Cơ chế nào trong theme cho phép việc này?
- A. `{% section 'header' %}` — vì section số ít cũng cho thêm/bớt section khác được
- B. Classic block khai trong `schema.blocks[]` của `header.liquid`
- C. Section Group — `{% sections 'header-group' %}` trỏ tới file `sections/header-group.json` chứa `sections{}` + `order[]`, cho phép merchant thêm/bớt/sắp xếp lại nhiều section trong nhóm
- D. Thêm `presets.blocks` vào schema của `header.liquid`

**Câu 4.** Dev cần 1 khối "Testimonial" (đánh giá khách hàng) có thể chèn vào cả `sections/home-page.liquid` lẫn `sections/product-page.liquid` mà không phải copy code 2 lần, và sửa 1 chỗ thì cả 2 nơi cùng cập nhật. Nên dùng cách nào?
- A. Classic block — khai `"blocks": [{"type":"testimonial","settings":[...]}]` riêng trong từng file section
- B. Theme block `@theme` — tạo file riêng `blocks/testimonial.liquid`, mỗi section chỉ cần khai `"blocks":[{"type":"@theme"}]` và dùng `{% content_for 'blocks' %}` để tái sử dụng chéo
- C. Copy nguyên khối HTML testimonial vào cả 2 file `.liquid`
- D. Đặt testimonial vào `config/settings_schema.json` để dùng chung toàn site

**Câu 5.** Một dev viết trong `{% schema %}` của section `feature-banner.liquid`:
```json
"presets": [
  {
    "name": "Feature Banner — Đỏ nổi bật",
    "settings": { "background_color": "#ff0000", "border-radius": "20px", "font-family": "Arial" }
  }
]
```
Với `settings[]` của section chỉ khai đúng 1 field `{ "type": "color", "id": "background_color" }`. Điều gì SAI trong đoạn `presets.settings` trên?
- A. Không sai gì, `presets.settings` cho phép viết CSS property tự do như `border-radius`, `font-family`
- B. `presets.settings` chỉ được set giá trị mặc định cho đúng những `id` **đã tồn tại** trong `settings[]` của schema; `border-radius` và `font-family` không phải `id` hợp lệ nên sẽ bị bỏ qua/không có tác dụng, đây không phải nơi viết CSS tự do
- C. Sai vì `name` phải là tên hiển thị thật trên trang, không phải tên trong khay Add section
- D. Sai vì `presets` không được phép chứa `settings`, chỉ được chứa `blocks`

**Câu 6.** Section `trust-badge.liquid` có khai `blocks[]` cho type `icon_with_text` nhưng KHÔNG khai `presets.blocks`. Khi merchant lần đầu bấm "Add section" để thêm section này vào trang, điều gì xảy ra?
- A. Shopify tự tạo sẵn 3 block mặc định vì `limit: 3`
- B. Section thêm vào sẽ **trống trơn** (không có block nào cả), merchant phải tự bấm "Add block" từng cái vì không có `presets.blocks` để tạo sẵn block instance
- C. Lỗi validate schema, section không thêm được vào trang
- D. Section tự lấy CSS mặc định để hiển thị dù chưa có block

**Câu 7.** Trong file JSON template, một dev viết block instance sau, kỳ vọng thêm 1 thuộc tính `"note"` để ghi chú riêng cho dev đọc:
```json
"text_1": {
  "type": "text",
  "settings": { "text": "Sale 50%" },
  "note": "nhớ đổi text này trước Tết"
}
```
Điều gì xảy ra với key `"note"` này khi Shopify render trang?
- A. Hiện ra dưới dạng comment HTML `<!-- nhớ đổi text này trước Tết -->`
- B. Gây lỗi 500, trang không render được
- C. Bị Shopify **âm thầm bỏ qua** — vì 1 block instance chỉ đúng 5 field built-in hợp lệ (`type`, `settings`, `blocks`, `block_order`, `disabled`), key lạ không có tác dụng gì và không báo lỗi
- D. Tự động được thêm làm 1 setting mới vào `schema.blocks[]` của block `text`

**Câu 8.** File `config/settings_schema.json` bắt đầu bằng:
```json
[
  { "name": "Colors", "settings": [...] },
  { "name": "Typography", "settings": [...] }
]
```
Đoạn code này có vấn đề gì so với cấu trúc chuẩn Shopify yêu cầu?
- A. Không có vấn đề gì, thứ tự nhóm không quan trọng
- B. Thiếu phần tử **đầu tiên bắt buộc** là `theme_info` (metadata theme, không có `settings`) — `settings_schema.json` luôn phải mở đầu bằng object này trước các nhóm setting khác
- C. `settings[]` phải là mảng phẳng duy nhất, không được lồng trong từng nhóm
- D. Tên nhóm `"name"` phải trùng với tên section đang dùng field đó

**Câu 9.** Dev khai `type_primary_font` là `font_picker` trong `settings_schema.json`, rồi viết CSS:
```css
body { font-family: {{ settings.type_primary_font.family }}; }
```
Merchant chọn font "Work Sans" trong Theme Editor, nhưng trên storefront trình duyệt vẫn hiển thị font hệ thống mặc định, không phải Work Sans thật. Vì sao?
- A. `font_picker` trả về string, không phải Font Object, nên `.family` không có giá trị
- B. `.family` chỉ trả ra **tên font** ("Work Sans") để khai trong CSS, nhưng chưa có filter `font_face` (VD `{{ settings.type_primary_font | font_face: font_display: 'swap' }}`) nên trình duyệt **chưa thực sự tải file font** về
- C. Phải dùng `font_modify` thay vì `font_face`, `font_face` đã bị deprecated
- D. `settings.type_primary_font` chỉ đọc được trong `{% schema %}`, không đọc được trong CSS

**Câu 10.** Dev cần sinh CSS variable động từ `settings.button_bg_color` (global setting) trong 1 file nằm ở `snippets/css-variables.liquid`, được `render` vào `<head>` của `layout/theme.liquid`. Nên dùng thẻ nào và vì sao?
- A. `{% stylesheet %}` — vì đây là CSS, dùng ở đâu cũng được
- B. `{% style %}` — vì `{% stylesheet %}` **chỉ dùng được trong `sections/`, `blocks/`**, còn snippet nằm ngoài phạm vi đó; `{% style %}` dùng được ở bất kỳ đâu và cho phép chạy Liquid động như `{{ settings.xxx }}`
- C. Cả hai đều như nhau, không có khác biệt về nơi dùng được
- D. Không thẻ nào dùng được trong snippet, phải chuyển logic này vào `theme.liquid` trực tiếp

**Câu 11.** Dev đã tạo đầy đủ `locales/vi.json` với key `{ "cart": { "checkout": "Thanh toán" } }`, nhưng trên storefront nút vẫn hiện chữ `cart.checkout` y nguyên thay vì "Thanh toán". Nguyên nhân khả dĩ nhất?
- A. File `vi.json` phải đổi tên thành `vi.schema.json` mới hoạt động trên storefront
- B. Code HTML quên dùng filter `| t` (VD phải viết `{{ 'cart.checkout' | t }}`) — thiếu `| t` thì Shopify in ra **nguyên văn chuỗi key**, không tra cứu file locale nào dù bản dịch có đầy đủ tới đâu
- C. `vi.json` chỉ dùng cho Theme Editor, không dùng cho storefront
- D. Phải thêm tiền tố `t:` trước `'cart.checkout'` mới tra được file locale

**Câu 12.** Sau khi tạo xong `locales/vi.json` và `locales/vi.schema.json`, dev nghĩ rằng khách hàng đã có thể chọn xem site bằng tiếng Việt ngay trên storefront thật. Điều này đúng hay sai?
- A. Đúng, tạo file locale xong là tự động publish luôn
- B. Sai — thêm file locale trong code chỉ là **tạo sẵn nội dung dịch**; phải vào Admin → Settings → Languages → Add language, và ngôn ngữ mới sẽ ở trạng thái "Not published" cho tới khi bấm **Publish** thì khách hàng mới chọn được
- C. Sai — phải dịch xong 100% nội dung sản phẩm (tên, mô tả) trước thì ngôn ngữ mới hiện ra được
- D. Đúng, nhưng chỉ áp dụng nếu store đang ở gói Shopify Plus

**Câu 13.** Trong menu Admin → Online Store → Navigation, merchant chọn resource là 1 Collection cụ thể (không tự gõ URL) khi tạo 1 nav item. Trong code Liquid render menu, `link.url` của item đó sẽ trả về gì?
- A. Y hệt chuỗi rỗng, phải tự viết `link.url = '/collections/' | append: link.title` mới ra đúng URL
- B. Tự động resolve thành `/collections/<handle>` — vì `link.url` không phải href tĩnh tự gõ, mà **luôn tự resolve** theo đúng loại resource merchant đã chọn (ở đây `link.type` sẽ là `collection_link`)
- C. Trả về ID số của collection, dev phải tự convert sang URL
- D. Trả về `nil` vì `link.url` chỉ hoạt động với Product, không hoạt động với Collection

**Câu 14.** Đang code `sections/header.liquid`, dev viết trước đoạn Liquid `{% for link in section.settings.menu.links %}` cho nav menu, nhưng CHƯA thêm `{% schema %}` khai field `menu` kiểu `link_list`. Điều gì đúng nhất về tình trạng này?
- A. Đoạn code này thuộc nhóm "cần setting merchant tự nhập" — nó tạm thời không chạy ra gì (rỗng) vì `section.settings.menu` chưa tồn tại cho tới khi có `{% schema %}` khai field đó; khác với nhóm "bind object có sẵn" như `cart.item_count` vốn làm ngay không cần schema
- B. Code sẽ báo lỗi 500 ngay lập tức vì thiếu schema
- C. Không có sự khác biệt giữa 2 nhóm này, section nào cũng bắt buộc phải có `{% schema %}` trước khi viết bất kỳ dòng Liquid nào
- D. `section.settings.menu.links` tự động lấy menu "Main menu" mặc định dù chưa khai schema

---

### Bài tập viết code/JSON

**Bài 1.** Viết đúng 1 block instance JSON có cấu trúc **3 tầng lồng nhau** cho `templates/index.json`, theo mô tả:
- Section cha key `promo_banner_1`, `type: "custom-section"`, có setting `background_image` = `"promo.jpg"`.
- Bên trong, 1 block con key `group_1`, `type: "group"`, setting `layout_direction` = `"group--vertical"`.
- Bên trong `group_1`, 2 block cháu: `text_1` (`type: "text"`, setting `text` = `"Giảm giá cuối tuần"`) và `text_2` (`type: "text"`, setting `text` = `"Chỉ áp dụng Thứ 7 - Chủ Nhật"`), thứ tự `text_1` trước `text_2`.
- Nhớ khai đủ `block_order` ở cả 2 cấp lồng.

**Bài 2.** Viết đoạn Liquid đọc metafield `product.metafields.custom.warranty_months` (type `number_integer`) một cách AN TOÀN: chỉ hiển thị dòng `<p>Bảo hành: X tháng</p>` nếu metafield đã được merchant nhập (khác `blank`/`nil`); nếu chưa nhập thì không in gì cả.

**Bài 3.** Viết 1 field `settings[]` kiểu `link_list` trong `{% schema %}` của section `header.liquid`, cho phép merchant chọn menu điều hướng nào sẽ áp dụng cho section này, với:
- `id`: `menu`
- `label`: `"Menu điều hướng"`
- Giá trị mặc định là handle menu Shopify tự tạo sẵn cho mọi store mới.

**Bài 4.** Viết cấu trúc đầy đủ `config/settings_schema.json` (dạng rút gọn, chỉ cần đủ khung) gồm: phần tử `theme_info` bắt buộc đầu tiên, và 1 nhóm mới tên `"Buttons"` chứa đúng 1 setting kiểu `color` với `id: "button_bg_color"`, `label: "Màu nền nút"`.

**Bài 5.** Viết đoạn `{% style %}` (không dùng `{% stylesheet %}`) đặt trong `snippets/css-variables.liquid`, sinh ra 1 CSS custom property `--color-button-bg` lấy giá trị động từ `settings.button_bg_color`, để `:root` áp dụng biến này.

---

## 🔑 Đáp án & Giải thích

### Đáp án trắc nghiệm

**Câu 1:** B — Metafield luôn là Object có type riêng, bắt buộc truy cập qua `.value` mới ra giá trị dùng được (VD `{{ product.metafields.custom.material.value }}`). Ngoài ra cần nhớ metafield rất hay `nil` nếu admin chưa nhập liệu, nên thực tế nên kiểm tra `!= blank` trước khi in.

**Câu 2:** B — Type `list.single_line_text_field` trả về **array** trong `.value`, nên phải `{% for item in ...ingredients.value %}` rồi in `{{ item }}` trực tiếp (không phải `item.value` — bản thân từng phần tử trong list đã là string sẵn, `.value` chỉ áp dụng ở tầng ngoài cùng của metafield).

**Câu 3:** C — Đây đúng là công dụng của Section Group: `{% sections 'header-group' %}` trỏ tới `sections/header-group.json` (cấu trúc `sections{}` + `order[]` giống template), cho phép merchant tự thêm/bớt/sắp xếp lại nhiều section trong nhóm mà dev không cần sửa code. `{% section 'x' %}` (số ít) chỉ render đúng 1 section cố định, không cho thêm/bớt.

**Câu 4:** B — Theme block `@theme` được thiết kế đúng cho mục đích tái sử dụng chéo section: code nằm ở 1 file riêng trong `blocks/`, mọi section chỉ cần tham chiếu `"blocks":[{"type":"@theme"}]` và `{% content_for 'blocks' %}`. Classic block (A) chỉ tồn tại trong đúng 1 file section đã khai nó, muốn dùng lại phải copy nguyên khai báo — sửa 1 bên không tự đồng bộ bên kia.

**Câu 5:** B — `presets.settings` **không phải nơi viết CSS tự do**; nó chỉ được set giá trị mặc định cho đúng các `id` đã tồn tại trong `settings[]` của schema. Ở đây chỉ có `id: "background_color"` là hợp lệ, còn `border-radius` và `font-family` không phải `id` đã khai nên không có tác dụng gì (đây chính là 1 trong các hiểu lầm phổ biến ghi trong tài liệu gốc Ngày 9).

**Câu 6:** B — Không có `presets.blocks` thì khi merchant lần đầu thêm section, section sẽ **trống trơn**, merchant phải tự bấm "Add block" từng cái. `presets.blocks` mới là cơ chế tạo sẵn các block instance thật có data cụ thể ngay từ đầu.

**Câu 7:** C — 1 block instance trong JSON template chỉ đúng 5 field built-in hợp lệ: `type`, `settings`, `blocks`, `block_order`, `disabled`. Key lạ như `"note"` sẽ bị Shopify **âm thầm bỏ qua**, không gây lỗi, không có tác dụng gì (không hiện comment, không thêm setting).

**Câu 8:** B — `config/settings_schema.json` là 1 mảng, và phần tử **đầu tiên luôn bắt buộc phải là `theme_info`** (metadata theme, không có `settings`), rồi mới tới các nhóm setting khác (mỗi nhóm là 1 tab trong Theme Settings panel). Thiếu `theme_info` là sai cấu trúc chuẩn.

**Câu 9:** B — `font_picker` trả về **Font Object**, `.family` chỉ ra **tên** font để dùng trong CSS, nhưng file font thật sự chỉ được trình duyệt tải về khi có filter `font_face` (VD `{{ settings.type_primary_font | font_face: font_display: 'swap' }}`). Thiếu `font_face`, `.family` chỉ khai tên trong CSS nhưng không có file font nào được nạp, nên trình duyệt fallback về font hệ thống.

**Câu 10:** B — `{% stylesheet %}` chỉ dùng được trong `sections/` và `blocks/`; snippet nằm ngoài phạm vi này nên phải dùng `{% style %}`, thẻ dùng được ở **bất kỳ đâu** (snippet, layout...) và hỗ trợ chạy Liquid động như `{{ settings.xxx }}` — đúng như cách `snippets/css-variables.liquid` trong tài liệu gốc dùng để sinh CSS variable từ setting toàn cục.

**Câu 11:** B — Thiếu filter `| t` là nguyên nhân phổ biến nhất: Shopify chỉ tra cứu file locale khi code gọi đúng `{{ 'key.path' | t }}`; thiếu `| t` (hoặc thiếu `t:` trong schema) thì Shopify in ra **nguyên văn chuỗi key**, dù file `vi.json` có dịch đầy đủ tới đâu cũng không được dùng tới.

**Câu 12:** B — Tạo file locale trong code (`vi.json`/`vi.schema.json`) chỉ là chuẩn bị sẵn nội dung dịch. Phải thêm ngôn ngữ ở Admin → Settings → Languages → Add language, ngôn ngữ mới mặc định ở trạng thái "Not published" và cần bấm Publish thủ công thì khách hàng mới chọn được — không cần chờ dịch xong nội dung sản phẩm vì chữ cố định của theme đã dùng được ngay.

**Câu 13:** B — `link.url` không phải href tĩnh tự gõ tay, nó **luôn tự động resolve** dựa theo loại resource merchant chọn ở Admin. Chọn 1 Collection cụ thể → `link.type` là `collection_link` và `link.url` tự ra `/collections/<handle>`. Merchant đổi collection nào link tới, `link.url` tự cập nhật theo, theme code không cần đổi gì.

**Câu 14:** A — Đây thuộc nhóm "cần setting merchant tự nhập": phải có `{% schema %}` khai field đó thì `section.settings.menu` mới thật sự tồn tại và "sống". Trước khi thêm schema, dòng code này chạy nhưng cho ra rỗng (không lỗi rõ ràng) — khác hẳn nhóm "bind object có sẵn" (VD `cart.item_count`, `routes.account_url`) vốn làm được ngay không cần khai báo gì thêm. Thứ tự luyện tập đúng: HTML tĩnh → bind object có sẵn → thêm schema từng setting một.

### Đáp án bài tập

**Bài 1:**
```json
"promo_banner_1": {
  "type": "custom-section",
  "settings": { "background_image": "promo.jpg" },
  "blocks": {
    "group_1": {
      "type": "group",
      "settings": { "layout_direction": "group--vertical" },
      "blocks": {
        "text_1": { "type": "text", "settings": { "text": "Giảm giá cuối tuần" } },
        "text_2": { "type": "text", "settings": { "text": "Chỉ áp dụng Thứ 7 - Chủ Nhật" } }
      },
      "block_order": ["text_1", "text_2"]
    }
  },
  "block_order": ["group_1"]
}
```

**Bài 2:**
```liquid
{%- assign warranty = product.metafields.custom.warranty_months -%}
{%- if warranty != blank -%}
  <p>Bảo hành: {{ warranty.value }} tháng</p>
{%- endif -%}
```
Lưu ý: kiểm tra `!= blank` trên chính object metafield (`warranty`) trước, chỉ gọi `.value` bên trong nhánh `if` — vì nếu merchant chưa nhập, `product.metafields.custom.warranty_months` trả về `nil`, in trực tiếp `.value` không kiểm tra trước sẽ có nguy cơ ra rỗng/lỗi logic hiển thị dòng trống.

**Bài 3:**
```json
{
  "type": "link_list",
  "id": "menu",
  "label": "Menu điều hướng",
  "default": "main-menu"
}
```
`"main-menu"` là handle menu mặc định Shopify tự tạo cho mọi store mới, dùng làm giá trị fallback hợp lý khi merchant chưa tự chọn menu nào khác.

**Bài 4:**
```json
[
  {
    "name": "theme_info",
    "theme_name": "My First Theme",
    "theme_version": "1.0.0",
    "theme_author": "Your Name",
    "theme_documentation_url": "",
    "theme_support_url": ""
  },
  {
    "name": "Buttons",
    "settings": [
      {
        "type": "color",
        "id": "button_bg_color",
        "label": "Màu nền nút"
      }
    ]
  }
]
```

**Bài 5:**
```liquid
{% style %}
  :root {
    --color-button-bg: {{ settings.button_bg_color }};
  }
{% endstyle %}
```
Dùng `{% style %}` (không phải `{% stylesheet %}`) vì file nằm trong `snippets/` — `{% stylesheet %}` chỉ hợp lệ trong `sections/`/`blocks/`; `{% style %}` vừa dùng được ở snippet vừa cho phép chạy Liquid động (`{{ settings.button_bg_color }}`) để lấy giá trị merchant đã chọn trong Theme Editor.
