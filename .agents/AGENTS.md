# 🤖 Rule cho AI Agent — Shopify Theme Development

Tài liệu này quy định các nguyên tắc làm việc, tiêu chuẩn viết code và quy trình tương tác dành cho AI Agent khi hỗ trợ phát triển dự án Shopify Theme.

---

## ⚡ 1. Quy Tắc Tương Tác & Lệnh Nhanh (Interaction & Slash Commands)

1. **Điều kiện thực thi code (`/process`):**
   - Agent **KHÔNG ĐƯỢC TỰ ĐỘNG THỰC HÀNH / SỬA CODE** đối với bất kỳ yêu cầu nào nếu câu lệnh của người dùng **chưa có tiền tố hoặc dòng lệnh `/process`**.
   - Khi chưa có `/process`, Agent chỉ thảo luận, phân tích hoặc phỏng vấn làm rõ.
2. **Đánh dấu câu hỏi kiến thức (`/qs`):**
   - Người dùng sẽ dùng `/qs` để đánh dấu các câu hỏi thắc mắc kiến thức. Agent giải thích chi tiết, trực quan kèm ví dụ và cập nhật file ghi chú khi được yêu cầu.
3. **Tự động kích hoạt `interview-me`:**
   - Đối với **mỗi lần nhận prompt từ người dùng**, Agent **TỰ ĐỘNG KÍCH HOẠT SKILL `interview-me`**: phỏng vấn từng câu một để khai thác và làm rõ chính xác 95% mong muốn của người dùng trước khi tiến hành bước tiếp theo.
4. **Tự động khôi phục quy tắc với `/rule`:**
   - Khi người dùng gõ lệnh hoặc nhắc tới **`/rule`**, Agent **TỰ ĐỘNG ĐỌC VÀ KÍCH HOẠT LẠI TOÀN BỘ QUY TẮC** trong file [.agents/AGENTS.md](file:///Users/dawn/Documents/workspace/shopify-liquid/.agents/AGENTS.md) để đảm bảo không bỏ sót bất kỳ nguyên tắc tương tác và tiêu chuẩn viết code nào.
5. **Toàn quyền thực thi (Full Permission), trừ Git:**
   - Với mọi tool/lệnh **không liên quan tới git**, Agent được **toàn quyền chạy ngay, không cần hỏi xác nhận permission**.
   - Với các lệnh **liên quan tới git** (`git add`, `git commit`, `git push`, `git pull`, `git merge`, `git reset`,...), Agent **KHÔNG được tự ý chạy toàn quyền** — chỉ được phép chạy trực tiếp không hỏi khi người dùng có gõ kèm từ khóa kích hoạt dạng `/git-push`, `/git-pull`, `/git-commit`,... trong prompt của lượt đó.
   - Nếu prompt của người dùng không chứa từ khóa `/git-*` tương ứng, Agent vẫn phải xin xác nhận trước khi chạy lệnh git đó, kể cả khi đang ở chế độ toàn quyền.
6. **Timeline thay đổi code ngay trong chat:**
   - Sau **mỗi lần tác động vào code** (sửa/xoá/tạo file, dù qua `/process` hay do người dùng tự làm rồi agent verify), Agent phải **in tóm tắt diff ngay trong câu trả lời** — liệt kê rõ dòng/đoạn nào **thêm (+)**, **xoá (-)**, **sửa (~)**, kèm tên file.
   - Lý do: **Local History của VSCode không bắt được các lần agent sửa file trực tiếp** (chỉ bắt lần Save thủ công qua UI editor) — nên không dùng Timeline/Local History của VSCode làm nguồn xem lại lịch sử sửa code được. Bản tóm tắt trong chat là nguồn thay thế đáng tin cậy.
   - Không cần tạo file log riêng, không tự động git commit cho mục đích này (git vẫn theo đúng rule mục 5 — chỉ commit khi có `/git-commit`).

---

## 🎯 2. Nguyên Tắc Tổng Quan (Core Rules)

1. **Ngôn ngữ phản hồi:** Trả lời người dùng bằng **Tiếng Việt**, sử dụng thuật ngữ kỹ thuật chính xác, định dạng Markdown rõ ràng, trực quan.
2. **Căn cứ tài liệu chính thức (Source-Driven):** Mọi cú pháp Liquid, lệnh Shopify CLI, cấu trúc Section/Block đều tuân thủ chuẩn mới nhất từ Shopify Official Documentation (Shopify OS 2.0).
3. **Xác minh trước khi kết thúc:** Luôn chạy kiểm tra cú pháp (`shopify theme check --path my-first-theme`) trước khi thông báo hoàn thành công việc.

---

## 🎨 3. Quy Tắc Kiến Trúc CSS & Layout

1. **Thứ tự nạp CSS trong `<head>` (Bắt buộc):**
   ```liquid
   {% # 1. Inline CSS Variables & Font Face %}
   {% render 'css-variables' %}

   {% # 2. Critical CSS (Preload - Vẽ khung giao diện ban đầu) %}
   {{ 'critical.css' | asset_url | stylesheet_tag: preload: true }}

   {% # 3. Custom Theme CSS (Ghi đè style chi tiết - Luôn nạp SAU critical.css) %}
   {{ 'theme.css' | asset_url | stylesheet_tag }}
   ```
2. **Biến CSS toàn cục:** Định nghĩa biến màu sắc, typography trong `snippets/css-variables.liquid` hoặc `:root` của `theme.css`, tránh hardcode giá trị màu rải rác.

---

## 💻 4. Quy Tắc Viết Code Liquid & Theme

1. **Định dạng Liquid:**
   - Sử dụng thẻ `{% liquid ... %}` cho các khối logic dài để code gọn gàng.
   - Thẻ hiển thị dữ liệu dùng `{{ ... }}` với khoảng trắng chuẩn: `{{ product.title }}`.
   - Luôn sử dụng Liquid filter `money` hoặc `money_with_currency` đối với các giá trị giá sản phẩm (tránh hiển thị dạng cents nguyên).
2. **LiquidDoc Documentation:**
   - Mọi file `snippet` tạo mới hoặc chỉnh sửa phức tạp đều cần có khối `{% doc %}` ở đầu file để mô tả `@param` và `@example`.
3. **Cấu trúc Thư mục Theme:**
   - Giữ nguyên cấu trúc chuẩn: `assets/`, `config/`, `layout/`, `locales/`, `sections/`, `snippets/`, `templates/`.
   - Không tự ý tạo thư mục ngoài danh mục tiêu chuẩn của Shopify Theme.

---

## 📝 5. Lưu Trữ Kiến Thức & Ghi Chú Dự Án

1. **File ghi chú cá nhân:** Khi người dùng yêu cầu lưu kiến thức đã học, tự động cập nhật và duy trì nội dung trong file [docs/my-first-theme-notes.md](file:///Users/dawn/Documents/workspace/shopify-liquid/docs/my-first-theme-notes.md).
2. **Trình bày rõ ràng:** Trình bày kiến thức bằng dạng bảng, sơ đồ Mermaid hoặc đoạn code minh họa để dễ tra cứu lại.
