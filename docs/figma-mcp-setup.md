# 🔌 Kết nối Figma cho Claude Code (MCP Server)

## Trước tiên — vì sao cần làm lại?

Trước đó có 1 MCP server tên `figma` được cấu hình sẵn trong `~/.claude.json`, nhưng **chưa từng hoạt động** vì 2 lý do:

1. **Sai tên gói**: config trỏ tới `@modelcontextprotocol/server-figma` — gói này **không tồn tại** trên npm (không phải MCP chính thức nào). `npx` luôn báo lỗi 404 → server tự tắt ngay khi khởi động.
2. **Token đã bị lộ**: token dùng để cấu hình đã bị dán trực tiếp vào khung chat với Claude (2 lần) — dù còn hạn sử dụng, token đó vẫn nên coi là không an toàn vì đã lưu trong lịch sử chat.

→ Đã **xoá sạch** cấu hình `figma` cũ (cả token) khỏi `~/.claude.json`. Làm lại theo hướng dẫn dưới đây — lần này **bạn tự chạy lệnh, tự nhập token trên máy mình**, token sẽ không đi qua khung chat với Claude nữa.

## MCP là gì? (giải thích ngắn)

MCP (Model Context Protocol) là cách Claude Code "cắm" thêm công cụ ngoài — ở đây là 1 chương trình nhỏ (chạy qua `npx`) biết cách gọi Figma API thay cho việc gõ `curl` thủ công. Có kết nối MCP thì Claude có thể tự đọc file Figma qua các lệnh có sẵn (list file, lấy node, xuất ảnh...) mà không cần bạn tạo token rồi dán vào chat mỗi lần.

## Bước 1 — Tạo token mới trên Figma

1. Vào Figma → **Settings** (icon avatar góc trên) → tab **Security**.
2. Kéo tới mục **Personal access tokens** → **Generate new token**.
3. Ở popup **"Generate new token"**: chọn **Expiration** (thời hạn) và tick đúng **Scopes** cần dùng — xem giải thích chi tiết từng option ngay dưới đây.
4. Copy token hiện ra (dạng `figd_...`) — **chỉ hiện đúng 1 lần**, đóng popup là mất.

> ⚠️ Lưu ý: 2 token cũ đã dùng trong chat trước đây nên **thu hồi (revoke)** luôn ở trang Security này nếu chưa hết hạn, tránh để token chết vẫn tồn tại.

### 🕐 Expiration (thời hạn token)

Dropdown chọn số ngày token còn sống (VD `90 days` → hết hạn 15/11/2026). Hết hạn thì token ngừng hoạt động, phải tạo token mới — không tự gia hạn được. Chọn `90 days` là hợp lý cho việc dùng thử/học tập; nếu dùng lâu dài cho project thật thì cân nhắc `No expiration` (nếu Figma cho chọn) và nhớ tự thu hồi khi không dùng nữa.

### 🔐 Scopes (quyền truy cập) — giải thích từng nhóm

Token càng ít scope càng an toàn (nguyên tắc *least privilege* — chỉ cấp đúng quyền cần dùng). Dưới đây là toàn bộ option trong popup:

| Nhóm | Scope | Ý nghĩa | Có cần cho việc đọc design không? |
|---|---|---|---|
| **Users** | `current_user:read` | Đọc tên, email, ảnh đại diện của chính bạn | ❌ Không cần |
| **Files** | `file_comments:read` | Đọc comment trong file | ❌ Không cần |
| | `file_comments:write` | Tạo/sửa/xoá comment trong file | ❌ Không cần |
| | **`file_content:read`** | **Đọc nội dung file + render ảnh từ file** (đây chính là API mình đã dùng: lấy cấu trúc frame, màu, font, xuất ảnh) | ✅ **BẮT BUỘC** |
| | `file_metadata:read` | Đọc metadata file (tên, ngày sửa...) | ⚠️ Nên có — hỗ trợ thêm, không bắt buộc |
| | `file_versions:read` | Đọc lịch sử version của file | ❌ Không cần (trừ khi muốn xem lịch sử chỉnh sửa) |
| **Design systems** | `library_assets:read` | Đọc data về từng component/style riêng lẻ | ❌ Không cần (trừ khi làm việc với Design System library) |
| | `library_content:read` | Đọc component/style được publish từ 1 file | ❌ Không cần |
| | `team_library_content:read` | Đọc component/style publish ở cấp team library | ❌ Không cần |
| **Development** | `file_dev_resources:read` | Đọc dev resource (link code, docs gắn vào design) | ❌ Không cần |
| | `file_dev_resources:write` | Tạo/sửa dev resource | ❌ Không cần |
| **Folders** | `folders:read` | Đọc cấu trúc thư mục của team | ❌ Không cần |
| **Webhooks** | `webhooks:read` | Đọc danh sách webhook | ❌ Không cần |
| | `webhooks:write` | Tạo/sửa/xoá webhook | ❌ Không cần |

**→ Khuyến nghị: chỉ tick đúng 1 ô `file_content:read`** (tick thêm `file_metadata:read` cũng được, không bắt buộc). Các quyền `write` (comments, dev_resources, webhooks) **không nên bật** cho mục đích chỉ đọc thiết kế — bật thừa quyền `write` nếu token lỡ lộ sẽ rủi ro cao hơn hẳn (kẻ xấu có thể sửa/xoá comment, tạo webhook...).

