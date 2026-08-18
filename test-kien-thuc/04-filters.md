# 📝 Test kiến thức — Nhóm 4: Liquid Filters

> Làm hết phần Câu hỏi trước, KHÔNG kéo xuống xem Đáp án sớm.

---

## 📋 Câu hỏi

### Trắc nghiệm

**Câu 1.** `{{ "hello shopify" | upcase }}` in ra gì?
- A. HELLO SHOPIFY
- B. Hello Shopify
- C. hello shopify
- D. Lỗi, vì `upcase` chỉ áp dụng cho số

**Câu 2.** Filter `strip_html` dùng để làm gì?
- A. Loại bỏ toàn bộ thẻ HTML, chỉ giữ lại phần chữ (text content)
- B. Escape các ký tự đặc biệt thành HTML entity
- C. Cắt bớt chuỗi còn N ký tự
- D. Chuyển chuỗi thành chữ in hoa

**Câu 3.** Cho `{{ "Ground control to Major Tom." | truncate: 20 }}`. Kết quả đúng là gì (biết dấu `...` mặc định tính luôn vào tổng 20 ký tự)?
- A. `Ground control to...`
- B. `Ground control to Ma...`
- C. `Ground control...`
- D. `Ground control to Major Tom...`

**Câu 4.** `{{ "<script>alert('x')</script>" | escape }}` sẽ cho kết quả như thế nào khi hiển thị ra trình duyệt?
- A. Hiển thị nguyên văn chuỗi `<script>alert('x')</script>` dưới dạng TEXT (không thực thi), vì các ký tự `<`, `>` đã được đổi thành HTML entity
- B. Đoạn script sẽ chạy thật trong trang
- C. Toàn bộ thẻ `<script>` bị xoá, chỉ còn `alert('x')`
- D. Báo lỗi cú pháp Liquid

**Câu 5.** `{{ "New Arrivals 2026!" | handleize }}` cho kết quả nào?
- A. `new-arrivals-2026`
- B. `New-Arrivals-2026`
- C. `new_arrivals_2026`
- D. `new-arrivals-2026!`

**Câu 6.** `product.price` là số nguyên tính bằng **cents**. Giả sử `product.price = 450000` và tiền tệ cửa hàng là USD, `{{ product.price | money }}` in ra gì?
- A. `$4,500.00`
- B. `$450,000.00`
- C. `$45.00`
- D. `450000`

**Câu 7.** `{{ 12.345 | round: 2 }}` in ra gì?
- A. `12.35`
- B. `12.34`
- C. `12`
- D. Lỗi, vì `round` không nhận tham số

**Câu 8.** Cho `compare_at_price = 500000` và `price = 400000` (đơn vị cents). `{{ compare_at_price | minus: price }}` in ra gì?
- A. `100000`
- B. `-100000`
- C. `900000`
- D. `900000000`

**Câu 9.** `{{ 5 | times: 3 }}` in ra gì?
- A. `15`
- B. `8`
- C. `1.666...`
- D. `35`

**Câu 10.** So sánh `{{ 10 | divided_by: 3 }}` và `{{ 10.0 | divided_by: 3 }}`. Kết quả nào đúng?
- A. `10 | divided_by: 3` → `3` (chia số nguyên, bị cắt phần thập phân); `10.0 | divided_by: 3` → `3.3333333333333335` (có ít nhất 1 số thập phân → chia lấy số thực)
- B. Cả hai đều ra `3.33`
- C. Cả hai đều ra `3`
- D. `divided_by` không hỗ trợ số thập phân

**Câu 11.** Cho `{% assign fruits = "Cam, Xoài, Táo" | split: ", " %}`. `{{ fruits | first }}` in ra gì?
- A. `Cam`
- B. `Táo`
- C. `Cam, Xoài, Táo`
- D. `3`

**Câu 12.** Với mảng `fruits` ở Câu 11, `{{ fruits | last }}` in ra gì?
- A. `Táo`
- B. `Cam`
- C. `Xoài`
- D. `nil`

**Câu 13.** Với mảng `fruits` ở Câu 11, `{{ fruits | join: " - " }}` in ra gì?
- A. `Cam - Xoài - Táo`
- B. `Cam, Xoài, Táo`
- C. `["Cam","Xoài","Táo"]`
- D. `Cam-Xoài-Táo`

**Câu 14.** Với mảng `fruits` ở Câu 11, `{{ fruits | size }}` in ra gì?
- A. `3`
- B. `"Cam, Xoài, Táo"`
- C. `Táo`
- D. Lỗi vì `size` chỉ dùng cho chuỗi

