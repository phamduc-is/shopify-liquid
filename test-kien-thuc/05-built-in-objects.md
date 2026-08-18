# 📝 Test kiến thức — Nhóm 5: Built-in Objects (link, product, cart, shop, request)

> Làm hết phần Câu hỏi trước, KHÔNG kéo xuống xem Đáp án sớm.

---

## 📋 Câu hỏi

### Trắc nghiệm — Đối tượng `link`

**Câu 1.** `link.title` dùng để làm gì trong menu navigation?
- A. Trả về URL đích của menu item
- B. Trả về tên hiển thị (label) của menu item do merchant tự đặt ở Admin
- C. Trả về `true`/`false` nếu menu đang active
- D. Trả về số lượng menu con

**Câu 2.** Trong menu Admin, merchant tạo 1 link trỏ tới collection "Áo Nam". Giá trị `link.url` sẽ là gì?
- A. Chuỗi merchant gõ tay giống HTML tĩnh
- B. Luôn là `/`
- C. Tự động resolve ra đường dẫn thật của resource, ví dụ `/collections/ao-nam`
- D. `null` vì Liquid không tự resolve URL

**Câu 3.** `link.links` trả về gì?
- A. Số cấp menu con sâu nhất
- B. Boolean cho biết item có menu con hay không
- C. Mảng (array) chứa các menu item con cấp 2 (child links) của item hiện tại
- D. Chuỗi title của menu cha

**Câu 4.** `link.active` trả về `true` khi nào?
- A. Khi menu item này có ít nhất 1 menu con
- B. Khi URL của chính link này trùng khớp với trang hiện tại đang xem
- C. Khi một trong các menu con của nó đang được active
- D. Khi merchant bật link này lên trong Admin

**Câu 5.** `link.child_active` trả về `true` khi nào?
- A. Khi chính link cha đang được active
- B. Khi trang hiện tại đang mở nằm trong một menu con (child link) của item này, dù bản thân link cha không phải URL đang xem
- C. Khi link không có menu con nào
- D. Khi menu con bị ẩn

**Câu 6.** `link.levels` cho biết điều gì?
- A. Số lượng link ở cấp cao nhất của toàn menu
- B. Độ sâu (số cấp) menu con còn lại bên dưới link hiện tại — `0` nghĩa là không có menu con
- C. Vị trí thứ tự của link trong danh sách
- D. Số ký tự của `link.title`

**Câu 7.** `link.type` có thể nhận giá trị nào trong các giá trị sau?
- A. `'string_link'`
- B. `'collection_link'`, `'product_link'`, `'frontpage_link'`
- C. `'active_link'`, `'inactive_link'`
- D. `'menu_link'` duy nhất, không phân loại chi tiết hơn

**Câu 8 (Phân biệt `active` vs `child_active`).** Menu có cấu trúc: "Danh mục" (cha) → "Áo Nam" (con). Khách đang xem trang `/collections/ao-nam` (tức đang ở đúng link con "Áo Nam", KHÔNG phải trang danh mục cha). Xét 2 phát biểu:
(1) Với link "Danh mục" (cha): `link.active` = ?, `link.child_active` = ?
(2) Với link "Áo Nam" (con): `link.active` = ?, `link.child_active` = ?
- A. (1) active=true, child_active=false | (2) active=false, child_active=true
- B. (1) active=false, child_active=true | (2) active=true, child_active=false
- C. (1) active=true, child_active=true | (2) active=true, child_active=true
- D. (1) active=false, child_active=false | (2) active=false, child_active=true

---

### Trắc nghiệm — Đối tượng `product`

**Câu 9.** `product.title` trả về gì?
- A. Handle (slug URL) của sản phẩm
- B. Tên đầy đủ của sản phẩm
- C. Mô tả ngắn của sản phẩm
- D. Loại sản phẩm (product type)

**Câu 10.** `product.handle` khác với `product.title` ở điểm nào?
- A. Không khác gì, chỉ là 2 tên gọi của cùng 1 giá trị
- B. `handle` là chuỗi slug dùng trong URL (chữ thường, nối bằng dấu gạch ngang), còn `title` là tên hiển thị đầy đủ có thể có dấu, hoa thường tự do
- C. `handle` chỉ tồn tại nếu sản phẩm hết hàng
- D. `handle` là số ID sản phẩm

