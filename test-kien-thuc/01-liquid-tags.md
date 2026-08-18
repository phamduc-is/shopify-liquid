# 📝 Test kiến thức — Nhóm 1: Liquid Tags

> Làm hết phần Câu hỏi trước, KHÔNG kéo xuống xem Đáp án sớm.

---

## 📋 Câu hỏi

### Trắc nghiệm

**Câu 1.** Cho đoạn code:
```liquid
{% assign product_price = 40000 %}
{% assign compare_price = 50000 %}
{% assign available = true %}

{% if available == false %}
  <span class="badge badge--sold-out">Hết hàng</span>
{% elsif compare_price > product_price %}
  <span class="badge badge--sale">Đang giảm giá</span>
{% else %}
  <span class="badge badge--normal">Đang bán</span>
{% endif %}
```
Output HTML render ra là gì? - B
- A. `<span class="badge badge--sold-out">Hết hàng</span>`
- B. `<span class="badge badge--sale">Đang giảm giá</span>`
- C. `<span class="badge badge--normal">Đang bán</span>`
- D. Không render gì vì `available` là `true` nên bỏ qua toàn bộ khối `if`

**Câu 2.** Thẻ `{% unless %}` chạy nội dung bên trong khi nào? - B
- A. Khi điều kiện là `true`
- B. Khi điều kiện là `false`
- C. Luôn luôn chạy, không quan tâm điều kiện
- D. Chỉ chạy khi biến là `nil`

**Câu 3.** Cho đoạn code: 
```liquid
{% assign is_featured = false %}
{% unless is_featured %}
  <p class="text-error">Không phải sản phẩm nổi bật</p>
{% endunless %}
```
Output là gì? - B 
- A. Không in gì cả
- B. `<p class="text-error">Không phải sản phẩm nổi bật</p>`
- C. Báo lỗi vì `unless` không nhận biến `boolean`
- D. In ra `false`

**Câu 4.** Cho đoạn code:
```liquid
{% assign product_type = 'Pants' %}

{% case product_type %}
  {% when 'Shirts' %}
    <p>Danh mục: Áo sơ mi</p>
  {% when 'Pants' %}
    <p>Danh mục: Quần dài</p>
  {% else %}
    <p>Danh mục khác</p>
{% endcase %}
```
Output là gì? - B
- A. `<p>Danh mục: Áo sơ mi</p>`
- B. `<p>Danh mục: Quần dài</p>`
- C. `<p>Danh mục khác</p>`
- D. In ra cả 2 dòng "Áo sơ mi" và "Quần dài"

**Câu 5.** Đâu là cú pháp **SAI** khi dùng `case`/`when` để render classic block theo `block.type`? - B 
- A. `{% case block.type %}{% when 'icon_with_text' %}...{% endcase %}`
- B. `{% case block.type %}{% when 'icon_with_text' %}...{% else %}...{% endcase %}`
- C. `{% case block.icon_with_text %}...{% endcase %}`
- D. `{% case block.type %}{% when 'icon_with_text' %}...{% when 'image_with_text' %}...{% endcase %}`

**Câu 6.** Toán tử `contains` dùng để làm gì? - A
- A. Kiểm tra 1 array/string có chứa 1 giá trị con hay không
- B. Gộp 2 array lại thành 1
- C. Đếm số phần tử trong array
- D. So sánh 2 số với nhau

**Câu 7.** Cho đoạn code:
```liquid
{% assign product_tags = "new-arrival, summer, sale" | split: ", " %}

{% if product_tags contains 'new-arrival' %}
  <span class="badge">Hàng Mới Phủ Sóng</span>
{% endif %}
```
Output là gì? - B
- A. Không in gì vì `product_tags` là string chứ không phải array
- B. `<span class="badge">Hàng Mới Phủ Sóng</span>`
- C. Báo lỗi vì `contains` không dùng được với `if`
- D. In ra `true`

**Câu 8.** Cho đoạn code:
```liquid
{% assign product_titles = "Áo Thun, Áo Sơ Mi, Quần Jeans, Váy" | split: ", " %}
{% for title in product_titles limit: 2 offset: 1 %}
  <p>SP {{ forloop.index }}: {{ title }}</p>
{% endfor %}
```
Output là gì? - B
- A. `SP 1: Áo Thun`, `SP 2: Áo Sơ Mi`
- B. `SP 1: Áo Sơ Mi`, `SP 2: Quần Jeans`
- C. `SP 2: Áo Sơ Mi`, `SP 3: Quần Jeans`
- D. `SP 1: Quần Jeans`, `SP 2: Váy`

**Câu 9.** `offset: 1` trong thẻ `for` nghĩa là gì? - B
- A. Chỉ lấy 1 phần tử duy nhất
- B. Bỏ qua 1 phần tử đầu tiên rồi mới bắt đầu lặp
- C. Bắt đầu đếm `forloop.index` từ 1 thay vì 0
- D. Giới hạn tối đa 1 vòng lặp

**Câu 10.** Cho đoạn code:
```liquid
{% for i in (1..5) %}
  {% if i == 2 %}{% continue %}{% endif %}
  {% if i == 4 %}{% break %}{% endif %}
  <span>Số {{ i }}</span>
{% endfor %}
```
Output là gì? - B
- A. `Số 1`, `Số 2`, `Số 3`, `Số 4`
- B. `Số 1`, `Số 3`
- C. `Số 1`, `Số 2`, `Số 3`
- D. `Số 3`, `Số 4`, `Số 5`

