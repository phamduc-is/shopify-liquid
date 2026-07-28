# 📁 Cấu trúc Thư mục Dawn Theme

Thư mục `dawn/` chứa đầy đủ cấu trúc tiêu chuẩn của một Shopify Theme:

```text
dawn/
├── assets/         ← Chứa CSS, JavaScript và hình ảnh của theme
├── config/         ← Chứa settings_schema.json và settings_data.json
├── layout/         ← Chứa theme.liquid (Khung layout chính)
├── locales/        ← Chứa các file đa ngôn ngữ (.json)
├── sections/       ← Chứa các thành phần giao diện động (Sections)
├── snippets/       ← Chứa các đoạn code tái sử dụng (Snippets)
└── templates/      ← Chứa các file cấu hình trang (JSON & Liquid templates)
```

---

## 🔍 Giải thích Chi tiết từng Thư mục

### 1. `assets/`
Chứa toàn bộ các tệp tĩnh phục vụ giao diện và tương tác của theme:
- **CSS / Stylesheets**: Các file kiểu dáng như `base.css`, `component-card.css`, `section-main-product.css`...
- **JavaScript**: Các file xử lý logic client-side như `global.js`, `cart.js`, `product-form.js`...
- **Hình ảnh & Icon**: Các file `.png`, `.svg` dùng chung trong theme.

### 2. `config/`
Quản lý cấu hình tùy chỉnh của merchant và schema của Theme Editor:
- **`settings_schema.json`**: Định nghĩa các tùy chọn cấu hình toàn trang (Colors, Typography, Layout, Social Media...) hiển thị trong phần **Theme settings** của Theme Editor.
- **`settings_data.json`**: Lưu trữ giá trị cấu hình hiện tại do merchant thiết lập.

### 3. `layout/`
Chứa khung bao quanh (outer container/wrapper) cho mọi trang web:
- **`theme.liquid`**: File layout chính chứa các thẻ HTML cơ bản (`<html>`, `<head>`, `<body>`), bao gồm cả thẻ `{{ content_for_header }}` và `{{ content_for_layout }}` để Shopify render nội dung động.
- **`password.liquid`**: Layout dành cho trang mật khẩu bảo vệ cửa hàng.

### 4. `locales/`
Quản lý đa ngôn ngữ (Localization/i18n) cho cửa hàng:
- **`en.default.json`**: Ngôn ngữ mặc định (Tiếng Anh) chứa tất cả chuỗi văn bản được tham chiếu qua hàm Liquid t (`{{ 'general.search.search' | t }}`).
- **`vi.json`**, **`fr.json`**...: Bản dịch sang các ngôn ngữ khác.

### 5. `sections/`
Chứa các thành phần giao diện độc lập và có thể kéo thả (Drag-and-drop) trong Theme Editor:
- Mỗi section là một file độc lập chứa cả Liquid code, HTML, CSS, JavaScript và khối `{% schema %}`.
- Ví dụ: `header.liquid`, `footer.liquid`, `featured-collection.liquid`, `image-banner.liquid`, `main-product.liquid`.

### 6. `snippets/`
Chứa các đoạn mã nhỏ tái sử dụng nhiều lần trong các section hoặc layout khác nhau:
- Đón nạp thông qua thẻ `{% render 'snippet-name' %}`.
- Ví dụ: `card-product.liquid`, `icon-search.liquid`, `price.liquid`, `pagination.liquid`.

### 7. `templates/`
Định nghĩa cấu trúc giao diện cho từng loại trang của Shopify (Trang chủ, Trang sản phẩm, Trang bộ sưu tập...):
- Hầu hết các templates hiện đại (Online Store 2.0) là dạng **JSON templates** (như `index.json`, `product.json`, `collection.json`, `cart.json`).
- Các file JSON này định nghĩa thứ tự hiển thị và tham số của các **sections** trên trang tương ứng.
