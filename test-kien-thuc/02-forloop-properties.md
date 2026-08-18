# 📝 Test kiến thức — Nhóm 2: Đối tượng `forloop`

> Làm hết phần Câu hỏi trước, KHÔNG kéo xuống xem Đáp án sớm.

---

## 📋 Câu hỏi

### Trắc nghiệm

**Câu 1.** Cho đoạn code sau:
```liquid
{% assign colors = "Đỏ, Xanh, Vàng, Tím" | split: ", " %}
{% for color in colors %}
  {% if forloop.first %}Bắt đầu duyệt màu: {% endif %}{{ color }}{% unless forloop.last %}, {% endunless %}
{% endfor %}
```
`forloop.first` trả về giá trị gì và ở lượt lặp nào?
- A. Kiểu String, trả về `"true"` ở lượt lặp cuối cùng
- B. Kiểu Boolean, trả về `true` chỉ ở lượt lặp **đầu tiên** (color = "Đỏ"), các lượt khác là `false`
- C. Kiểu Boolean, trả về `true` ở **mọi** lượt lặp
- D. Kiểu Integer, trả về `1` ở lượt lặp đầu tiên

**Câu 2.** Với cùng mảng `colors = ["Đỏ", "Xanh", "Vàng", "Tím"]`, tại lượt lặp nào thì `forloop.last` trả về `true`?
- A. Lượt lặp có `color = "Đỏ"`
- B. Lượt lặp có `color = "Xanh"`
- C. Lượt lặp có `color = "Vàng"`
- D. Lượt lặp có `color = "Tím"`

**Câu 3.** Cho đoạn code:
```liquid
{% assign colors = "Đỏ, Xanh, Vàng, Tím" | split: ", " %}
{% for color in colors %}
  <p>{{ forloop.index }}: {{ color }}</p>
{% endfor %}
```
Output HTML của dòng đầu tiên là gì?
- A. `<p>0: Đỏ</p>`
- B. `<p>1: Đỏ</p>`
- C. `<p>4: Đỏ</p>`
- D. `<p>Đỏ: 1</p>`

**Câu 4.** Vẫn với đoạn code ở Câu 3, nếu đổi `forloop.index` thành `forloop.index0`, dòng ứng với `color = "Vàng"` (phần tử thứ 3 trong mảng) sẽ in ra số nào?
- A. `0`
- B. `1`
- C. `2`
- D. `3`

**Câu 5.** Đây là câu hỏi hay nhầm nhất: `forloop.index` và `forloop.index0` khác nhau ở điểm nào?
- A. `index` đếm từ 0, `index0` đếm từ 1
- B. `index` đếm từ 1, `index0` đếm từ 0 — cả hai đều tăng dần theo chiều thuận của vòng lặp
- C. `index` chỉ dùng được trong vòng lặp `for`, còn `index0` chỉ dùng được trong `tablerow`
- D. Không có sự khác biệt, hai thuộc tính là alias của nhau

**Câu 6.** Cho đoạn code:
```liquid
{% assign colors = "Đỏ, Xanh, Vàng, Tím" | split: ", " %}
{% for color in colors %}
  {% if forloop.index == 1 %}Tổng cộng có {{ forloop.length }} màu.{% endif %}
{% endfor %}
```
`forloop.length` trong trường hợp này trả về giá trị nào và ý nghĩa của nó là gì?
- A. `3` — vì mảng có 3 màu được duyệt hết
- B. `4` — tổng số phần tử mà vòng lặp `for` đang duyệt qua
- C. `1` — vì đây là lượt lặp thứ nhất
- D. `0` — vì `forloop.length` chỉ có giá trị khi dùng `limit`

**Câu 7.** Với mảng `colors = ["Đỏ", "Xanh", "Vàng", "Tím"]` (4 phần tử), tại lượt lặp `color = "Xanh"` (phần tử thứ 2), giá trị của `forloop.rindex` là bao nhiêu?
- A. `1`
- B. `2`
- C. `3`
- D. `4`

**Câu 8.** Cũng tại lượt lặp `color = "Xanh"` như Câu 7, giá trị của `forloop.rindex0` là bao nhiêu?
- A. `1`
- B. `2`
- C. `3`
- D. `4`

