# Spec: Thiết lập Môi trường Phát triển Shopify Theme (Developer Environment Setup)

## Objective
Thiết lập và xác minh toàn bộ bộ công cụ phát triển trên môi trường macOS cục bộ phục vụ việc lập trình, xem trước (dev preview) và đẩy (deploy) Shopify Theme theo [Lộ trình Chi tiết Ngày 1](file:///Users/dawn/Documents/workspace/shopify-liquid/shopify_theme_roadmap_detail.md#L132-L163).

---

## Tech Stack & Tooling

| Công cụ / Thư viện | Phiên bản yêu cầu | Trạng thái hiện tại | Công dụng |
|---|---|---|---|
| **Node.js** | $\ge$ v18.0.0 | `v22.14.0` (Đã sẵn sàng) | Runtime môi trường JavaScript |
| **npm** | $\ge$ v10.0.0 | `10.9.2` (Đã sẵn sàng) | Trình quản lý gói phụ thuộc |
| **Shopify CLI** (`@shopify/cli`) | $\ge$ 3.0.0 | `4.5.2` (Đã sẵn sàng) | Công cụ dòng lệnh tương tác với Shopify Theme & Store |
| **IDE Development Tool** | Antigravity IDE | Antigravity IDE | IDE tích hợp sẵn AI Agent, Terminal, File Editor & Syntax Highlighting |

---

## Commands

Các câu lệnh kiểm tra và thao tác môi trường:

```bash
# 1. Kiểm tra phiên bản Node.js & npm
node --version
npm --version

# 2. Kiểm tra phiên bản Shopify CLI
shopify version

# 3. Đăng nhập vào Shopify Store (Chuẩn bị cho Ngày 2)
shopify auth login --store your-dev-store.myshopify.com

# 4. Chạy Theme Dev Server (Chuẩn bị cho Ngày 2)
shopify theme dev --store your-dev-store.myshopify.com
```

---

## Antigravity IDE Capabilities

Antigravity IDE đã tích hợp sẵn đầy đủ các công cụ phục vụ lập trình Shopify Theme mà không cần cài đặt thêm extensions:

1. **Liquid & Code Editor**: Hỗ trợ mở, xem và chỉnh sửa file `.liquid`, `.json`, `.css`, `.js` trực tiếp với syntax highlighting và preview.
2. **Integrated Terminal**: Chạy trực tiếp các lệnh Shopify CLI (`shopify theme dev`, `shopify auth login`...).
3. **Built-in Git & Diff**: Xem lịch sử commit, diff thay đổi code và quản lý nhánh Git trực quan.
4. **Agent Skills & Tools**: Đã được trang bị bộ 24 Agent Skills (như `planning-and-task-breakdown`, `spec-driven-development`, `frontend-ui-engineering`...) để hỗ trợ tự động hoá công việc.

---

## Boundaries

- **Always (Luôn làm):**
  - Chạy `shopify version` để xác nhận CLI không bị đứt gãy sau mỗi lần update.
  - Sử dụng phiên bản Node.js LTS ($\ge$ v18).
- **Ask first (Hỏi trước):**
  - Cài đặt thêm các package npm toàn cục (`-g`) ngoài danh mục lộ trình.
- **Never (Không bao giờ):**
  - Dùng `sudo npm install -g` nếu không cần thiết (tránh hỏng phân quyền file hệ thống).

---

## Success Criteria & Verification

- [x] Node.js đạt phiên bản $\ge$ v18.0.0 (`v22.14.0`).
- [x] npm đạt phiên bản $\ge$ v10.0.0 (`10.9.2`).
- [x] Shopify CLI hoạt động bình thường (`v4.5.2`).
- [x] Antigravity IDE sẵn sàng làm môi trường phát triển chính (không phụ thuộc VS Code).