**Câu 11.** Khác biệt chính giữa `break` và `continue` trong vòng lặp `for` là gì? - A
- A. `break` thoát hẳn vòng lặp, `continue` chỉ bỏ qua lượt lặp hiện tại rồi tiếp tục lượt sau
- B. `continue` thoát hẳn vòng lặp, `break` chỉ bỏ qua lượt lặp hiện tại
- C. Cả hai đều thoát hẳn vòng lặp, chỉ khác cú pháp
- D. `break` dùng cho `for`, `continue` dùng cho `if`

**Câu 12.** Thẻ `{% assign %}` có tự động in giá trị ra HTML không? - B
- A. Có, `assign` luôn in ra ngay giá trị vừa gán
- B. Không, `assign` chỉ khai báo/cập nhật biến, muốn hiển thị phải dùng `{{ }}`
- C. Chỉ in ra nếu giá trị là số
- D. Chỉ in ra nếu đặt trong thẻ `{%- -%}`

**Câu 13.** Cho đoạn code:
```liquid
{% assign original_price = 100000 %}
{% assign discount_price = original_price | times: 0.8 %}
<p>Giá ưu đãi: {{ discount_price | money }}</p>
```
Giả sử shop dùng USD, output gần đúng là gì? (biết `original_price` lưu theo cents) - B
- A. `Giá ưu đãi: $1000.00`
- B. `Giá ưu đãi: $800.00`
- C. `Giá ưu đãi: $100000.00`
- D. `Giá ưu đãi: 80000`

**Câu 14.** Thẻ `{% capture %}` dùng để làm gì? - B
- A. Chụp ảnh màn hình trang web
- B. Lưu 1 khối nội dung (có thể nhiều dòng HTML) vào 1 biến để dùng lại sau 
- C. Chặn (block) không cho render 1 đoạn code
- D. Giống hệt `{% assign %}`, chỉ là tên khác

**Câu 15.** Cho đoạn code:
```liquid
{% capture placeholder_name %}product-1{% endcapture %}
<ul class="media-wrapper">
  {{ placeholder_name | placeholder_svg_tag: 'placeholder-svg' }}
</ul>
```
`placeholder_name` sau khi `capture` có giá trị (kiểu string) là gì? - Không biết
- A. `{% capture placeholder_name %}`
- B. `product-1`
- C. `nil`
- D. `placeholder-svg`

**Câu 16.** Cho đoạn code:
```liquid
<p>Lần 1: {% increment my_counter %}</p>
<p>Lần 2: {% increment my_counter %}</p>
<p>Lần 3: {% increment my_counter %}</p>
```
Output là gì? - D
- A. `Lần 1: 1`, `Lần 2: 2`, `Lần 3: 3`
- B. `Lần 1: 0`, `Lần 2: 1`, `Lần 3: 2`
- C. `Lần 1: 0`, `Lần 2: 0`, `Lần 3: 0`
- D. Báo lỗi vì `my_counter` chưa được `assign` trước

**Câu 17.** `{% increment %}` khác gì với dùng `{% assign x = x | plus: 1 %}` lặp lại nhiều lần? - B
- A. Không khác gì, chỉ là viết gọn hơn
- B. `increment` tự động khởi tạo biến đếm riêng từ 0, biến này độc lập với biến `assign` cùng tên, và giá trị được in ra ngay (không cần `{{ }}`)
- C. `increment` chỉ dùng được trong vòng lặp `for`
- D. `increment` giảm giá trị biến đi 1 mỗi lần gọi

**Câu 18.** Cho đoạn code:
```liquid
{% paginate collection.products by 12 %}
  {% for product in collection.products %} ... {% endfor %}
  {{ paginate | default_pagination }}
{% endpaginate %}
```
Tham số `by 12` trong `paginate` có nghĩa gì? - B
- A. Chỉ hiển thị tối đa 12 trang phân trang
- B. Số sản phẩm hiển thị tối đa trên mỗi trang là 12 (`paginate.page_size`)
- C. Bỏ qua 12 sản phẩm đầu tiên
- D. Chờ 12 giây trước khi load trang tiếp theo

**Câu 19.** Vì sao bắt buộc phải dùng `{% paginate %}` khi render `collection.products` thay vì chỉ dùng `{% for %}` thuần? - B 
- A. Vì `for` không hỗ trợ object `collection.products`
- B. Vì Liquid giới hạn `for` loop tối đa 50 iterations — vượt quá sẽ bị cắt ngang
- C. Vì `paginate` chạy nhanh hơn `for` gấp nhiều lần
- D. Vì `collection.products` chỉ hoạt động bên trong `paginate`

**Câu 20.** Thuộc tính nào của object `paginate` cho biết tổng số trang được chia ra? - B
- A. `paginate.current_page`
- B. `paginate.page_size`
- C. `paginate.pages`
- D. `paginate.items`

**Câu 21.** `{% render 'header-icon' %}` (cú pháp cơ bản, không truyền tham số) có đặc điểm gì? - B
- A. Tự động kế thừa toàn bộ biến local của file cha
- B. Snippet chỉ đọc được HTML/CSS tĩnh hoặc các object Global (`shop`, `settings`...), không thấy biến local của file gọi nó
- C. Bắt buộc phải truyền ít nhất 1 tham số mới chạy được
- D. Tương đương hoàn toàn với `{% include 'header-icon' %}`

**Câu 22.** Cho snippet `snippets/product-card.liquid` với nội dung: 
```liquid
<h3>{{ product.title }}</h3>
```
Và 2 cách gọi:
```liquid
{% for item in recommended_products %}
  {% render 'product-card' %}
{% endfor %}
```
Điều gì xảy ra? - B 
- A. Chạy đúng bình thường vì `render` tự kế thừa biến `item`
- B. `product` bên trong snippet là `nil` (không báo lỗi, chỉ ra rỗng) vì biến lặp tên là `item` chứ không phải `product`, và `render` chạy Isolated Scope
- C. Báo lỗi biên dịch ngay lập tức
- D. Tự động đổi tên biến `item` thành `product`

