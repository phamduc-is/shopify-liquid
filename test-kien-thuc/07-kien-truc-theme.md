# 📝 Test kiến thức — Nhóm 7: Kiến trúc Theme (Architecture)

> Làm hết phần Câu hỏi trước, KHÔNG kéo xuống xem Đáp án sớm.

---

## 📋 Câu hỏi

### Trắc nghiệm — Cấu trúc thư mục

**Câu 1.** Thư mục `assets/` trong theme Shopify OS 2.0 dùng để chứa gì?
- A. Các file CSS, JS, hình ảnh của theme.
- B. Cấu hình Admin như `settings_schema.json`.
- C. Các file đa ngôn ngữ `.json`.
- D. Các trang giao diện kéo-thả `.json`.

**Câu 2.** Thư mục `config/` chịu trách nhiệm cho việc gì?
- A. Chứa mã tái sử dụng gọi bằng `{% render %}`.
- B. Chứa cấu hình Admin: `settings_schema.json` (định nghĩa các field cài đặt) và `settings_data.json` (giá trị merchant đã lưu).
- C. Chứa khối giao diện kéo-thả trong Theme Editor.
- D. Chứa vỏ khung chính `theme.liquid`.

**Câu 3.** Thư mục `layout/` có vai trò gì?
- A. Chứa các trang giao diện theo route như `index.json`, `product.json`.
- B. Chứa file "vỏ khung" chính bao toàn bộ trang, ví dụ `theme.liquid`, `password.liquid`.
- C. Chứa file dịch ngôn ngữ `en.default.json`.
- D. Chứa CSS/JS của theme.

**Câu 4.** Thư mục `locales/` dùng để làm gì?
- A. Chứa CSS biến màu và font.
- B. Chứa các file đa ngôn ngữ, ví dụ `en.default.json`, phục vụ đa dạng hoá ngôn ngữ storefront.
- C. Chứa cấu hình Admin.
- D. Chứa các section kéo-thả.

**Câu 5.** Thư mục `sections/` dùng để chứa gì?
- A. Snippet tái sử dụng nhỏ gọi bằng `{% render %}`.
- B. Các khối giao diện lớn, kéo-thả được trong Theme Editor (VD `header.liquid`, `collection.liquid`).
- C. Các file đa ngôn ngữ.
- D. File vỏ khung chính của trang.

**Câu 6.** Thư mục `snippets/` khác `sections/` ở điểm nào?
- A. `snippets/` chứa mã component nhỏ, tái sử dụng qua `{% render %}`, KHÔNG tự kéo-thả được trong Theme Editor như section.
- B. `snippets/` chỉ chứa file JSON, không chứa `.liquid`.
- C. `snippets/` là nơi merchant thêm/bớt trực tiếp trong Theme Editor.
- D. `snippets/` chứa cấu hình `settings_schema.json`.

**Câu 7.** Thư mục `templates/` có vai trò gì?
- A. Chứa file quyết định nội dung hiển thị theo từng route/URL, ví dụ `index.json`, `product.json`, `collection.json`.
- B. Chứa toàn bộ CSS và JS của theme.
- C. Chứa file dịch ngôn ngữ.
- D. Chứa vỏ khung `theme.liquid` dùng chung cho mọi trang.

---

### Trắc nghiệm — Giới hạn (Limits)

**Câu 8.** Số lượng section tối đa được phép trong 1 template là bao nhiêu?
- A. 15
- B. 25
- C. 50
- D. 100

**Câu 9.** Số lượng block tối đa trong 1 section là bao nhiêu?
- A. 8
- B. 25
- C. 50
- D. 100

**Câu 10.** Độ sâu lồng block (nested blocks depth) tối đa là bao nhiêu tầng?
- A. 4
- B. 8
- C. 25
- D. 50

**Câu 11.** Vòng lặp `{% for %}` cho phép lặp tối đa bao nhiêu phần tử trước khi Liquid tự cắt ngang, và giải pháp bắt buộc khi cần hiển thị nhiều hơn số đó là gì?
- A. Tối đa 100 phần tử; giải pháp là dùng `limit`.
- B. Tối đa 50 phần tử; giải pháp bắt buộc là dùng `{% paginate %}`.
- C. Tối đa 25 phần tử; giải pháp là dùng `{% break %}`.
- D. Không giới hạn, Liquid tự động xử lý.

**Câu 12.** Kích thước file tối đa cho phép của 1 section hoặc snippet là bao nhiêu?
- A. 50KB
- B. 100KB
- C. 250KB
- D. 1MB

