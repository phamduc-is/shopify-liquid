# 📝 Test kiến thức — Nhóm 3: Đối tượng `paginate`

> Làm hết phần Câu hỏi trước, KHÔNG kéo xuống xem Đáp án sớm.

---

## 📋 Câu hỏi

### Trắc nghiệm

**Câu 1.** Cú pháp nào sau đây khai báo đúng thẻ `paginate` để phân trang `collection.products`, mỗi trang tối đa 12 sản phẩm?
- A. `{% paginate collection.products limit: 12 %} ... {% endpaginate %}`
- B. `{% paginate collection.products by 12 %} ... {% endpaginate %}`
- C. `{% for product in collection.products by 12 %} ... {% endfor %}`
- D. `{% paginate by 12 in collection.products %} ... {% endpaginate %}`

**Câu 2.** `paginate.current_page` trả về giá trị gì?
- A. Tổng số trang được chia ra
- B. Số thứ tự của trang mà người dùng đang xem hiện tại
- C. Tổng số sản phẩm trên trang hiện tại
- D. URL của trang hiện tại

**Câu 3.** Nếu `collection.products` có 50 sản phẩm và bạn dùng `{% paginate collection.products by 12 %}`, thì `paginate.pages` sẽ bằng bao nhiêu?
- A. 4
- B. 5
- C. 12
- D. 50

**Câu 4.** `paginate.items` dùng để lấy giá trị nào?
- A. Số sản phẩm hiển thị trên 1 trang (bằng đúng `by N`)
- B. Tổng số phần tử (toàn bộ) đang được phân trang, không phụ thuộc trang hiện tại
- C. Danh sách (Array) các sản phẩm trên trang hiện tại
- D. Số trang còn lại phía sau trang hiện tại

**Câu 5.** Với `{% paginate collection.products by 12 %}`, thuộc tính `paginate.page_size` sẽ có giá trị là:
- A. Luôn luôn là `50` bất kể `by N` là bao nhiêu
- B. Bằng giá trị `N` đã khai báo sau từ khóa `by` (ở đây là `12`)
- C. Bằng tổng số sản phẩm của `collection.products`
- D. Bằng số trang hiện có (`paginate.pages`)

**Câu 6.** Đang ở **trang 3** với `page_size = 12`. Vậy `paginate.current_offset` bằng bao nhiêu?
- A. `3`
- B. `12`
- C. `24`
- D. `36`

**Câu 7.** Khi người dùng đang ở **trang 1** (trang đầu tiên), `paginate.previous` sẽ trả về giá trị gì?
- A. Một Object rỗng `{}`
- B. `nil`
- C. Số `0`
- D. Chuỗi rỗng `""`

**Câu 8.** Đoạn code sau có lỗi tiềm ẩn nào?
```liquid
<a href="{{ paginate.previous.url }}">« Trang trước</a>
```
- A. Không có lỗi gì, luôn chạy đúng trong mọi trường hợp
- B. Sai tên thuộc tính, phải là `paginate.prev.url`
- C. Nếu đang ở trang đầu tiên, `paginate.previous` là `nil` → link sẽ render ra `href=""`, cần kiểm tra `{% if paginate.previous %}` trước khi hiển thị
- D. `paginate.previous.url` không tồn tại, chỉ có `paginate.previous.title`

**Câu 9.** `paginate.next` có đặc điểm giống với `paginate.previous` ở điểm nào?
- A. Cả hai đều luôn là Array
- B. Cả hai đều có thể là `nil` (khi không còn trang kế tiếp/trước đó) và khi tồn tại đều có `.url` và `.title`
- C. Cả hai đều là kiểu Integer thể hiện số trang
- D. Cả hai đều không thể dùng trong `{% if %}`

**Câu 10.** `paginate.parts` trả về kiểu dữ liệu gì và dùng để làm gì?
- A. Một Integer, dùng để đếm số trang
- B. Một String đã render sẵn HTML pagination
- C. Một Array chứa các "part" (mỗi phần tử có `title`, `url`, `is_link`), dùng để tự dựng giao diện phân trang (custom pagination UI)
- D. Một Boolean cho biết có nên hiển thị pagination hay không

**Câu 11.** Filter/tag nào sau đây giúp render **nhanh** một thanh phân trang mặc định của Shopify mà không cần tự viết HTML?
- A. `{{ paginate | default_pagination }}`
- B. `{{ paginate | render_pagination }}`
- C. `{% include paginate %}`
- D. `{{ paginate.html }}`

