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
2. ~~Bước 1 — Thiết lập Design System~~ ✅ Đã xong — màu (9 màu: background/foreground/text_secondary/border/surface_secondary/sale_badge_bg+text/rating_star/success) + font (heading: Archivo Black, primary: Work Sans) đã vào `config/settings_schema.json`, sinh CSS variable trong `snippets/css-variables.liquid`, verify qua Theme Editor thật + `theme check` sạch (39 file, 0 lỗi). Tiện tay fix luôn 1 bug JSON có sẵn từ skeleton (trailing comma ở `locales/en.default.schema.json`).
3. ~~Bước 2 — Dựng khung `layout/theme.liquid`~~ ✅ Đã xong — thứ tự đúng rule Ngày 2: `content_for_header` (đầu `<head>`, để theme CSS load sau ghi đè được style app) → `css-variables` → font preload → `critical.css` (preload) → `theme.css` (SCSS build ra, không preload) → `meta-tags`. Tiện thể setup xong tooling **SCSS** (`src/scss/` theo 7-1 pattern, `npm run build:css`/`watch:css`, output ra `assets/theme.css`) — xem chi tiết [ecommerce-theme/docs/project-structure.md](../ecommerce-theme/docs/project-structure.md).
4. ~~Bước 3 — Build trước các Snippet dùng chung~~ ✅ Đã xong — `snippets/product-card.liquid` (ảnh + tên + rating sao + giá + giá gạch + badge giảm giá %) và `snippets/rating-stars.liquid` (2 lớp sao `__track`/`__fill` overlay theo `fill_percent`), CSS riêng trong `src/scss/components/product-card.scss` + `rating-stars.scss`. `theme check` sạch (43 file, 0 lỗi thật) — 2 warning `OrphanedSnippet` (`product-card`, `rating-stars`) là bình thường ở giai đoạn này (xem mục bài học dưới), sẽ tự hết khi Bước 4 render snippet vào 1 section thật. 2 việc để-sau có chủ đích (KHÔNG phải sót, đã cân nhắc khi review):
   - `.rating-stars__track` và `.rating-stars__fill` đều `position: absolute` nên container `.rating-stars` không tự có kích thước từ 2 lớp sao (chỉ còn div "X/5" trong flow) → khi ráp vào Collection/Product Card thật sẽ set `width`/`height` cố định cho container; nếu lúc đó thấy sao đè lên chữ rating thì quay lại sửa ở `src/scss/components/_rating-stars.scss`.
   - Class `rating-stars_value` (1 gạch dưới) lệch BEM so với `__track`/`__fill` (2 gạch dưới) — gộp sửa chung 1 lượt dọn naming ở cuối project, không sửa riêng lẻ bây giờ.
5. **Bước 4 — Build từng trang theo đúng thứ tự roadmap: Header/Footer (13-14) → Homepage → Product Page (15-16) → Category (17) → Cart (18)** — **ĐANG LÀM, tiếp tục từ Header**. Mỗi section vẫn theo phương pháp đã luyện: **HTML tĩnh (dữ liệu giả) → thay dần bằng Liquid động (dùng kỹ thuật `render for as` — Ngày 6 — để loop `product-card` qua danh sách sản phẩm) → thêm `{% schema %}` cuối cùng, thêm dần từng setting một, test bằng Theme Editor sau mỗi lần thêm**.

   **Tiến độ `sections/header.liquid` (đang làm):**
   - ✅ HTML tĩnh: announcement-bar + header (logo, nav, search, cart/account icon) — 4 icon SVG mới: `assets/icon-menu.svg`, `icon-search.svg`, `icon-close.svg`, `icon-chevron-down.svg`.
   - ✅ Utility `.container`/`.container-fullwidth` mới — `src/scss/base/_container.scss`, dùng chung cho mọi section sau này (không chỉ header): `max-width: calc(1240px + var(--page-margin) * 2)` để trừ `padding-inline` xong content vẫn đúng 1240px theo Figma (do `box-sizing: border-box` toàn theme).
   - ✅ Liquid động — phần bind vào object có sẵn (không cần schema): cart icon (`cart.item_count` + badge `.header__cart-count`), account icon (`routes.account_url`), search form (`<form action="{{ routes.search_url }}" method="get">` + `name="q"` + `value="{{ search.terms }}"`).
   - ⏳ Nav — **đã đưa code loop `section.settings.menu.links` cho user paste vào**, cần xác nhận đã paste vào `sections/header.liquid` chưa trước khi làm schema (lần trước ghi nhầm "đã viết" nhưng thực tế chỉ nằm trong chat, chưa vào file — đã phát hiện và sửa lại đúng).
   - ❌ Announcement-bar text/link — vẫn hardcode HTML tĩnh, chưa đổi (chủ đích, để làm cùng lúc với schema).
   - ❌ Chưa làm: `{% schema %}` cho header (setting `menu` kiểu `link_list` + 3 setting announcement-bar) — **làm tiếp từ đây, sau khi xác nhận nav đã có code loop trong file**.
   - ❌ `sections/footer.liquid` — chưa bắt đầu.