**Câu 9.** Phát biểu nào sau đây mô tả đúng và đầy đủ nhất mối quan hệ giữa `rindex` và `rindex0`?
- A. `rindex` đếm ngược về 1 (lượt cuối cùng `rindex = 1`), `rindex0` đếm ngược về 0 (lượt cuối cùng `rindex0 = 0`); cả hai đều **giảm dần** khi vòng lặp tiến tới lượt cuối
- B. `rindex` và `rindex0` đều tăng dần giống `index` và `index0`, chỉ khác điểm bắt đầu
- C. `rindex` là số lượt đã lặp qua, `rindex0` là số lượt chưa lặp tới, không liên quan đến chiều đếm ngược
- D. `rindex0` luôn bằng `forloop.length`, không phụ thuộc vào lượt lặp hiện tại

**Câu 10.** Cho đoạn code vòng lặp lồng nhau (nested loop):
```liquid
{% assign categories = "Áo, Quần" | split: ", " %}
{% assign sizes = "S, M, L" | split: ", " %}
{% for category in categories %}
  {% for size in sizes %}
    {% if forloop.first %}
      <p>Vòng lặp cha đang ở lượt số {{ forloop.parentloop.index }}: {{ category }}</p>
    {% endif %}
    <span>{{ category }} - Size {{ size }}</span>
  {% endfor %}
{% endfor %}
```
`forloop.parentloop.index` bên trong vòng lặp `size` dùng để làm gì, và khi `category = "Quần"` thì nó trả về giá trị gì?
- A. Nó truy cập đối tượng `forloop` của chính vòng lặp `size`, giá trị luôn là `1`
- B. Nó không hợp lệ vì Liquid không hỗ trợ nested loop
- C. Nó truy cập đối tượng `forloop` của vòng lặp cha (`category`) từ bên trong vòng lặp con (`size`); vì "Quần" là phần tử thứ 2 trong `categories` nên giá trị trả về là `2`
- D. Nó truy cập `forloop.length` của vòng lặp cha, giá trị là `2`

---

### Bài tập viết code

**Bài 1.** Cho mảng `{% assign fruits = "Táo, Cam, Xoài" | split: ", " %}`. Viết vòng lặp `for` sử dụng `forloop.first` và `forloop.last` để in ra danh sách HTML sau (chỉ mở thẻ `<ul>` ở lượt đầu tiên và chỉ đóng thẻ `</ul>` ở lượt cuối cùng):
```html
<ul>
  <li>Táo</li>
  <li>Cam</li>
  <li>Xoài</li>
</ul>
```

**Bài 2.** Cho mảng `{% assign products = "Áo thun, Quần jeans, Mũ lưỡi trai" | split: ", " %}`. Viết vòng lặp `for` dùng `forloop.index` để mỗi thẻ `<div>` sản phẩm có class riêng biệt dạng `item-1`, `item-2`, `item-3`... Output mong muốn:
```html
<div class="item-1">Áo thun</div>
<div class="item-2">Quần jeans</div>
<div class="item-3">Mũ lưỡi trai</div>
```

**Bài 3.** Cho hai mảng:
```liquid
{% assign categories = "Đồ điện tử, Thời trang" | split: ", " %}
{% assign items = "Sản phẩm A, Sản phẩm B" | split: ", " %}
```
Viết vòng lặp lồng nhau (`category` bên ngoài, `item` bên trong). Ở mỗi sản phẩm trong vòng lặp con, in ra số thứ tự của danh mục cha (dùng `forloop.parentloop.index`) và số thứ tự sản phẩm trong danh mục đó (dùng `forloop.index`). Output mong muốn:
```html
Danh mục 1 - Sản phẩm 1: Sản phẩm A
Danh mục 1 - Sản phẩm 2: Sản phẩm B
Danh mục 2 - Sản phẩm 1: Sản phẩm A
Danh mục 2 - Sản phẩm 2: Sản phẩm B
```

---

## 🔑 Đáp án & Giải thích

### Đáp án trắc nghiệm

**Câu 1:** B — `forloop.first` là kiểu Boolean, chỉ trả về `true` ở lượt lặp **đầu tiên** (khi `color = "Đỏ"`), tất cả các lượt còn lại đều là `false`. Ứng dụng phổ biến: mở thẻ container như `<ul>`.