**Câu 23.** Cách sửa đúng cho tình huống ở Câu 22 để snippet luôn nhận đúng dữ liệu, bất kể tên biến lặp bên ngoài là gì? - B
- A. Đổi tên biến lặp ngoài thành `product` ở mọi nơi gọi
- B. Truyền tường minh qua tham số: `{% render 'product-card', product: item %}`
- C. Dùng `{% include %}` thay vì `{% render %}`
- D. Không cần sửa, vì đây là hành vi đúng thiết kế

**Câu 24.** Cú pháp `{% render 'product-card' for collection.products as product %}` hoạt động như thế nào? - B
- A. Chỉ render snippet 1 lần duy nhất cho toàn bộ `collection.products`
- B. Tự động lặp qua mảng `collection.products`, mỗi lượt lặp render lại snippet 1 lần và gán phần tử vào biến `product` (có sẵn `forloop`)
- C. Giống hệt `{% render 'product-card' with collection.products as product %}`
- D. Bắt buộc phải khai báo `{% for %}` bao ngoài mới chạy được

**Câu 25.** `{% doc %} ... {% enddoc %}` (LiquidDoc) dùng để làm gì và cú pháp `@param` viết thế nào?  - B
- A. Chỉ là comment thường, `@param` không có ý nghĩa đặc biệt gì
- B. Dùng để ghi chú mô tả input của snippet, khai báo qua `@param {Type} tên - mô tả`, ví dụ `@param {Boolean} [show_vendor=false] - Hiển thị nhà sản xuất hay không`
- C. Dùng để khai báo biến toàn cục cho cả theme
- D. Bắt buộc phải có trong mọi file `.liquid`, nếu thiếu Shopify sẽ báo lỗi build

**Câu 26.** `{% comment %} ... {% endcomment %}` khác gì với `{% # ... %}` (inline comment)? - B
- A. Không khác gì, chỉ là 2 cách viết tương đương cho comment nhiều dòng
- B. `{% comment %}` dùng cho ghi chú nhiều dòng/khối, `{% # %}` là comment ngắn gọn cho 1 dòng
- C. `{% # %}` mới là cú pháp comment nhiều dòng, `{% comment %}` chỉ dùng 1 dòng
- D. `{% comment %}` sẽ render ra HTML dưới dạng `<!-- -->`, còn `{% # %}` thì không

**Câu 27.** Nội dung bên trong `{% comment %} ... {% endcomment %}` có được Liquid thực thi (parse logic bên trong) không? - B
- A. Có, Liquid vẫn chạy logic bên trong nhưng không in kết quả ra
- B. Không, toàn bộ nội dung bị bỏ qua hoàn toàn, không render ra HTML
- C. Chỉ chạy nếu bên trong có thẻ `{% if %}`
- D. Có, và kết quả vẫn hiện ra dưới dạng text thường

**Câu 28.** Cho đoạn code:
```liquid
{%- for i in (1..3) -%}
  <li>{{ i }}</li>
{%- endfor -%}
```
Whitespace control `-` (dấu gạch ngang) trong `{%- -%}` có tác dụng gì? - A
- A. Xóa khoảng trắng/xuống dòng thừa quanh thẻ, giúp output HTML gọn hơn (không ảnh hưởng logic)
- B. Đảo ngược thứ tự vòng lặp
- C. Chuyển vòng lặp từ đếm xuôi sang đếm ngược
- D. Bắt buộc phải escape HTML

**Câu 29.** `{%- tag %}` (chỉ có dấu `-` phía trước) khác gì với `{% tag -%}` (chỉ có dấu `-` phía sau)? - B
- A. Không khác gì
- B. `{%- tag %}` xóa khoảng trắng phía trước thẻ, `{% tag -%}` xóa khoảng trắng phía sau thẻ
- C. `{%- tag %}` chỉ dùng được cho `if`, `{% tag -%}` chỉ dùng cho `for`
- D. `{%- tag %}` xóa toàn bộ HTML sau đó

**Câu 30.** Theo quy tắc thực dụng đã ghi trong tài liệu, nên dùng `-` (whitespace control) cho loại thẻ nào là hợp lý nhất? - A
- A. Tag logic thuần không xuất HTML (`assign`, `if`, `for`, `liquid`, `paginate`...)
- B. Tag output `{{ product.title }}` vì luôn cần gọn
- C. Chỉ dùng cho `{% schema %}`
- D. Không nên dùng `-` ở bất kỳ đâu vì dễ gây lỗi

**Câu 31.** `{% style %}` khác `{% stylesheet %}` ở điểm cốt lõi nào? - B
- A. `{% style %}` chỉ dùng trong `sections/`, còn `{% stylesheet %}` dùng được ở mọi nơi
- B. `{% style %}` dùng được ở bất kỳ đâu (kể cả snippet, layout) và chạy được Liquid động (VD `{{ settings.xxx }}`), còn `{% stylesheet %}` chỉ dùng trong `sections/`, `blocks/` và không chạy Liquid động
- C. Cả hai hoàn toàn giống nhau, chỉ khác tên gọi
- D. `{% stylesheet %}` mới là thẻ chạy được Liquid động, `{% style %}` thì không

**Câu 32.** File `snippets/css-variables.liquid` trong project dùng `{% style %}` (không phải `{% stylesheet %}`) vì lý do gì? - B
- A. Vì snippet không được phép dùng `{% stylesheet %}`
- B. Vì cần sinh CSS variable động dựa trên `settings.xxx` (global settings), mà `{% stylesheet %}` không hỗ trợ chạy Liquid
- C. Vì `{% style %}` chạy nhanh hơn
- D. Vì đây là file duy nhất trong theme cần CSS