### Lỗi/bài học đã rút ra, nhớ áp dụng tiếp
- Luôn chạy `shopify theme check` sau mỗi thay đổi — đã từng bắt được lỗi locale key sai (`cart.general.title` không tồn tại, phải dùng đúng `cart.title` khớp `en.default.json`).
- Đối chiếu `id` giữa nơi khai báo setting và nơi dùng biến CSS — dễ gõ nhầm (VD Ngày 11: `buttno_corner_radius` vs `button_corner_radius`).
- Các file trong theme (`my-first-theme` lẫn `ecommerce-theme`) đều có thể **xoá sạch viết lại từ đầu** khi thực hành — không cần giữ nguyên code skeleton gốc (agent đã ghi nhớ điều này trong memory, không cảnh báo khi thấy file bị ghi đè toàn bộ).
- Warning `OrphanedSnippet` của `theme check` xét theo **reachability xuyên chuỗi từ entry point thật** (layout → section/template → render), không phải "có ai render tên này không" theo kiểu grep phẳng. Nên nếu snippet A gọi `render` snippet B, nhưng A tự nó chưa được section/template nào gọi tới, thì B vẫn bị báo orphan luôn dù code có render B thật (case `product-card` → `rating-stars` ở Bước 3). Không phải bug, tự hết khi A được wire vào 1 section thật.
- `sections/header-group.json` / `footer-group.json` là JSON auto-generated có comment header `/* ... */` — Shopify tự strip trước khi parse nên hợp lệ, nhưng **vẫn phải tự soi trailing comma** vì `theme check` không luôn bắt được (đã gặp ở `header-group.json`, sửa xong).
- Chỉ dùng SCSS mixin khi pattern lặp lại ≥ 2 nơi (VD `respond-to` dùng chung breakpoint toàn theme) — pattern chỉ 1 nơi dùng (VD `flex-center` cho riêng announcement-bar) thì viết thẳng CSS, gọi qua mixin chỉ gây phải nhảy file đọc thêm không cần thiết.
- `box-sizing: border-box` (set global ở `critical.css`) làm `padding-inline` ăn vào `max-width` — muốn content bên trong đúng N px theo Figma thì `max-width: calc(Npx + var(--page-margin) * 2)`, không phải `max-width: Npx` trần.
- Phân loại "Liquid động": bind vào **object có sẵn** (`cart`, `routes`, `search`...) làm được ngay không cần schema; phần cần **setting merchant tự nhập** (menu, text tuỳ chỉnh) phải viết code Liquid trước (tạm rỗng) rồi mới thêm schema sau — đúng thứ tự đã thống nhất, không đảo ngược. Chi tiết pattern `<form method="get">` cho search (vì sao GET không phải POST, `name="q"` là key cố định) đã lưu trong `shopify-liquid-summary.md` (Ngày 13).

---

## ✅ Việc cần làm ngay khi tiếp tục
Header đã xong HTML tĩnh + phần Liquid động bind-object-có-sẵn (cart/account/search). **Việc tiếp theo**: thêm `{% schema %}` cho `sections/header.liquid` — 1 setting `menu` (`link_list`) cho nav + 3 setting cho announcement-bar (`announcement_text`, `announcement_link_url`, `announcement_link_text`), thêm dần từng setting một, test bằng Theme Editor sau mỗi lần thêm (đúng phương pháp đã luyện). Xong header thì làm `sections/footer.liquid` (Ngày 14) theo đúng quy trình 3 bước tương tự. Snippet dùng chung (`product-card`, `rating-stars`) đã có sẵn từ Bước 3, chưa dùng tới — 2 warning `OrphanedSnippet` sẽ tự hết khi Homepage render chúng.

### Quy tắc làm việc mới (từ Bước 2 trở đi)
Agent **không tự sửa code trực tiếp vào file theme** nữa — chỉ **hướng dẫn từng bước** (giải thích cần sửa gì, ở đâu, tại sao), người dùng tự gõ code. Agent chỉ verify (theme check, đọc lại file) sau khi người dùng báo đã làm xong. Ngoại lệ: các file tracking tiến độ (`docs/continue-here.md`) và chạy lệnh CLI kiểm tra/pull vẫn agent tự làm được, không tính là "code vào theme".