**Câu 15.** Phát biểu nào đúng về `asset_url`?
- A. Trả về URL CDN đầy đủ tới 1 file nằm trong thư mục `assets/` của theme, bản thân filter này KHÔNG tự bọc thành thẻ `<link>` hay `<script>`
- B. Tự động sinh thẻ `<link rel="stylesheet">` hoàn chỉnh
- C. Chỉ dùng được cho file ảnh
- D. Trả về đường dẫn tương đối trên máy local, không phải CDN

**Câu 16.** `{{ 'theme.css' | asset_url | stylesheet_tag }}` sinh ra gì?
- A. Thẻ `<link rel="stylesheet" href="https://cdn.shopify.com/.../theme.css?v=..." media="all">` hoàn chỉnh
- B. Chỉ 1 chuỗi URL, chưa có thẻ HTML
- C. Thẻ `<script src="theme.css">`
- D. Báo lỗi vì `stylesheet_tag` phải đứng trước `asset_url`

**Câu 17.** `{{ 'global.js' | asset_url | script_tag }}` sinh ra gì?
- A. Thẻ `<script src="https://cdn.shopify.com/.../global.js?v=..." defer="defer"></script>` hoàn chỉnh
- B. Thẻ `<link rel="stylesheet">`
- C. Chỉ 1 chuỗi URL
- D. Không hợp lệ vì `script_tag` chỉ nhận file `.css`

**Câu 18.** Điểm khác biệt chính giữa `image_url` và `script_tag`/`stylesheet_tag` là gì?
- A. `image_url` chỉ trả về chuỗi URL ảnh (đã resize theo tham số như `width`), người viết code vẫn phải tự bọc trong thẻ `<img>`; còn `script_tag`/`stylesheet_tag` tự sinh luôn thẻ HTML hoàn chỉnh
- B. `image_url` cũng tự sinh thẻ `<img>` hoàn chỉnh giống `script_tag`
- C. `image_url` không nhận tham số `width`/`height`
- D. Hai filter này hoàn toàn giống nhau

**Câu 19.** `{{ "2026-08-14" | date: "%d/%m/%Y" }}` in ra gì?
- A. `14/08/2026`
- B. `08/14/2026`
- C. `2026/08/14`
- D. `14-08-2026`

**Câu 20.** Filter `json` dùng để làm gì?
- A. Chuyển 1 biến/object Liquid thành chuỗi JSON — thường dùng để debug xem cấu trúc dữ liệu, hoặc truyền dữ liệu Liquid sang JavaScript qua thẻ `<script>`
- B. Kiểm tra chuỗi có đúng định dạng JSON hay không rồi trả về `true`/`false`
- C. Xoá toàn bộ khoảng trắng trong chuỗi
- D. Chuyển JSON string ngược lại thành HTML

**Câu 21.** `{{ 'cart.checkout' | t }}` hoạt động như thế nào?
- A. Tra key `checkout` nằm lồng trong `cart` ở file locale đang active (VD `locales/vi.json`) và in ra chuỗi đã dịch tương ứng; nếu quên filter `| t`, Liquid sẽ in ra nguyên văn chuỗi key `cart.checkout`
- B. Luôn in ra tiếng Anh mặc định, không quan tâm file locale
- C. Chỉ hoạt động trong `{% schema %}`, không dùng được ở template
- D. Tự động dịch cả nội dung merchant tự nhập như tên sản phẩm

**Câu 22.** Vì sao khi dùng `font_picker`, bắt buộc phải gọi thêm filter `font_face` thì trình duyệt mới tải font?
- A. Vì `settings.type_primary_font` chỉ là 1 Font Object chứa metadata (`family`, `weight`, `style`...); `font_face` mới là filter sinh ra khối CSS `@font-face` thật để trình duyệt biết đường dẫn file font mà tải về
- B. Vì `font_picker` trả về file font nhị phân, cần `font_face` để giải mã
- C. `font_face` không liên quan đến việc tải font, chỉ đổi màu chữ
- D. Không cần `font_face`, chỉ cần `{{ settings.type_primary_font.family }}` là đủ để trình duyệt tải font

**Câu 23.** Để lấy biến thể **in đậm (bold)** của 1 font object trước khi gọi `font_face`, dùng filter nào?
- A. `font_modify: 'weight', 'bold'`
- B. `font_face: 'weight', 'bold'`
- C. `upcase`
- D. `round`

