# 📍 Continue Here — Trạng thái hiện tại & Bài tập đang làm

> File này để lưu context lúc dừng lại, pull về máy khác là làm tiếp được ngay không cần hỏi lại từ đầu.

---

## 🗺️ Tiến độ chung

- **`my-first-theme/`** — đã hoàn thành đầy đủ **Ngày 1–12** (lý thuyết + bài tập theo roadmap). Toàn bộ kiến thức đã tổng hợp trong [docs/shopify-liquid-summary.md](shopify-liquid-summary.md) (mục lục Cmd+F ở đầu file). Bài tập dở dang `sections/header.liquid` (Ngày 13, Bước 1 — HTML tĩnh dropdown) ở theme này **coi như dừng lại, không tiếp tục nữa** — đã quyết định chuyển hẳn Ngày 13 trở đi sang project mới bên dưới.
- **`ecommerce-theme/`** — theme Shopify **độc lập, mới tinh** (chạy `shopify theme init`, chưa custom gì — mới ở dạng skeleton mặc định, 39 file, `theme check` sạch). Đây là nơi tiếp tục học **Ngày 13 trở đi**, đồng thời là project thật: **clone lại thiết kế Figma** bên dưới.
- File rule agent: [.agents/AGENTS.md](../.agents/AGENTS.md) — gõ `/process` khi muốn agent tự sửa code, `/git-push`/`/git-pull` khi muốn agent chạy git không cần hỏi permission.

---

## 🎨 Dự án đang làm: Clone Figma "SHOP.CO" E-commerce Template

**Link Figma**: https://www.figma.com/design/AXFvzD9Zu9A2xkNwOItGWL/E-commerce-Website-Template--Freebie---Community-?node-id=0-1

> ⚠️ Agent không tự mở được link Figma — cần cung cấp lại screenshot nếu agent cần xem lại thiết kế ở máy khác.

### Các trang cần build (đã phân tích từ ảnh Figma)

| Trang | Thành phần chính |
|---|---|
| **Homepage** | Header (logo + nav + search + cart), Hero "Find Clothes That Matches Your Style" + banner brand logos, "New Arrivals"/"Top Selling" (product grid + rating), "Browse by Dress Style" (grid ảnh categories), "Our Happy Customers" (testimonial), Newsletter banner đen, Footer |
| **Product Detail Page** | Gallery ảnh (thumbnail dọc), giá + rating + review count, chọn màu/size, quantity + Add to cart, tab Reviews (rating breakdown + list), "You might also like" |
| **Category Page** | Sidebar Filters (dropdown Casual, Price slider, Colors, Size, Dress Style), product grid + pagination |
| **Cart** | Danh sách item (ảnh + size/color + số lượng), Order Summary (subtotal/discount/delivery/total), promo code input |

### Component dùng chung — PHẢI build 1 lần, tái sử dụng (đã nhận diện ở Bước 0)

| Component | Xuất hiện ở |
|---|---|
| **Product Card** (ảnh + tên + rating sao + giá + giá gạch) | Homepage (New Arrivals, Top Selling), Category Page, Product Detail (You might also like) |
| **Rating sao** (★★★★☆) | Product Card, trang Review |
| **Badge giảm giá %** | Product Card |
| **Newsletter banner đen** | Cuối mọi trang |
| **Footer** | Mọi trang |

---

## 📋 Quy trình đã thống nhất — làm theo đúng thứ tự này, KHÔNG nhảy cóc

1. ~~Bước 0 — Phân tích Figma tìm component dùng chung~~ ✅ Đã xong (bảng trên).
2. **Bước 1 — Thiết lập Design System** (màu/font/spacing từ Figma → `config/settings_schema.json` + `snippets/css-variables.liquid`, đúng kỹ thuật Ngày 11) — **CHƯA LÀM, làm tiếp từ đây**.
3. Bước 2 — Dựng khung `layout/theme.liquid` (content_for_header/layout, thứ tự load CSS đúng rule Ngày 2).
4. Bước 3 — Build trước các Snippet dùng chung (`product-card`, `rating-stars`...) — dùng kỹ thuật `render for as` (Ngày 6).
5. Bước 4 — Build từng trang theo đúng thứ tự roadmap: **Header/Footer (13-14) → Homepage → Product Page (15-16) → Category (17) → Cart (18)**. Mỗi section vẫn theo phương pháp đã luyện: **HTML tĩnh (dữ liệu giả) → thay dần bằng Liquid động → thêm `{% schema %}` cuối cùng, thêm dần từng setting một, test bằng Theme Editor sau mỗi lần thêm**.

### Lỗi/bài học đã rút ra, nhớ áp dụng tiếp
- Luôn chạy `shopify theme check` sau mỗi thay đổi — đã từng bắt được lỗi locale key sai (`cart.general.title` không tồn tại, phải dùng đúng `cart.title` khớp `en.default.json`).
- Đối chiếu `id` giữa nơi khai báo setting và nơi dùng biến CSS — dễ gõ nhầm (VD Ngày 11: `buttno_corner_radius` vs `button_corner_radius`).
- Các file trong theme (`my-first-theme` lẫn `ecommerce-theme`) đều có thể **xoá sạch viết lại từ đầu** khi thực hành — không cần giữ nguyên code skeleton gốc (agent đã ghi nhớ điều này trong memory, không cảnh báo khi thấy file bị ghi đè toàn bộ).

---

## ✅ Việc cần làm ngay khi tiếp tục
Bắt đầu từ **Bước 1** — lấy màu sắc (đen/trắng + accent) và font chữ từ ảnh Figma "SHOP.CO", thêm vào `ecommerce-theme/config/settings_schema.json` + sinh CSS variable trong `ecommerce-theme/snippets/css-variables.liquid` (file này **đã có sẵn từ skeleton** với vài biến mặc định — chỉnh/thêm tiếp theo đúng màu Figma, không cần tạo file mới).