**Câu 33.** `{% stylesheet %}` bên trong 1 file section có đặc điểm gì về việc load khi section đó xuất hiện nhiều lần trên 1 trang? - B
- A. Shopify tự dedupe — dù section lặp nhiều lần/trang, CSS chỉ load 1 lần
- B. CSS sẽ được load lại y hệt số lần section xuất hiện
- C. Chỉ load CSS ở lần xuất hiện đầu tiên, các lần sau không có style
- D. Gây lỗi trùng class CSS nếu section lặp lại

**Câu 34.** `{% javascript %}` trong 1 file section dùng để làm gì? - A
- A. Viết JS riêng cho section đó, đặt cùng file `.liquid` với HTML markup, `{% schema %}` và `{% stylesheet %}`
- B. Bắt buộc phải có trong mọi section, nếu thiếu section sẽ không render được
- C. Thay thế hoàn toàn cho file `.js` trong thư mục `assets/`
- D. Chỉ dùng được trong `layout/theme.liquid`

**Câu 35.** `{% schema %} ... {% endschema %}` trong 1 file section chứa những gì? - B
- A. Chỉ chứa CSS của section
- B. Cấu hình JSON định nghĩa `name`, `tag`, `class`, `settings[]`, `blocks[]`, `max_blocks`, `presets[]` — nơi merchant tùy chỉnh qua Theme Editor
- C. Danh sách các file snippet được `render` bên trong section
- D. Bản dịch đa ngôn ngữ của section

**Câu 36.** `{% section 'header' %}` (số ít) khác `{% sections 'header-group' %}` (số nhiều) như thế nào? - B
- A. Không khác gì, chỉ là 2 cách viết tương đương
- B. `{% section 'x' %}` render đúng 1 section cố định; `{% sections 'x-group' %}` render cả 1 NHÓM section khai báo trong file `x-group.json`, cho phép merchant thêm/bớt/sắp xếp lại nhiều section trong nhóm đó
- C. `{% sections %}` chỉ dùng được trong `templates/`, không dùng được trong `layout/`
- D. `{% section %}` mới hỗ trợ merchant thêm/bớt section, `{% sections %}` thì không

**Câu 37.** File cấu hình đi kèm `{% sections 'header-group' %}` cần có cấu trúc gì? - B
- A. Không cần file cấu hình nào
- B. `sections/header-group.json` với cấu trúc `sections{}` + `order[]`, giống cấu trúc file template JSON
- C. `sections/header-group.liquid` chứa HTML thuần
- D. `config/header-group.json` trong thư mục `config/`

**Câu 38.** Thẻ `{% liquid %} ... {% endliquid %}` (hoặc chỉ `{% liquid ... %}`) dùng để làm gì? - B
- A. Chỉ dùng được cho đúng 1 dòng logic duy nhất
- B. Cho phép gộp nhiều dòng logic Liquid (assign, if, for...) vào 1 khối, mỗi dòng không cần bọc riêng `{% %}`, giúp code gọn hơn khi có nhiều thẻ logic liên tiếp
- C. Thay thế hoàn toàn cho `{{ }}` output tag
- D. Chỉ dùng được trong file `{% schema %}`

**Câu 39.** Bên trong khối `{% liquid %}`, cú pháp mỗi dòng có gì khác so với viết `{% %}` thông thường? - B
- A. Mỗi dòng vẫn phải có đủ dấu `{%` và `%}` bao quanh
- B. Mỗi dòng chỉ viết phần lệnh (VD `assign x = 1`, `if x > 0`), không cần dấu `{%` `%}` cho từng dòng vì cả khối đã nằm trong 1 cặp `{% liquid %}` duy nhất
- C. Bắt buộc phải viết toàn bộ trên 1 dòng duy nhất, không được xuống dòng
- D. Không hỗ trợ `if`/`for` bên trong, chỉ hỗ trợ `assign`

**Câu 40.** Bên trong `{% liquid %}`, có thể dùng `{{ }}` output tag để in HTML trực tiếp không? - B (cần giải thích thêm về cơ chế echo..)
- A. Có, dùng y hệt như bên ngoài
- B. Không — bên trong `{% liquid %}` chỉ chứa các dòng logic (tag), không dùng `{{ }}` output hay HTML thô; muốn in ra phải dùng `echo` hoặc `assign`/`capture` rồi in ở ngoài khối
- C. Có, nhưng phải thêm dấu `-` phía trước
- D. Chỉ dùng được `{{ }}` nếu nằm trong `{% if %}`

---

### Bài tập viết code

**Bài 1.** Viết đoạn Liquid dùng `{% assign %}` để khai báo biến `stock_quantity = 0`, sau đó dùng `{% if %}/{% elsif %}/{% else %}` để hiển thị:
- Nếu `stock_quantity == 0` → in `<span class="badge badge--sold-out">Hết hàng</span>`
- Nếu `stock_quantity > 0` và `stock_quantity <= 5` → in `<span class="badge badge--low-stock">Sắp hết hàng</span>`
- Ngược lại → in `<span class="badge badge--in-stock">Còn hàng</span>` Với `stock_quantity = 0`, output mong muốn: `<span class="badge badge--sold-out">Hết hàng</span>`

-> liquid 
"
  {% assign stock_quantity = 0 %}
  {% if stock_quantity == 0 or stock_quantity = 0 %}
    <span class="badge badge--sold-out">Hết hàng</span>
  {% elsif stock_quantity > 0 and stock_quantity <= 5 %}
    <span class="badge badge--low-stock">Sắp hết hàng</span>
  {% else %}
    <span class="badge badge--in-stock">Còn hàng</span>
  {% endif%}
"

