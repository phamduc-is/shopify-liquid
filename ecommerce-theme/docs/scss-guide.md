# 🎨 SCSS Cheatsheet — Lệnh & Cú Pháp Thường Dùng

Tài liệu tra cứu nhanh khi quên lệnh/cú pháp SCSS. Xem [project-structure.md](project-structure.md) cho cấu trúc folder & luồng dữ liệu design token.

---

## ⚙️ 1. Lệnh npm (chạy trong thư mục `ecommerce-theme/`)

```bash
# Cài dependency (chỉ cần 1 lần, hoặc sau khi clone máy mới / pull code mới)
npm install

# Build 1 lần — output ra assets/theme.css (minified)
npm run build:css

# Build tự động mỗi khi save file .scss — CHẠY SONG SONG với `shopify theme dev`
npm run watch:css
```

> ⚠️ **Lưu ý quan trọng**: sửa file `.scss` xong mà **quên chạy `build:css` / không bật `watch:css`** → `assets/theme.css` **không tự cập nhật**. Đây là lỗi hay quên nhất — luôn để 1 terminal chạy `npm run watch:css` khi đang code style.

---

## 📁 2. Cấu trúc file `.scss` (partial)

- File bắt đầu bằng `_` (VD `_reset.scss`) = **partial** — không tự compile ra file CSS riêng, phải được 1 file khác `@use` vào.
- `main.scss` (không có `_`) = **entry point** — file duy nhất Sass thật sự compile, gom hết mọi partial qua `@use`.

```scss
// main.scss
@use 'base/reset';           // → tìm file src/scss/base/_reset.scss
@use 'components/button';    // → tìm file src/scss/components/_button.scss
```

---

## 🧩 3. Cú pháp SCSS hay dùng

### `@use` — import file khác (thay cho `@import` cũ, đã deprecated)
```scss
@use 'abstracts/variables' as vars;   // đặt alias "vars" để gọi $vars.ten-bien
@use 'abstracts/mixins' as mix;       // gọi @include mix.ten-mixin(...)
```

### Biến (`$variable`) — chỉ dùng cho thứ KHÔNG có ở CSS custom property
```scss
$breakpoint-md: 768px;   // dùng được trong @media (var() không dùng được ở đây)
```
> Màu/font/radius **không** khai báo `$variable` ở đây — dùng thẳng `var(--color-foreground)` (đã setup từ `settings_schema.json` + `css-variables.liquid`).

### Nesting — lồng selector con vào cha
```scss
.card {
  padding: 16px;

  &__title {        // & = nối trực tiếp → compile ra .card__title
    font-weight: 700;
  }

  &:hover {         // & = nối pseudo-class → compile ra .card:hover
    opacity: 0.9;
  }
}
```

### Mixin — khối code CSS tái sử dụng, có thể nhận tham số
```scss
// Khai báo (trong _mixins.scss)
@mixin flex-center($direction: row) {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: $direction;
}

// Dùng lại (trong file component bất kỳ)
.button {
  @include mix.flex-center;             // dùng default (row)
  @include mix.flex-center(column);     // truyền tham số khác
}
```

### `@media` qua mixin (breakpoint) — xem `abstracts/_mixins.scss`
```scss
.grid {
  grid-template-columns: 1fr;

  @include mix.respond-to('md') {
    grid-template-columns: repeat(3, 1fr);
  }
}
```

### Toán tử & nội suy chuỗi
```scss
width: math.div(100%, 3);      // phép chia — Dart Sass mới bắt buộc dùng math.div(), không dùng / trực tiếp
content: 'icon-#{$name}';       // #{} = interpolation, chèn biến vào giữa string
```

---

## ✅ Quy tắc riêng của project này (đừng quên)

| Loại giá trị | Định nghĩa ở đâu | Dùng trong `.scss` như nào |
|---|---|---|
| Màu, font, border-radius | `config/settings_schema.json` → `snippets/css-variables.liquid` | `var(--color-foreground)` |
| Breakpoint | `abstracts/_variables.scss` (bắt buộc SCSS vì `@media` không đọc `var()`) | `@include mix.respond-to('md') { ... }` |
| z-index | `abstracts/_variables.scss` | `z-index: vars.$z-modal;` |