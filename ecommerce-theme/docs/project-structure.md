# 🗂️ Cấu trúc project — `ecommerce-theme`

Theme Shopify (Liquid, Online Store 2.0) + custom style bằng SCSS. Doc này chỉ mô tả **cấu trúc thư mục hiện tại**, không lặp lại nội dung roadmap/tiến độ (xem [`docs/continue-here.md`](../../docs/continue-here.md) ở root repo cho phần đó).

```
ecommerce-theme/
├── config/
│   ├── settings_schema.json      # Khai báo TẤT CẢ theme setting (màu, font, radius, layout...)
│   │                              # → nguồn sự thật gốc của design token, có "default" value
│   └── settings_data.json        # Auto-generated — giá trị merchant đã Save qua Theme Editor
│                                  # (override lên default trong settings_schema.json). KHÔNG sửa tay.
│
├── layout/
│   ├── theme.liquid               # Khung HTML chính (<head>, <body>), nơi load CSS/font/section groups
│   └── password.liquid
│
├── locales/
│   ├── en.default.json           # Text hiển thị cho khách (labels, buttons...)
│   └── en.default.schema.json    # Text hiển thị trong Theme Editor (label setting/schema)
│
├── sections/                     # Mỗi file = 1 block nội dung có thể thêm/sửa/xoá/kéo-thả trong Theme Editor
│   ├── header.liquid / footer.liquid / header-group.json / footer-group.json
│   ├── cart.liquid, collection.liquid, product.liquid, search.liquid, ...
│   └── custom-section.liquid, hello-world.liquid (mẫu/demo từ skeleton)
│
├── blocks/                       # Theme block (Online Store 2.0) — thành phần nhỏ hơn section, nest được
│   ├── group.liquid
│   └── text.liquid
│
├── snippets/                     # Đoạn code Liquid tái sử dụng, gọi bằng {% render 'ten-snippet' %}
│   ├── css-variables.liquid      # Đọc settings.* → render ra CSS custom properties (:root { --... })
│   ├── image.liquid
│   └── meta-tags.liquid
│
├── templates/                    # Ghép các section thành 1 trang cụ thể (index, product, cart, collection...)
│
├── assets/                       # DUY NHẤT nơi Shopify serve được static file (css/js/svg/font...)
│   ├── critical.css              # CSS tối giản, load trước để tránh render-blocking (xem theme.liquid)
│   ├── theme.css                 # ⚠️ AUTO-GENERATED — output compile từ src/scss/, ĐỪNG sửa tay ở đây
│   └── icon-*.svg
│
├── src/scss/                     # ✍️ Code SCSS thật viết ở đây (KHÔNG nằm trong assets/, Shopify không đọc được)
│   ├── main.scss                 # Entry point — @use tất cả file con vào đây
│   ├── abstracts/
│   │   ├── _variables.scss       # Breakpoint, z-index scale — KHÔNG chứa màu/font (đã có ở settings_schema.json)
│   │   └── _mixins.scss          # flex-center, respond-to, truncate-lines...
│   ├── base/                     # Reset, typography chung (chưa có file — làm dần theo Bước 2+)
│   ├── layout/                   # Style cho header/footer/khung trang
│   ├── components/               # button, product-card, rating-stars... (component dùng nhiều nơi)
│   └── sections/                 # Style riêng cho từng section Shopify (hero, feature-banner...)
│
├── package.json                  # Script build/watch SCSS (devDependency: sass)
└── .theme-check.yml               # Config cho `shopify theme check`
```

---

## 🔗 Luồng dữ liệu Design System (đã setup ở Bước 1)

```
config/settings_schema.json   (khai báo setting + default value — nguồn sự thật)
        ↓  merchant chọn màu/font trong Theme Editor → bấm Save
config/settings_data.json     (override thật đang chạy trên store — auto-generated)
        ↓  Liquid đọc qua settings.<id>
snippets/css-variables.liquid (render thành CSS custom property, in ra <head> qua {% render %})
        ↓  toàn theme dùng var(--ten-bien), KHÔNG hardcode lại
src/scss/**/*.scss            (viết style dùng var(--color-foreground), var(--font-heading--family)...)
        ↓  npm run build:css / watch:css (Dart Sass compile)
assets/theme.css              (file CSS thật, Shopify serve được)
        ↓  layout/theme.liquid link vào bằng {{ 'theme.css' | asset_url | stylesheet_tag }}
```

**Nguyên tắc**: không bao giờ có 2 nguồn giá trị cho cùng 1 token. Màu/font/radius chỉ định nghĩa **một lần** ở `settings_schema.json`; SCSS chỉ dùng lại qua `var(--...)`, không khai báo `$color-primary` trùng lặp.

---

## ⚙️ Lệnh SCSS thường dùng

```bash
npm install          # cài sass (chỉ cần 1 lần, hoặc sau khi pull code mới clone máy khác)
npm run build:css    # build 1 lần, minify — dùng trước khi push code
npm run watch:css    # tự build lại mỗi khi sửa .scss — chạy song song với `shopify theme dev`
```