**Bài 2.** Cho mảng `{% assign sizes = "S, M, L, XL, XXL" | split: ", " %}`. Viết vòng lặp `{% for %}` chỉ lặp qua **2 phần tử ở giữa** (`M`, `L`) bằng cách dùng `limit` và `offset` phù hợp, in ra mỗi phần tử dạng `<li>Size: M</li>`, `<li>Size: L</li>`.

-> liquid
"
  {% assign sizes = "S, M, L, XL, XXL" | split: ", " %}
  {% for i in sizes limit: 2 offset: 1 %}
  <li>Size: {{i}}</li>
  {% endfor %}
"

**Bài 3.** Viết snippet `snippets/product-card-mini.liquid` với nội dung `<h4>{{ product.title }} - {{ product.price | money }}</h4>`, có khai báo `{% doc %}` mô tả tham số `@param {Object} product - Sản phẩm cần hiển thị (Bắt buộc)`. Sau đó viết đoạn code gọi snippet này để render lặp qua toàn bộ `collection.products`, dùng đúng 1 dòng `{% render %}` (không dùng `{% for %}` bao ngoài) theo cú pháp `for ... as`.

-> liquid
"
// snippets/product-card-mini.liquid
{% doc %}
  Render product card mini
  @param {Object} product - Sản phẩm cần hiển thị (Bắt buộc)
{% enddoc%}
  h4>{{ product.title }} - {{ product.price | money }}</h4>

// render
{% render 'product-card-mini' for collection.products as product %}
"

**Bài 4.** Viết đoạn code dùng `{% capture %}` để gom nhóm 3 dòng `<li>` (Đỏ, Xanh, Vàng) vào 1 biến tên `color_list_html`, sau đó in biến đó ra bằng `{{ color_list_html }}` bên trong 1 thẻ `<ul>...</ul>`.

-> liquid
"
{% capture color_list_html %}
<li>Do</li>
<li>Xanh</li>
<li>Vang</li>
{% endcapture %}

<ul class="media-wrapper">
  {{ color_list_html }}
</ul>
"

**Bài 5.** Viết đoạn code dùng `{% paginate collection.products by 8 %}` để phân trang, bên trong render `product-card` cho từng sản phẩm bằng cú pháp `render 'for as'`, in số lượng sản phẩm đang hiển thị dạng `Hiển thị X / Y sản phẩm` (dùng `collection.products.size` và `collection.products_count`), và chỉ hiện `{{ paginate | default_pagination }}` khi có nhiều hơn 1 trang. Áp dụng whitespace control `{%- -%}` cho toàn bộ tag logic.

-> liquid
"
{%- paginate collection.products by 8 -%}
  {%- render 'product-card' for collection.products as product -%}
  <div> Hiển thị {{ collection.products_count }} / {{ collection.products.size }} sản phẩm </div>
  {%- if collection.product.size > 8 -%}
    {{ paginate | default_pagination }}
  {%- endif -%}
{%- endpaginate -%}
"

**Bài 6.** Viết lại đoạn code sau bằng khối `{% liquid %}` gộp chung (thay vì viết rời từng thẻ `{% %}`):
```liquid
{% assign total = 0 %}
{% for item in cart.items %}
  {% assign total = total | plus: item.final_line_price %}
{% endfor %}
{{ total | money }}
```
Lưu ý: dòng cuối in ra `{{ total | money }}` vẫn phải viết ở ngoài khối `{% liquid %}` như bình thường.

-> liquid
"
  {% liquid
  assign total = 0 
  for item in cart.items
    assign total = total | plus: item.final_line_price
  endfor
    echo total | money
%}
  
"

---

## 🔑 Đáp án & Giải thích

### Đáp án trắc nghiệm

**Câu 1:** B — `available` là `true` nên `available == false` sai, chuyển sang kiểm tra `elsif compare_price > product_price` (50000 > 40000 → đúng), render nhánh `badge--sale`.

**Câu 2:** B — `unless` là "if ngược": chỉ chạy nội dung bên trong khi điều kiện là `false` (đối lập với `if`).

**Câu 3:** B — `is_featured = false`, `unless false` tương đương "chạy khi false" → điều kiện đúng, render đoạn `<p>` bên trong.

**Câu 4:** B — `product_type` là `'Pants'`, khớp `when 'Pants'` → in "Danh mục: Quần dài". `case` chỉ chạy đúng 1 nhánh khớp đầu tiên, không rơi tiếp xuống các `when` khác.

**Câu 5:** C — `case` phải đặt biến cần so sánh (`block.type`), còn tên loại (`'icon_with_text'`) chỉ được dùng trong `when` để so sánh giá trị. Viết `{% case block.icon_with_text %}` là sai cú pháp/logic vì `block.icon_with_text` không tồn tại.

**Câu 6:** A — `contains` kiểm tra 1 string/array có chứa 1 substring/phần tử con nào đó hay không, trả về `true`/`false`.

**Câu 7:** B — `split: ", "` biến chuỗi thành array `["new-arrival", "summer", "sale"]`, array này chứa `'new-arrival'` nên `contains` trả `true`, render badge.

**Câu 8:** B — `offset: 1` bỏ qua phần tử đầu (`Áo Thun`), `limit: 2` lấy 2 phần tử tiếp theo (`Áo Sơ Mi`, `Quần Jeans`); `forloop.index` vẫn đếm lại từ 1 cho các phần tử được lặp thực tế, không giữ nguyên vị trí gốc trong mảng.

**Câu 9:** B — `offset: N` bỏ qua N phần tử đầu tiên của tập dữ liệu trước khi bắt đầu vòng lặp; không liên quan tới cách đếm `forloop.index`.

**Câu 10:** B — Lượt `i=2` bị `continue` bỏ qua (không in), lượt `i=4` gặp `break` nên thoát hẳn vòng lặp trước khi in — kết quả chỉ có `Số 1` và `Số 3`.