**Câu 11.** `product.description` trả về nội dung gì?
- A. Chuỗi HTML mô tả chi tiết sản phẩm (rich text từ Admin)
- B. Giá sản phẩm dạng chuỗi
- C. Danh sách tag của sản phẩm
- D. Tên nhà cung cấp

**Câu 12.** Giả sử sản phẩm có giá niêm yết 250.000đ. Giá trị thô của `product.price` khi output trực tiếp (không qua filter) sẽ như thế nào?
- A. `"250.000₫"` — đã format sẵn tiền tệ
- B. `250000` — số nguyên đơn vị cents/xu nhỏ nhất, PHẢI qua filter `money` mới ra định dạng tiền tệ đúng
- C. `250.000` — số thực có phần thập phân
- D. `"250000 VND"` — chuỗi kèm mã tiền tệ

**Câu 13.** `product.compare_at_price` có giá trị `nil` trong trường hợp nào?
- A. Khi sản phẩm đang giảm giá
- B. Khi sản phẩm chưa từng thiết lập giá so sánh/giá gốc (không có "Compare at price" ở Admin)
- C. Khi sản phẩm hết hàng
- D. `compare_at_price` không bao giờ là `nil`, luôn có giá trị mặc định `0`

**Câu 14.** `product.price_varies` trả về `true` khi nào?
- A. Khi sản phẩm có nhiều ảnh khác nhau
- B. Khi các variant của sản phẩm có mức giá KHÔNG giống nhau (VD: size S giá khác size L)
- C. Khi sản phẩm đang được giảm giá theo thời gian
- D. Khi `compare_at_price` khác `price`

**Câu 15.** `product.available` trả về `false` khi nào?
- A. Khi sản phẩm không có `compare_at_price`
- B. Khi tất cả variant của sản phẩm đều hết hàng (không thể mua/đặt hàng thêm)
- C. Khi sản phẩm mới được tạo, chưa publish
- D. Khi `price_varies` là `true`

**Câu 16.** `product.featured_image` trả về gì?
- A. Mảng toàn bộ ảnh của sản phẩm
- B. Đối tượng Image đại diện chính (ảnh đầu tiên / ảnh nổi bật) của sản phẩm
- C. Chuỗi alt text của ảnh
- D. URL dạng string thuần, không phải object

**Câu 17.** `product.images` khác `product.featured_image` như thế nào?
- A. Không khác gì
- B. `images` là mảng (array) chứa TẤT CẢ ảnh, còn `featured_image` chỉ là MỘT object ảnh đại diện
- C. `images` chỉ có khi sản phẩm hết hàng
- D. `featured_image` là mảng, `images` là 1 object

**Câu 18.** `product.variants` trả về gì?
- A. Danh sách tag của sản phẩm
- B. Mảng các biến thể sản phẩm (ví dụ tổ hợp màu sắc/kích thước), mỗi phần tử có giá, SKU, tồn kho riêng
- C. Số lượng ảnh sản phẩm
- D. Boolean cho biết sản phẩm có biến thể hay không

**Câu 19.** `product.vendor` trả về gì?
- A. Loại sản phẩm (category)
- B. Tên nhà sản xuất / thương hiệu của sản phẩm
- C. Tên cửa hàng đang bán
- D. Danh sách tag

**Câu 20.** `product.type` khác `product.vendor` ở điểm nào?
- A. Không khác, cùng 1 dữ liệu
- B. `type` là phân loại/category sản phẩm (VD: "Áo Nam"), còn `vendor` là tên thương hiệu/nhà sản xuất (VD: "Nike")
- C. `type` luôn là số, `vendor` luôn là chuỗi
- D. `type` chỉ dùng cho biến thể

**Câu 21.** `product.tags` trả về gì và thường dùng để làm gì?
- A. Một chuỗi string duy nhất nối bằng dấu phẩy, không lặp được
- B. Mảng (array) các nhãn tag gắn cho sản phẩm, thường dùng để lọc/phân loại hoặc gắn badge (VD: "Mới", "Sale")
- C. Danh sách các biến thể sản phẩm
- D. Đối tượng Image

