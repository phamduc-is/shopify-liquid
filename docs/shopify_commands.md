# 🛠️ Shopify Theme Development — Cheatsheet Các Câu Lệnh Thường Dùng

Tài liệu tổng hợp toàn bộ các câu lệnh Shopify CLI (`@shopify/cli`) và thao tác dòng lệnh thiết yếu cho lập trình viên phát triển giao diện (Shopify Theme Developer).

---

## 🔑 1. Đăng Nhập & Quản Lý Tài Khoản (Authentication)

| Câu lệnh | Mô tả & Công dụng |
|---|---|
| `shopify auth login` | Đăng nhập tài khoản Shopify CLI qua trình duyệt |
| `shopify auth logout` | Đăng xuất tài khoản hiện tại |
| `shopify auth whoami` | Kiểm tra tài khoản và cửa hàng đang kết nối hiện tại |

---

## ⚡ 2. Chạy Dev Server Xem Trước (Dev Preview)

> 💡 **Lưu ý**: Cần đứng ở thư mục gốc của theme (ví dụ: thư mục `dawn/` hoặc theme của bạn) khi chạy các lệnh này.

```bash
# 1. Chạy Dev Server mặc định (CLI sẽ tự hỏi chọn store)
shopify theme dev

# 2. Chạy Dev Server chỉ định đích danh cửa hàng (KHUYÊN DÙNG)
shopify theme dev --store dawn-jwz4ezbj.myshopify.com

# 3. Chạy Dev Server với cổng (Port) cố định
shopify theme dev --store dawn-jwz4ezbj.myshopify.com --port 9292

# 4. Chạy Dev Server tắt tự động reload (Live Reload off)
shopify theme dev --store dawn-jwz4ezbj.myshopify.com --no-live-reload
```

> 🌐 Khi lệnh chạy thành công, mở trình duyệt tại: **`http://127.0.0.1:9292`**

---

## 🚀 3. Đẩy & Tải Mã Nguồn Theme (Push & Pull)

### 📤 Push (Tải từ Local lên Shopify Admin)
```bash
# Đẩy theme dưới máy lên danh sách Theme nháp (Development Theme / Unpublished)
shopify theme push --store dawn-jwz4ezbj.myshopify.com

# Đẩy theme lên và XUẤT BẢN làm giao diện chính thức (Live/Published Theme) - CẨN THẬN
shopify theme push --store dawn-jwz4ezbj.myshopify.com --live

# Đẩy đè lên một Theme ID cụ thể
shopify theme push --store dawn-jwz4ezbj.myshopify.com --theme <THEME_ID>
```

### 📥 Pull (Tải từ Shopify Admin về Local)
```bash
# Tải theme từ cửa hàng về thư mục máy cục bộ
shopify theme pull --store dawn-jwz4ezbj.myshopify.com

# Tải theme Live chính thức từ store về
shopify theme pull --store dawn-jwz4ezbj.myshopify.com --live

# Tải một Theme ID cụ thể từ store về
shopify theme pull --store dawn-jwz4ezbj.myshopify.com --theme <THEME_ID>

# Force pull ngay 1 file cụ thể (VD settings_data.json sau khi Save trong Theme Editor)
# Dùng khi không muốn chờ auto 2-way sync của `theme dev` (có thể delay vài chục giây)
shopify theme pull --theme <THEME_ID> --only=config/settings_data.json
```

---

## 📋 4. Quản Lý Danh Sách Theme trên Store

```bash
# Hiển thị tất cả các Theme đang có trên cửa hàng (bao gồm Live, Unpublished, Dev themes)
shopify theme list --store dawn-jwz4ezbj.myshopify.com

# Xóa một theme nháp trên cửa hàng theo ID
shopify theme delete --store dawn-jwz4ezbj.myshopify.com --theme <THEME_ID>

# Xuất bản (Publish) một theme nháp thành theme chính thức
shopify theme publish --store dawn-jwz4ezbj.myshopify.com --theme <THEME_ID>
```

---

## 🆕 5. Khởi Tạo Theme Mới (Init)

```bash
# Tạo một theme mới từ khung chuẩn (Skeleton theme)
shopify theme init my-new-theme

# Tạo một theme mới bằng cách clone từ một git repository khác
shopify theme init my-theme --clone-url https://github.com/Shopify/dawn.git
```

---

## 🔍 6. Kiểm Trả Lỗi Code & Chuẩn Hóa (Theme Check & Quality)

```bash
# Kiểm tra lỗi cấu trúc, cú pháp Liquid, hiệu năng & bảo mật của theme
shopify theme check

# Tự động sửa các lỗi nhỏ có thể tự fix được
shopify theme check --auto-correct
```

---

## 💡 Mẹo & Thủ Thuật Thường Dùng (Pro Tips)

1. **Xem thông tin phiên bản CLI:**
   ```bash
   shopify version
   ```
2. **Xóa Cache CLI khi bị lỗi kết nối/đăng nhập:**
   ```bash
   shopify auth logout
   # Sau đó chạy đăng nhập lại:
   shopify auth login
   ```
3. **Chạy theme dev với dữ liệu ngẫu nhiên (Hot Reload mượt hơn):**
   Đảm bảo Development Store của bạn được chọn tùy chọn **"Start with test data"** khi tạo trên Partner Dashboard.