**Câu 12.** Vì sao khi render `collection.products` (hoặc mảng lớn khác), Shopify **bắt buộc** phải dùng `{% paginate %}` thay vì chỉ dùng `{% for %}` thuần?
- A. Vì `{% for %}` không hỗ trợ vòng lặp qua sản phẩm
- B. Vì thẻ `for` trong Liquid có giới hạn tối đa **50 iterations** mỗi lần lặp — nếu collection có nhiều hơn 50 sản phẩm mà không `paginate`, vòng lặp sẽ bị cắt ngang, không hiển thị đủ sản phẩm
- C. Vì `paginate` chạy nhanh hơn `for` về mặt hiệu năng server
- D. Vì Shopify Admin không cho phép dùng `for` với `collection.products`

---

### Bài tập viết code

**Bài 1.** Viết đoạn Liquid tự dựng **custom pagination UI** (không dùng `default_pagination`) bằng cách lặp qua `paginate.parts`. Yêu cầu:
- Nếu `part.is_link` là `true` → render thẻ `<a href="{{ part.url }}">{{ part.title }}</a>`.
- Nếu `part.is_link` là `false` (là trang hiện tại, hoặc dấu `…` biểu thị các trang bị bỏ qua) → render `<span>{{ part.title }}</span>` (không có link).
- Bọc toàn bộ trong `<div class="custom-pagination">...</div>`.

**Bài 2.** Viết đoạn Liquid tạo 2 nút **"« Trước"** và **"Sau »"** dùng `paginate.previous` và `paginate.next`. Yêu cầu:
- Chỉ hiển thị nút "« Trước" khi `paginate.previous` khác `nil` (có trang trước), link trỏ tới `paginate.previous.url`.
- Chỉ hiển thị nút "Sau »" khi `paginate.next` khác `nil` (có trang sau), link trỏ tới `paginate.next.url`.
- Nếu không có trang trước/sau thì **không render** thẻ `<a>` tương ứng (tránh bug href rỗng).

**Bài 3.** Cho `{% paginate collection.products by 12 %}` và giả sử đang ở **trang 2**, `collection.products_count` (tổng số sản phẩm toàn collection) là `30`. Viết đoạn Liquid tính và hiển thị dòng chữ dạng:
```
Hiển thị sản phẩm 13–24 trong tổng số 30 sản phẩm
```
Gợi ý: số bắt đầu = `current_offset + 1`; số kết thúc = `current_offset + paginate.items trên trang hiện tại` (dùng `collection.products.size` để lấy số sản phẩm thực tế đang hiển thị trên trang, đề phòng trang cuối không đủ 12 sản phẩm).

---

## 🔑 Đáp án & Giải thích

### Đáp án trắc nghiệm

**Câu 1:** B — Cú pháp chuẩn là `{% paginate <array> by <N> %} ... {% endpaginate %}`. Không có tag `limit:` hay đảo ngược thứ tự như C, D.

**Câu 2:** B — `paginate.current_page` là Integer thể hiện số thứ tự trang hiện tại (VD: `1`, `2`...), đếm từ 1.

**Câu 3:** B — 50 sản phẩm chia cho `page_size = 12` → `ceil(50/12) = 5` trang (4 trang đủ 12 + 1 trang cuối còn 2 sản phẩm). `paginate.pages` luôn làm tròn lên đủ để chứa hết `items`.

**Câu 4:** B — `paginate.items` là **tổng số phần tử toàn bộ** đang được phân trang (VD tổng 50 sản phẩm), khác với số sản phẩm hiển thị trên 1 trang (đó là `page_size`).

**Câu 5:** B — `page_size` = giá trị `N` khai báo sau `by` trong tag `paginate` (tối đa Shopify cho phép là `50`; nếu khai `by` lớn hơn 50, Shopify sẽ giới hạn lại ở 50).

**Câu 6:** C — Công thức `current_offset = (current_page - 1) * page_size = (3 - 1) * 12 = 24`. Đây là số sản phẩm đã bị "bỏ qua" trước trang hiện tại.

**Câu 7:** B — `paginate.previous` trả về `nil` khi đang ở trang đầu tiên (không có trang nào trước nó). Tương tự `paginate.next` trả về `nil` ở trang cuối cùng.