**Câu 22.** `product.url` trả về gì?
- A. URL đầy đủ có domain, ví dụ `https://shop.com/products/ao-thun`
- B. Đường dẫn tương đối tới trang sản phẩm, ví dụ `/products/ao-thun`
- C. Trùng với `product.handle`
- D. Chỉ tồn tại khi sản phẩm có `compare_at_price`

---

### Trắc nghiệm — Đối tượng `cart`

**Câu 23.** `cart.item_count` trả về gì?
- A. Số lượng dòng sản phẩm khác nhau (line items) trong giỏ
- B. Tổng số lượng (quantity) của tất cả sản phẩm hiện có trong giỏ hàng
- C. Tổng tiền của giỏ hàng
- D. `true`/`false` cho biết giỏ có trống hay không

**Câu 24.** `cart.total_price` có đơn vị và kiểu dữ liệu như thế nào?
- A. Chuỗi đã format sẵn có ký hiệu tiền tệ
- B. Số nguyên (integer), đơn vị cents/xu nhỏ nhất — cần filter `money` để hiển thị đúng định dạng tiền tệ
- C. Số thực (float) đơn vị là đồng/tệ chính
- D. Mảng các giá của từng sản phẩm

**Câu 25.** `cart.items` trả về gì?
- A. Số lượng sản phẩm trong giỏ (một số nguyên)
- B. Mảng chứa từng line item trong giỏ hàng (mỗi phần tử có product, variant, quantity, price riêng)
- C. Tổng tiền giỏ hàng
- D. Ghi chú khách để lại

**Câu 26.** `cart.note` dùng để lưu trữ thông tin gì?
- A. Tổng số lượng sản phẩm
- B. Ghi chú/lời nhắn của khách hàng gửi kèm đơn hàng (VD: yêu cầu gói quà)
- C. Danh sách mã giảm giá đã áp dụng
- D. Lịch sử các đơn hàng trước đó

---

### Trắc nghiệm — Đối tượng `shop`

**Câu 27.** `shop.name` trả về gì?
- A. Tên miền (domain) của cửa hàng
- B. Tên của cửa hàng (Shop name) đã đặt ở Admin
- C. Tên chủ sở hữu tài khoản Shopify
- D. Tên theme đang sử dụng

**Câu 28.** `shop.email` trả về gì?
- A. Email của khách hàng đang đăng nhập
- B. Email liên hệ chính (sender email) của cửa hàng
- C. Email đăng ký tài khoản Shopify Partner
- D. Danh sách toàn bộ email khách hàng

**Câu 29.** `shop.currency` trả về gì?
- A. Số tiền hiện có trong giỏ hàng
- B. Mã tiền tệ của cửa hàng, ví dụ `"USD"`, `"VND"`
- C. Tỷ giá quy đổi giữa các loại tiền
- D. Ký hiệu tiền tệ như `"$"`, `"₫"`

**Câu 30.** `shop.description` thường được dùng vào việc gì?
- A. Mô tả chi tiết từng sản phẩm
- B. Mô tả ngắn về cửa hàng, thường dùng cho SEO (meta description mặc định)
- C. Ghi chú nội bộ merchant không hiển thị ra ngoài
- D. Danh sách chính sách đổi trả

**Câu 31.** `shop.url` trả về gì?
- A. URL của trang sản phẩm đang xem
- B. Đường dẫn gốc (URL trang chủ) của cửa hàng, ví dụ `https://yourstore.com`
- C. URL trang giỏ hàng
- D. URL trang Admin đăng nhập

---

### Trắc nghiệm — Đối tượng `request`

**Câu 32.** `request.page_type` trả về gì và dùng để làm gì?
- A. Loại trình duyệt khách đang dùng
- B. Chuỗi cho biết loại trang hiện tại (`'index'`, `'product'`, `'collection'`, `'cart'`...), thường dùng để render nội dung khác nhau tùy loại trang
- C. Ngôn ngữ hiện tại của trang
- D. Số lượng section trên trang