**Câu 11:** A — `break` thoát hoàn toàn khỏi vòng lặp (các lượt sau không chạy nữa), `continue` chỉ bỏ qua phần còn lại của lượt hiện tại rồi tiếp tục lượt kế tiếp bình thường.

**Câu 12:** B — `assign` chỉ khai báo/gán giá trị biến, hoàn toàn không tự in ra HTML; phải dùng `{{ tên_biến }}` mới hiển thị được giá trị.

**Câu 13:** B — `original_price = 100000` (cents) × 0.8 = `80000` cents, filter `money` đổi cents → tiền tệ hiển thị = `$800.00`.

**Câu 14:** B — `capture` gom 1 khối nội dung (có thể nhiều dòng, gồm cả HTML/logic Liquid) và lưu kết quả dạng string vào 1 biến để tái sử dụng sau đó.

**Câu 15:** B — Sau `{% capture placeholder_name %}product-1{% endcapture %}`, biến `placeholder_name` có giá trị string là `"product-1"`.

**Câu 16:** B — `increment` bắt đầu đếm từ `0` và tự tăng dần sau mỗi lần gọi: lần 1 trả về `0`, lần 2 trả về `1`, lần 3 trả về `2`.

**Câu 17:** B — `increment` tạo và quản lý 1 biến đếm nội bộ độc lập (không đụng biến `assign` cùng tên nếu có), tự tăng dần từ 0, và giá trị được in ra ngay tại chỗ gọi mà không cần bọc `{{ }}`.

**Câu 18:** B — `by 12` thiết lập `paginate.page_size = 12`, tức số phần tử tối đa hiển thị trên mỗi trang (tối đa cho phép là `50`).

**Câu 19:** B — Liquid giới hạn vòng lặp `for` tối đa **50 iterations**; nếu `collection.products` có nhiều hơn 50 sản phẩm, `for` thuần sẽ bị cắt ngang, nên bắt buộc dùng `paginate` để chia nhỏ dữ liệu ra từng trang.

**Câu 20:** C — `paginate.pages` là tổng số trang được tự động chia ra dựa trên `paginate.items` và `page_size`.

**Câu 21:** B — Cú pháp cơ bản `render 'x'` không truyền tham số nào, snippet chỉ đọc được nội dung tĩnh hoặc các object Global (`shop`, `settings`...) — không thấy được biến local của file gọi nó do cơ chế Isolated Scope.

**Câu 22:** B — Do `render` chạy Isolated Scope, snippet không tự thấy biến lặp `item`; biến `product` bên trong snippet là `nil`, Liquid không báo lỗi mà chỉ render ra rỗng (`{{ nil.title }}` → chuỗi rỗng).

**Câu 23:** B — Luôn truyền tường minh qua tham số (`product: item`) để snippet không phụ thuộc vào việc tên biến ngoài có "tình cờ" trùng tên hay không — đây là cách viết snippet an toàn giống thiết kế 1 hàm (function) với input rõ ràng.

**Câu 24:** B — `for collection.products as product` vừa tự động lặp qua mảng, vừa gán từng phần tử vào biến `product` ở mỗi lượt render snippet, đồng thời tự có sẵn đối tượng `forloop` bên trong snippet — đây là cách tối ưu hiệu năng nhất khi render danh sách.

**Câu 25:** B — `{% doc %}...{% enddoc %}` (LiquidDoc) dùng để ghi chú mô tả input/param của snippet một cách chuẩn hóa; cú pháp `@param {Type} tên_biến - mô tả`, có thể thêm giá trị mặc định bằng ngoặc vuông: `@param {Boolean} [show_vendor=false] - mô tả`.

**Câu 26:** B — `{% comment %}` phù hợp cho ghi chú dài/nhiều dòng hoặc "tắt tạm" cả 1 khối code; `{% # %}` là cú pháp inline comment gọn cho ghi chú ngắn trên 1 dòng.

**Câu 27:** B — Toàn bộ nội dung bên trong `{% comment %}...{% endcomment %}` bị Liquid bỏ qua hoàn toàn (không parse, không thực thi, không render), kể cả khi bên trong có cú pháp Liquid hợp lệ.

**Câu 28:** A — `{%- -%}` chỉ có tác dụng loại bỏ khoảng trắng/ký tự xuống dòng thừa quanh vị trí đặt thẻ trong HTML output, hoàn toàn không ảnh hưởng tới logic xử lý bên trong.

**Câu 29:** B — Dấu `-` ở vị trí nào thì xóa khoảng trắng ở phía đó: `{%- tag %}` xóa phía trước thẻ, `{% tag -%}` xóa phía sau thẻ, `{%- tag -%}` xóa cả hai phía.

**Câu 30:** A — Nên ưu tiên dùng `-` cho các tag logic thuần (không tự xuất HTML) như `assign`, `if`, `for`, `liquid`, `paginate`; còn tag output `{{ }}` thường giữ nguyên (không thêm `-`) để tránh dính chữ liền nhau khi nối 2 giá trị.

**Câu 31:** B — Khác biệt cốt lõi: `{% style %}` dùng được ở bất kỳ đâu (snippet, layout, section...) và hỗ trợ Liquid động (đọc được `settings.xxx`); `{% stylesheet %}` chỉ dùng trong `sections/`, `blocks/` và không chạy được logic Liquid bên trong.

**Câu 32:** B — `snippets/css-variables.liquid` cần sinh giá trị CSS variable dựa trên dữ liệu động (`settings.xxx` từ Theme Editor), điều mà `{% stylesheet %}` không hỗ trợ vì nó không chạy Liquid; do đó bắt buộc phải dùng `{% style %}`.