## Bước 2 — Thêm MCP server bằng command (tự chạy trên Terminal của bạn)

> ⚠️ **Sửa lại so với bản trước**: bản đầu ghi `-s project` là **sai** — scope `project` của `claude mcp` lưu vào file `.mcp.json` ngay tại thư mục hiện tại, file này thường được **commit vào git** để chia sẻ cho team. Nếu bạn đang đứng trong repo `shopify-liquid` lúc chạy lệnh, token sẽ có nguy cơ bị đẩy thẳng lên GitHub. Đã đổi sang `-s local` (đúng với scope mà config `figma` cũ trước đây thực sự dùng — verify lúc gỡ nó, `claude mcp remove` báo rõ *"Removed MCP server "figma" from **local** config"*).

### ⚠️ Quan trọng — phải `cd` đúng thư mục trước khi chạy lệnh

Scope `local` lưu trong `~/.claude.json`, và Claude Code **khoá theo đúng đường dẫn thư mục (cwd) tại thời điểm bạn chạy lệnh** — không phải theo repo git. Project thật Claude Code dùng cho session của bạn là:

```
/Users/dawn/Documents/workspace
```
**KHÔNG PHẢI** `shopify-liquid/` hay bất kỳ thư mục con nào bên trong (bài học y hệt lỗi từng gặp khi đặt hook nhầm ở `shopify-liquid/.claude/` — xem [continue-here.md](continue-here.md)). Nếu bạn chạy lệnh trong lúc `cd` vào `shopify-liquid/`, config sẽ bị lưu vào 1 project-key khác (`/Users/dawn/Documents/workspace/shopify-liquid`), và Claude Code (mở ở `workspace/`) **sẽ không thấy** server này.

Luôn `cd` về đúng thư mục gốc trước:

```bash
cd /Users/dawn/Documents/workspace
```

Rồi mới chạy lệnh thêm server, thay `TOKEN_MOI_CUA_BAN` bằng token vừa copy ở Bước 1:

```bash
claude mcp add figma -s local -e FIGMA_API_KEY=TOKEN_MOI_CUA_BAN -- npx -y figma-developer-mcp --stdio
```

> ⚠️ **KHÔNG gõ dấu `<` `>`** quanh token — đó chỉ là ký hiệu placeholder ("chỗ này thay bằng..."), không phải cú pháp thật. Trong shell, `<`/`>` là ký tự redirect file, gõ kèm sẽ báo lỗi `no such file or directory`. Chỉ thay đúng chuỗi `TOKEN_MOI_CUA_BAN` bằng token thật, không thêm ký tự gì khác. Cũng để ý gõ đúng tên gói **`figma-developer-mcp`** (đủ chữ "er"), gõ thiếu sẽ lại gặp lỗi 404 như gói sai trước đây.

> ⚠️ **Sửa lại lần nữa**: tên biến môi trường đúng là **`FIGMA_API_KEY`**, không phải `FIGMA_PERSONAL_ACCESS_TOKEN` như bản trước — verify bằng cách tự chạy `npx figma-developer-mcp --stdio` thủ công, gói báo rõ: *"Either FIGMA_API_KEY or FIGMA_OAUTH_TOKEN is required"*. Đây là tên biến riêng của gói `figma-developer-mcp`, không phải chuẩn chung cho mọi MCP Figma.

Giải thích từng phần:
| Phần | Ý nghĩa |
|---|---|
| `figma` | Tên server tự đặt (tuỳ ý, dùng lại tên này để nhất quán) |
| `-s local` | Scope: lưu vào `~/.claude.json`, khoá theo đúng thư mục (`/Users/dawn/Documents/workspace`) bạn đang đứng lúc chạy lệnh — **không** ghi vào file nào trong git repo, an toàn hơn `project` |
| `-e FIGMA_API_KEY=...` | Biến môi trường chứa token — đúng tên biến gói `figma-developer-mcp` yêu cầu (xem `--help` của gói: `--figma-api-key`) |
| `-- npx -y figma-developer-mcp --stdio` | Lệnh thực sự chạy server — dùng gói **`figma-developer-mcp`** (Framelink, chính chủ, còn maintain), khác với gói sai `@modelcontextprotocol/server-figma` trước đây |

## Bước 3 — Kiểm tra kết nối

```bash
claude mcp list
```

Phải thấy dòng:
```
figma: npx -y figma-developer-mcp --stdio - ✔ Connected
```

Nếu vẫn báo lỗi kết nối, chạy lệnh sau để xem log lỗi thật (thay token vào):

```bash
FIGMA_API_KEY=TOKEN_MOI_CUA_BAN npx -y figma-developer-mcp --stdio
```

## Bước 4 — Dùng thử trong Claude Code

Khởi động lại session Claude Code (hoặc gõ `/mcp` để nạp lại), sau đó paste link Figma và nhờ Claude đọc — lúc này Claude sẽ gọi qua MCP server thay vì phải xin token qua chat.

## Gỡ bỏ nếu không cần nữa

```bash
claude mcp remove figma
```

---
**Tóm tắt bảo mật**: token không bao giờ nên dán trực tiếp vào tin nhắn chat với Claude — luôn dùng `claude mcp add ... -e KEY=value` chạy trên Terminal của chính bạn như hướng dẫn trên, token chỉ nằm trong config local máy bạn.