---

## 🔧 Setup Claude Code hooks trên máy mới (làm TAY — không tự sync qua git được)

> ⚠️ Config nằm ở `~/Documents/workspace/.claude/` — **NGOÀI** repo `shopify-liquid` này, vì đó mới là project root thật Claude Code dùng (Primary working directory), không phải subfolder `shopify-liquid/`. Đã từng nhầm đặt hook ở `shopify-liquid/.claude/` và phát hiện nó **không hề chạy** (verify bằng flag file `/tmp/claude-git-allow-*` không bao giờ được tạo). Vì `workspace/` không phải git repo nên 2 file dưới đây **không tự đồng bộ qua máy khác** — copy tay theo hướng dẫn cuối mục này.

**Gồm 2 hook:**
1. `/git-push`/`/git-pull`/`/git-commit` keyword-check — enforce đúng rule AGENTS.md mục 1.5 (git cần từ khoá kích hoạt mới chạy thẳng không hỏi).
2. `code-timeline.py` — mỗi lần Edit/Write vào code, tự tính diff thật (`difflib`) và append vào `docs/code-timeline.md` (rule AGENTS.md mục 1.6).

<!-- CLONE BẮT ĐẦU: ~/Documents/workspace/.claude/settings.json -->
```json
{
  "permissions": {
    "allow": [
      "Bash(*)",
      "Read(*)",
      "Write(*)",
      "Edit(*)",
      "Glob(*)",
      "Grep(*)"
    ],
    "ask": [
      "Bash(git *)"
    ]
  },
  "hooks": {
    "UserPromptSubmit": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "INPUT=$(cat); SID=$(echo \"$INPUT\" | jq -r '.session_id'); PROMPT=$(echo \"$INPUT\" | jq -r '.prompt // empty'); FLAG=\"/tmp/claude-git-allow-${SID}\"; if echo \"$PROMPT\" | grep -qE '/git-[a-zA-Z-]+'; then touch \"$FLAG\"; else rm -f \"$FLAG\"; fi",
            "statusMessage": "Checking for /git-* trigger keyword"
          }
        ]
      }
    ],
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "if": "Bash(git *)",
            "command": "INPUT=$(cat); SID=$(echo \"$INPUT\" | jq -r '.session_id'); FLAG=\"/tmp/claude-git-allow-${SID}\"; if [ -f \"$FLAG\" ]; then echo '{\"hookSpecificOutput\":{\"hookEventName\":\"PreToolUse\",\"permissionDecision\":\"allow\",\"permissionDecisionReason\":\"Git trigger keyword (/git-push, /git-pull, ...) detected in prompt\"}}'; else echo '{\"hookSpecificOutput\":{\"hookEventName\":\"PreToolUse\",\"permissionDecision\":\"ask\",\"permissionDecisionReason\":\"No /git-* trigger keyword in prompt\"}}'; fi"
          }
        ]
      },
      {
        "matcher": "Write",
        "hooks": [
          {
            "type": "command",
            "command": "python3 /Users/dawn/Documents/workspace/.claude/hooks/code-timeline.py",
            "statusMessage": "Caching pre-write content for timeline diff"
          }
        ]
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "python3 /Users/dawn/Documents/workspace/.claude/hooks/code-timeline.py",
            "statusMessage": "Logging code diff to docs/code-timeline.md"
          }
        ]
      }
    ]
  }
}
```
<!-- CLONE KẾT THÚC: ~/Documents/workspace/.claude/settings.json -->