**Câu 2:** D — `forloop.last` chỉ trả về `true` ở lượt lặp **cuối cùng** của vòng lặp, tức khi `color = "Tím"` (phần tử cuối trong mảng gồm 4 phần tử). Ứng dụng phổ biến: đóng thẻ container `</ul>`.

**Câu 3:** B — `forloop.index` đếm từ **1**, nên ở lượt lặp đầu tiên (`color = "Đỏ"`) giá trị là `1`, cho ra `<p>1: Đỏ</p>`.

**Câu 4:** C — `forloop.index0` đếm từ **0**. Mảng `["Đỏ", "Xanh", "Vàng", "Tím"]` có "Vàng" là phần tử thứ 3 (vị trí `index = 3`), nên `index0` tương ứng là `2` (đúng bằng chỉ số mảng theo kiểu lập trình 0-based).

**Câu 5:** B — Khác biệt duy nhất giữa `index` và `index0` là điểm xuất phát: `index` bắt đầu từ `1`, `index0` bắt đầu từ `0`. Cả hai đều **tăng dần** theo cùng một chiều (chiều thuận của vòng lặp), không có sự khác biệt nào về hướng đếm.

**Câu 6:** B — `forloop.length` trả về **tổng số phần tử** mà vòng lặp đang duyệt qua, không thay đổi theo từng lượt lặp. Vì mảng `colors` có 4 phần tử ("Đỏ, Xanh, Vàng, Tím") nên `forloop.length = 4` ở mọi lượt lặp.

**Câu 7:** C — `forloop.rindex` đếm ngược về **1** (tức là số lượt lặp *còn lại tính cả lượt hiện tại*, công thức `rindex = length - index + 1`). Với mảng 4 phần tử, tại phần tử thứ 2 ("Xanh", `index = 2`): `rindex = 4 - 2 + 1 = 3` (còn "Xanh, Vàng, Tím" = 3 lượt tính cả lượt hiện tại).

**Câu 8:** B — `forloop.rindex0` đếm ngược về **0** (công thức `rindex0 = length - index`). Tại phần tử thứ 2 ("Xanh", `index = 2`): `rindex0 = 4 - 2 = 2` (còn "Vàng, Tím" = 2 lượt sau lượt hiện tại, không tính lượt hiện tại).

**Câu 9:** A — `rindex` đếm ngược về **1**: ở lượt lặp cuối cùng, `rindex = 1`. `rindex0` đếm ngược về **0**: ở lượt lặp cuối cùng, `rindex0 = 0`. Cả hai đều **giảm dần** khi vòng lặp tiến gần tới lượt cuối, ngược chiều với `index`/`index0`.

**Câu 10:** C — `forloop.parentloop` cho phép truy cập vào đối tượng `forloop` của vòng lặp **cha** khi có vòng lặp lồng nhau. Trong vòng lặp con `size`, viết `forloop.parentloop.index` sẽ lấy `index` của vòng lặp cha `category`. Vì `"Quần"` là phần tử thứ 2 trong mảng `categories`, nên `forloop.parentloop.index = 2`.

### Đáp án bài tập code

**Bài 1:**
```liquid
{% assign fruits = "Táo, Cam, Xoài" | split: ", " %}
{% for fruit in fruits %}
  {% if forloop.first %}<ul>{% endif %}
  <li>{{ fruit }}</li>
  {% if forloop.last %}</ul>{% endif %}
{% endfor %}
```

**Bài 2:**
```liquid
{% assign products = "Áo thun, Quần jeans, Mũ lưỡi trai" | split: ", " %}
{% for product in products %}
  <div class="item-{{ forloop.index }}">{{ product }}</div>
{% endfor %}
```

**Bài 3:**
```liquid
{% assign categories = "Đồ điện tử, Thời trang" | split: ", " %}
{% assign items = "Sản phẩm A, Sản phẩm B" | split: ", " %}

{% for category in categories %}
  {% for item in items %}
    Danh mục {{ forloop.parentloop.index }} - Sản phẩm {{ forloop.index }}: {{ item }}
  {% endfor %}
{% endfor %}
```
👉 Giải thích: `forloop.parentloop.index` lấy số thứ tự của vòng lặp `category` (vòng lặp cha) ngay cả khi đang ở bên trong vòng lặp `item` (vòng lặp con), còn `forloop.index` bên trong thân vòng lặp con vẫn chỉ tham chiếu tới lượt lặp của chính vòng lặp `item`.