**Câu 33.** `request.path` trả về gì?
- A. Toàn bộ URL kèm domain, ví dụ `https://shop.com/products/abc`
- B. Đường dẫn tương đối của trang hiện tại, ví dụ `/products/gift-card`
- C. Loại trang hiện tại (giống `page_type`)
- D. Query string của URL (phần sau dấu `?`)

**Câu 34.** `request.locale.iso_code` trả về gì?
- A. Mã tiền tệ cửa hàng
- B. Mã ngôn ngữ hiện tại của trang, ví dụ `'en'`, `'vi'`
- C. Múi giờ máy chủ
- D. Mã quốc gia của khách hàng theo IP

---

### Bài tập viết code

**Bài 1.** Viết đoạn Liquid hiển thị badge **"Hết hàng"** trên thẻ sản phẩm khi sản phẩm không còn hàng, dùng `product.available`. Nếu còn hàng thì không hiển thị gì cả.
Input: `product.available = false`
Output mong muốn: `<span class="badge badge--sold-out">Hết hàng</span>`

**Bài 2.** Viết đoạn Liquid tính và hiển thị **phần trăm giảm giá** dựa trên `product.price` và `product.compare_at_price`. Chỉ hiển thị khi sản phẩm thực sự đang giảm giá (tức `compare_at_price` tồn tại và lớn hơn `price`).
Input: `product.price = 150000`, `product.compare_at_price = 200000`
Output mong muốn: `<span class="badge badge--sale">-25%</span>`

**Bài 3.** Viết đoạn Liquid render một menu dropdown 2 cấp dùng `link.links` để lấy menu con và `link.active` để tô đậm (class `is-active`) item đang được xem, dựa trên object `linklists['main-menu'].links`.
Input: menu "Danh mục" có 2 menu con "Áo Nam", "Áo Nữ"; khách đang xem trang "Áo Nam".
Output mong muốn (rút gọn):
```html
<li>
  <a href="/collections/danh-muc">Danh mục</a>
  <ul class="dropdown">
    <li><a href="/collections/ao-nam" class="is-active">Áo Nam</a></li>
    <li><a href="/collections/ao-nu">Áo Nữ</a></li>
  </ul>
</li>
```

**Bài 4.** Viết đoạn Liquid hiển thị số lượng sản phẩm trong giỏ hàng bên cạnh icon giỏ hàng, dùng `cart.item_count`. Nếu giỏ hàng trống (`item_count == 0`) thì ẩn số đếm đi (không render badge số).
Input: `cart.item_count = 3`
Output mong muốn: `<span class="cart-count">3</span>`
Input: `cart.item_count = 0`
Output mong muốn: (không render gì, hoặc chuỗi rỗng)

**Bài 5.** Viết đoạn Liquid dùng `request.page_type` để CHỈ hiển thị 1 khối thông báo (banner) khi khách đang ở đúng trang giỏ hàng (`page_type == 'cart'`), không hiển thị ở bất kỳ trang nào khác.
Input: `request.page_type = 'cart'`
Output mong muốn: `<div class="cart-banner">Miễn phí vận chuyển cho đơn từ 500.000đ!</div>`
Input: `request.page_type = 'product'`
Output mong muốn: (không hiển thị gì)

**Bài 6.** Viết đoạn Liquid trong footer hiển thị dòng bản quyền kết hợp `shop.name` và tên miền lấy từ `shop.url`, cùng với ghi chú giỏ hàng của khách nếu có (`cart.note`), chỉ hiển thị ghi chú khi `cart.note` không rỗng.
Input: `shop.name = "Cửa Hàng ABC"`, `shop.url = "https://abc-shop.com"`, `cart.note = "Giao trước 18h"`
Output mong muốn:
```html
<p>© Cửa Hàng ABC — https://abc-shop.com</p>
<p class="cart-note">Ghi chú đơn hàng: Giao trước 18h</p>
```

---

## 🔑 Đáp án & Giải thích

### Đáp án trắc nghiệm

**Câu 1:** B — `link.title` là label hiển thị merchant tự đặt ở Admin (Navigation), không liên quan tên thật của resource.