**Câu 8:** C — Đây là bẫy hay gặp: khi ở trang 1, `paginate.previous` là `nil`, nên `paginate.previous.url` cũng là `nil`/blank → thẻ `<a>` vẫn render nhưng với `href=""`, dễ gây lỗi UX (click vào không đi đâu, hoặc reload trang). Luôn cần bọc `{% if paginate.previous %}` (hoặc `!= blank`) trước khi in `.url`/`.title`.

**Câu 9:** B — Cả `previous` và `next` đều là Object có thể là `nil` tùy vị trí trang hiện tại; khi tồn tại (không phải `nil`) thì đều có 2 thuộc tính con là `.url` (đường dẫn) và `.title` (nhãn hiển thị, VD "« Previous", "Next »").

**Câu 10:** C — `paginate.parts` là một **Array**, mỗi phần tử đại diện cho 1 "mảnh" trong thanh phân trang (số trang cụ thể hoặc dấu `…`), có các thuộc tính `title`, `url`, `is_link` — dùng để tự code giao diện pagination theo ý muốn thay vì dùng HTML mặc định.

**Câu 11:** A — `{{ paginate | default_pagination }}` là filter dựng sẵn của Shopify, tự render toàn bộ HTML pagination mặc định (kèm class `.pagination`) mà không cần viết tay từng phần tử.

**Câu 12:** B — Thẻ `{% for %}` trong Liquid có giới hạn cứng **50 lượt lặp (50 iterations)** mỗi lần chạy. Nếu `collection.products` (hoặc mảng bất kỳ) có nhiều hơn 50 phần tử mà không bọc trong `{% paginate %}`, vòng lặp `for` sẽ tự động dừng lại ở phần tử thứ 50, khiến các sản phẩm còn lại "biến mất" khỏi trang — vì vậy `paginate` là bắt buộc, không chỉ để có giao diện phân trang đẹp mà còn để tránh mất dữ liệu do giới hạn của `for`.

---

### Đáp án bài tập code

**Bài 1:**
```liquid
{% paginate collection.products by 12 %}
  {% for product in collection.products %}
    {% comment %} ... render sản phẩm ... {% endcomment %}
  {% endfor %}

  <div class="custom-pagination">
    {% for part in paginate.parts %}
      {% if part.is_link %}
        <a href="{{ part.url }}">{{ part.title }}</a>
      {% else %}
        <span>{{ part.title }}</span>
      {% endif %}
    {% endfor %}
  </div>
{% endpaginate %}
```
Giải thích: `part.is_link` là `true` khi phần tử đó có thể click (số trang khác trang hiện tại); `false` khi đó là trang hiện tại hoặc dấu `…` (không có link để trỏ tới) — nên chỉ render `<span>` cho các trường hợp này, tránh in `href=""` gây lỗi.

**Bài 2:**
```liquid
{% paginate collection.products by 12 %}
  {% for product in collection.products %}
    {% comment %} ... render sản phẩm ... {% endcomment %}
  {% endfor %}

  <div class="pagination-nav">
    {% if paginate.previous %}
      <a href="{{ paginate.previous.url }}" class="btn-prev">« Trước</a>
    {% endif %}

    {% if paginate.next %}
      <a href="{{ paginate.next.url }}" class="btn-next">Sau »</a>
    {% endif %}
  </div>
{% endpaginate %}
```
Giải thích: Bọc `{% if paginate.previous %}` / `{% if paginate.next %}` trước khi render `<a>` — vì hai thuộc tính này có thể là `nil` (trang đầu không có `previous`, trang cuối không có `next`). Nhờ vậy nút chỉ xuất hiện khi thực sự có trang để điều hướng tới.

**Bài 3:**
```liquid
{% paginate collection.products by 12 %}
  {% assign start_item = paginate.current_offset | plus: 1 %}
  {% assign end_item = paginate.current_offset | plus: collection.products.size %}

  <p>Hiển thị sản phẩm {{ start_item }}–{{ end_item }} trong tổng số {{ collection.products_count }} sản phẩm</p>

  {% for product in collection.products %}
    {% comment %} ... render sản phẩm ... {% endcomment %}
  {% endfor %}
{% endpaginate %}
```
Giải thích: Ở trang 2, `current_offset = (2 - 1) * 12 = 12` → `start_item = 12 + 1 = 13`. `collection.products.size` là số sản phẩm thực tế đang hiển thị trên trang hiện tại (thường bằng `page_size`, nhưng ở trang cuối có thể ít hơn) → `end_item = 12 + 12 = 24`. Kết quả in ra đúng: "Hiển thị sản phẩm 13–24 trong tổng số 30 sản phẩm".
