# 📓 Ghi Chú Kiến Thức Shopify Theme Development

Tài liệu lưu trữ các kiến thức quan trọng, quy tắc kiến trúc và kinh nghiệm lập trình Shopify Theme trong quá trình học.

---

## 1. 🎨 Kiến trúc CSS & Lý do phân chia 3 loại file CSS trong `<head>`

Trong cấu trúc theme Shopify hiện đại (Shopify OS 2.0 / Dawn Architecture), CSS được phân chia làm 3 thành phần riêng biệt trong `<head>` của `layout/theme.liquid`:

```liquid
<head>
  {% # 1. Inline CSS Variables %}
  {% render 'css-variables' %}

  {% # 2. Critical CSS (Preload) %}
  {{ 'critical.css' | asset_url | stylesheet_tag: preload: true }}

  {% # 3. Custom Theme CSS %}
  {{ 'theme.css' | asset_url | stylesheet_tag }}
</head>
```

### 🔍 Chi tiết vai trò & Lý do phân chia:

| Tên File / Snippet | Loại File & Vị trí | Vai trò & Mục đích | Lý do tại sao phải phân chia |
|---|---|---|---|
| **`css-variables.liquid`** | Snippet Liquid (`snippets/`) <br>*(Inline `<style>` trực tiếp)* | Khai báo các **CSS Custom Properties** (Variables) trên `:root` và nạp `@font-face`. | • **Chạy Liquid**: Cần mã `.liquid` để đọc tùy chỉnh từ Theme Settings (như `settings.background_color`, `settings.type_primary_font`).<br>• **Chống FOUC**: Inlined trực tiếp đầu `<head>` giúp trình duyệt có sẵn biến màu/font ngay lập tức, tránh hiện tượng nhấp nháy giao diện. |
| **`critical.css`** | CSS File (`assets/`) <br>*(Nạp bằng `preload: true`)* | Chứa CSS Reset (`box-sizing`, `margin: 0`), cấu trúc khung `body`, và Grid layout chính. | • **Tối ưu Core Web Vitals (LCP/CLS)**: Dung lượng cực nhẹ (vài KB), cho phép trình duyệt vẽ ngay lập tức khung trang (Above the Fold) trong vài miligiây đầu tiên trước khi tải tài nguyên khác. |
| **`theme.css`** | CSS File (`assets/`) <br>*(Nạp sau `critical.css`)* | Chứa các style tùy chỉnh chi tiết, màu sắc trang trí, hiệu ứng hover, component... | • **Dễ bảo trì (Modular Code)**: Tách riêng giao diện custom khỏi bộ khung hệ thống.<br>• **Thứ tự ghi đè (Cascade)**: Phải đặt **SAU** `critical.css` để các thuộc tính tự định nghĩa (`body`, font, color) ghi đè thành công lên style mặc định. |

---

### ⚠️ Quy tắc quan trọng về Thứ tự Cascade trong CSS:
Khi nhúng file `theme.css` tùy chỉnh, **luôn nhúng SAU `critical.css`**. Nếu nhúng trước `critical.css`, các thuộc tính cùng độ ưu tiên (như `body { font-family: ... }`) trong `theme.css` sẽ bị `critical.css` ghi đè hoàn toàn.

```text
Thứ tự thực thi trong <head>:
1. css-variables.liquid  ---> Nạp các biến Màu & Font từ Shopify Admin (Inline)
        │
2. critical.css          ---> Vẽ khung lưới & Layout nền tảng (Preload)
        │
3. theme.css             ---> Đè style tùy chỉnh & trang trí chi tiết lên trên
```
