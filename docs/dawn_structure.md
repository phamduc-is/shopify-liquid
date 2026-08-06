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
├── templates/      ← Chứa các file cấu hình trang (JSON & Liquid templates)
└── blocks/         ← Chứa các Theme Block tái sử dụng chéo qua nhiều section
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

### 8. `blocks/`
Chứa **Theme Block** — khác với "block" khai báo bên trong `{% schema %}` của 1 section cụ thể, mỗi file trong `blocks/` là 1 component **độc lập, tái sử dụng chéo qua nhiều section khác nhau** (thậm chí lồng vào nhau):
- Có `{% schema %}` riêng (giống Section) để merchant tự customize trong Theme Editor.
- Chỉ được 1 section/block khác "nhúng" vào nếu section/block đó khai báo `"blocks": [{ "type": "@theme" }]` và gọi `{% content_for 'blocks' %}` tại vị trí muốn hiển thị.
- Ví dụ: `group.liquid` (bọc layout cho các block con), `text.liquid` (hiển thị đoạn text tuỳ chỉnh).
- Giới hạn: tối đa 8 tầng lồng nhau (nested blocks depth) — xem [shopify_theme_roadmap_detail.md](shopify_theme_roadmap_detail.md) mục Ngày 8.
