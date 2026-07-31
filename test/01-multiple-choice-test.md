# 📝 Bài Test 1: Kiểm Tra Trắc Nghiệm Tổng Hợp (Ngày 1 – Ngày 5)

> 🎯 **Hướng dẫn:** Bạn hãy điền đáp án chọn (A, B, C, hoặc D) vào phần **Bảng Trả Lời** ở cuối file này.

---

### ❓ Câu 1: Thứ tự nạp CSS & Lý do kiến trúc (Ngày 2)
Trong `<head>` của `layout/theme.liquid`, tại sao file `theme.css` bắt buộc phải nạp **SAU** `critical.css`?
- **A.** Vì `theme.css` chứa mã JavaScript cần chờ HTML tải xong.
- **B.** Để các thuộc tính CSS tùy chỉnh trong `theme.css` có thể ghi đè (Cascade) thành công lên khung CSS cơ bản của `critical.css`.
- **C.** Vì Shopify CLI không cho phép nạp `theme.css` trước.
- **D.** Để tránh lỗi hiển thị font chữ hệ thống.

---

### ❓ Câu 2: Sự khác biệt giữa Sections và Snippets (Ngày 2 & 6)
Điểm khác biệt cơ bản nhất giữa thư mục `sections/` và `snippets/` trong kiến trúc Shopify OS 2.0 là gì?
- **A.** `sections/` chứa file CSS, còn `snippets/` chứa file JavaScript.
- **B.** `sections/` chứa các khối giao diện có thể kéo thả và cấu hình trong Theme Editor (Admin), còn `snippets/` là các đoạn code partial được nhúng cố định qua thẻ `{% render %}`.
- **C.** `snippets/` chỉ dùng cho trang sản phẩm, còn `sections/` dùng cho trang chủ.
- **D.** Không có sự khác biệt nào.

---

### ❓ Câu 3: Liquid Objects & Định dạng tiền tệ (Ngày 3)
Biến `product.price` trong Liquid trả về giá trị số nguyên `150000`. Giá thực tế hiển thị trên cửa hàng là bao nhiêu và làm sao để hiển thị đúng?
- **A.** Giá là $150,000; dùng `{{ product.price }}`.
- **B.** Giá là $1,500.00 (vì đơn vị lưu là cents/xu); dùng `{{ product.price | money }}`.
- **C.** Giá là $150.00; dùng `{{ product.price | round }}`.
- **D.** Giá là $15.00; dùng `{{ product.price | divided_by: 100 }}`.

---

### ❓ Câu 4: Chuỗi Filter Chaining (Ngày 4)
Cho đoạn code Liquid sau:  
`{{ "<p>Áo Thun <b>Mùa Hè</b> Thoáng Mát</p>" | strip_html | truncate: 15 | upcase }}`  
Kết quả hiển thị ra màn hình sẽ là gì?
- **A.** `<P>ÁO THUN <b>MÙA...</B></P>`
- **B.** `ÁO THUN MÙA HÈ...`
- **C.** `ÁO THUN MÙA HÈ`
- **D.** `ÁO THUN MÙA...`

---

### ❓ Câu 5: Filter mảng `join` vs `split` (Ngày 4)
Nếu bạn có một chuỗi `{% assign tags_str = "Red, Blue, Green" %}`, câu lệnh nào chuyển chuỗi này thành một mảng, sau đó nối các phần tử lại thành chuỗi phân cách bởi dấu gạch ngang `"Red - Blue - Green"`?
- **A.** `{{ tags_str | split: ", " | join: " - " }}`
- **B.** `{{ tags_str | join: " - " | split: ", " }}`
- **C.** `{{ tags_str | concat: " - " }}`
- **D.** `{{ tags_str | append: " - " }}`

---

### ❓ Câu 6: Control Flow `unless` vs `if` (Ngày 5)
Đoạn code nào dưới đây có logic hoạt động **tương đương** với `{% unless product.available %} <p>Hết hàng</p> {% endunless %}`?
- **A.** `{% if product.available == true %} <p>Hết hàng</p> {% endif %}`
- **B.** `{% if product.available == false %} <p>Hết hàng</p> {% endif %}`
- **C.** `{% if product.available %} <p>Hết hàng</p> {% endif %}`
- **D.** `{% case product.available %} {% when true %} <p>Hết hàng</p> {% endcase %}`

---

### ❓ Câu 7: Thuộc tính của đối tượng `forloop` (Ngày 5)
Trong vòng lặp `{% for item in cart.items %}`, thuộc tính nào giúp bạn kiểm tra xem đây có phải là phần tử cuối cùng của giỏ hàng hay không?
- **A.** `forloop.index == 1`
- **B.** `forloop.last == true`
- **C.** `forloop.first == true`
- **D.** `forloop.length == 0`

---

### ❓ Câu 8: Toán tử `contains` (Ngày 5)
Biến `product.tags` chứa mảng `["new-arrival", "summer-collection", "bestseller"]`.  
Điều kiện nào kiểm tra đúng xem sản phẩm có nhãn `"bestseller"` hay không?
- **A.** `{% if product.tags == "bestseller" %}`
- **B.** `{% if product.tags contains "bestseller" %}`
- **C.** `{% if product.tags in "bestseller" %}`
- **D.** `{% if product.tags.has("bestseller") %}`

---

### ❓ Câu 9: Thẻ `assign` vs `capture` (Ngày 5)
Khi nào bạn NÊN sử dụng `capture` thay vì `assign`?
- **A.** Khi cần tạo biến kiểu dữ liệu số nguyên.
- **B.** Khi muốn gom và lưu trữ một khối mã HTML / Liquid phức tạp gồm nhiều dòng vào một biến.
- **C.** Khi muốn thực hiện phép cộng trừ số học.
- **D.** Khi muốn in giá trị trực tiếp ra giao diện.

---

### ❓ Câu 10: Phân trang `paginate` (Ngày 5)
Khi sử dụng thẻ `{% paginate collection.products by 12 %}`, biến nào dưới đây cung cấp liên kết (URL) để chuyển sang trang kế tiếp?
- **A.** `paginate.current_page`
- **B.** `paginate.next.url`
- **C.** `paginate.pages`
- **D.** `paginate.items`

---

## ✏️ BẢNG TRẢ LỜI CỦA BẠN

| Câu Hỏi | Đáp Án Của Bạn (Điền A / B / C / D) |
|---|---|
| Câu 1 |B |
| Câu 2 |B |
| Câu 3 |B |
| Câu 4 |C |
| Câu 5 |A |
| Câu 6 |B |
| Câu 7 |B |
| Câu 8 |B |
| Câu 9 |B |
| Câu 10 B |