<!-- CLONE BẮT ĐẦU: ~/Documents/workspace/.claude/hooks/code-timeline.py -->
```python
#!/usr/bin/env python3
"""
Hook Write|Edit — ghi lại diff thật (code thêm/xoá/sửa) vào docs/code-timeline.md
mỗi lần agent tác động vào 1 file code.

- PreToolUse (matcher: Write)  -> cache nội dung file TRƯỚC khi bị Write ghi đè
                                   (Write không có old_string như Edit, nên phải
                                   tự lưu lại trước khi mất).
- PostToolUse (matcher: Write|Edit) -> tính diff thật (difflib) và append vào log.
  - Edit: diff old_string/new_string có sẵn trong tool_input.
  - Write: đọc lại cache đã lưu ở bước Pre (nếu file mới tạo thì cache rỗng).
"""
import sys
import json
import os
import hashlib
import difflib
import datetime

REPO_ROOT = "/Users/dawn/Documents/workspace/shopify-liquid"
LOG_FILE = os.path.join(REPO_ROOT, "docs", "code-timeline.md")
CACHE_DIR = "/tmp/claude-write-precache"
MAX_DIFF_LINES = 200


def cache_key(file_path):
    return hashlib.sha1(file_path.encode("utf-8")).hexdigest()


def write_diff_log(file_path, old_text, new_text, action):
    diff_lines = list(
        difflib.unified_diff(
            old_text.splitlines(keepends=True),
            new_text.splitlines(keepends=True),
            fromfile="before",
            tofile="after",
        )
    )
    if diff_lines and not diff_lines[-1].endswith("\n"):
        diff_lines[-1] += "\n"
    if not diff_lines:
        return  # không có thay đổi thật (VD Edit no-op) -> không log

    added = sum(1 for l in diff_lines if l.startswith("+") and not l.startswith("+++"))
    removed = sum(1 for l in diff_lines if l.startswith("-") and not l.startswith("---"))
    ts = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")

    body = diff_lines
    truncated_note = ""
    if len(body) > MAX_DIFF_LINES:
        truncated_note = f"\n... ({len(body) - MAX_DIFF_LINES} dòng diff bị cắt, xem file thật để đủ) ...\n"
        body = body[:MAX_DIFF_LINES]

    entry = (
        f"\n### {ts} — {action} `{file_path}`\n"
        f"+{added} / -{removed}\n\n"
        "```diff\n"
        + "".join(body)
        + truncated_note
        + "```\n"
    )

    os.makedirs(os.path.dirname(LOG_FILE), exist_ok=True)
    with open(LOG_FILE, "a", encoding="utf-8") as f:
        f.write(entry)


def main():
    try:
        data = json.load(sys.stdin)
    except Exception:
        return

    tool_name = data.get("tool_name")
    tool_input = data.get("tool_input", {}) or {}
    is_post = "tool_response" in data
    file_path = tool_input.get("file_path")
    if not file_path:
        return

    if tool_name == "Write":
        key = os.path.join(CACHE_DIR, cache_key(file_path))
        if not is_post:
            # PreToolUse: lưu lại nội dung cũ trước khi bị ghi đè
            os.makedirs(CACHE_DIR, exist_ok=True)
            old_content = ""
            if os.path.exists(file_path):
                try:
                    with open(file_path, encoding="utf-8") as f:
                        old_content = f.read()
                except Exception:
                    old_content = ""
            with open(key, "w", encoding="utf-8") as f:
                f.write(old_content)
        else:
            # PostToolUse: lấy lại cache, diff với content mới, rồi xoá cache
            old_content = ""
            if os.path.exists(key):
                with open(key, encoding="utf-8") as f:
                    old_content = f.read()
                os.remove(key)
            new_content = tool_input.get("content", "")
            action = "Tạo file" if old_content == "" else "Ghi đè file (Write)"
            write_diff_log(file_path, old_content, new_content, action)

    elif tool_name == "Edit" and is_post:
        old_string = tool_input.get("old_string", "")
        new_string = tool_input.get("new_string", "")
        write_diff_log(file_path, old_string, new_string, "Sửa file (Edit)")


if __name__ == "__main__":
    main()
```
<!-- CLONE KẾT THÚC: ~/Documents/workspace/.claude/hooks/code-timeline.py -->

**Cách setup ở máy mới:**
1. Tạo thư mục `~/Documents/workspace/.claude/hooks/`.
2. Copy đúng nội dung 2 khối code trên vào `~/Documents/workspace/.claude/settings.json` và `~/Documents/workspace/.claude/hooks/code-timeline.py`.
3. `chmod +x ~/Documents/workspace/.claude/hooks/code-timeline.py`.
4. Nếu máy mới có path khác `/Users/dawn/Documents/workspace` — sửa lại **toàn bộ đường dẫn tuyệt đối** trong cả 2 file (2 dòng `command` gọi `python3 ...` trong `settings.json`, và `REPO_ROOT` trong script) cho khớp path thật của máy đó.
5. Gõ `/hooks` trong Claude Code (hoặc restart) để nạp lại config — bắt buộc, config mới tạo giữa phiên không tự nạp.
6. Verify: gõ 1 prompt có `/git-push` rồi thử 1 lệnh git — không bị hỏi permission là đúng. Sửa 1 file bất kỳ — `docs/code-timeline.md` phải xuất hiện entry mới.