**Câu 24.** Trong 1 block `{% paginate collection.products by 8 %}`, filter nào giúp in ra ngay toàn bộ HTML điều hướng phân trang (nút trang trước/sau, số trang) mà không cần tự viết vòng lặp qua `paginate.parts`?
- A. `default_pagination`
- B. `paginate_tag`
- C. `json`
- D. `t`

**Câu 25.** Filter `placeholder_svg_tag` dùng khi nào?
- A. Khi sản phẩm/collection không có ảnh, dùng để in ra 1 ảnh SVG placeholder mặc định của Shopify (VD: `{{ 'product-1' | placeholder_svg_tag: 'placeholder-svg' }}`)
- B. Dùng để nén ảnh thật về dung lượng nhỏ hơn
- C. Chỉ dùng cho file JavaScript
- D. Thay thế hoàn toàn cho `image_url` trong mọi trường hợp

---

### Bài tập viết code (chaining filter)

**Bài 1.** Cho `product.price = 320000` và `product.compare_at_price = 400000` (đơn vị cents). Viết code Liquid in ra:
- Giá gốc (`compare_at_price`) có gạch ngang, đã format tiền tệ.
- Giá bán hiện tại (`price`), đã format tiền tệ.
- Phần trăm giảm giá, làm tròn thành số nguyên, kèm dấu `%` (chỉ hiện khi có giảm giá).

Gợi ý output mong muốn (nếu shop dùng USD): giá gốc gạch ngang `$4,000.00`, giá bán `$3,200.00`, badge `20%`.

**Bài 2.** Cho `product.description` là HTML: `"<p>Áo thun nam chất liệu cotton 100%, thoáng mát, form rộng, phù hợp mặc hằng ngày và tập gym. Nhiều màu sắc, nhiều size từ S đến XXL, giao hàng toàn quốc.</p>"`. Viết code Liquid tạo thẻ `<meta name="description" content="...">` với nội dung đã được loại bỏ toàn bộ thẻ HTML và cắt tối đa 160 ký tự.

**Bài 3.** Viết code Liquid tạo thẻ `<img>` có thuộc tính `srcset` responsive với 3 kích thước `400px`, `800px`, `1200px` từ `product.featured_image`, dùng đúng filter `image_url`.

**Bài 4.** Viết code trong `{% style %}` để nạp đủ 4 biến thể của font `settings.type_primary_font`: chữ thường (normal), in đậm (bold), in nghiêng (italic), và vừa đậm vừa nghiêng (bold italic) — dùng chaining `font_modify` + `font_face`.

**Bài 5.** Viết code Liquid nạp file `theme.css` (từ thư mục `assets/`) thành thẻ `<link>` hoàn chỉnh, và file `global.js` thành thẻ `<script>` hoàn chỉnh — dùng đúng thứ tự chaining filter.

**Bài 6.** Viết 1 dòng code Liquid in ra: nhãn "Ngày đặt hàng" lấy từ key locale `order.date_label` (đa ngôn ngữ), theo sau là ngày `order.created_at` định dạng `dd/mm/yyyy`. Ví dụ output mong muốn: `Ngày đặt hàng: 14/08/2026`.

---

## 🔑 Đáp án & Giải thích

### Đáp án trắc nghiệm

**Câu 1:** A — `upcase` chuyển toàn bộ chuỗi thành chữ in hoa: `HELLO SHOPIFY`.

**Câu 2:** A — `strip_html` loại bỏ mọi thẻ HTML, chỉ giữ lại nội dung text thuần (VD: `"<p>Sản phẩm <strong>mới</strong></p>" | strip_html` → `"Sản phẩm mới"`).

**Câu 3:** A — `truncate` mặc định thêm `...` (3 ký tự) và TÍNH LUÔN 3 ký tự đó vào tổng số ký tự được truyền vào. `truncate: 20` nghĩa là tổng chuỗi kết quả (bao gồm `...`) dài 20 ký tự: `"Ground control to..."` (17 ký tự gốc + 3 ký tự `...` = 20).

**Câu 4:** A — `escape` chuyển các ký tự đặc biệt (`<`, `>`, `&`, `"`, `'`) thành HTML entity (`&lt;`, `&gt;`...), nên trình duyệt hiển thị nguyên văn dưới dạng chữ, không thực thi như code thật. Dùng để hiển thị an toàn code mẫu hoặc dữ liệu người dùng nhập.