**Câu 13.** Tổng số file tối đa được phép trong toàn bộ 1 theme là bao nhiêu?
- A. 500
- B. 1,000
- C. 2,000
- D. 5,000

---

### Trắc nghiệm — Layout & Template

**Câu 14.** Cơ chế nào cho phép 1 template dùng `layout/password.liquid` thay vì `layout/theme.liquid` mặc định?
- A. Đặt tên file template là `password.liquid`.
- B. Khai báo field `"layout": "password"` ngay trong file JSON của template đó.
- C. Shopify tự động nhận diện theo `request.page_type`.
- D. Sửa trực tiếp nội dung `layout/theme.liquid`.

**Câu 15.** Điểm khác biệt cốt lõi giữa template `.json` (OS 2.0) và template `.liquid` (Vintage) là gì?
- A. `.json` chỉ dùng cho trang chủ, `.liquid` dùng cho mọi trang khác.
- B. `.json` cho phép merchant kéo-thả section trong Theme Editor mà không cần code; `.liquid` là code viết cứng, merchant không tuỳ chỉnh được.
- C. `.liquid` mới là chuẩn hiện tại, `.json` đã bị deprecated.
- D. Không có khác biệt, chỉ khác đuôi file.

**Câu 16.** Nếu file `layout/theme.liquid` thiếu thẻ `content_for_header` hoặc `content_for_layout`, điều gì xảy ra?
- A. Thiếu `content_for_header` trong `<head>` làm hỏng nhiều app/script tracking (Analytics, Pixel, checkout meta...); thiếu `content_for_layout` khiến nội dung template không được bơm vào, trang luôn trống rỗng.
- B. Không ảnh hưởng gì, cả hai đều là thẻ tuỳ chọn.
- C. Chỉ ảnh hưởng tới SEO, không ảnh hưởng nội dung trang.
- D. Theme sẽ không build được, Shopify CLI báo lỗi cú pháp ngay lập tức.

---

### Bài tập

**Bài 1.** Viết cấu trúc tối thiểu (khung sườn, không cần đầy đủ CSS/JS thật) của file `layout/theme.liquid` chuẩn OS 2.0, đảm bảo có đủ các phần bắt buộc: `<head>` với `content_for_header`, `<body>` với `content_for_layout`, và ít nhất 1 vị trí gọi section (header/footer).

**Bài 2.** Một lập trình viên viết section `collection.liquid` như sau để hiển thị toàn bộ sản phẩm trong collection:

```liquid
{% for product in collection.products %}
  <div class="product-card">{{ product.title }}</div>
{% endfor %}
```

Collection này có 120 sản phẩm nhưng khi deploy, merchant phản ánh chỉ thấy đúng 50 sản phẩm đầu tiên hiển thị, không có sản phẩm nào từ vị trí 51 trở đi, dù không có lỗi nào hiện ra. Hãy giải thích nguyên nhân (dựa trên giới hạn nào) và sửa lại đoạn code trên cho đúng.

**Bài 3.** Cho biết: bạn cần tạo 1 template riêng cho trang "Giới thiệu" (About) trong mục Pages của Admin, sử dụng chuẩn OS 2.0 (không dùng `.liquid` kiểu Vintage). Hãy nêu tên file cần tạo (đúng quy ước đặt tên) và giải thích ngắn gọn việc này sẽ hiện ra như thế nào trong Admin khi merchant chọn template cho 1 trang Page.

---

## 🔑 Đáp án & Giải thích

### Đáp án trắc nghiệm

**Câu 1:** A — `assets/` chứa CSS, JS, hình ảnh của theme.

**Câu 2:** B — `config/` chứa cấu hình Admin: `settings_schema.json` (định nghĩa field) và `settings_data.json` (giá trị đã lưu).

**Câu 3:** B — `layout/` là vỏ khung chính bao toàn bộ trang, mặc định là `theme.liquid`.

**Câu 4:** B — `locales/` chứa file đa ngôn ngữ, ví dụ `en.default.json`.

**Câu 5:** B — `sections/` chứa khối giao diện kéo-thả được trong Theme Editor.

**Câu 6:** A — `snippets/` là mã tái sử dụng nhỏ gọi bằng `{% render %}`, không tự xuất hiện trong danh sách "Add section" của Theme Editor như section.

**Câu 7:** A — `templates/` quyết định nội dung hiển thị theo route, ví dụ `index.json`, `product.json`, `collection.json`.