**Câu 33:** A — Shopify tự động dedupe CSS trong `{% stylesheet %}`: dù 1 section được thêm lặp lại nhiều lần trên cùng 1 trang, khối CSS đó chỉ được load đúng 1 lần duy nhất.

**Câu 34:** A — `{% javascript %}` cho phép viết JS riêng ngay trong cùng file `.liquid` của section, đi kèm HTML markup, `{% schema %}` và `{% stylesheet %}` (là 1 phần tùy chọn — optional — trong "anatomy" của 1 file section).

**Câu 35:** B — `{% schema %}...{% endschema %}` chứa cấu hình JSON mô tả toàn bộ metadata của section: `name` (tên hiện trong "Add section"), `tag`, `class`, `settings[]`, `blocks[]`, `max_blocks`, `presets[]` — là nơi merchant có thể tùy chỉnh qua Theme Editor.

**Câu 36:** B — `{% section 'x' %}` (số ít) luôn render đúng 1 section cố định, merchant không thêm/bớt được section khác tại vị trí đó; `{% sections 'x-group' %}` (số nhiều) render cả 1 nhóm section được khai báo trong `x-group.json`, cho phép merchant thêm/bớt/sắp xếp lại nhiều section (VD dùng cho header/footer để chèn thêm thanh khuyến mãi).

**Câu 37:** B — Cần file `sections/<tên>-group.json` với cấu trúc gồm object `sections{}` (định nghĩa từng section trong nhóm) và mảng `order[]` (thứ tự hiển thị) — cùng cấu trúc với file template JSON thông thường (VD `index.json`).

**Câu 38:** B — `{% liquid %}` cho phép viết nhiều dòng lệnh logic Liquid liên tiếp (assign, if, for, case...) trong 1 khối duy nhất, mỗi dòng chỉ cần viết phần lệnh mà không cần bọc riêng từng cặp `{% %}`, giúp code gọn và dễ đọc hơn khi có nhiều bước xử lý logic nối tiếp nhau.

**Câu 39:** B — Bên trong `{% liquid %}`, mỗi dòng chỉ viết đúng phần thân lệnh (VD `assign total = 0`, `if total > 0`, `endif`) mà không cần dấu `{%`/`%}` bao quanh từng dòng, vì toàn bộ khối đã nằm trong 1 cặp `{% liquid %}...{% endliquid %}` (hoặc `{% liquid ... %}`) chung.

**Câu 40:** B — `{% liquid %}` chỉ chứa các dòng tag logic thuần túy, không được viết `{{ }}` output hay HTML thô xen vào giữa; muốn xuất giá trị ra, dùng lệnh `echo` bên trong khối (VD `echo total`), hoặc `assign`/`capture` giá trị rồi in ra bằng `{{ }}` ở bên ngoài khối `{% liquid %}`.

### Đáp án bài tập code

**Bài 1:**
```liquid
{% assign stock_quantity = 0 %}

{% if stock_quantity == 0 %}
  <span class="badge badge--sold-out">Hết hàng</span>
{% elsif stock_quantity > 0 and stock_quantity <= 5 %}
  <span class="badge badge--low-stock">Sắp hết hàng</span>
{% else %}
  <span class="badge badge--in-stock">Còn hàng</span>
{% endif %}
```
Với `stock_quantity = 0`, nhánh `if` đầu tiên khớp ngay, output: `<span class="badge badge--sold-out">Hết hàng</span>`.

**Bài 2:**
```liquid
{% assign sizes = "S, M, L, XL, XXL" | split: ", " %}
{% for size in sizes limit: 2 offset: 1 %}
  <li>Size: {{ size }}</li>
{% endfor %}
```
Mảng `sizes` có 5 phần tử (`S`, `M`, `L`, `XL`, `XXL` — chỉ số 0 đến 4). `offset: 1` bỏ qua `S`, bắt đầu từ `M`; `limit: 2` lấy đúng 2 phần tử (`M`, `L`). Output:
```html
<li>Size: M</li>
<li>Size: L</li>
```

**Bài 3:**
```liquid
{% doc %}
  @file snippets/product-card-mini.liquid
  @param {Object} product - Sản phẩm cần hiển thị (Bắt buộc)
{% enddoc %}
<h4>{{ product.title }} - {{ product.price | money }}</h4>
```
Đoạn gọi để render lặp qua toàn bộ sản phẩm:
```liquid
{% render 'product-card-mini' for collection.products as product %}
```
Cú pháp `for ... as` tự lặp qua `collection.products`, mỗi lượt render lại snippet và gán phần tử vào biến `product` — không cần viết `{% for %}` bao ngoài.

**Bài 4:**
```liquid
{% capture color_list_html %}
  <li>Đỏ</li>
  <li>Xanh</li>
  <li>Vàng</li>
{% endcapture %}

<ul>{{ color_list_html }}</ul>
```
`capture` gom 3 dòng `<li>` thành 1 chuỗi HTML lưu vào biến `color_list_html`, sau đó in ra bên trong `<ul>`.

**Bài 5:**
```liquid
{%- paginate collection.products by 8 -%}
  <p>Hiển thị {{ collection.products.size }} / {{ collection.products_count }} sản phẩm</p>

  <ul class="collection-page__grid">
    {%- render 'product-card' for collection.products as product -%}
  </ul>

  {%- if paginate.pages > 1 -%}
    {{ paginate | default_pagination }}
  {%- endif -%}
{%- endpaginate -%}
```
`by 8` giới hạn `page_size = 8`; toàn bộ tag logic (`paginate`, `render`, `if`, `endif`, `endpaginate`) dùng whitespace control `{%- -%}` để tránh dư dòng trắng, còn output tag `{{ }}` giữ nguyên.

