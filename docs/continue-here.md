# 📍 Continue Here — Trạng thái hiện tại & Bài tập đang làm

> File này để lưu context lúc dừng lại, pull về máy khác là làm tiếp được ngay không cần hỏi lại từ đầu.

---

## 🗺️ Tiến độ chung

Đang học tới **Ngày 11 — Global Settings, Config & Theme Editor** (`config/settings_schema.json`, `settings.xxx`, `font_picker`, `{% style %}`).

- Ngày 1–10: đã hoàn thành, tổng hợp đầy đủ lý thuyết + code mẫu trong [docs/shopify-liquid-summary.md](shopify-liquid-summary.md) (mục lục ở đầu file, Cmd+F theo tên ngày).
- Ngày 8/10/11: roadmap không có mini-project riêng — bài tập do agent tự đề xuất thêm để thực hành (không phải nội dung gốc của roadmap).
- File rule của agent: [.agents/AGENTS.md](../.agents/AGENTS.md) — nhớ gõ `/process` khi muốn agent tự sửa code, `/git-push` (hoặc `/git-pull`...) khi muốn agent chạy lệnh git không cần hỏi permission.

---

## 🎯 Bài tập đang làm (Ngày 11) — CHƯA XONG, làm tiếp ở đây

**Mục tiêu**: Thêm 1 setting mới vào Theme Settings (`config/settings_schema.json`) → sinh CSS variable trong `snippets/css-variables.liquid` → áp dụng vào nút "Thêm Nhanh" đã có sẵn trong `product-card.liquid`.

### Bước 1 — Thêm nhóm mới vào [config/settings_schema.json](../my-first-theme/config/settings_schema.json)
Thêm object này vào **cuối mảng** (nhớ dấu `,` sau `}` của nhóm `"t:general.colors"` phía trước):
```json
{
  "name": "Buttons",
  "settings": [
    {
      "type": "color",
      "id": "button_bg_color",
      "label": "Button background",
      "default": "#2980b9"
    },
    {
      "type": "color",
      "id": "button_text_color",
      "label": "Button text",
      "default": "#ffffff"
    },
    {
      "type": "range",
      "id": "button_corner_radius",
      "label": "Button corner radius",
      "min": 0,
      "max": 20,
      "step": 1,
      "unit": "px",
      "default": 4
    }
  ]
}
```

### Bước 2 — Thêm CSS variable vào [snippets/css-variables.liquid](../my-first-theme/snippets/css-variables.liquid)
Thêm 3 dòng vào bên trong `:root { ... }`:
```liquid
--color-button-bg: {{ settings.button_bg_color }};
--color-button-text: {{ settings.button_text_color }};
--button-corner-radius: {{ settings.button_corner_radius }}px;
```

### Bước 3 — Áp dụng vào nút Quick Add trong [snippets/product-card.liquid](../my-first-theme/snippets/product-card.liquid)
Tìm đoạn:
```liquid
<button
  class="product-card__quick-add"
  style="margin-top: 10px; width: 100%; padding: 8px; background: #2980b9; color: #fff; border: none; border-radius: 4px; cursor: pointer;"
  data-product-id="{{ product.id }}"
>
```
Đổi phần `style` thành:
```liquid
style="margin-top: 10px; width: 100%; padding: 8px; background: var(--color-button-bg); color: var(--color-button-text); border: none; border-radius: var(--button-corner-radius); cursor: pointer;"
```

### Bước 4 — Test
```bash
cd my-first-theme
shopify theme dev
```
Vào **Customize** → **Theme settings** (KHÔNG phải setting riêng của 1 section) → tìm tab **"Buttons"** → đổi màu/bo góc → xem nút "Thêm Nhanh" đổi theo thời gian thực.

### Lỗi hay gặp cần soi lại khi review
- Quên dấu `,` giữa 2 object trong mảng ngoài cùng của `settings_schema.json`.
- `id` viết lệch giữa `settings_schema.json` và `css-variables.liquid` (VD `button_bg_color` vs `button_bg`).
- Quên đổi `var(--...)` đúng tên biến trong `product-card.liquid`.

---

## ✅ Khi làm xong bài tập này
1. Chạy `shopify theme check` xác nhận không lỗi.
2. Gọi agent review lại 3 file đã sửa (theo đúng flow đã làm ở Ngày 9/10).
3. Tiếp tục **Ngày 12 — Locales, Ôn tập & Best Practices tổng quan** (roadmap: [docs/shopify_theme_roadmap_detail.md:1247](shopify_theme_roadmap_detail.md#L1247)).