**Câu 8:** B — 25 sections tối đa trên 1 template. Ý nghĩa: không nên nhồi nhét quá nhiều section vào 1 trang, vừa ảnh hưởng hiệu năng vừa khó quản lý trong Theme Editor.

**Câu 9:** C — 50 blocks tối đa trong 1 section.

**Câu 10:** B — 8 tầng lồng block tối đa (áp dụng cho Theme block `@theme`, ví dụ Group chứa Text lồng nhiều lớp).

**Câu 11:** B — Tối đa 50 phần tử mỗi vòng lặp `for`; vượt quá bắt buộc phải dùng `{% paginate %}` vì Liquid sẽ tự cắt ngang, không chỉ là vấn đề UX mà là giới hạn kỹ thuật cứng.

**Câu 12:** B — 100KB tối đa cho 1 file section hoặc snippet.

**Câu 13:** B — 1,000 file tối đa cho toàn bộ theme (tính tất cả các thư mục cộng lại).

**Câu 14:** B — Khai báo `"layout": "password"` trong file JSON của template, Shopify sẽ dùng `layout/password.liquid` thay vì `layout/theme.liquid` mặc định.

**Câu 15:** B — `.json` chuẩn OS 2.0 cho phép merchant kéo-thả section trong Theme Editor không cần code; `.liquid` (Vintage) là code viết cứng, merchant không customize được. Lưu ý thêm: nếu 1 route đã có template `.json` cùng tên, file `.liquid` cùng tên sẽ không được Shopify route tới.

**Câu 16:** A — Thiếu `content_for_header` trong `<head>` làm hỏng app/script tracking (Analytics, Pixel, meta checkout...); thiếu `content_for_layout` khiến trang luôn trống rỗng vì nội dung template (section) không có chỗ để "bơm" vào.

### Đáp án bài tập

**Bài 1:**

```liquid
<!doctype html>
<html lang="{{ request.locale.iso_code }}">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width,initial-scale=1">
    <title>{{ page_title }}</title>
    {{ content_for_header }}
    {{ 'theme.css' | asset_url | stylesheet_tag }}
  </head>
  <body>
    {% section 'header' %}

    <main>
      {{ content_for_layout }}
    </main>

    {% section 'footer' %}
  </body>
</html>
```

Bắt buộc phải có: `{{ content_for_header }}` nằm trong `<head>` (không được đặt sai vị trí), và `{{ content_for_layout }}` nằm trong `<body>` — đây là 2 "điểm neo" hệ thống mà Shopify tự bơm nội dung vào, thiếu 1 trong 2 sẽ hỏng theo đúng như Câu 16.

**Bài 2:**

Nguyên nhân: giới hạn **`for` loop iterations tối đa = 50** — Liquid tự động cắt ngang vòng lặp khi vượt quá 50 phần tử, nên chỉ 50 sản phẩm đầu được render, không có lỗi hiển thị ra vì đây không phải là exception mà là giới hạn "âm thầm" theo thiết kế của Liquid.

Sửa lại bằng cách bọc trong `{% paginate %}` (mặc định chia 50 sản phẩm/trang, có thể chỉnh số nhỏ hơn):

```liquid
{% paginate collection.products by 24 %}
  {% for product in collection.products %}
    <div class="product-card">{{ product.title }}</div>
  {% endfor %}

  {{ paginate | default_pagination }}
{% endpaginate %}
```

`paginate` sẽ tự động chia collection thành nhiều trang, mỗi trang chỉ chứa tối đa số sản phẩm được chỉ định (`by 24`), và sinh sẵn thanh điều hướng trang qua `default_pagination` — giải quyết triệt để việc bị cắt ngang do vượt limit 50.

**Bài 3:**

Tên file cần tạo: `templates/page.about.json` (quy ước: `page.<tên-tuỳ-chọn>.json`).

Khi tạo file này, trong Admin → Online Store → Pages, khi merchant chỉnh sửa 1 trang Page bất kỳ, ở mục "Theme template" sẽ xuất hiện thêm lựa chọn có tên "about" (lấy từ phần sau `page.` trong tên file) bên cạnh template `page` mặc định. Merchant chỉ cần chọn "about" từ dropdown đó để áp dụng bố cục riêng cho trang này, hoàn toàn không cần dev can thiệp thêm — đúng tinh thần chuẩn OS 2.0: cấu hình qua Admin/JSON thay vì code cứng.
