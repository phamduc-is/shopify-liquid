# 🗺️ Lộ trình Chi tiết — Shopify Theme Development
### Từ Zero đến Theme hoàn chỉnh

> **Dành cho**: Developer có HTML/CSS cơ bản, chưa biết Liquid hay Shopify  
> **Thời gian**: 4–6 tuần (học 2–4 giờ/ngày)  
> **Kết quả**: Tự build và deploy một Shopify theme từ đầu

---

## 🧭 Bản đồ Navigation → Ngày học

Bảng dưới đây map từng mục trong sidebar docs [shopify.dev/docs/storefronts/themes](https://shopify.dev/docs/storefronts/themes) tới ngày học tương ứng.

### GETTING STARTED

| Nav Item | URL | Ngày học |
|---|---|---|
| Overview | [/themes](https://shopify.dev/docs/storefronts/themes) | Ngày 1 |
| Quick start | [/themes/getting-started/create](https://shopify.dev/docs/storefronts/themes/getting-started/create) | Ngày 1–2 |
| Developer preview | [/themes/getting-started/developer-preview](https://shopify.dev/docs/storefronts/themes/getting-started) | Ngày 2 |

### KEY CONCEPTS

| Nav Item | URL | Ngày học |
|---|---|---|
| Architecture | [/themes/architecture](https://shopify.dev/docs/storefronts/themes/architecture) | Ngày 8 |
| Limits | [/themes/architecture/limits](https://shopify.dev/docs/storefronts/themes/architecture) | Ngày 8 |
| Layouts | [/themes/architecture/layouts](https://shopify.dev/docs/storefronts/themes/architecture/layouts) | Ngày 8 |
| Templates | [/themes/architecture/templates](https://shopify.dev/docs/storefronts/themes/architecture/templates) | Ngày 8 |
| Sections | [/themes/architecture/sections](https://shopify.dev/docs/storefronts/themes/architecture/sections) | Ngày 9 |
| Section groups | [/themes/architecture/section-groups](https://shopify.dev/docs/storefronts/themes/architecture/sections) | Ngày 10 |
| Blocks | [/themes/architecture/blocks](https://shopify.dev/docs/storefronts/themes/architecture/blocks) | Ngày 10 |
| Snippets | [/themes/architecture/snippets](https://shopify.dev/docs/storefronts/themes/architecture/snippets) | Ngày 6 |
| Settings | [/themes/architecture/settings](https://shopify.dev/docs/storefronts/themes/architecture/settings) | Ngày 11 |
| Config | [/themes/architecture/config](https://shopify.dev/docs/storefronts/themes/architecture/config) | Ngày 11 |
| Locales | [/themes/architecture/locales](https://shopify.dev/docs/storefronts/themes/architecture/locales) | Ngày 12 |

### BEST PRACTICES

| Nav Item | URL | Ngày học |
|---|---|---|
| Overview | [/themes/best-practices](https://shopify.dev/docs/storefronts/themes/best-practices) | Ngày 12 |
| Sections and blocks | [/themes/best-practices/sections-blocks](https://shopify.dev/docs/storefronts/themes/best-practices) | Ngày 10 |
| JavaScript and stylesheet tags | [/themes/best-practices/javascript-stylesheet-tags](https://shopify.dev/docs/storefronts/themes/best-practices) | Ngày 20 |
| Events and actions | [/themes/best-practices/events-actions](https://shopify.dev/docs/storefronts/themes/best-practices) | Ngày 20 |
| Performance | [/themes/best-practices/performance](https://shopify.dev/docs/storefronts/themes/best-practices/performance) | Ngày 24 |
| Accessibility | [/themes/best-practices/accessibility](https://shopify.dev/docs/storefronts/themes/best-practices/accessibility) | Ngày 24 |
| Theme editor | [/themes/best-practices/theme-editor](https://shopify.dev/docs/storefronts/themes/best-practices) | Ngày 11 |
| Design | [/themes/best-practices/design](https://shopify.dev/docs/storefronts/themes/best-practices) | Ngày 34–35 |
| Merchant stores | [/themes/best-practices/merchant-stores](https://shopify.dev/docs/storefronts/themes/best-practices) | Ngày 38 |
| Version control | [/themes/best-practices/version-control](https://shopify.dev/docs/storefronts/themes/best-practices) | Ngày 2 |
| File transformation | [/themes/best-practices/file-transformation](https://shopify.dev/docs/storefronts/themes/best-practices) | Ngày 24 |
| Deceptive code | [/themes/best-practices/deceptive-code](https://shopify.dev/docs/storefronts/themes/best-practices) | Ngày 36 |

### DEVELOPER TOOLS

| Nav Item | URL | Ngày học |
|---|---|---|
| Overview | [/themes/tools](https://shopify.dev/docs/storefronts/themes/tools) | Ngày 1 |
| Shopify CLI | [/themes/tools/cli](https://shopify.dev/docs/storefronts/themes/tools/cli) | Ngày 1–2 |
| GitHub | [/themes/tools/github](https://shopify.dev/docs/storefronts/themes/tools/github) | Ngày 2 |
| Theme Check | [/themes/tools/theme-check](https://shopify.dev/docs/storefronts/themes/tools/theme-check) | Ngày 24, 36 |

### LIQUID API (docs riêng)

| Nav Item | URL | Ngày học |
|---|---|---|
| Liquid Overview | [/api/liquid](https://shopify.dev/docs/api/liquid) | Ngày 3 |
| Objects | [/api/liquid/objects](https://shopify.dev/docs/api/liquid/objects) | Ngày 3 |
| Filters | [/api/liquid/filters](https://shopify.dev/docs/api/liquid/filters) | Ngày 4 |
| Tags | [/api/liquid/tags](https://shopify.dev/docs/api/liquid/tags) | Ngày 5 |

### AJAX API (docs riêng)

| Nav Item | URL | Ngày học |
|---|---|---|
| Ajax API | [/api/ajax](https://shopify.dev/docs/api/ajax) | Ngày 20–21 |
| Section Rendering API | [/api/ajax/section-rendering](https://shopify.dev/docs/api/ajax/section-rendering) | Ngày 22–23 |

### THEME STORE

| Nav Item | URL | Ngày học |
|---|---|---|
| Theme Store | [/themes/store](https://shopify.dev/docs/storefronts/themes/store) | Ngày 38 (bonus) |

---

## 🗓️ GIAI ĐOẠN 1 — Thiết lập môi trường
### ⏱️ Thời gian: 2 ngày | Ngày 1–2

---

### 📅 Ngày 1 — Setup & Khám phá Shopify Admin

> 📚 **Docs cần đọc:**
> | Section | Nav Item | Link |
> |---|---|---|
> | GETTING STARTED | **Overview** | [shopify.dev/docs/storefronts/themes](https://shopify.dev/docs/storefronts/themes) |
> | GETTING STARTED | **Quick start** | [/themes/getting-started/create](https://shopify.dev/docs/storefronts/themes/getting-started/create) |
> | DEVELOPER TOOLS | **Overview** | [/themes/tools](https://shopify.dev/docs/storefronts/themes/tools) |
> | DEVELOPER TOOLS | **Shopify CLI** | [/themes/tools/cli](https://shopify.dev/docs/storefronts/themes/tools/cli) |

#### Buổi sáng (1.5 giờ) — Tạo tài khoản & Dev Store

**Bước 1: Tạo Shopify Partner Account (30 phút)**
```
1. Vào https://partners.shopify.com
2. Đăng ký bằng email
3. Vào Dashboard → Stores → Add store
4. Chọn "Development store"
5. Đặt tên store (vd: "mydev-store-2026")
6. Chọn "Start with test data" → Có sẵn sản phẩm mẫu
```

> ✅ **Checkpoint**: Truy cập được `https://yourstore.myshopify.com`

**Bước 2: Khám phá Shopify Admin (30 phút)**
```
Shopify Admin → Online Store → Themes
→ Xem cấu trúc theme Dawn (theme mặc định)
→ Click "Customize" → Xem Theme Editor
→ Thử kéo thả sections
→ Xem panel Settings
```

**Bước 3: Tải về Dawn theme để tham khảo (30 phút)**
```bash
git clone https://github.com/Shopify/dawn.git
# Mở bằng VS Code → xem cấu trúc thư mục
```

#### Buổi chiều (1.5 giờ) — Cài công cụ

**Cài đặt theo thứ tự:**

```bash
# 1. Cài Homebrew (nếu chưa có - macOS)
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# 2. Cài Node.js (cần cho Shopify CLI)
brew install node

# 3. Cài Shopify CLI
npm install -g @shopify/cli @shopify/theme

# 4. Verify
shopify version   # Phải hiện version number
node --version    # v18+ là ok
```

**Cài VS Code Extensions:**
```
1. Mở VS Code → Extensions (Ctrl+Shift+X)
2. Tìm và cài:
   - "Shopify Liquid" (by Shopify) — autocomplete Liquid
   - "Prettier" — format code
   - "GitLens" — xem git history
   - "Liquid" (by sissel) — syntax highlight thêm
```

> ✅ **Checkpoint**: `shopify version` trả về version number, không lỗi

---

### 📅 Ngày 2 — Tạo theme đầu tiên & Workflow cơ bản

> 📚 **Docs cần đọc:**
> | Section | Nav Item | Link |
> |---|---|---|
> | GETTING STARTED | **Quick start** (tiếp) | [/themes/getting-started/create](https://shopify.dev/docs/storefronts/themes/getting-started/create) |
> | GETTING STARTED | **Developer preview** | [/themes/getting-started](https://shopify.dev/docs/storefronts/themes/getting-started) |
> | DEVELOPER TOOLS | **Shopify CLI** (đọc kỹ commands) | [/themes/tools/cli](https://shopify.dev/docs/storefronts/themes/tools/cli) |
> | DEVELOPER TOOLS | **GitHub** | [/themes/tools/github](https://shopify.dev/docs/storefronts/themes/tools/github) |
> | BEST PRACTICES | **Version control** | [/themes/best-practices/version-control](https://shopify.dev/docs/storefronts/themes/best-practices) |

#### Buổi sáng (1.5 giờ) — Theme Init & Dev Server

```bash
# 1. Tạo theme từ Skeleton (theme tối giản nhất)
mkdir ~/shopify-projects
cd ~/shopify-projects
shopify theme init my-first-theme

# 2. Vào thư mục
cd my-first-theme

# 3. Login vào Shopify store
shopify auth login --store yourstore.myshopify.com

# 4. Chạy dev server
shopify theme dev --store yourstore.myshopify.com
```

**Khi dev server chạy:**
- Terminal hiển thị URL preview (thường `http://127.0.0.1:9292`)
- Mở URL đó trên browser
- Sửa file → browser tự reload

**Cấu trúc thư mục ban đầu:**
```
my-first-theme/
├── assets/            ← CSS, JS, images
├── config/
│   ├── settings_data.json
│   └── settings_schema.json
├── layout/
│   └── theme.liquid   ← File quan trọng nhất
├── locales/
│   └── en.default.json
├── sections/          ← Hiện tại rỗng
├── snippets/          ← Hiện tại rỗng
└── templates/
    ├── index.json
    ├── product.json
    └── ...
```

#### Buổi chiều (1.5 giờ) — Thực hành sửa file đầu tiên

**Bài tập 1**: Sửa `layout/theme.liquid`
```liquid
<!-- Tìm thẻ <title> và thêm tên bạn vào -->
<title>{{ page_title }} — Học Shopify với [Tên bạn]</title>
```
→ Lưu → Xem browser tự reload

**Bài tập 2**: Tạo file CSS đầu tiên
```bash
# Tạo file assets/theme.css
touch assets/theme.css
```
```css
/* assets/theme.css */
:root {
  --color-primary: #2c3e50;
  --color-accent: #e74c3c;
  --font-size-base: 16px;
}

* { box-sizing: border-box; }
body {
  font-family: system-ui, sans-serif;
  font-size: var(--font-size-base);
  color: var(--color-primary);
}
```

**Bài tập 3**: Link CSS vào layout
```liquid
<!-- Trong layout/theme.liquid, trong <head> -->
{{ 'theme.css' | asset_url | stylesheet_tag }}
```

**Workflow hàng ngày:**
```
shopify theme dev → code → xem browser → commit git → push
```

> ✅ **Checkpoint cuối ngày 2**: Dev server chạy, thay đổi CSS thấy ngay trên browser

---

## 🌊 GIAI ĐOẠN 2 — Liquid Templating Language
### ⏱️ Thời gian: 5 ngày | Ngày 3–7

> **Triết lý**: Liquid = HTML + logic. Học từ đơn giản → phức tạp.

---

### 📅 Ngày 3 — Objects & Output

> 📚 **Docs cần đọc:**
> | Section | Nav Item | Link |
> |---|---|---|
> | LIQUID API | **Liquid Overview** | [/api/liquid](https://shopify.dev/docs/api/liquid) |
> | LIQUID API | **Objects** | [/api/liquid/objects](https://shopify.dev/docs/api/liquid/objects) |
>
> Đọc kỹ các object: `shop`, `product`, `collection`, `cart`, `customer`, `page`, `request`

#### Lý thuyết (1 giờ)

**Liquid Output syntax: `{{ }}`**
```liquid
{{ shop.name }}          → "My Awesome Store"
{{ shop.email }}         → "owner@store.com"
{{ shop.currency }}      → "USD"
{{ shop.description }}   → Mô tả store

{{ product.title }}      → "Awesome T-Shirt"
{{ product.price }}      → 1999 (cents!)
{{ product.vendor }}     → "Nike"
{{ product.type }}       → "Apparel"
{{ product.handle }}     → "awesome-t-shirt" (URL slug)
```

> ⚠️ **Quan trọng**: `product.price` là số nguyên (cents).  
> `1999` = $19.99. Luôn dùng filter `money` để format!

**Các Global objects quan trọng:**
```liquid
{{ shop }}          → Thông tin store
{{ product }}       → Sản phẩm (chỉ có trong product template)
{{ collection }}    → Bộ sưu tập (trong collection template)
{{ cart }}          → Giỏ hàng
{{ customer }}      → Khách hàng đang login
{{ page }}          → Trang (trong page template)
{{ blog }}          → Blog
{{ article }}       → Bài viết blog
{{ request }}       → Thông tin request hiện tại
{{ settings }}      → Theme settings (từ settings_schema.json)
```

**Truy cập properties:**
```liquid
{{ product.title }}
{{ product.images.first }}
{{ product.variants.first.price }}
{{ cart.item_count }}
{{ customer.first_name }}
{{ settings.color_primary }}
```

#### Thực hành (1.5 giờ)

**Bài tập 1**: Tạo `snippets/store-info.liquid`
```liquid
<div class="store-info">
  <h1>{{ shop.name }}</h1>
  <p>{{ shop.description }}</p>
  <p>Currency: {{ shop.currency }}</p>
  <p>Email: {{ shop.email }}</p>
</div>
```

Render snippet trong `layout/theme.liquid`:
```liquid
{% render 'store-info' %}
```

**Bài tập 2**: Khám phá `product` object
Mở trang product bất kỳ, thêm vào template:
```liquid
<!-- Tạm thời debug -->
{{ product | json }}
```
→ Xem toàn bộ data structure của product

> 💡 `| json` filter cực hữu ích để debug, xem object có những field gì

**Quiz ngày 3:**
1. Câu lệnh nào hiển thị số lượng items trong cart?
2. Tại sao `{{ product.price }}` lại không phải là `$19.99`?
3. `{{ customer }}` trả về gì khi khách chưa login?

---

### 📅 Ngày 4 — Filters (Bộ lọc & Biến đổi)

> 📚 **Docs cần đọc:**
> | Section | Nav Item | Link |
> |---|---|---|
> | LIQUID API | **Filters** | [/api/liquid/filters](https://shopify.dev/docs/api/liquid/filters) |
>
> Tập trung: String filters, Money filters, Array filters, URL filters, Color filters

#### Lý thuyết (1 giờ)

**Nhóm 1: String filters**
```liquid
{{ "hello world" | upcase }}          → "HELLO WORLD"
{{ "HELLO" | downcase }}              → "hello"
{{ "  hello  " | strip }}            → "hello"
{{ "hello world" | capitalize }}      → "Hello world"
{{ "hello" | append: " world" }}      → "hello world"
{{ "hello world" | replace: "world", "Shopify" }} → "hello Shopify"
{{ product.description | truncate: 150 }} → "Cắt bớt sau 150 chars..."
{{ product.description | strip_html }}    → Bỏ HTML tags
{{ product.title | handleize }}           → "product-title" (URL slug)
{{ product.title | escape }}              → Escape special chars (XSS protection)
```

**Nhóm 2: Number & Money filters**
```liquid
{{ product.price | money }}           → "$19.99"
{{ product.price | money_with_currency }} → "$19.99 USD"
{{ product.price | money_without_currency }} → "19.99"
{{ 1.5 | round }}                     → 2
{{ 1.5 | floor }}                     → 1
{{ 1.5 | ceil }}                      → 2
{{ 3 | divided_by: 2.0 }}            → 1.5
{{ 3 | modulo: 2 }}                  → 1
{{ -5 | abs }}                        → 5
{{ 3.14159 | round: 2 }}             → 3.14
```

**Nhóm 3: Array filters**
```liquid
{{ product.images | first }}          → Ảnh đầu tiên
{{ product.images | last }}           → Ảnh cuối
{{ product.tags | join: ", " }}       → "tag1, tag2, tag3"
{{ collection.products | size }}      → Số lượng sản phẩm
{{ collection.products | reverse }}   → Đảo ngược mảng
{{ collection.products | map: "title" }} → ["Title1", "Title2"]
{{ collection.products | sort: "price" }} → Sắp xếp theo price
{{ collection.products | where: "available", true }} → Chỉ hàng còn hàng
{{ collection.products | uniq }}      → Bỏ trùng lặp
```

**Nhóm 4: URL & Asset filters**
```liquid
{{ 'theme.css' | asset_url }}        → URL của file CSS
{{ 'theme.css' | asset_url | stylesheet_tag }} → <link rel="stylesheet">
{{ 'theme.js' | asset_url | script_tag }}      → <script src="...">
{{ product.featured_image | image_url: width: 800 }} → Optimized image URL
{{ image | image_url: width: 400, height: 400, crop: 'center' }}
{{ product.url }}                    → "/products/my-product"
{{ product.url | within: collection }} → URL trong context collection
```

**Nhóm 5: Date filters**
```liquid
{{ article.published_at | date: "%B %d, %Y" }} → "July 27, 2026"
{{ article.published_at | date: "%d/%m/%Y" }}  → "27/07/2026"
{{ "now" | date: "%H:%M" }}                    → "16:21"
```

**Chaining filters:**
```liquid
{{ product.title | upcase | truncate: 20 }}
{{ product.description | strip_html | truncate: 100 }}
{{ image | image_url: width: 400 | img_tag: product.title }}
```

#### Thực hành (1.5 giờ)

**Bài tập 1**: Tạo `snippets/product-price.liquid`
```liquid
<div class="product-price">
  {% if product.compare_at_price > product.price %}
    <span class="price-compare">{{ product.compare_at_price | money }}</span>
    <span class="price-sale">{{ product.price | money }}</span>
    {% assign discount = product.compare_at_price | minus: product.price %}
    {% assign discount_pct = discount | times: 100 | divided_by: product.compare_at_price %}
    <span class="badge-discount">-{{ discount_pct | round }}%</span>
  {% else %}
    <span class="price">{{ product.price | money }}</span>
  {% endif %}
</div>
```

**Bài tập 2**: Format date cho article
```liquid
<time datetime="{{ article.published_at | date: '%Y-%m-%d' }}">
  {{ article.published_at | date: "%d tháng %m, %Y" }}
</time>
```

**Bài tập 3**: Tạo meta description SEO
```liquid
<meta name="description" content="{{ product.description | strip_html | truncate: 160 | escape }}">
```

---

### 📅 Ngày 5 — Tags: Logic điều khiển

> 📚 **Docs cần đọc:**
> | Section | Nav Item | Link |
> |---|---|---|
> | LIQUID API | **Tags** | [/api/liquid/tags](https://shopify.dev/docs/api/liquid/tags) |
>
> Tập trung: Control flow tags, Iteration tags, Variable tags, Theme tags (render, section, layout)

#### Lý thuyết (1 giờ)

**Control Flow:**
```liquid
{% if condition %}
  ...
{% elsif other_condition %}
  ...
{% else %}
  ...
{% endif %}

{% unless product.available %}
  <p>Hết hàng</p>
{% endunless %}

{% case product.type %}
  {% when "shirts" %}
    <p>Đây là áo</p>
  {% when "pants" %}
    <p>Đây là quần</p>
  {% else %}
    <p>Loại khác</p>
{% endcase %}
```

**Operators:**
```liquid
==   !=   >   <   >=   <=   
and  or   not
contains   (kiểm tra string hoặc array)

{% if product.tags contains "new-arrival" %}
{% if product.price >= 100000 %}
{% if customer and customer.tags contains "vip" %}
```

**Iteration:**
```liquid
{% for product in collection.products %}
  <div class="product-card">
    <h2>{{ product.title }}</h2>
    <p>{{ product.price | money }}</p>
  </div>
{% endfor %}

<!-- forloop object -->
{% for item in cart.items %}
  <p>Item {{ forloop.index }} of {{ forloop.length }}: {{ item.title }}</p>
  {% if forloop.first %}<ul>{% endif %}
    <li>{{ item.title }}</li>
  {% if forloop.last %}</ul>{% endif %}
{% endfor %}

<!-- limit & offset -->
{% for product in collection.products limit: 4 %}
{% for product in collection.products limit: 4 offset: 4 %}

<!-- break & continue -->
{% for product in collection.products %}
  {% if product.available == false %}
    {% continue %}
  {% endif %}
  {{ product.title }}
{% endfor %}
```

**Biến & Assignment:**
```liquid
{% assign my_string = "hello" %}
{% assign my_number = 42 %}
{% assign my_array = "a,b,c" | split: "," %}
{% assign is_sale = product.compare_at_price > product.price %}

<!-- capture: assign multi-line string -->
{% capture product_card %}
  <div class="card">
    <h2>{{ product.title }}</h2>
    <p>{{ product.price | money }}</p>
  </div>
{% endcapture %}

{{ product_card }}
```

**Pagination:**
```liquid
{% paginate collection.products by 12 %}
  {% for product in collection.products %}
    <!-- product card -->
  {% endfor %}
  
  {{ paginate | default_pagination }}
{% endpaginate %}
```

#### Thực hành (1.5 giờ)

**Bài tập 1**: Danh sách sản phẩm với badges
```liquid
{% for product in collection.products limit: 8 %}
  <div class="product-card">
    {% if product.available == false %}
      <span class="badge badge--sold-out">Hết hàng</span>
    {% elsif product.compare_at_price > product.price %}
      <span class="badge badge--sale">Sale</span>
    {% endif %}
    
    <img src="{{ product.featured_image | image_url: width: 400 }}" 
         alt="{{ product.featured_image.alt | escape }}">
    <h3>{{ product.title }}</h3>
    <p>{{ product.price | money }}</p>
  </div>
{% else %}
  <p>Không có sản phẩm nào.</p>
{% endfor %}
```

**Bài tập 2**: Navigation menu
```liquid
{% assign main_menu = linklists.main-menu %}
<nav>
  <ul>
    {% for link in main_menu.links %}
      <li class="{% if link.active %}is-active{% endif %}">
        <a href="{{ link.url }}">{{ link.title }}</a>
        
        {% if link.links.size > 0 %}
          <ul class="submenu">
            {% for child_link in link.links %}
              <li>
                <a href="{{ child_link.url }}">{{ child_link.title }}</a>
              </li>
            {% endfor %}
          </ul>
        {% endif %}
      </li>
    {% endfor %}
  </ul>
</nav>
```

---

### 📅 Ngày 6 — Snippets, Render & Includes

> 📚 **Docs cần đọc:**
> | Section | Nav Item | Link |
> |---|---|---|
> | KEY CONCEPTS | **Snippets** | [/themes/architecture/snippets](https://shopify.dev/docs/storefronts/themes/architecture/snippets) |
> | LIQUID API | **Tags** → `render`, `section` | [/api/liquid/tags](https://shopify.dev/docs/api/liquid/tags) |

#### Lý thuyết (45 phút)

**Snippets** là partial templates — code tái sử dụng.

```liquid
<!-- ❌ Cũ (deprecated): -->
{% include 'product-card' %}

<!-- ✅ Mới (dùng cái này): -->
{% render 'product-card' %}

<!-- Truyền biến vào snippet: -->
{% render 'product-card', product: product, show_vendor: true %}

<!-- Render với loop (tối ưu hơn for + render): -->
{% render 'product-card' for collection.products as product %}
```

**Sự khác biệt `render` vs `include`:**
| | `include` | `render` |
|---|---|---|
| Scope | Kế thừa parent scope | Scope riêng biệt |
| Performance | Chậm hơn | Nhanh hơn |
| Truyền biến | Tự động | Phải khai báo rõ |
| Status | Deprecated | Khuyến dùng |

#### Thực hành (1.5 giờ)

**Tạo `snippets/product-card.liquid`** — snippet chuẩn:
```liquid
{% comment %}
  Renders a product card.
  Accepts:
  - product: {Object} Product object.
  - show_vendor: {Boolean} Show product vendor. Default: false
  - show_quick_add: {Boolean} Show quick add button. Default: false
  - lazy_load: {Boolean} Use lazy loading. Default: true
  Usage:
  {% render 'product-card', product: product, show_vendor: true %}
{% endcomment %}

{%- liquid
  assign ratio = 1
  if product.featured_image and product.featured_image.width > 0
    assign ratio = product.featured_image.height | divided_by: product.featured_image.width | times: 100.0
  endif
-%}

<div class="product-card">
  <a href="{{ product.url }}" class="product-card__link">
    
    <!-- Image -->
    <div class="product-card__image-wrapper" style="padding-top: {{ ratio }}%">
      {%- if product.featured_image -%}
        <img
          src="{{ product.featured_image | image_url: width: 400 }}"
          srcset="
            {{ product.featured_image | image_url: width: 200 }} 200w,
            {{ product.featured_image | image_url: width: 400 }} 400w,
            {{ product.featured_image | image_url: width: 600 }} 600w
          "
          sizes="(max-width: 768px) 50vw, 25vw"
          alt="{{ product.featured_image.alt | escape }}"
          loading="{{ lazy_load | default: true | ternary: 'lazy', 'eager' }}"
          width="{{ product.featured_image.width }}"
          height="{{ product.featured_image.height }}"
        >
      {%- else -%}
        {{ 'product-1' | placeholder_svg_tag: 'product-card__placeholder' }}
      {%- endif -%}
    </div>
    
    <!-- Info -->
    <div class="product-card__info">
      {%- if show_vendor -%}
        <p class="product-card__vendor">{{ product.vendor }}</p>
      {%- endif -%}
      
      <h3 class="product-card__title">{{ product.title }}</h3>
      
      <div class="product-card__price">
        {%- if product.price_varies -%}
          <span>From {{ product.price_min | money }}</span>
        {%- else -%}
          {%- if product.compare_at_price > product.price -%}
            <span class="price--compare">{{ product.compare_at_price | money }}</span>
          {%- endif -%}
          <span class="price">{{ product.price | money }}</span>
        {%- endif -%}
      </div>
    </div>
    
  </a>
  
  {%- if show_quick_add and product.available -%}
    <button 
      class="product-card__quick-add"
      data-product-id="{{ product.id }}"
      data-variant-id="{{ product.selected_or_first_available_variant.id }}"
    >
      Quick Add
    </button>
  {%- endif -%}
</div>
```

---

### 📅 Ngày 7 — Liquid nâng cao & Thực hành tổng hợp

> 📚 **Docs cần đọc:**
> | Section | Nav Item | Link |
> |---|---|---|
> | LIQUID API | **Objects** (đọc lại) | [/api/liquid/objects](https://shopify.dev/docs/api/liquid/objects) |
> | LIQUID API | **Filters** (đọc lại) | [/api/liquid/filters](https://shopify.dev/docs/api/liquid/filters) |
>
> Tập trung: Metafield objects, `forloop` object, whitespace control

#### Lý thuyết (45 phút)

**Liquid nâng cao:**

```liquid
<!-- Whitespace control: dùng - để xóa khoảng trắng -->
{%- assign x = 1 -%}
{{- product.title -}}

<!-- Ternary với liquid tag -->
{%- liquid
  if product.available
    assign button_text = "Add to Cart"
  else
    assign button_text = "Sold Out"
  endif
-%}
<button>{{ button_text }}</button>

<!-- Multiple assignments trong một block -->
{%- liquid
  assign on_sale = false
  if product.compare_at_price > product.price
    assign on_sale = true
    assign discount_amount = product.compare_at_price | minus: product.price
  endif
-%}

<!-- Increment & decrement -->
{% increment my_counter %}  → 0, 1, 2, 3...
{% decrement my_counter %}  → -1, -2, -3...
```

**Metafields** (dữ liệu tuỳ chỉnh):
```liquid
<!-- Metafields cho product -->
{{ product.metafields.custom.size_guide }}
{{ product.metafields.reviews.rating }}

<!-- Access với bracket notation -->
{{ product.metafields["custom"]["size_guide"] }}
```

**Global Liquid variables:**
```liquid
{{ content_for_header }}    <!-- Scripts của Shopify — BẮT BUỘC trong <head> -->
{{ content_for_layout }}    <!-- Nội dung template — BẮT BUỘC trong <body> -->
{{ content_for_index }}     <!-- Dành cho homepage (cũ) -->
```

#### Thực hành tổng hợp (1.5 giờ)

**Mini-project**: Tạo trang collection đơn giản với:
- Grid 4 cột (desktop), 2 cột (mobile)
- Mỗi card có: ảnh, tên, giá, badge sale
- Phân trang (12 sản phẩm/trang)
- Hiển thị "X products found"
> ✅ **Checkpoint cuối Giai đoạn 2**: Hiểu và viết được Liquid để render data động từ Shopify.

---

## 🏗️ GIAI ĐOẠN 3 — Theme Architecture (Online Store 2.0)
### ⏱️ Thời gian: 5 ngày | Ngày 8–12

---

### 📅 Ngày 8 — Layout & Templates

> 📚 **Docs cần đọc:**
> | Section | Nav Item | Link |
> |---|---|---|
> | KEY CONCEPTS | **Architecture** | [/themes/architecture](https://shopify.dev/docs/storefronts/themes/architecture) |
> | KEY CONCEPTS | **Limits** | [/themes/architecture](https://shopify.dev/docs/storefronts/themes/architecture) |
> | KEY CONCEPTS | **Layouts** | [/themes/architecture/layouts](https://shopify.dev/docs/storefronts/themes/architecture/layouts) |
> | KEY CONCEPTS | **Templates** | [/themes/architecture/templates](https://shopify.dev/docs/storefronts/themes/architecture/templates) |
>
> ⚠️ Ngày quan trọng — đọc kỹ cả 4 trang, nắm rõ luồng render.

#### Layout (`layout/theme.liquid`) — Xương sống của theme

```liquid
<!DOCTYPE html>
<html lang="{{ request.locale.iso_code }}" class="no-js">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  
  <!-- SEO -->
  <title>
    {%- if page_title == shop.name -%}
      {{ shop.name }}{% if shop.description %} — {{ shop.description }}{% endif %}
    {%- else -%}
      {{ page_title }} — {{ shop.name }}
    {%- endif -%}
  </title>
  <meta name="description" content="{{ page_description | default: shop.description | escape }}">
  
  <!-- Canonical -->
  <link rel="canonical" href="{{ canonical_url }}">
  
  <!-- Open Graph -->
  {%- if settings.share_image -%}
    <meta property="og:image" content="{{ settings.share_image | image_url: width: 1200 }}">
  {%- endif -%}
  
  <!-- Preconnect for performance -->
  <link rel="preconnect" href="https://fonts.shopifycdn.com" crossorigin>
  
  <!-- CSS -->
  {{ 'theme.css' | asset_url | stylesheet_tag }}
  
  <!-- Shopify required — PHẢI CÓ -->
  {{ content_for_header }}
  
  <!-- Remove no-js class when JS loads -->
  <script>document.documentElement.classList.remove('no-js');</script>
</head>

<body class="template-{{ request.page_type }}">
  
  <!-- Skip to content (accessibility) -->
  <a href="#main-content" class="skip-link">Skip to content</a>
  
  <!-- Header section -->
  {% section 'header' %}
  
  <!-- Announcement bar -->
  {% section 'announcement-bar' %}
  
  <!-- Main content -->
  <main id="main-content" tabindex="-1">
    {{ content_for_layout }}
  </main>
  
  <!-- Footer -->
  {% section 'footer' %}
  
  <!-- JS -->
  <script defer src="{{ 'theme.js' | asset_url }}"></script>
  
</body>
</html>
```

#### Template types

```
templates/
├── 404.liquid              → Page not found
├── article.json            → Blog post
├── blog.json               → Blog listing
├── cart.liquid             → Cart page
├── collection.json         → Product collection
├── collection.list.liquid  → All collections
├── gift_card.liquid        → Gift card (Shopify managed)
├── index.json              → Homepage ← QUAN TRỌNG
├── page.json               → Generic page
├── page.contact.json       → Custom page template
├── product.json            → Product detail
├── search.liquid           → Search results
├── password.liquid         → Password page
└── customers/
    ├── account.liquid
    ├── activate_account.liquid
    ├── addresses.liquid
    ├── login.liquid
    ├── order.liquid
    └── register.liquid
```

**Tạo custom page template:**
```json
// templates/page.about.json
{
  "sections": {
    "hero": {
      "type": "hero-banner",
      "settings": {}
    },
    "team": {
      "type": "team-grid",
      "settings": {}
    }
  },
  "order": ["hero", "team"]
}
```
→ Merchant vào Admin → Pages → Chọn template "About"

#### Limits cần nhớ

| Giới hạn | Giá trị |
|---|---|
| Max sections trên 1 template | 25 |
| Max blocks trên 1 section | 50 |
| Max nested blocks depth | 8 |
| Max `for` loop iterations | 50 (dùng `paginate` nếu nhiều hơn) |
| Max file size (section/snippet) | 100KB |
| Max total theme files | 1,000 |

---

### 📅 Ngày 9 — Sections cơ bản

> 📚 **Docs cần đọc:**
> | Section | Nav Item | Link |
> |---|---|---|
> | KEY CONCEPTS | **Sections** | [/themes/architecture/sections](https://shopify.dev/docs/storefronts/themes/architecture/sections) |
>
> Đọc kỹ: section schema, settings types, presets, tag/class attributes

**Anatomy của một Section:**
```
sections/my-section.liquid
├── HTML markup (dùng {{ section.settings.xxx }})
├── {% schema %} ... {% endschema %}
│   ├── name (tên hiển thị trong Theme Editor)
│   ├── class (CSS class thêm vào wrapper)
│   ├── tag (HTML tag wrapper: section/article/div)
│   ├── settings[] (các control trong Theme Editor)
│   ├── blocks[] (các block có thể thêm)
│   ├── max_blocks (giới hạn số blocks)
│   └── presets[] (preset khi add section mới)
├── {% stylesheet %} ... {% endstylesheet %} (optional, inline CSS)
└── {% javascript %} ... {% endjavascript %} (optional, inline JS)
```

**Tạo `sections/hero-banner.liquid`:**
```liquid
<section 
  class="hero-banner hero-banner--{{ section.settings.height }}"
  style="
    background-color: {{ section.settings.overlay_color }};
    min-height: {% if section.settings.height == 'small' %}400px{% elsif section.settings.height == 'medium' %}600px{% else %}100vh{% endif %};
  "
>
  {%- if section.settings.image -%}
    <div class="hero-banner__image">
      <img
        src="{{ section.settings.image | image_url: width: 1920 }}"
        srcset="
          {{ section.settings.image | image_url: width: 768 }} 768w,
          {{ section.settings.image | image_url: width: 1200 }} 1200w,
          {{ section.settings.image | image_url: width: 1920 }} 1920w
        "
        sizes="100vw"
        alt="{{ section.settings.image.alt | escape }}"
        loading="eager"
        fetchpriority="high"
        width="{{ section.settings.image.width }}"
        height="{{ section.settings.image.height }}"
      >
    </div>
  {%- endif -%}
  
  <div class="hero-banner__content hero-banner__content--{{ section.settings.content_alignment }}">
    {%- if section.settings.subheading != blank -%}
      <p class="hero-banner__subheading">{{ section.settings.subheading }}</p>
    {%- endif -%}
    
    {%- if section.settings.heading != blank -%}
      <h1 class="hero-banner__heading" style="font-size: {{ section.settings.heading_size }}px">
        {{ section.settings.heading }}
      </h1>
    {%- endif -%}
    
    {%- if section.settings.text != blank -%}
      <div class="hero-banner__text">{{ section.settings.text }}</div>
    {%- endif -%}
    
    {%- if section.settings.button_label != blank -%}
      <a 
        href="{{ section.settings.button_link }}"
        class="btn btn--{{ section.settings.button_style }}"
      >
        {{ section.settings.button_label }}
      </a>
    {%- endif -%}
  </div>
</section>

{% schema %}
{
  "name": "Hero Banner",
  "tag": "section",
  "class": "section-hero-banner",
  "settings": [
    { "type": "image_picker", "id": "image", "label": "Background image" },
    {
      "type": "select", "id": "height", "label": "Section height",
      "options": [
        { "value": "small",  "label": "Small (400px)" },
        { "value": "medium", "label": "Medium (600px)" },
        { "value": "large",  "label": "Full screen" }
      ],
      "default": "medium"
    },
    { "type": "header", "content": "Content" },
    { "type": "text", "id": "subheading", "label": "Subheading", "default": "Welcome to our store" },
    { "type": "text", "id": "heading", "label": "Heading", "default": "Shop the Latest" },
    { "type": "range", "id": "heading_size", "label": "Heading size", "min": 24, "max": 80, "step": 4, "unit": "px", "default": 48 },
    { "type": "richtext", "id": "text", "label": "Description" },
    {
      "type": "select", "id": "content_alignment", "label": "Content alignment",
      "options": [
        { "value": "left", "label": "Left" },
        { "value": "center", "label": "Center" },
        { "value": "right", "label": "Right" }
      ],
      "default": "center"
    },
    { "type": "header", "content": "Button" },
    { "type": "text", "id": "button_label", "label": "Button label", "default": "Shop now" },
    { "type": "url", "id": "button_link", "label": "Button link" },
    {
      "type": "select", "id": "button_style", "label": "Button style",
      "options": [
        { "value": "primary", "label": "Primary" },
        { "value": "secondary", "label": "Secondary" },
        { "value": "outline", "label": "Outline" }
      ],
      "default": "primary"
    },
    { "type": "color", "id": "overlay_color", "label": "Overlay color", "default": "rgba(0,0,0,0)" }
  ],
  "presets": [
    { "name": "Hero Banner", "settings": { "heading": "Shop the Latest", "button_label": "Shop now" } }
  ]
}
{% endschema %}
```

---

### 📅 Ngày 10 — Sections nâng cao: Blocks & Section Groups

> 📚 **Docs cần đọc:**
> | Section | Nav Item | Link |
> |---|---|---|
> | KEY CONCEPTS | **Section groups** | [/themes/architecture/sections](https://shopify.dev/docs/storefronts/themes/architecture/sections) |
> | KEY CONCEPTS | **Blocks** | [/themes/architecture/blocks](https://shopify.dev/docs/storefronts/themes/architecture/blocks) |
> | BEST PRACTICES | **Sections and blocks** | [/themes/best-practices](https://shopify.dev/docs/storefronts/themes/best-practices) |

**Section với draggable blocks:**

```liquid
<!-- sections/feature-columns.liquid -->
<div class="feature-columns feature-columns--{{ section.blocks.size }}-cols">
  {% for block in section.blocks %}
    <div class="feature-col" {{ block.shopify_attributes }}>
      {%- case block.type -%}
        
        {%- when 'image_with_text' -%}
          {%- if block.settings.image -%}
            <img src="{{ block.settings.image | image_url: width: 600 }}" alt="{{ block.settings.image.alt | escape }}">
          {%- else -%}
            {{ 'collection-1' | placeholder_svg_tag }}
          {%- endif -%}
          <h3>{{ block.settings.heading }}</h3>
          <p>{{ block.settings.text }}</p>
          
        {%- when 'icon_with_text' -%}
          <div class="icon">{% render 'icon', name: block.settings.icon %}</div>
          <h3>{{ block.settings.heading }}</h3>
          <p>{{ block.settings.text }}</p>
          
      {%- endcase -%}
    </div>
  {% endfor %}
</div>

{% schema %}
{
  "name": "Feature Columns",
  "blocks": [
    {
      "type": "image_with_text",
      "name": "Image with text",
      "limit": 4,
      "settings": [
        { "type": "image_picker", "id": "image", "label": "Image" },
        { "type": "text", "id": "heading", "label": "Heading", "default": "Feature" },
        { "type": "textarea", "id": "text", "label": "Text" }
      ]
    },
    {
      "type": "icon_with_text",
      "name": "Icon with text",
      "limit": 4,
      "settings": [
        {
          "type": "select", "id": "icon", "label": "Icon",
          "options": [
            { "value": "truck", "label": "Free shipping" },
            { "value": "lock", "label": "Secure payment" },
            { "value": "refresh", "label": "Easy returns" },
            { "value": "headset", "label": "24/7 support" }
          ]
        },
        { "type": "text", "id": "heading", "label": "Heading" },
        { "type": "textarea", "id": "text", "label": "Text" }
      ]
    }
  ],
  "presets": [
    {
      "name": "Feature Columns",
      "blocks": [
        { "type": "icon_with_text", "settings": { "icon": "truck", "heading": "Free Shipping" }},
        { "type": "icon_with_text", "settings": { "icon": "lock", "heading": "Secure Payment" }},
        { "type": "icon_with_text", "settings": { "icon": "refresh", "heading": "Easy Returns" }}
      ]
    }
  ]
}
{% endschema %}
```

> 💡 `{{ block.shopify_attributes }}` — **LUÔN phải có** để Theme Editor highlight block khi hover

---

### 📅 Ngày 11 — Global Settings, Config & Theme Editor

> 📚 **Docs cần đọc:**
> | Section | Nav Item | Link |
> |---|---|---|
> | KEY CONCEPTS | **Settings** | [/themes/architecture/settings](https://shopify.dev/docs/storefronts/themes/architecture/settings) |
> | KEY CONCEPTS | **Config** | [/themes/architecture/config](https://shopify.dev/docs/storefronts/themes/architecture/config) |
> | BEST PRACTICES | **Theme editor** | [/themes/best-practices](https://shopify.dev/docs/storefronts/themes/best-practices) |
>
> Đọc kỹ: tất cả setting input types, `settings_data.json` vs `settings_schema.json`

**`config/settings_schema.json`** — Toàn bộ Theme Settings panel:

```json
[
  {
    "name": "theme_info",
    "theme_name": "Minimal Commerce",
    "theme_version": "1.0.0",
    "theme_author": "Your Name",
    "theme_documentation_url": "https://example.com"
  },
  {
    "name": "Typography",
    "settings": [
      { "type": "header", "content": "Headings" },
      { "type": "font_picker", "id": "heading_font", "label": "Font", "default": "helvetica_n4" },
      { "type": "range", "id": "heading_base_size", "label": "Base size", "min": 12, "max": 24, "step": 1, "unit": "px", "default": 16 },
      { "type": "header", "content": "Body" },
      { "type": "font_picker", "id": "body_font", "label": "Font", "default": "helvetica_n4" },
      { "type": "range", "id": "body_base_size", "label": "Base size", "min": 12, "max": 20, "step": 1, "unit": "px", "default": 16 }
    ]
  },
  {
    "name": "Colors",
    "settings": [
      { "type": "header", "content": "Brand" },
      { "type": "color", "id": "color_primary", "label": "Primary", "default": "#2c3e50" },
      { "type": "color", "id": "color_secondary", "label": "Secondary", "default": "#e74c3c" },
      { "type": "color", "id": "color_accent", "label": "Accent", "default": "#3498db" },
      { "type": "header", "content": "Background" },
      { "type": "color", "id": "color_bg", "label": "Background", "default": "#ffffff" },
      { "type": "color", "id": "color_bg_secondary", "label": "Secondary bg", "default": "#f8f9fa" },
      { "type": "header", "content": "Text" },
      { "type": "color", "id": "color_text", "label": "Body text", "default": "#333333" },
      { "type": "color", "id": "color_text_light", "label": "Light text", "default": "#666666" }
    ]
  },
  {
    "name": "Social media",
    "settings": [
      { "type": "url", "id": "social_facebook", "label": "Facebook" },
      { "type": "url", "id": "social_instagram", "label": "Instagram" },
      { "type": "url", "id": "social_twitter", "label": "Twitter/X" },
      { "type": "url", "id": "social_tiktok", "label": "TikTok" }
    ]
  }
]
```

**Dùng settings trong CSS variables:**
```liquid
<!-- Trong layout/theme.liquid <head> -->
<style>
  :root {
    --color-primary: {{ settings.color_primary }};
    --color-secondary: {{ settings.color_secondary }};
    --color-bg: {{ settings.color_bg }};
    --color-text: {{ settings.color_text }};
    --font-heading: {{ settings.heading_font.family }}, {{ settings.heading_font.fallback_families }};
    --font-body: {{ settings.body_font.family }}, {{ settings.body_font.fallback_families }};
    --page-width: {{ settings.page_width }}px;
  }
</style>

{{ settings.heading_font | font_face }}
{{ settings.body_font | font_face }}
```

---

### 📅 Ngày 12 — Locales, Ôn tập & Best Practices tổng quan

> 📚 **Docs cần đọc:**
> | Section | Nav Item | Link |
> |---|---|---|
> | KEY CONCEPTS | **Locales** | [/themes/architecture/locales](https://shopify.dev/docs/storefronts/themes/architecture/locales) |
> | BEST PRACTICES | **Overview** | [/themes/best-practices](https://shopify.dev/docs/storefronts/themes/best-practices) |
>
> Đọc lướt Best Practices Overview để có cái nhìn tổng quan — sẽ đi sâu từng phần vào ngày sau.

**Locales (i18n):**

```json
// locales/en.default.json
{
  "general": {
    "404": {
      "title": "Page not found",
      "body": "The page you were looking for doesn't exist."
    }
  },
  "products": {
    "product": {
      "add_to_cart": "Add to cart",
      "sold_out": "Sold out",
      "quantity": "Quantity",
      "price": "Price"
    }
  },
  "cart": {
    "general": {
      "title": "Your cart",
      "empty": "Your cart is empty"
    }
  }
}
```

```liquid
<!-- Dùng trong Liquid -->
<h1>{{ 'general.404.title' | t }}</h1>
<button>{{ 'products.product.add_to_cart' | t }}</button>
```

**Tạo skeleton hoàn chỉnh:**
```bash
# Cấu trúc cần build xong sau ngày 12
sections/
├── announcement-bar.liquid
├── header.liquid
├── footer.liquid
├── hero-banner.liquid      ← xong ngày 9
├── featured-collection.liquid
├── rich-text.liquid
└── feature-columns.liquid  ← xong ngày 10
```

> ✅ **Checkpoint cuối Giai đoạn 3**: Tạo được theme với sections drag-and-drop trong Theme Editor.

---

## 📄 GIAI ĐOẠN 4 — Xây dựng các trang cốt lõi
### ⏱️ Thời gian: 7 ngày | Ngày 13–19

---

### 📅 Ngày 13–14 — Header & Footer hoàn chỉnh

> 📚 **Docs cần đọc:**
> | Section | Nav Item | Link |
> |---|---|---|
> | KEY CONCEPTS | **Sections** (review) | [/themes/architecture/sections](https://shopify.dev/docs/storefronts/themes/architecture/sections) |
> | LIQUID API | **Objects** → `linklists`, `routes`, `shop` | [/api/liquid/objects](https://shopify.dev/docs/api/liquid/objects) |
>
> Tra cứu objects: `linklists`, `routes`, `request`, `cart`

**`sections/header.liquid`** — Section phức tạp nhất:
```liquid
<header class="site-header {% if section.settings.sticky_header %}site-header--sticky{% endif %}">
  <div class="container">
    
    <!-- Logo -->
    <a href="{{ routes.root_url }}" class="site-header__logo">
      {%- if section.settings.logo -%}
        <img src="{{ section.settings.logo | image_url: height: 60 }}"
          alt="{{ shop.name | escape }}" height="60">
      {%- else -%}
        {{ shop.name }}
      {%- endif -%}
    </a>
    
    <!-- Main Nav -->
    <nav class="site-nav" aria-label="Main navigation">
      <ul class="site-nav__list">
        {%- for link in linklists[section.settings.menu].links -%}
          <li class="site-nav__item {% if link.links.size > 0 %}has-dropdown{% endif %}">
            <a href="{{ link.url }}" 
               class="site-nav__link {% if link.active %}is-active{% endif %}"
               {% if link.current %}aria-current="page"{% endif %}>
              {{ link.title }}
            </a>
            {%- if link.links.size > 0 -%}
              <ul class="site-nav__dropdown">
                {%- for child in link.links -%}
                  <li><a href="{{ child.url }}">{{ child.title }}</a></li>
                {%- endfor -%}
              </ul>
            {%- endif -%}
          </li>
        {%- endfor -%}
      </ul>
    </nav>
    
    <!-- Actions: Search, Cart, Account -->
    <div class="site-header__actions">
      <a href="{{ routes.cart_url }}" class="btn-icon site-header__cart"
         aria-label="{{ 'cart.general.title' | t }} ({{ cart.item_count }})">
        {% render 'icon', name: 'cart' %}
        {%- if cart.item_count > 0 -%}
          <span class="cart-count">{{ cart.item_count }}</span>
        {%- endif -%}
      </a>
      <button class="btn-icon site-header__menu-toggle" id="mobile-menu-toggle" aria-label="Menu">
        {% render 'icon', name: 'menu' %}
      </button>
    </div>
  </div>
</header>

{% schema %}
{
  "name": "Header",
  "settings": [
    { "type": "image_picker", "id": "logo", "label": "Logo" },
    { "type": "range", "id": "logo_width", "label": "Logo width", "min": 50, "max": 250, "step": 10, "unit": "px", "default": 120 },
    { "type": "link_list", "id": "menu", "label": "Navigation menu", "default": "main-menu" },
    { "type": "checkbox", "id": "sticky_header", "label": "Sticky header", "default": true }
  ]
}
{% endschema %}
```

---

### 📅 Ngày 15–16 — Product Page (Trang quan trọng nhất)

> 📚 **Docs cần đọc:**
> | Section | Nav Item | Link |
> |---|---|---|
> | KEY CONCEPTS | **Templates** (review product) | [/themes/architecture/templates](https://shopify.dev/docs/storefronts/themes/architecture/templates) |
> | LIQUID API | **Objects** → `product`, `variant`, `image` | [/api/liquid/objects](https://shopify.dev/docs/api/liquid/objects) |
>
> Tra cứu kỹ: `product.options_with_values`, `product.selected_or_first_available_variant`, `product.images`, `variant.available`

**Product page snippet quan trọng — Variant Selector + Add to Cart:**
```liquid
<!-- Add to Cart Form -->
<form action="{{ routes.cart_add_url }}" method="post" id="product-form">
  <input type="hidden" name="form_type" value="product">
  <input type="hidden" name="utf8" value="✓">
  
  {%- unless product.has_only_default_variant -%}
    {%- for option in product.options_with_values -%}
      <div class="product-option">
        <label>{{ option.name }}: <span id="selected-{{ option.position }}">{{ option.selected_value }}</span></label>
        <div class="option-buttons" role="radiogroup" aria-label="{{ option.name }}">
          {%- for value in option.values -%}
            <button type="button"
              class="option-btn {% if option.selected_value == value %}is-selected{% endif %}"
              data-option="{{ option.position }}"
              data-value="{{ value | escape }}"
              aria-pressed="{{ option.selected_value == value }}">
              {{ value }}
            </button>
          {%- endfor -%}
        </div>
      </div>
    {%- endfor -%}
  {%- endunless -%}
  
  <input type="hidden" name="id" id="selected-variant-id"
    value="{{ product.selected_or_first_available_variant.id }}">
  
  <button type="submit" id="add-to-cart-btn" class="btn btn--primary btn--full"
    {% unless product.selected_or_first_available_variant.available %}disabled{% endunless %}>
    {%- if product.selected_or_first_available_variant.available -%}
      {{ 'products.product.add_to_cart' | t }} — {{ product.selected_or_first_available_variant.price | money }}
    {%- else -%}
      {{ 'products.product.sold_out' | t }}
    {%- endif -%}
  </button>
</form>

<script type="application/json" id="product-variants-json">
  {{ product.variants | json }}
</script>
```

---

### 📅 Ngày 17 — Collection Page

> 📚 **Docs cần đọc:**
> | Section | Nav Item | Link |
> |---|---|---|
> | LIQUID API | **Objects** → `collection`, `filter`, `sort_option` | [/api/liquid/objects](https://shopify.dev/docs/api/liquid/objects) |
> | LIQUID API | **Tags** → `paginate` | [/api/liquid/tags](https://shopify.dev/docs/api/liquid/tags) |

Áp dụng kiến thức từ ngày 5 (pagination) và ngày 6 (snippets) để build collection page với filter, sort, và grid.

---

### 📅 Ngày 18 — Cart Page

> 📚 **Docs cần đọc:**
> | Section | Nav Item | Link |
> |---|---|---|
> | LIQUID API | **Objects** → `cart`, `line_item` | [/api/liquid/objects](https://shopify.dev/docs/api/liquid/objects) |
> | LIQUID API | **Objects** → `routes` | [/api/liquid/objects](https://shopify.dev/docs/api/liquid/objects) |

Dùng `cart.items`, `item.final_price`, `item.final_line_price`, `cart.total_price`, `routes.cart_url`.

---

### 📅 Ngày 19 — Search, 404 & các trang còn lại

> 📚 **Docs cần đọc:**
> | Section | Nav Item | Link |
> |---|---|---|
> | LIQUID API | **Objects** → `search`, `predictive_search` | [/api/liquid/objects](https://shopify.dev/docs/api/liquid/objects) |
> | KEY CONCEPTS | **Templates** (review customers/) | [/themes/architecture/templates](https://shopify.dev/docs/storefronts/themes/architecture/templates) |

Build: `templates/search.liquid`, `templates/404.liquid`, `templates/page.liquid`

> ✅ **Checkpoint cuối Giai đoạn 4**: Tất cả các trang cốt lõi đã build xong và hoạt động.

---

## ⚡ GIAI ĐOẠN 5 — JavaScript, Ajax API & Performance
### ⏱️ Thời gian: 5 ngày | Ngày 20–24

---

### 📅 Ngày 20–21 — JavaScript cơ bản cho Shopify

> 📚 **Docs cần đọc:**
> | Section | Nav Item | Link |
> |---|---|---|
> | AJAX API | **Ajax API** | [/api/ajax](https://shopify.dev/docs/api/ajax) |
> | BEST PRACTICES | **JavaScript and stylesheet tags** | [/themes/best-practices](https://shopify.dev/docs/storefronts/themes/best-practices) |
> | BEST PRACTICES | **Events and actions** | [/themes/best-practices](https://shopify.dev/docs/storefronts/themes/best-practices) |
>
> Đọc kỹ: `/cart/add.js`, `/cart/change.js`, `/cart/update.js`, `/cart.js` endpoints

**Cart Module cơ bản:**
```javascript
const Cart = {
  async get() {
    return (await fetch('/cart.js')).json();
  },
  async add(variantId, quantity = 1) {
    const res = await fetch('/cart/add.js', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ id: variantId, quantity })
    });
    if (!res.ok) throw new Error((await res.json()).description);
    return res.json();
  },
  async update(key, quantity) {
    return (await fetch('/cart/change.js', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ id: key, quantity })
    })).json();
  },
  async remove(key) { return this.update(key, 0); }
};
```

**Variant Switching:**
```javascript
class ProductForm {
  constructor(form) {
    this.form = form;
    this.variants = JSON.parse(
      document.getElementById('product-variants-json')?.textContent || '[]'
    );
    this.form.querySelectorAll('[data-option]').forEach(btn =>
      btn.addEventListener('click', () => this.onOptionChange(btn))
    );
    this.form.addEventListener('submit', (e) => this.onSubmit(e));
  }
  
  onOptionChange(btn) {
    // Update UI, find variant, update price/button/URL
  }
  
  async onSubmit(e) {
    e.preventDefault();
    // Ajax add to cart
  }
}
```

---

### 📅 Ngày 22–23 — Section Rendering API & Cart Drawer

> 📚 **Docs cần đọc:**
> | Section | Nav Item | Link |
> |---|---|---|
> | AJAX API | **Section Rendering API** | [/api/ajax/section-rendering](https://shopify.dev/docs/api/ajax/section-rendering) |
>
> Đọc kỹ: `?sections=...` parameter, response format, bundled requests

```javascript
// Render lại section mà không reload
async function refreshSection(sectionId) {
  const res = await fetch(`/?sections=${sectionId}`);
  const data = await res.json();
  document.querySelector(`[data-section-id="${sectionId}"]`).innerHTML = data[sectionId];
}
```

---

### 📅 Ngày 24 — Performance, Accessibility & Theme Check

> 📚 **Docs cần đọc:**
> | Section | Nav Item | Link |
> |---|---|---|
> | BEST PRACTICES | **Performance** | [/themes/best-practices/performance](https://shopify.dev/docs/storefronts/themes/best-practices/performance) |
> | BEST PRACTICES | **Accessibility** | [/themes/best-practices/accessibility](https://shopify.dev/docs/storefronts/themes/best-practices/accessibility) |
> | BEST PRACTICES | **File transformation** | [/themes/best-practices](https://shopify.dev/docs/storefronts/themes/best-practices) |
> | DEVELOPER TOOLS | **Theme Check** | [/themes/tools/theme-check](https://shopify.dev/docs/storefronts/themes/tools/theme-check) |
>
> ⚠️ Ngày quan trọng — Performance & a11y quyết định chất lượng theme.

**Performance checklist:**
```liquid
<!-- ✅ Lazy load ảnh dưới fold -->
<img loading="lazy" ...>

<!-- ✅ Hero image: eager + preload -->
<img loading="eager" fetchpriority="high" ...>
<link rel="preload" as="image" href="{{ section.settings.image | image_url: width: 1920 }}">

<!-- ✅ Defer JS -->
<script defer src="{{ 'theme.js' | asset_url }}"></script>

<!-- ✅ Dùng image_url với đúng width -->
{{ image | image_url: width: 400 }}
```

**Accessibility checklist:**
- Skip-to-content link
- Semantic HTML (`<nav>`, `<main>`, `<header>`, `<footer>`)
- ARIA labels trên interactive elements
- Keyboard navigation
- Color contrast ≥ 4.5:1
- Focus states visible

```bash
# Validate
shopify theme check
shopify theme check --auto-correct
```

> ✅ **Checkpoint cuối Giai đoạn 5**: Add to cart Ajax, cart drawer, `theme check` pass.

---

## 🚀 GIAI ĐOẠN 6 — Dự án tổng hợp: Build Theme "Minimal Commerce"
### ⏱️ Thời gian: 10–14 ngày | Ngày 25–38

### Roadmap dự án

> 📚 **Docs tham khảo trong suốt dự án:**
> | Section | Nav Item | Khi nào dùng |
> |---|---|---|
> | BEST PRACTICES | **Design** | Ngày 34–35 (CSS/animations) |
> | BEST PRACTICES | **Deceptive code** | Ngày 36 (review code) |
> | BEST PRACTICES | **Merchant stores** | Ngày 38 (deployment) |
> | DEVELOPER TOOLS | **Theme Check** | Ngày 36 (validation) |
> | THEME STORE | **Theme Store** | Ngày 38 (bonus — nếu muốn publish) |

```
Tuần 1 (Ngày 25-31): Foundation
  ├── Day 25: shopify theme init, settings_schema.json, layout/theme.liquid
  ├── Day 26: header.liquid + mobile nav
  ├── Day 27: footer.liquid + announcement bar
  ├── Day 28: hero-banner.liquid + featured-collection.liquid
  ├── Day 29: product.liquid (markup hoàn chỉnh)
  ├── Day 30: collection.liquid + search.liquid
  └── Day 31: cart.liquid + 404.liquid

Tuần 2 (Ngày 32-38): Polish & Ship
  ├── Day 32: JavaScript — add to cart, variant switching
  ├── Day 33: Cart drawer (Ajax + Section Rendering)
  ├── Day 34: CSS — responsive design (mobile-first) ← đọc Best Practices > Design
  ├── Day 35: CSS — animations & transitions ← đọc Best Practices > Design
  ├── Day 36: shopify theme check — fix all errors ← đọc Best Practices > Deceptive code
  ├── Day 37: Lighthouse audit & performance fixes
  └── Day 38: Deploy & test end-to-end ← đọc Best Practices > Merchant stores
```

### Deliverables cuối cùng

- [ ] Theme chạy được trên dev store
- [ ] Tất cả trang hoạt động đúng
- [ ] Add to cart không reload trang
- [ ] Cart count update realtime
- [ ] Responsive trên mobile (375px+)
- [ ] `shopify theme check` — 0 errors
- [ ] Lighthouse Performance ≥ 75
- [ ] Theme Editor: 5+ settings để tùy chỉnh

---

## 📊 Bảng theo dõi tiến độ

| Ngày | Nội dung | Docs chính | ✓ |
|------|---------|------------|---|
| 1 | Setup môi trường | GETTING STARTED > Overview, Quick start; DEV TOOLS > CLI | ☐ |
| 2 | Theme init & workflow | GETTING STARTED > Developer preview; DEV TOOLS > GitHub; BEST PRACTICES > Version control | ☐ |
| 3 | Liquid Objects | LIQUID API > Objects | ☐ |
| 4 | Liquid Filters | LIQUID API > Filters | ☐ |
| 5 | Liquid Tags | LIQUID API > Tags | ☐ |
| 6 | Snippets & Render | KEY CONCEPTS > Snippets | ☐ |
| 7 | Liquid tổng hợp | LIQUID API (review) | ☐ |
| 8 | Layout & Templates | KEY CONCEPTS > Architecture, Limits, Layouts, Templates | ☐ |
| 9 | Sections cơ bản | KEY CONCEPTS > Sections | ☐ |
| 10 | Blocks & Section groups | KEY CONCEPTS > Section groups, Blocks; BEST PRACTICES > Sections and blocks | ☐ |
| 11 | Global Settings | KEY CONCEPTS > Settings, Config; BEST PRACTICES > Theme editor | ☐ |
| 12 | Locales & ôn tập | KEY CONCEPTS > Locales; BEST PRACTICES > Overview | ☐ |
| 13–14 | Header & Footer | Liquid Objects: `linklists`, `routes` | ☐ |
| 15–16 | Product Page | Liquid Objects: `product`, `variant`, `image` | ☐ |
| 17 | Collection Page | Liquid Objects: `collection`, `filter`; Tags: `paginate` | ☐ |
| 18 | Cart Page | Liquid Objects: `cart`, `line_item` | ☐ |
| 19 | Search, 404 | Liquid Objects: `search`; Templates review | ☐ |
| 20–21 | JavaScript | AJAX API; BEST PRACTICES > JavaScript and stylesheet tags, Events and actions | ☐ |
| 22–23 | Ajax & Cart Drawer | AJAX API > Section Rendering API | ☐ |
| 24 | Performance & a11y | BEST PRACTICES > Performance, Accessibility, File transformation; DEV TOOLS > Theme Check | ☐ |
| 25–31 | Dự án: Foundation | Tất cả KEY CONCEPTS (tham khảo) | ☐ |
| 32–35 | Dự án: Polish | BEST PRACTICES > Design | ☐ |
| 36–38 | Dự án: Ship | BEST PRACTICES > Deceptive code, Merchant stores; DEV TOOLS > Theme Check | ☐ |

---

> 💡 **Mẹo quan trọng**: Sau mỗi ngày học, commit code lên GitHub với message rõ ràng.  
> Ví dụ: `feat: add hero-banner section with image/text settings`  
> Đây vừa giúp backup, vừa tạo portfolio đẹp cho nhà tuyển dụng xem.