**Câu 2:** C — Khác HTML tĩnh, `link.url` không phải chuỗi gõ tay mà Shopify tự resolve dựa trên loại resource merchant chọn (`collection_link` → `/collections/handle`).

**Câu 3:** C — `link.links` là mảng chứa các menu item con cấp 2 (child links) của item đang xét; dùng `{% if link.links != blank %}` để biết có phải render dropdown hay không.

**Câu 4:** B — `link.active` chỉ `true` khi URL của CHÍNH link đó trùng với trang đang xem, không quan tâm menu con.

**Câu 5:** B — `link.child_active` là `true` khi có một menu con (bất kỳ cấp nào bên dưới) đang được active, dù bản thân link cha có thể không khớp URL hiện tại — hai thuộc tính này độc lập với nhau.

**Câu 6:** B — `link.levels` là số cấp menu con còn lại bên dưới link: `0` = không có con, `1` = có 1 cấp con (tổng 2 cấp), `2` = có 2 cấp con (tổng 3 cấp).

**Câu 7:** B — Các giá trị hợp lệ gồm `'catalog_link'`, `'collection_link'`, `'product_link'`, `'frontpage_link'`, `'page_link'`, `'http_link'`...

**Câu 8:** B — Với link cha "Danh mục": trang đang xem (`/collections/ao-nam`) KHÔNG phải URL của chính nó nên `active=false`, nhưng vì link con "Áo Nam" của nó đang active nên `child_active=true`. Với link con "Áo Nam": chính nó khớp URL đang xem nên `active=true`, và vì nó không có menu con riêng nên `child_active=false`. *(Lưu ý: đây là ví dụ kinh điển hay gây nhầm lẫn — `active` chỉ xét CHÍNH link đó, `child_active` chỉ xét CON của nó, hai giá trị không tự động giống nhau.)*

**Câu 9:** B — `product.title` là tên đầy đủ của sản phẩm.

**Câu 10:** B — `handle` là slug dùng trong URL (thường hoá, nối gạch ngang, không dấu), `title` là tên hiển thị tự do.

**Câu 11:** A — `product.description` trả về nội dung HTML mô tả chi tiết, lấy từ rich text editor ở Admin.

**Câu 12:** B — Toàn bộ giá tiền trong Liquid (`price`, `compare_at_price`, `cart.total_price`...) là số nguyên đơn vị cents/xu nhỏ nhất; phải qua filter `| money` (hoặc `money_with_currency`...) để ra chuỗi định dạng tiền tệ đúng, không tự động format.

**Câu 13:** B — `compare_at_price` là `nil` khi merchant chưa từng nhập "Compare at price" cho sản phẩm/variant đó — tức sản phẩm không đang ở trạng thái giảm giá.

**Câu 14:** B — `price_varies` = `true` khi các variant có mức giá khác nhau (VD size khác giá); nếu tất cả variant cùng giá thì `false`.

**Câu 15:** B — `product.available` = `false` khi KHÔNG còn variant nào có thể mua thêm (hết hàng và không cho phép đặt trước).

**Câu 16:** B — `featured_image` là MỘT object Image đại diện (thường là ảnh đầu tiên/ảnh chính), không phải mảng.

**Câu 17:** B — `images` là mảng toàn bộ ảnh; `featured_image` chỉ là 1 object đại diện — dùng `images` khi cần lặp qua gallery, dùng `featured_image` khi chỉ cần 1 ảnh đại diện (thẻ sản phẩm).

**Câu 18:** B — `variants` là mảng các biến thể, mỗi variant có `price`, `sku`, `available`, tổ hợp option (màu/size) riêng.

**Câu 19:** B — `vendor` là tên nhà sản xuất/thương hiệu (VD: "Nike", "Adidas").

**Câu 20:** B — `type` là category/phân loại sản phẩm nội bộ shop tự đặt (VD "Áo Nam"), khác với `vendor` là thương hiệu.

**Câu 21:** B — `tags` là mảng nhãn gắn cho sản phẩm, hay dùng để lọc theo tag hoặc check tag cụ thể để gắn badge (VD: `{% if product.tags contains 'Mới' %}`).