**Bài 6:**
```liquid
{% liquid
  assign total = 0
  for item in cart.items
    assign total = total | plus: item.final_line_price
  endfor
%}
{{ total | money }}
```
Toàn bộ `assign`, `for`, `endfor` được gộp vào 1 khối `{% liquid %}` duy nhất, mỗi dòng chỉ viết phần lệnh không cần `{% %}` riêng; dòng in kết quả `{{ total | money }}` vẫn viết ở ngoài khối như output tag bình thường.

---

## 📊 Nhận xét kết quả bài test

### Điểm số
- **Trắc nghiệm:** 36/40 (90%)
- **Bài tập code:** 2/6 hoàn hảo (Bài 2, 4), 4/6 còn lỗi ở các mức khác nhau (Bài 1, 3, 5, 6)

### Chi tiết câu trắc nghiệm sai/bỏ trống
| Câu | Đã chọn | Đáp án đúng | Vì sao sai |
|---|---|---|---|
| 5 | B | **C** | Câu hỏi hỏi đâu là cú pháp **SAI**. A, B, D đều `case block.type` hợp lệ. Riêng C dùng `{% case block.icon_with_text %}` — case sai biến (phải case theo `block.type`) → C mới là đáp án cần chọn. |
| 15 | Không biết | **B** | `capture` lưu nội dung bên trong dạng chuỗi → `placeholder_name` = `"product-1"`. |
| 16 | D | **B** | `{% increment %}` không cần `assign` trước — tự khởi tạo biến đếm riêng từ 0. Kết quả: Lần 1: `0`, Lần 2: `1`, Lần 3: `2`. |
| 20 | B | **C** | `paginate.page_size` là số sản phẩm/trang (cấu hình), còn **tổng số trang** là `paginate.pages`. |
| 33 | B | **A** | Shopify tự động dedupe CSS trong `{% stylesheet %}` — section lặp lại nhiều lần trên trang thì CSS vẫn chỉ load đúng 1 lần. |

### Lỗi trong bài tập code
- **Bài 1:** điều kiện `or stock_quantity = 0` vừa thừa (lặp lại điều kiện đầu) vừa sai cú pháp so sánh (`=` thay vì `==`). Output cuối vẫn đúng nhưng code có lỗi cần sửa.
- **Bài 3:** thiếu dấu `<` khi mở thẻ — `h4>...` phải là `<h4>...</h4>`.
- **Bài 5:** 2 lỗi logic — (1) đảo ngược thứ tự `collection.products.size` / `collection.products_count` so với yêu cầu đề; (2) điều kiện hiện phân trang sai hoàn toàn, dùng `collection.product.size > 8` (vừa sai logic vừa typo thiếu "s") thay vì `paginate.pages > 1`, khiến phân trang không bao giờ hiện.
- **Bài 6:** không đúng yêu cầu đề — đưa `echo total | money` vào **trong** khối `{% liquid %}` dù đề yêu cầu rõ dòng in kết quả phải viết ở **ngoài** khối bằng `{{ total | money }}`.

### Điểm mạnh — nắm chắc
- **Control flow cơ bản:** `if/elsif/else`, `unless`, `case/when`, `contains`, `for` với `limit/offset`, `break/continue` — nhóm câu 1–11 gần như tuyệt đối.
- **`render` & Isolated Scope (Câu 21–24):** đây thường là điểm mù của người mới học Liquid, nhưng trả lời đúng cả cụm — hiểu rõ vì sao phải truyền tham số tường minh và cách dùng `for...as`.
- **`capture`, comment (`{% comment %}` vs `{% # %}`), whitespace control `-`:** đúng hết, hiểu đúng bản chất.
- **`style` vs `stylesheet`, `schema`, `section` vs `sections`:** nắm được sự khác biệt cốt lõi.

### Điểm cần củng cố — có lỗ hổng rõ
1. **Object `paginate` — nhầm lẫn thuộc tính:** sai Câu 20 (`page_size` vs `pages`) **và** lặp lại đúng lỗi này ở Bài 5 → lỗ hổng có hệ thống, nên học lại kỹ toàn bộ thuộc tính của `paginate` (`page_size`, `pages`, `current_page`, `items`...).
2. **Cơ chế `echo` trong khối `{% liquid %}`:** tự nhận chưa chắc ở Câu 40, và áp dụng sai ở Bài 6 — cần phân biệt khi nào dùng `echo` bên trong khối và khi nào nên `assign`/`capture` rồi in bằng `{{ }}` bên ngoài.
3. **`increment`:** hiểu sai rằng phải `assign` trước mới dùng được — chưa nắm đặc điểm "tự khởi tạo độc lập từ 0".
4. **CSS dedupe trong `stylesheet`:** chưa biết Shopify tự động gộp CSS khi section lặp lại nhiều lần trên trang.
5. **Câu hỏi dạng phủ định ("đâu là cú pháp SAI"):** dễ chọn nhầm phương án đúng thay vì sai — cần đọc kỹ chiều câu hỏi.

### Lỗi khi viết code — chủ yếu do cẩu thả, không phải thiếu kiến thức
Các lỗi ở Bài 1, 3, 5 (điều kiện thừa, thiếu dấu `<`, đảo thứ tự tham số) không phản ánh thiếu kiến thức mà là thiếu bước kiểm tra lại sau khi viết — nên tập thói quen đọc lại yêu cầu đề bài từng từ trước khi chốt đáp án.

### Kết luận
Nền tảng logic Liquid (if/for/case, scope, capture) **vững**, đặc biệt phần `render`/isolated scope thường bị hiểu sai nhưng làm tốt. Ưu tiên ôn lại: **object `paginate`** (lỗ hổng lặp lại 2 lần), cơ chế **`echo` trong `{% liquid %}`**, và tập thói quen **đọc lại đề + review code** trước khi hoàn thiện.
