# 📚 Tổng Hợp Bộ Agent Skills & Subagents cho Antigravity

Tài liệu tổng hợp toàn bộ **24 Skills**, **4 Subagents** và các **Slash Commands** thuộc bộ [addyosmani/agent-skills](https://github.com/addyosmani/agent-skills).

---

## ⚡ Các Lệnh Nhanh (Slash Commands)

Bạn có thể gõ trực tiếp các lệnh này vào ô chat của Antigravity để kích hoạt nhanh quy trình tương ứng:

| Lệnh (Command) | Skill / Subagent Kích Hoạt | Công Dụng Chính |
| :--- | :--- | :--- |
| `/spec` | `spec-driven-development` | Lập bản mô tả kĩ thuật (Spec/PRD) chuẩn chỉnh trước khi viết code |
| `/planning` | `planning-and-task-breakdown` | Phân rã yêu cầu thành các công việc nhỏ, có thứ tự rõ ràng |
| `/build` | `incremental-implementation` | Thực thi cài đặt từng task một cách tuần tự và bài bản |
| `/test` | `test-driven-development` | Chạy quy trình TDD (Red $\rightarrow$ Green $\rightarrow$ Refactor) |
| `/review` | `code-review-and-quality` / `code-reviewer` | Đánh giá toàn diện mã nguồn theo 5 tiêu chí chất lượng |
| `/code-simplify` | `code-simplification` | Đơn giản hóa và refactor code giúp loại bỏ độ phức tạp dư thừa |
| `/webperf` | `web-performance-auditor` | Audit hiệu năng Web, tốc độ tải trang và chỉ số Core Web Vitals |
| `/ship` | `shipping-and-launch` | Rà soát checklist an toàn và chuẩn bị phát hành lên Production |

---

## 🛠️ Chi Tiết 24 Agent Skills

### 1. Nhóm Định Hình & Lập Kế Hoạch (Define & Plan)

| Tên Skill | Mã / Từ Khóa Kích Hoạt | Công Dụng |
| :--- | :--- | :--- |
| **`idea-refine`** | `"ideate"`, `"refine this idea"` | Tinh chỉnh các ý tưởng thô thành kế hoạch sắc bén thông qua tư duy phân kỳ và hội tụ. |
| **`interview-me`** | `"interview me"`, `"grill me"` | Phỏng vấn từng câu hỏi một để làm rõ 95% mong muốn thực sự của người dùng trước khi code. |
| **`spec-driven-development`** | `/spec` | Ép buộc tạo spec/PRD rõ ràng, loại bỏ sự mơ hồ trước khi triển khai. |
| **`planning-and-task-breakdown`** | `/planning` | Chia nhỏ bài toán lớn thành các task atomic, dễ đo lường và theo dõi tiến độ. |
| **`context-engineering`** | Trình tạo session / Rules | Cấu hình và duy trì ngữ cảnh tối ưu, giúp AI hiểu đúng luật dự án. |

---

### 2. Nhóm Lập Trình & Thiết Kế Kiến Trúc (Build & Architecture)

| Tên Skill | Mã / Từ Khóa Kích Hoạt | Công Dụng |
| :--- | :--- | :--- |
| **`api-and-interface-design`** | Thiết kế API / Type contracts | Hướng dẫn thiết kế REST/GraphQL API ổn định, chuẩn hóa type giữa Frontend & Backend. |
| **`incremental-implementation`** | `/build` | Triển khai tính năng theo từng lát nhỏ (incremental), tránh sửa quá nhiều file cùng lúc. |
| **`test-driven-development`** | `/test` | Điều khiển phát triển bằng kiểm thử (viết test trước, viết code sau). |
| **`frontend-ui-engineering`** | Xây dựng UI / Component | Tạo giao diện đẹp mắt (modern aesthetics), responsive, đạt chuẩn truy cập WCAG. |
| **`source-driven-development`** | Đọc tài liệu framework | Căn cứ mọi dòng code theo tài liệu chính thức (Official Docs) mới nhất của thư viện. |
| **`code-simplification`** | `/code-simplify` | Refactor làm sạch code, loại bỏ đoạn trùng lặp giúp code dễ đọc mà không đổi hành vi. |

---

### 3. Nhóm Kiểm Thử & Chẩn Đoán Lỗi (Test & Debug)

| Tên Skill | Mã / Từ Khóa Kích Hoạt | Công Dụng |
| :--- | :--- | :--- |
| **`debugging-and-error-recovery`** | Runtime Error / Test fail | Chẩn đoán lỗi bài bản từ vết log (stack trace), tìm nguyên nhân gốc rễ thay vì sửa ngọn. |
| **`browser-testing-with-devtools`** | Chrome DevTools MCP | Kiểm thử giao diện và ứng dụng Web trực tiếp trên trình duyệt thật (DOM, Console, Network). |
| **`code-review-and-quality`** | `/review` | Review mã nguồn trên 5 trục: *Tính đúng đắn, Độ dễ đọc, Kiến trúc, Bảo mật, Hiệu năng*. |
| **`doubt-driven-development`** | Rủi ro cao / Production | Phản biện đối kháng (Adversarial review) mọi quyết định lớn để tránh sai sót đắt giá. |

---

### 4. Nhóm Bảo Mật & Tối Ưu Hiệu Năng (Security & Performance)

| Tên Skill | Mã / Từ Khóa Kích Hoạt | Công Dụng |
| :--- | :--- | :--- |
| **`security-and-hardening`** | Xử lý User input / Auth | Quét và gia cố mã nguồn chống lại các lỗ hổng phổ biến (XSS, SQLi, Auth leak, CSRF). |
| **`performance-optimization`** | App bị chậm / N+1 query | Tối ưu hiệu năng toàn diện trên Frontend, Backend, Query Database và Caching. |
| **`observability-and-instrumentation`**| Thêm Log / Metrics | Thêm hệ thống theo dõi (logging, metrics, tracing) để quan sát khi chạy thật trên Server. |

---

### 5. Nhóm Triển Khai & Quy Trình (Deploy & Workflow)

| Tên Skill | Mã / Từ Khóa Kích Hoạt | Công Dụng |
| :--- | :--- | :--- |
| **`shipping-and-launch`** | `/ship` | Đánh giá pre-launch checklist, xây dựng chiến lược Rollback và kế hoạch phát hành. |
| **`ci-cd-and-automation`** | Setup CI/CD | Tự động hóa đường ống CI/CD, tự động chạy test và quality gates trước khi merge. |
| **`deprecation-and-migration`** | Xóa API cũ / Di chuyển | Quản lý quy trình loại bỏ hệ thống cũ và chuyển đổi dữ liệu/người dùng an toàn. |
| **`documentation-and-adrs`** | Thay đổi architecture / API | Ghi chép các quyết định kiến trúc (ADR) giúp lưu lại lịch sử thiết kế dự án. |
| **`git-workflow-and-versioning`** | Git commit / Tag / Release | Chuẩn hóa thông điệp commit, đánh số phiên bản Semantic Versioning và xuất changelog. |
| **`using-agent-skills`** | Điều hướng tự động | Meta-skill chịu trách nhiệm phát hiện nhu cầu của bạn để gọi đúng skill phù hợp. |

---

## 🤖 4 Subagents Chuyên Biệt (Specialized Personas)

Bạn có thể gọi riêng các trợ lý AI chuyên môn này trong phiên làm việc:

| Tên Subagent | Mã Kích Hoạt | Vai Trò & Công Dụng |
| :--- | :--- | :--- |
| **`code-reviewer`** | `Role: code-reviewer` | Senior Reviewer chịu trách nhiệm soi kỹ từng thay đổi code trước khi merge. |
| **`security-auditor`** | `Role: security-auditor` | Kỹ sư an ninh mạng chuyên kiểm tra an toàn thông tin và đánh giá rủi ro. |
| **`test-engineer`** | `Role: test-engineer` | Kỹ sư QA chuyên thiết kế kịch bản test và gia tăng độ bao phủ (coverage). |
| **`web-performance-auditor`** | `Role: web-performance-auditor` | Chuyên gia tối ưu Web, chuyên xử lý tốc độ tải trang và chỉ số Core Web Vitals. |