**Câu 22:** B — `product.url` là đường dẫn TƯƠNG ĐỐI (không kèm domain), ví dụ `/products/ao-thun`.

**Câu 23:** B — `cart.item_count` là tổng SỐ LƯỢNG (cộng dồn quantity) của mọi sản phẩm trong giỏ, không phải số dòng line item khác nhau.

**Câu 24:** B — `cart.total_price` là số nguyên đơn vị cents/xu, giống mọi giá trị tiền tệ khác trong Liquid, cần filter `money` để hiển thị.

**Câu 25:** B — `cart.items` là mảng các line item, mỗi phần tử có `product`, `variant`, `quantity`, `price`... riêng.

**Câu 26:** B — `cart.note` lưu ghi chú khách hàng gửi kèm khi đặt hàng (VD yêu cầu đặc biệt), có thể set qua form `<textarea name="note">`.

**Câu 27:** B — `shop.name` là tên cửa hàng đặt trong Admin (Settings → General).

**Câu 28:** B — `shop.email` là email liên hệ chính/sender email của shop, không phải email khách hàng.

**Câu 29:** B — `shop.currency` trả về MÃ tiền tệ (VD `"USD"`, `"VND"`), không phải ký hiệu (`$`, `₫`) hay tỷ giá.

**Câu 30:** B — `shop.description` thường dùng làm meta description mặc định cho SEO khi trang không có mô tả riêng.

**Câu 31:** B — `shop.url` là URL gốc/trang chủ của cửa hàng.

**Câu 32:** B — `request.page_type` cho biết loại trang hiện tại, hay dùng trong `{% if request.page_type == 'product' %}` để hiển thị nội dung riêng theo từng loại trang.

**Câu 33:** B — `request.path` là đường dẫn tương đối (không kèm domain) của trang hiện tại.

**Câu 34:** B — `request.locale.iso_code` trả về mã ngôn ngữ hiện tại (VD `'en'`, `'vi'`), dùng để hiển thị nội dung đa ngôn ngữ tùy chỉnh.

---

### Đáp án bài tập code

**Bài 1:**
```liquid
{% unless product.available %}
  <span class="badge badge--sold-out">Hết hàng</span>
{% endunless %}
```

**Bài 2:**
```liquid
{% if product.compare_at_price > product.price %}
  {% assign discount_percent = product.compare_at_price | minus: product.price | times: 100.0 | divided_by: product.compare_at_price | round %}
  <span class="badge badge--sale">-{{ discount_percent }}%</span>
{% endif %}
```
*Giải thích: `product.compare_at_price` có thể là `nil`, khi đó phép so sánh `nil > price` trả về `false` nên khối `{% if %}` tự động không chạy — không cần check `!= blank` riêng, nhưng viết tường minh hơn có thể dùng `{% if product.compare_at_price and product.compare_at_price > product.price %}`.*

**Bài 3:**
```liquid
{% for link in linklists['main-menu'].links %}
  <li>
    <a href="{{ link.url }}">{{ link.title }}</a>
    {% if link.links != blank %}
      <ul class="dropdown">
        {% for child_link in link.links %}
          <li>
            <a href="{{ child_link.url }}" class="{% if child_link.active %}is-active{% endif %}">
              {{ child_link.title }}
            </a>
          </li>
        {% endfor %}
      </ul>
    {% endif %}
  </li>
{% endfor %}
```

**Bài 4:**
```liquid
<a href="{{ routes.cart_url }}" class="cart-icon">
  🛒
  {% if cart.item_count > 0 %}
    <span class="cart-count">{{ cart.item_count }}</span>
  {% endif %}
</a>
```

**Bài 5:**
```liquid
{% if request.page_type == 'cart' %}
  <div class="cart-banner">Miễn phí vận chuyển cho đơn từ 500.000đ!</div>
{% endif %}
```

**Bài 6:**
```liquid
<p>&copy; {{ shop.name }} — {{ shop.url }}</p>
{% if cart.note != blank %}
  <p class="cart-note">Ghi chú đơn hàng: {{ cart.note }}</p>
{% endif %}
```