**Câu 5:** A — `handleize` (alias: `handle`) chuyển chuỗi thành dạng URL-friendly: viết thường, khoảng trắng → dấu gạch ngang `-`, ký tự không hợp lệ (như `!`) bị loại bỏ chứ không đổi thành gạch ngang.

**Câu 6:** A — `product.price` lưu dưới dạng cents (số nguyên). `money` filter tự chia cho 100 và format theo đơn vị tiền tệ cửa hàng: `450000 / 100 = 4500.00` → `$4,500.00`.

**Câu 7:** A — `round` nhận tham số tuỳ chọn là số chữ số thập phân muốn giữ lại: `round: 2` giữ 2 số lẻ, làm tròn `12.345` → `12.35`.

**Câu 8:** A — `minus` trừ theo thứ tự: `giá_trị_gốc | minus: tham_số` = `500000 - 400000 = 100000`.

**Câu 9:** A — `times` nhân: `5 * 3 = 15`.

**Câu 10:** A — Đây là điểm dễ sai nhất của `divided_by`: nếu CẢ HAI toán hạng đều là số nguyên (integer), Liquid thực hiện phép chia nguyên (bỏ phần thập phân): `10 | divided_by: 3` → `3`. Chỉ cần một trong hai là số thực (float, có dấu `.`) thì kết quả mới là phép chia thực: `10.0 | divided_by: 3` → `3.3333333333333335`. Đây chính là lý do trong công thức tính % giảm giá phải nhân với `100.0` (có `.0`) chứ không phải `100`.

**Câu 11:** A — `first` trả về phần tử đầu tiên của mảng: `"Cam"`.

**Câu 12:** A — `last` trả về phần tử cuối cùng của mảng: `"Táo"`.

**Câu 13:** A — `join` nối các phần tử mảng thành 1 chuỗi, ngăn cách bởi tham số truyền vào: `"Cam - Xoài - Táo"`.

**Câu 14:** A — `size` trả về số phần tử của mảng (ở đây là 3). Lưu ý: `size` cũng dùng được cho chuỗi, khi đó trả về số ký tự.

**Câu 15:** A — `asset_url` chỉ sinh ra 1 chuỗi URL trỏ tới file trong `assets/` (dạng CDN, có thể là CSS, JS, ảnh, font...), không tự bọc thành thẻ HTML — muốn ra thẻ hoàn chỉnh phải chain thêm `stylesheet_tag`/`script_tag`.

**Câu 16:** A — `stylesheet_tag` nhận vào 1 URL (thường là kết quả của `asset_url`) và sinh ra thẻ `<link rel="stylesheet" href="..." media="all">` hoàn chỉnh.

**Câu 17:** A — `script_tag` nhận vào 1 URL và sinh ra thẻ `<script src="..." defer="defer"></script>` hoàn chỉnh.

**Câu 18:** A — `image_url` chỉ trả về chuỗi URL ảnh đã được Shopify CDN resize/tối ưu theo tham số (`width`, `height`, `format`...); vẫn cần tự viết thẻ `<img src="...">` để hiển thị. Ngược lại `stylesheet_tag`/`script_tag` tự sinh nguyên thẻ HTML.

**Câu 19:** A — `date` dùng cú pháp định dạng kiểu strftime: `%d` = ngày, `%m` = tháng, `%Y` = năm 4 chữ số → `"14/08/2026"`.

**Câu 20:** A — `json` chuyển 1 biến Liquid (string, object, array...) thành chuỗi JSON hợp lệ, hay dùng để debug nhanh giá trị 1 biến, hoặc nhúng dữ liệu Liquid vào script JS phía client.

**Câu 21:** A — `t` (translate) tra cứu key theo đường dẫn (`cart.checkout` = key `checkout` lồng trong nhóm `cart`) trong file locale JSON đang active của storefront. Nếu thiếu filter `| t`, Shopify không tra cứu gì cả mà in thẳng chuỗi key ra màn hình.

**Câu 22:** A — `font_picker` trả về 1 Font Object (metadata: `family`, `weight`, `style`, `fallback_families`...) chứ không phải file font thật. `font_face` mới là filter sinh khối CSS `@font-face { src: url(...) }` để trình duyệt biết địa chỉ file font cần tải về và đăng ký font đó.

**Câu 23:** A — `font_modify: 'weight', 'bold'` lấy ra 1 Font Object mới đại diện cho biến thể in đậm của font gốc; object này vẫn cần chain tiếp `| font_face` thì trình duyệt mới thực sự tải file đó.

