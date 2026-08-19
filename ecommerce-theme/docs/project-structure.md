# 🗂️ Cấu trúc project — `ecommerce-theme`

Theme Shopify (Liquid, Online Store 2.0) + custom style bằng SCSS. Doc này chỉ mô tả **cấu trúc thư mục hiện tại**, không lặp lại nội dung roadmap/tiến độ (xem [`docs/continue-here.md`](../../docs/continue-here.md) ở root repo cho phần đó).

> **Phạm vi**: theme này chỉ build **2 trang** — Homepage và Product detail. Toàn bộ file skeleton của các trang khác đã bị xoá có chủ đích (xem mục [Đã cắt khỏi skeleton](#-đã-cắt-khỏi-skeleton) ở cuối).

```
ecommerce-theme/
├── config/
│   ├── settings_schema.json      # Khai báo TẤT CẢ theme setting (màu, font, radius, layout...)
│   │                              # → nguồn sự thật gốc của design token, có "default" value
│   └── settings_data.json        # Auto-generated — giá trị merchant đã Save qua Theme Editor
│                                  # (override lên default trong settings_schema.json). KHÔNG sửa tay.
│
├── layout/
│   └── theme.liquid              # File DUY NHẤT Shopify bắt buộc phải có. Khung HTML chính
│                                  # (<head>, <body>), nơi load CSS/font/section group.
│
├── locales/
│   ├── en.default.json           # Text hiển thị cho khách — 3 group: footer, products, sections
│   └── en.default.schema.json    # Text hiển thị trong Theme Editor (label/info/option của setting)
│                                  # Cả 2 file đã prune sạch key mồ côi, mọi key còn lại đều đang được dùng.
│
├── sections/                     # Mỗi file = 1 block nội dung thêm/sửa/xoá/kéo-thả được trong Theme Editor
│   ├── header.liquid             # + announcement bar, mobile nav (<dialog>), search popup
│   ├── footer.liquid             # + newsletter band, social, payment icon
│   ├── header-group.json         # Section group — Shopify render qua {% sections 'header-group' %}
│   ├── footer-group.json
│   ├── slideshow.liquid          # ┐
│   ├── logo-list.liquid          # │
│   ├── featured-collection.liquid# ├─ Homepage (thứ tự ráp trong templates/index.json)
│   ├── collections-grid.liquid   # │
│   ├── testimonials.liquid       # ┘
│   └── product.liquid            # Product detail — gallery + variant picker + quantity + add to cart
│
├── blocks/                       # Theme block (OS 2.0) — nhỏ hơn section, merchant thêm/xoá/sắp xếp được
│   ├── slide.liquid              # dùng trong slideshow
│   ├── logo.liquid               # dùng trong logo-list
│   ├── collection-card.liquid    # dùng trong collections-grid
│   ├── testimonial.liquid        # dùng trong testimonials
│   └── link_list.liquid          # dùng trong footer
│
├── snippets/                     # Đoạn Liquid tái sử dụng, gọi bằng {% render 'ten-snippet' %}
│   ├── css-variables.liquid      # Đọc settings.* → render ra CSS custom properties (:root { --... })
│   ├── product-card.liquid       # Card sản phẩm dùng chung (home grid + sau này related products)
│   ├── rating-stars.liquid       # Dãy sao ★ 2 lớp track/fill overlay theo %
│   ├── image.liquid              # Wrapper image_url + image_tag (có {% doc %})
│   └── meta-tags.liquid          # <title>, og:*, twitter:* — theme.liquid render ở cuối <head>
│
├── templates/                    # Ghép section thành 1 trang cụ thể
│   ├── index.json                # Homepage — 6 section + toàn bộ default content theo Figma
│   └── product.json              # Product detail — 1 section "main" type product
│
├── assets/                       # DUY NHẤT nơi Shopify serve được static file (css/js/svg/font...)
│   ├── critical.css              # CSS tối giản load trước (từ skeleton). Chứa utility grid `.full-width`
│   │                              # mà header/footer/slideshow phụ thuộc → KHÔNG xoá.
│   ├── theme.css                 # ⚠️ AUTO-GENERATED — output compile từ src/scss/, ĐỪNG sửa tay ở đây
│   └── icon-*.svg                # 14 icon, nhúng inline bằng {{ 'icon-x.svg' | inline_asset_content }}
│
├── src/scss/                     # ✍️ Code SCSS thật viết ở đây (KHÔNG nằm trong assets/, Shopify không đọc)
│   ├── main.scss                 # Entry point — @use tất cả file con vào đây theo đúng thứ tự
│   ├── abstracts/
│   │   ├── _variables.scss       # Breakpoint, z-index scale — KHÔNG chứa màu/font (đã có ở settings_schema)
│   │   └── _mixins.scss          # flex-center, respond-to, respond-to-max, truncate-lines
│   ├── base/
│   │   ├── _reset.scss           # Chỉ bù phần critical.css chưa cover
│   │   ├── _typography.scss      # font-family/color gốc, heading scale
│   │   ├── _container.scss       # Utility .container / .container-fullwidth (max-width 1240px theo Figma)
│   │   └── _utilities.scss       # .visually-hidden
│   └── components/               # 13 file — 1 file cho mỗi section/component (_product.scss, _header.scss...)
│
├── docs/
│   ├── project-structure.md      # File này
│   └── scss-guide.md
│
├── package.json                  # Script build/watch SCSS (devDependency: sass)
├── .theme-check.yml              # Config cho `shopify theme check`
└── .gitignore / .gitattributes / .shopifyignore
```

> Không có thư mục `src/scss/layout/` hay `src/scss/sections/`. Quy ước hiện tại: mỗi section có 1 file trong
> `components/` (kể cả header/footer), vì section và component ở theme này ánh xạ 1-1 — thêm tầng thư mục chỉ
> gây phải nhảy file mà không tách được gì.

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
npm run watch:css    # tự build lại mỗi khi sửa .scss — chạy song với `shopify theme dev`
```

---

## 🗑️ Đã cắt khỏi skeleton

Theme init ra 53 file, giờ còn **29 file** (`shopify theme check` sạch). Đã xoá 30 file skeleton default + prune 70 locale key mồ côi, vì project chỉ làm Homepage + Product detail.

Căn cứ: [Shopify — Theme architecture](https://shopify.dev/docs/storefronts/themes/architecture) — *"Only a `layout` directory containing a `theme.liquid` file is required for the theme to be uploaded to Shopify. No templates are required. However, you need to have a matching template for any page type that you want to render."*

| Nhóm | Đã xoá |
|---|---|
| `sections/` | `404` `article` `blog` `cart` `collection` `collections` `page` `password` `search` + demo `custom-section` `hello-world` |
| `templates/` | `404` `article` `blog` `cart` `collection` `list-collections` `page` `password` `search` (`.json`) + `gift_card.liquid` |
| `layout/` | `password.liquid` |
| `blocks/` | `group.liquid` `text.liquid` — chỉ reachable qua `custom-section` (`"blocks": [{"type": "@theme"}]`) |
| `assets/` | `shoppy-x-ray.svg` (logo skeleton) · `theme.css.map` (artifact cũ, build script đã có `--no-source-map`) |
| Root | `README.md` `CODE_OF_CONDUCT.md` `CONTRIBUTING.md` `LICENSE.md` — boilerplate repo của Shopify Skeleton |
| `locales/` | 32 key storefront (`404.*` `blog.*` `cart.*` `customers.*` `collections.*` `gift_card.*` `password.*` `search.*`) + 38 key schema |

### ⚠️ Hệ quả đã biết — không phải bug

Header/footer/home vẫn còn link trỏ tới các trang đã cắt template:

| Link | Kết quả |
|---|---|
| Form search ở header → `routes.search_url` | `/search` → 404 |
| Submit "Add to Cart" (form non-AJAX) → `/cart` | 404 — **vướng khi test nút ATC** |
| Icon cart ở header → `routes.cart_url` | 404 |
| Nút "View All" ở home → `collection.url` | `/collections/...` → 404 |

Cần lại file nào thì lấy từ git (đã commit trước khi xoá):

```bash
cd <root repo>
git checkout <commit-trước-khi-xoá> -- ecommerce-theme/templates/cart.json ecommerce-theme/sections/cart.liquid
```