**Câu 24:** A — `default_pagination` nhận vào object `paginate` (từ tag `{% paginate %}`) và in ra sẵn toàn bộ HTML điều hướng phân trang theo style mặc định của Shopify, không cần tự viết vòng lặp qua `paginate.parts`/`paginate.pages`.

**Câu 25:** A — `placeholder_svg_tag` dùng khi không có ảnh thật (sản phẩm/collection trống ảnh), in ra 1 SVG icon placeholder có sẵn của Shopify, ví dụ `{{ 'product-1' | placeholder_svg_tag: 'placeholder-svg' }}`.

---

### Đáp án bài tập code

**Bài 1:**
```liquid
{% if product.compare_at_price > product.price %}
  <span class="price-compare" style="text-decoration: line-through;">
    {{ product.compare_at_price | money }}
  </span>
  <span class="price-sale">
    {{ product.price | money }}
  </span>

  {% assign discount = product.compare_at_price | minus: product.price %}
  {% assign discount_pct = discount | times: 100.0 | divided_by: product.compare_at_price | round %}
  <span class="badge-discount">{{ discount_pct }}%</span>
{% else %}
  <span class="price">{{ product.price | money }}</span>
{% endif %}
```
Với `price = 320000`, `compare_at_price = 400000`: `discount = 80000`; `discount_pct = 80000 × 100.0 / 400000 = 20.0` → `round` → `20` → in ra `20%`. Lưu ý bắt buộc nhân với `100.0` (float) chứ không phải `100` (integer), nếu không `divided_by` sẽ bị cắt phần thập phân do quy tắc ở Câu 10.

**Bài 2:**
```liquid
{% assign seo_description = product.description | strip_html | truncate: 160 %}
<meta name="description" content="{{ seo_description }}">
```
Chain 2 bước: `strip_html` loại bỏ hết thẻ `<p>`, `<strong>`... chỉ giữ text; `truncate: 160` cắt (kèm `...` nếu vượt quá) để tối ưu SEO (Google thường chỉ hiển thị ~150-160 ký tự đầu của meta description).

**Bài 3:**
```liquid
<img
  src="{{ product.featured_image | image_url: width: 800 }}"
  srcset="
    {{ product.featured_image | image_url: width: 400 }} 400w,
    {{ product.featured_image | image_url: width: 800 }} 800w,
    {{ product.featured_image | image_url: width: 1200 }} 1200w
  "
  sizes="(min-width: 750px) 50vw, 100vw"
  alt="{{ product.featured_image.alt | escape }}"
  loading="lazy"
>
```
`image_url: width: N` được gọi 3 lần với 3 tham số khác nhau để trình duyệt tự chọn ảnh phù hợp với kích thước màn hình, tránh tải ảnh quá to trên mobile.

**Bài 4:**
```liquid
{% style %}
  {{ settings.type_primary_font | font_face: font_display: 'swap' }}
  {{ settings.type_primary_font | font_modify: 'weight', 'bold' | font_face: font_display: 'swap' }}
  {{ settings.type_primary_font | font_modify: 'style', 'italic' | font_face: font_display: 'swap' }}
  {{ settings.type_primary_font | font_modify: 'weight', 'bold' | font_modify: 'style', 'italic' | font_face: font_display: 'swap' }}
{% endstyle %}
```
Mỗi dòng chain `font_modify` (đổi `weight` và/hoặc `style` của font object) rồi mới `font_face` (để trình duyệt tải đúng file biến thể đó về). Nếu không gọi đủ 4 dòng, khi CSS yêu cầu `font-weight: bold`, trình duyệt sẽ tự "giả lập" bold từ font normal (faux bold) khiến chữ bị vỡ nét thay vì dùng đúng file font bold thật.

**Bài 5:**
```liquid
{{ 'theme.css' | asset_url | stylesheet_tag }}
{{ 'global.js' | asset_url | script_tag }}
```
Luôn phải gọi `asset_url` TRƯỚC để lấy URL CDN đầy đủ của file, sau đó mới chain `stylesheet_tag`/`script_tag` để bọc URL đó vào đúng thẻ HTML tương ứng.

**Bài 6:**
```liquid
{{ 'order.date_label' | t }}: {{ order.created_at | date: "%d/%m/%Y" }}
```
Với `locales/vi.json` có `{ "order": { "date_label": "Ngày đặt hàng" } }` và `order.created_at` là ngày 14/08/2026, output: `Ngày đặt hàng: 14/08/2026`.
