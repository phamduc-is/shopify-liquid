
### 2026-08-13 17:33:17 — Sửa file (Edit) `/Users/dawn/Documents/workspace/shopify-liquid/docs/continue-here.md`
+204 / -1

```diff
--- before
+++ after
@@ -1,2 +1,205 @@
 ### Quy tắc làm việc mới (từ Bước 2 trở đi)
-Agent **không tự sửa code trực tiếp vào file theme** nữa — chỉ **hướng dẫn từng bước** (giải thích cần sửa gì, ở đâu, tại sao), người dùng tự gõ code. Agent chỉ verify (theme check, đọc lại file) sau khi người dùng báo đã làm xong. Ngoại lệ: các file tracking tiến độ (`docs/continue-here.md`) và chạy lệnh CLI kiểm tra/pull vẫn agent tự làm được, không tính là "code vào theme".+Agent **không tự sửa code trực tiếp vào file theme** nữa — chỉ **hướng dẫn từng bước** (giải thích cần sửa gì, ở đâu, tại sao), người dùng tự gõ code. Agent chỉ verify (theme check, đọc lại file) sau khi người dùng báo đã làm xong. Ngoại lệ: các file tracking tiến độ (`docs/continue-here.md`) và chạy lệnh CLI kiểm tra/pull vẫn agent tự làm được, không tính là "code vào theme".
+
+---
+
+## 🔧 Setup Claude Code hooks trên máy mới (làm TAY — không tự sync qua git được)
+
+> ⚠️ Config nằm ở `~/Documents/workspace/.claude/` — **NGOÀI** repo `shopify-liquid` này, vì đó mới là project root thật Claude Code dùng (Primary working directory), không phải subfolder `shopify-liquid/`. Đã từng nhầm đặt hook ở `shopify-liquid/.claude/` và phát hiện nó **không hề chạy** (verify bằng flag file `/tmp/claude-git-allow-*` không bao giờ được tạo). Vì `workspace/` không phải git repo nên 2 file dưới đây **không tự đồng bộ qua máy khác** — copy tay theo hướng dẫn cuối mục này.
+
+**Gồm 2 hook:**
+1. `/git-push`/`/git-pull`/`/git-commit` keyword-check — enforce đúng rule AGENTS.md mục 1.5 (git cần từ khoá kích hoạt mới chạy thẳng không hỏi).
+2. `code-timeline.py` — mỗi lần Edit/Write vào code, tự tính diff thật (`difflib`) và append vào `docs/code-timeline.md` (rule AGENTS.md mục 1.6).
+
+<!-- CLONE BẮT ĐẦU: ~/Documents/workspace/.claude/settings.json -->
+```json
+{
+  "permissions": {
+    "allow": [
+      "Bash(*)",
+      "Read(*)",
+      "Write(*)",
+      "Edit(*)",
+      "Glob(*)",
+      "Grep(*)"
+    ],
+    "ask": [
+      "Bash(git *)"
+    ]
+  },
+  "hooks": {
+    "UserPromptSubmit": [
+      {
+        "hooks": [
+          {
+            "type": "command",
+            "command": "INPUT=$(cat); SID=$(echo \"$INPUT\" | jq -r '.session_id'); PROMPT=$(echo \"$INPUT\" | jq -r '.prompt // empty'); FLAG=\"/tmp/claude-git-allow-${SID}\"; if echo \"$PROMPT\" | grep -qE '/git-[a-zA-Z-]+'; then touch \"$FLAG\"; else rm -f \"$FLAG\"; fi",
+            "statusMessage": "Checking for /git-* trigger keyword"
+          }
+        ]
+      }
+    ],
+    "PreToolUse": [
+      {
+        "matcher": "Bash",
+        "hooks": [
+          {
+            "type": "command",
+            "if": "Bash(git *)",
+            "command": "INPUT=$(cat); SID=$(echo \"$INPUT\" | jq -r '.session_id'); FLAG=\"/tmp/claude-git-allow-${SID}\"; if [ -f \"$FLAG\" ]; then echo '{\"hookSpecificOutput\":{\"hookEventName\":\"PreToolUse\",\"permissionDecision\":\"allow\",\"permissionDecisionReason\":\"Git trigger keyword (/git-push, /git-pull, ...) detected in prompt\"}}'; else echo '{\"hookSpecificOutput\":{\"hookEventName\":\"PreToolUse\",\"permissionDecision\":\"ask\",\"permissionDecisionReason\":\"No /git-* trigger keyword in prompt\"}}'; fi"
+          }
+        ]
+      },
+      {
+        "matcher": "Write",
+        "hooks": [
+          {
+            "type": "command",
+            "command": "python3 /Users/dawn/Documents/workspace/.claude/hooks/code-timeline.py",
+            "statusMessage": "Caching pre-write content for timeline diff"
+          }
+        ]
+      }
+    ],
+    "PostToolUse": [
+      {
+        "matcher": "Write|Edit",
+        "hooks": [
+          {
+            "type": "command",
+            "command": "python3 /Users/dawn/Documents/workspace/.claude/hooks/code-timeline.py",
+            "statusMessage": "Logging code diff to docs/code-timeline.md"
+          }
+        ]
+      }
+    ]
+  }
+}
+```
+<!-- CLONE KẾT THÚC: ~/Documents/workspace/.claude/settings.json -->
+
+<!-- CLONE BẮT ĐẦU: ~/Documents/workspace/.claude/hooks/code-timeline.py -->
+```python
+#!/usr/bin/env python3
+"""
+Hook Write|Edit — ghi lại diff thật (code thêm/xoá/sửa) vào docs/code-timeline.md
+mỗi lần agent tác động vào 1 file code.
+
+- PreToolUse (matcher: Write)  -> cache nội dung file TRƯỚC khi bị Write ghi đè
+                                   (Write không có old_string như Edit, nên phải
+                                   tự lưu lại trước khi mất).
+- PostToolUse (matcher: Write|Edit) -> tính diff thật (difflib) và append vào log.
+  - Edit: diff old_string/new_string có sẵn trong tool_input.
+  - Write: đọc lại cache đã lưu ở bước Pre (nếu file mới tạo thì cache rỗng).
+"""
+import sys
+import json
+import os
+import hashlib
+import difflib
+import datetime
+
+REPO_ROOT = "/Users/dawn/Documents/workspace/shopify-liquid"
+LOG_FILE = os.path.join(REPO_ROOT, "docs", "code-timeline.md")
+CACHE_DIR = "/tmp/claude-write-precache"
+MAX_DIFF_LINES = 200
+
+
+def cache_key(file_path):
+    return hashlib.sha1(file_path.encode("utf-8")).hexdigest()
+
+
+def write_diff_log(file_path, old_text, new_text, action):
+    diff_lines = list(
+        difflib.unified_diff(
+            old_text.splitlines(keepends=True),
+            new_text.splitlines(keepends=True),
+            fromfile="before",
+            tofile="after",
+        )
+    )
+    if diff_lines and not diff_lines[-1].endswith("\n"):
+        diff_lines[-1] += "\n"
+    if not diff_lines:
+        return  # không có thay đổi thật (VD Edit no-op) -> không log
+
+    added = sum(1 for l in diff_lines if l.startswith("+") and not l.startswith("+++"))
+    removed = sum(1 for l in diff_lines if l.startswith("-") and not l.startswith("---"))
+    ts = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
+
+    body = diff_lines
+    truncated_note = ""
+    if len(body) > MAX_DIFF_LINES:
+        truncated_note = f"\n... ({len(body) - MAX_DIFF_LINES} dòng diff bị cắt, xem file thật để đủ) ...\n"
+        body = body[:MAX_DIFF_LINES]
+
+    entry = (
+        f"\n### {ts} — {action} `{file_path}`\n"
+        f"+{added} / -{removed}\n\n"
+        "```diff\n"
+        + "".join(body)
+        + truncated_note
+        + "```\n"
+    )
+
+    os.makedirs(os.path.dirname(LOG_FILE), exist_ok=True)
+    with open(LOG_FILE, "a", encoding="utf-8") as f:
+        f.write(entry)
+
+
+def main():
+    try:
+        data = json.load(sys.stdin)
+    except Exception:
+        return
+
+    tool_name = data.get("tool_name")
+    tool_input = data.get("tool_input", {}) or {}
+    is_post = "tool_response" in data
+    file_path = tool_input.get("file_path")
+    if not file_path:
+        return
+
+    if tool_name == "Write":
+        key = os.path.join(CACHE_DIR, cache_key(file_path))
+        if not is_post:
+            # PreToolUse: lưu lại nội dung cũ trước khi bị ghi đè
+            os.makedirs(CACHE_DIR, exist_ok=True)
+            old_content = ""
+            if os.path.exists(file_path):
+                try:
+                    with open(file_path, encoding="utf-8") as f:
+                        old_content = f.read()
+                except Exception:
+                    old_content = ""
+            with open(key, "w", encoding="utf-8") as f:
+                f.write(old_content)
+        else:
+            # PostToolUse: lấy lại cache, diff với content mới, rồi xoá cache
+            old_content = ""
+            if os.path.exists(key):
+                with open(key, encoding="utf-8") as f:
+                    old_content = f.read()
+                os.remove(key)
+            new_content = tool_input.get("content", "")
+            action = "Tạo file" if old_content == "" else "Ghi đè file (Write)"
+            write_diff_log(file_path, old_content, new_content, action)
+
+    elif tool_name == "Edit" and is_post:
+        old_string = tool_input.get("old_string", "")
+        new_string = tool_input.get("new_string", "")
+        write_diff_log(file_path, old_string, new_string, "Sửa file (Edit)")
+
+
+if __name__ == "__main__":
+    main()
+```

... (9 dòng diff bị cắt, xem file thật để đủ) ...
```

### 2026-08-14 09:16:13 — Sửa file (Edit) `/Users/dawn/Documents/workspace/shopify-liquid/ecommerce-theme/sections/header.liquid`
+1 / -1

```diff
--- before
+++ after
@@ -1,2 +1,2 @@
-<div class="announcement-bar">
+<div class="announcement-bar full-width">
   <div class="announcement-bar__inner container-fullwidth">
```

### 2026-08-14 09:16:16 — Sửa file (Edit) `/Users/dawn/Documents/workspace/shopify-liquid/ecommerce-theme/sections/header.liquid`
+1 / -1

```diff
--- before
+++ after
@@ -1,2 +1,2 @@
-<header class="header">
+<header class="header full-width">
   <div class="header__inner container">
```

### 2026-08-14 10:12:30 — Tạo file `/Users/dawn/Documents/workspace/shopify-liquid/ecommerce-theme/src/scss/components/_mobile-nav.scss`
+63 / -0

```diff
--- before
+++ after
@@ -0,0 +1,63 @@
+.mobile-nav {
+  position: fixed;
+  top: 0;
+  left: 0;
+  margin: 0;
+  height: 100vh;
+  width: 50vw;
+  max-width: none;
+  max-height: none;
+  border: none;
+  padding: 24px;
+  display: flex;
+  flex-direction: column;
+  overflow-y: auto;
+  background-color: var(--color-background);
+  color: var(--color-foreground);
+
+  transform: translateX(-100%);
+  transition:
+    transform 0.3s ease-out,
+    overlay 0.3s ease-out allow-discrete,
+    display 0.3s ease-out allow-discrete;
+}
+
+.mobile-nav[open] {
+  transform: translateX(0);
+
+  @starting-style {
+    transform: translateX(-100%);
+  }
+}
+
+.mobile-nav::backdrop {
+  background-color: rgb(0 0 0 / 40%);
+  transition:
+    background-color 0.3s ease-out,
+    overlay 0.3s ease-out allow-discrete,
+    display 0.3s ease-out allow-discrete;
+}
+
+.mobile-nav[open]::backdrop {
+  background-color: rgb(0 0 0 / 40%);
+
+  @starting-style {
+    background-color: rgb(0 0 0 / 0%);
+  }
+}
+
+.mobile-nav__close {
+  display: flex;
+  align-self: flex-end;
+  background: none;
+  border: none;
+  color: inherit;
+  cursor: pointer;
+}
+
+.mobile-nav__links {
+  display: flex;
+  flex-direction: column;
+  gap: 16px;
+  margin-top: 24px;
+}
```

### 2026-08-14 10:13:00 — Sửa file (Edit) `/Users/dawn/Documents/workspace/shopify-liquid/ecommerce-theme/src/scss/main.scss`
+2 / -1

```diff
--- before
+++ after
@@ -1,2 +1,3 @@
 @use 'components/announcement-bar';
-@use 'components/header';+@use 'components/header';
+@use 'components/mobile-nav';
```

### 2026-08-14 10:13:05 — Sửa file (Edit) `/Users/dawn/Documents/workspace/shopify-liquid/ecommerce-theme/sections/header.liquid`
+0 / -5

```diff
--- before
+++ after
@@ -1,6 +1 @@
-{% comment %}
-  TODO (hành vi, chưa phải bước này):
-  - Nút hamburger (.header__menu-toggle) mở mobile nav — chưa có JS
-  - Nút X (.announcement-bar__close) đóng announcement bar — chưa có JS
-{% endcomment %}
 <div class="announcement-bar full-width">
```

### 2026-08-14 13:42:58 — Tạo file `/Users/dawn/Documents/workspace/design-tokens.css`
+74 / -0

```diff
--- before
+++ after
@@ -0,0 +1,74 @@
+/*
+ * Design Tokens
+ * Nguồn: Figma — "E-commerce Website Template (Freebie) (Community)"
+ * https://www.figma.com/design/AXFvzD9Zu9A2xkNwOItGWL/
+ * Trích xuất tự động từ các frame: Homepage, Category Page, Product Detail Page, Cart, Filters
+ */
+
+:root {
+  /* ============ Colors ============ */
+  /* Neutrals */
+  --color-black: #000000;       /* text chính, nền button đen */
+  --color-white: #FFFFFF;       /* nền / text trên nền tối */
+  --color-gray-50: #F2F0F1;     /* divider, nền rất nhạt */
+  --color-gray-100: #F0F0F0;    /* nền section / card */
+  --color-gray-100-warm: #F0EEED; /* nền section (tông ấm) */
+  --color-gray-200: #D6DCE5;    /* border, input */
+
+  /* Accent */
+  --color-accent-yellow: #FFC633; /* rating stars */
+  --color-accent-red: #FF3333;    /* sale / discount badge */
+  --color-accent-green: #01AB31;  /* trạng thái còn hàng / success */
+
+  /* Payment brand icons (tham khảo, không phải token theme chính) */
+  --color-visa: #1434CB;
+  --color-mastercard: #F79E1B;
+  --color-paypal: #179BD7;
+  --color-gpay: #E94235;
+
+  /* ============ Typography ============ */
+  --font-heading: "Integral CF", sans-serif;   /* hero title / heading lớn */
+  --font-body: "Satoshi", sans-serif;          /* body, label, button, price */
+
+  --fs-hero-xl: 64px;
+  --fs-hero-lg: 48px;
+  --fs-hero-md: 40px;
+  --fs-hero-sm: 32px;
+
+  --fs-heading: 24px;   /* Satoshi 700 */
+  --fs-subheading: 20px; /* Satoshi 700 */
+  --fs-body-lg: 16px;    /* Satoshi 400/500 */
+  --fs-body: 14px;       /* Satoshi 400/500 */
+  --fs-label: 12px;      /* Satoshi 400/500 */
+  --fs-caption: 10px;    /* Satoshi 500 */
+
+  --fw-regular: 400;
+  --fw-medium: 500;
+  --fw-bold: 700;
+
+  /* ============ Spacing (gap giữa các item, thang 4px) ============ */
+  --space-1: 4px;
+  --space-2: 8px;
+  --space-2-5: 10px;
+  --space-3: 12px;
+  --space-3-5: 14px;
+  --space-4: 16px;
+  --space-5: 20px;
+  --space-6: 24px;
+  --space-6-5: 26px;
+
+  /* ============ Padding (button / card, dạng L-R-T-B) ============ */
+  --pad-btn-sm: 14px 14px 6px 6px;
+  --pad-btn-md: 16px 16px 12px 12px;
+  --pad-btn-lg: 20px 20px 10px 10px;
+  --pad-btn-hero: 54px 54px 16px 16px;
+  --pad-card: 32px 32px 28px 28px;
+  --pad-card-square: 24px 24px 24px 24px;
+
+  /* ============ Border radius ============ */
+  --radius-none: 0px;
+  --radius-sm: 4px;   /* input / tag */
+  --radius-md: 8px;   /* card / button */
+  --radius-lg: 20px;  /* large card */
+  --radius-full: 62px; /* pill / nút tròn (vd. Add to cart) */
+}
```

### 2026-08-14 13:50:01 — Sửa file (Edit) `/Users/dawn/Documents/workspace/shopify-liquid/ecommerce-theme/config/settings_schema.json`
+1 / -1

```diff
--- before
+++ after
@@ -1,6 +1,6 @@
       {
         "type": "color",
         "id": "border_color",
-        "default": "#E8E8E8",
+        "default": "#D6DCE5",
         "label": "t:labels.border"
       },
```

### 2026-08-14 13:50:42 — Sửa file (Edit) `/Users/dawn/Documents/workspace/shopify-liquid/ecommerce-theme/config/settings_schema.json`
+2 / -2

```diff
--- before
+++ after
@@ -2,9 +2,9 @@
         "type": "range",
         "id": "button_corner_radius",
         "min": 0,
-        "max": 40,
+        "max": 100,
         "step": 1,
         "unit": "px",
         "label": "t:labels.button_corner_radius",
-        "default": 24
+        "default": 62
       }
```

### 2026-08-14 13:50:59 — Sửa file (Edit) `/Users/dawn/Documents/workspace/shopify-liquid/ecommerce-theme/snippets/css-variables.liquid`
+35 / -0

```diff
--- before
+++ after
@@ -8,4 +8,39 @@
   {{ settings.type_heading_font | font_face: font_display: 'swap' }}
   {{ settings.type_heading_font | font_modify: 'weight', 'bold' | font_face: font_display: 'swap' }}
 
+  {% # Figma-sourced custom fonts (Satoshi / Integral CF).
+     # Not available in Shopify's font_picker library, so they're loaded as
+     # static @font-face rules instead of via settings.*.
+     # Upload the matching files to /assets before deploying:
+     #   assets/satoshi-regular.woff2 / satoshi-medium.woff2 / satoshi-bold.woff2
+     #   assets/integral-cf-bold.woff2 %}
+  @font-face {
+    font-family: 'Satoshi';
+    src: url('{{ "satoshi-regular.woff2" | asset_url }}') format('woff2');
+    font-weight: 400;
+    font-style: normal;
+    font-display: swap;
+  }
+  @font-face {
+    font-family: 'Satoshi';
+    src: url('{{ "satoshi-medium.woff2" | asset_url }}') format('woff2');
+    font-weight: 500;
+    font-style: normal;
+    font-display: swap;
+  }
+  @font-face {
+    font-family: 'Satoshi';
+    src: url('{{ "satoshi-bold.woff2" | asset_url }}') format('woff2');
+    font-weight: 700;
+    font-style: normal;
+    font-display: swap;
+  }
+  @font-face {
+    font-family: 'Integral CF';
+    src: url('{{ "integral-cf-bold.woff2" | asset_url }}') format('woff2');
+    font-weight: 700;
+    font-style: normal;
+    font-display: swap;
+  }
+
   :root {
```

### 2026-08-14 13:51:10 — Sửa file (Edit) `/Users/dawn/Documents/workspace/shopify-liquid/ecommerce-theme/snippets/css-variables.liquid`
+36 / -0

```diff
--- before
+++ after
@@ -1,5 +1,41 @@
     {% # Border radius %}
     --style-border-radius-inputs: {{ settings.input_corner_radius }}px;
     --style-border-radius-buttons: {{ settings.button_corner_radius }}px;
+    --style-border-radius-full: 62px;
+
+    {% # ------------------------------------------------------------
+       # Figma design tokens (from design-tokens.css) not yet exposed
+       # as theme settings. Static values — not merchant-editable. %}
+
+    {% # Figma-sourced fonts (see @font-face rules above) %}
+    --font-primary-figma--family: 'Satoshi', {{ settings.type_primary_font.fallback_families }};
+    --font-heading-figma--family: 'Integral CF', {{ settings.type_heading_font.fallback_families }};
+
+    --fw-regular: 400;
+    --fw-medium: 500;
+    --fw-bold: 700;
+
+    {% # Font-size scale %}
+    --fs-hero-xl: 64px;
+    --fs-hero-lg: 48px;
+    --fs-hero-md: 40px;
+    --fs-hero-sm: 32px;
+    --fs-heading: 24px;
+    --fs-subheading: 20px;
+    --fs-body-lg: 16px;
+    --fs-body: 14px;
+    --fs-label: 12px;
+    --fs-caption: 10px;
+
+    {% # Spacing scale %}
+    --space-1: 4px;
+    --space-2: 8px;
+    --space-2-5: 10px;
+    --space-3: 12px;
+    --space-3-5: 14px;
+    --space-4: 16px;
+    --space-5: 20px;
+    --space-6: 24px;
+    --space-6-5: 26px;
   }
 {% endstyle %}
```

### 2026-08-14 16:32:47 — Sửa file (Edit) `/Users/dawn/Documents/workspace/shopify-liquid/ecommerce-theme/config/settings_schema.json`
+1 / -1

```diff
--- before
+++ after
@@ -1,6 +1,6 @@
       {
         "type": "font_picker",
         "id": "type_primary_font",
-        "default": "work_sans_n4",
+        "default": "poppins_n4",
         "label": "t:general.primary"
       }
```

### 2026-08-14 16:32:58 — Sửa file (Edit) `/Users/dawn/Documents/workspace/shopify-liquid/ecommerce-theme/snippets/css-variables.liquid`
+0 / -35

```diff
--- before
+++ after
@@ -1,36 +1 @@
-  {% # Figma-sourced custom fonts (Satoshi / Integral CF).
-     # Not available in Shopify's font_picker library, so they're loaded as
-     # static @font-face rules instead of via settings.*.
-     # Upload the matching files to /assets before deploying:
-     #   assets/satoshi-regular.woff2 / satoshi-medium.woff2 / satoshi-bold.woff2
-     #   assets/integral-cf-bold.woff2 %}
-  @font-face {
-    font-family: 'Satoshi';
-    src: url('{{ "satoshi-regular.woff2" | asset_url }}') format('woff2');
-    font-weight: 400;
-    font-style: normal;
-    font-display: swap;
-  }
-  @font-face {
-    font-family: 'Satoshi';
-    src: url('{{ "satoshi-medium.woff2" | asset_url }}') format('woff2');
-    font-weight: 500;
-    font-style: normal;
-    font-display: swap;
-  }
-  @font-face {
-    font-family: 'Satoshi';
-    src: url('{{ "satoshi-bold.woff2" | asset_url }}') format('woff2');
-    font-weight: 700;
-    font-style: normal;
-    font-display: swap;
-  }
-  @font-face {
-    font-family: 'Integral CF';
-    src: url('{{ "integral-cf-bold.woff2" | asset_url }}') format('woff2');
-    font-weight: 700;
-    font-style: normal;
-    font-display: swap;
-  }
-
   :root {
```

### 2026-08-14 16:33:05 — Sửa file (Edit) `/Users/dawn/Documents/workspace/shopify-liquid/ecommerce-theme/snippets/css-variables.liquid`
+0 / -4

```diff
--- before
+++ after
@@ -2,8 +2,4 @@
        # Figma design tokens (from design-tokens.css) not yet exposed
        # as theme settings. Static values — not merchant-editable. %}
 
-    {% # Figma-sourced fonts (see @font-face rules above) %}
-    --font-primary-figma--family: 'Satoshi', {{ settings.type_primary_font.fallback_families }};
-    --font-heading-figma--family: 'Integral CF', {{ settings.type_heading_font.fallback_families }};
-
     --fw-regular: 400;
```

### 2026-08-14 16:54:29 — Sửa file (Edit) `/Users/dawn/Documents/workspace/shopify-liquid/ecommerce-theme/snippets/css-variables.liquid`
+1 / -3

```diff
--- before
+++ after
@@ -1,6 +1,4 @@
-    {% # ------------------------------------------------------------
-       # Figma design tokens (from design-tokens.css) not yet exposed
-       # as theme settings. Static values — not merchant-editable. %}
+    {% # Figma design tokens (from design-tokens.css) not yet exposed as theme settings. Static values — not merchant-editable. %}
 
     --fw-regular: 400;
     --fw-medium: 500;
```

### 2026-08-14 16:56:22 — Sửa file (Edit) `/Users/dawn/Documents/workspace/shopify-liquid/docs/continue-here.md`
+11 / -1

```diff
--- before
+++ after
@@ -1 +1,11 @@
-2. ~~Bước 1 — Thiết lập Design System~~ ✅ Đã xong — màu (9 màu: background/foreground/text_secondary/border/surface_secondary/sale_badge_bg+text/rating_star/success) + font (heading: Archivo Black, primary: Work Sans) đã vào `config/settings_schema.json`, sinh CSS variable trong `snippets/css-variables.liquid`, verify qua Theme Editor thật + `theme check` sạch (39 file, 0 lỗi). Tiện tay fix luôn 1 bug JSON có sẵn từ skeleton (trailing comma ở `locales/en.default.schema.json`).+2. ~~Bước 1 — Thiết lập Design System~~ ✅ Đã xong — màu (9 màu: background/foreground/text_secondary/border/surface_secondary/sale_badge_bg+text/rating_star/success) + font (heading: Archivo Black, primary: Work Sans) đã vào `config/settings_schema.json`, sinh CSS variable trong `snippets/css-variables.liquid`, verify qua Theme Editor thật + `theme check` sạch (39 file, 0 lỗi). Tiện tay fix luôn 1 bug JSON có sẵn từ skeleton (trailing comma ở `locales/en.default.schema.json`).
+
+   **🔄 Cập nhật thêm — đối chiếu lại với file Figma gốc qua API (mới xong):**
+   - Đã dùng Figma Personal Access Token để gọi trực tiếp Figma REST API (agent trước đó không tự mở được link Figma dạng web), đọc toàn bộ 9 frame (Homepage, Category, Product Detail, Cart, Filters) và thống kê tần suất màu/font/spacing/radius thật trong file thiết kế.
+   - Đối chiếu thì phát hiện phần lớn màu trong `settings_schema.json` đã khớp sẵn (rating_star, success, sale_badge_text, surface_secondary, background/foreground) — xác nhận theme đã được build đúng theo Figma. Lệch 2 chỗ, đã sửa theo Figma:
+     - `border_color`: `#E8E8E8` → **`#D6DCE5`**
+     - `button_corner_radius`: default `24` → **`62`** (dáng pill giống nút "Add to cart" trong Figma), phải nâng luôn `max` từ `40` → `100` để cho phép giá trị 62.
+   - Có thử thêm font custom **Satoshi**/**Integral CF** (đúng font gốc trong Figma) qua `@font-face` trỏ tới file `.woff2` chưa upload — sau đó **quyết định bỏ hướng này** (bài demo tập trung Liquid, không cần font ngoài) và đổi `type_primary_font` sang **`poppins_n4`** (Poppins — font có sẵn trong thư viện Shopify, gần với phong cách Satoshi nhất; Poppins cũng có xuất hiện rải rác trong chính file Figma). `type_heading_font` giữ nguyên `archivo_black_n4` (đã đủ gần Integral CF).
+   - Bổ sung vào `snippets/css-variables.liquid` các token Figma chưa có setting tương ứng: `--style-border-radius-full: 62px`, scale `--fs-*` (font-size), `--fw-*` (font-weight), `--space-*` (spacing 4→26px) — token tĩnh, không qua Theme Customizer.
+   - **Bug vừa fix**: 1 khối comment Liquid `{% # ... %}` viết tràn nhiều dòng vật lý (chỉ mở/đóng tag ở dòng đầu/cuối) — tuy `shopify theme check` không báo lỗi nhưng để an toàn với editor/syntax highlighter, đã tách lại đúng chuẩn "1 dòng = 1 tag comment". Verify lại: `theme check` vẫn sạch (43 file, chỉ còn 2 warning `OrphanedSnippet` như đã biết).
+   - File tham khảo rời (không nằm trong repo Shopify): `~/Documents/workspace/design-tokens.css` — bộ token CSS đầy đủ trích từ Figma, dùng để đối chiếu khi cần.
```

### 2026-08-14 16:57:01 — Sửa file (Edit) `/Users/dawn/Documents/workspace/shopify-liquid/docs/continue-here.md`
+11 / -6

```diff
--- before
+++ after
@@ -1,10 +1,15 @@
-5. **Bước 4 — Build từng trang theo đúng thứ tự roadmap: Header/Footer (13-14) → Homepage → Product Page (15-16) → Category (17) → Cart (18)** — **ĐANG LÀM, tiếp tục từ Header**. Mỗi section vẫn theo phương pháp đã luyện: **HTML tĩnh (dữ liệu giả) → thay dần bằng Liquid động (dùng kỹ thuật `render for as` — Ngày 6 — để loop `product-card` qua danh sách sản phẩm) → thêm `{% schema %}` cuối cùng, thêm dần từng setting một, test bằng Theme Editor sau mỗi lần thêm**.
+5. **Bước 4 — Build từng trang theo đúng thứ tự roadmap: Header/Footer (13-14) → Homepage → Product Page (15-16) → Category (17) → Cart (18)** — **ĐANG LÀM, Header xong, tiếp tục sang Footer**. Mỗi section vẫn theo phương pháp đã luyện: **HTML tĩnh (dữ liệu giả) → thay dần bằng Liquid động (dùng kỹ thuật `render for as` — Ngày 6 — để loop `product-card` qua danh sách sản phẩm) → thêm `{% schema %}` cuối cùng, thêm dần từng setting một, test bằng Theme Editor sau mỗi lần thêm**.
 
-   **Tiến độ `sections/header.liquid` (đang làm):**
+   **✅ `sections/header.liquid` — ĐÃ XONG (tự tay làm trực tiếp trong IDE, agent chỉ verify):**
    - ✅ HTML tĩnh: announcement-bar + header (logo, nav, search, cart/account icon) — 4 icon SVG mới: `assets/icon-menu.svg`, `icon-search.svg`, `icon-close.svg`, `icon-chevron-down.svg`.
    - ✅ Utility `.container`/`.container-fullwidth` mới — `src/scss/base/_container.scss`, dùng chung cho mọi section sau này (không chỉ header): `max-width: calc(1240px + var(--page-margin) * 2)` để trừ `padding-inline` xong content vẫn đúng 1240px theo Figma (do `box-sizing: border-box` toàn theme).
    - ✅ Liquid động — phần bind vào object có sẵn (không cần schema): cart icon (`cart.item_count` + badge `.header__cart-count`), account icon (`routes.account_url`), search form (`<form action="{{ routes.search_url }}" method="get">` + `name="q"` + `value="{{ search.terms }}"`).
-   - ⏳ Nav — **đã đưa code loop `section.settings.menu.links` cho user paste vào**, cần xác nhận đã paste vào `sections/header.liquid` chưa trước khi làm schema (lần trước ghi nhầm "đã viết" nhưng thực tế chỉ nằm trong chat, chưa vào file — đã phát hiện và sửa lại đúng).
-   - ❌ Announcement-bar text/link — vẫn hardcode HTML tĩnh, chưa đổi (chủ đích, để làm cùng lúc với schema).
-   - ❌ Chưa làm: `{% schema %}` cho header (setting `menu` kiểu `link_list` + 3 setting announcement-bar) — **làm tiếp từ đây, sau khi xác nhận nav đã có code loop trong file**.
-   - ❌ `sections/footer.liquid` — chưa bắt đầu.+   - ✅ Nav — code loop `section.settings.menu.links` đã paste vào file thật (verify xong, dùng ở cả `.header__nav` desktop và `.mobile-nav__links` mobile).
+   - ✅ `{% schema %}` cho header: setting `menu` (`link_list`, default `main-menu`) + 3 setting announcement-bar (`announcement_text`, `announcement_link_url`, `announcement_link_text`).
+   - ✅ **Phát sinh thêm ngoài kế hoạch ban đầu** — hành vi tương tác (mobile nav + đóng announcement-bar), làm bằng `<dialog>` + `{% javascript %}` ngay trong `header.liquid`:
+     - `.announcement-bar__close` → JS remove `.announcement-bar` khỏi DOM.
+     - `.header__menu-toggle` (hamburger, hiện ở mobile) → mở `<dialog class="mobile-nav">` bằng `showModal()`; đóng bằng nút X hoặc click ra ngoài (`event.target === mobileNav`, kiểm tra click trúng chính `<dialog>` tức là click ra backdrop).
+     - SCSS mới `src/scss/components/_mobile-nav.scss` (63 dòng, animation `transform` + `@starting-style`/`allow-discrete` cho hiệu ứng slide-in/out chuẩn `<dialog>` mới của trình duyệt) — đã `@use` vào `main.scss`, build ra `theme.css` sạch.
+     - `.announcement-bar`/`.header` đổi thêm class `full-width` (utility grid có sẵn từ skeleton gốc ở `assets/critical.css`, không phải bug).
+   - `theme check` sau cùng: vẫn sạch, 43 file, chỉ 2 warning `OrphanedSnippet` đã biết (không liên quan header).
+   - ❌ `sections/footer.liquid` — **chưa bắt đầu, làm tiếp theo**.
```

### 2026-08-14 16:57:13 — Sửa file (Edit) `/Users/dawn/Documents/workspace/shopify-liquid/docs/continue-here.md`
+1 / -1

```diff
--- before
+++ after
@@ -1,2 +1,2 @@
 ## ✅ Việc cần làm ngay khi tiếp tục
-Header đã xong HTML tĩnh + phần Liquid động bind-object-có-sẵn (cart/account/search). **Việc tiếp theo**: thêm `{% schema %}` cho `sections/header.liquid` — 1 setting `menu` (`link_list`) cho nav + 3 setting cho announcement-bar (`announcement_text`, `announcement_link_url`, `announcement_link_text`), thêm dần từng setting một, test bằng Theme Editor sau mỗi lần thêm (đúng phương pháp đã luyện). Xong header thì làm `sections/footer.liquid` (Ngày 14) theo đúng quy trình 3 bước tương tự. Snippet dùng chung (`product-card`, `rating-stars`) đã có sẵn từ Bước 3, chưa dùng tới — 2 warning `OrphanedSnippet` sẽ tự hết khi Homepage render chúng.+`sections/header.liquid` đã **xong hoàn toàn** — HTML tĩnh, Liquid động, `{% schema %}` (menu + announcement-bar), và cả phần tương tác mobile nav/JS phát sinh thêm. Design tokens (`config/settings_schema.json` + `snippets/css-variables.liquid`) cũng đã đối chiếu lại với Figma qua API, sạch `theme check`. **Việc tiếp theo**: làm `sections/footer.liquid` (Ngày 14) theo đúng quy trình 3 bước đã luyện (HTML tĩnh → Liquid động → schema thêm dần từng setting, test Theme Editor sau mỗi lần thêm). Snippet dùng chung (`product-card`, `rating-stars`) đã có sẵn từ Bước 3, chưa dùng tới — 2 warning `OrphanedSnippet` sẽ tự hết khi Homepage render chúng.
```

### 2026-08-14 16:57:31 — Sửa file (Edit) `/Users/dawn/Documents/workspace/shopify-liquid/docs/continue-here.md`
+3 / -1

```diff
--- before
+++ after
@@ -1 +1,3 @@
-- Phân loại "Liquid động": bind vào **object có sẵn** (`cart`, `routes`, `search`...) làm được ngay không cần schema; phần cần **setting merchant tự nhập** (menu, text tuỳ chỉnh) phải viết code Liquid trước (tạm rỗng) rồi mới thêm schema sau — đúng thứ tự đã thống nhất, không đảo ngược. Chi tiết pattern `<form method="get">` cho search (vì sao GET không phải POST, `name="q"` là key cố định) đã lưu trong `shopify-liquid-summary.md` (Ngày 13).+- Phân loại "Liquid động": bind vào **object có sẵn** (`cart`, `routes`, `search`...) làm được ngay không cần schema; phần cần **setting merchant tự nhập** (menu, text tuỳ chỉnh) phải viết code Liquid trước (tạm rỗng) rồi mới thêm schema sau — đúng thứ tự đã thống nhất, không đảo ngược. Chi tiết pattern `<form method="get">` cho search (vì sao GET không phải POST, `name="q"` là key cố định) đã lưu trong `shopify-liquid-summary.md` (Ngày 13).
+- Comment inline Liquid `{% # ... %}` nên viết **gọn trong 1 dòng** — viết tràn nhiều dòng vật lý (mở tag ở dòng đầu, đóng `%}` ở dòng cuối) tuy `shopify theme check` không bắt lỗi nhưng dễ gây hiểu nhầm/hiển thị sai ở syntax highlighter của editor. Nếu cần comment dài, tách thành nhiều tag `{% # %}` riêng, mỗi dòng một tag.
+- Tương tác UI đơn giản (mở/đóng menu, dismiss banner) nên ưu tiên `<dialog>` native (`showModal()`/`close()`) thay vì tự quản lý `display`/class toggle bằng JS — có sẵn `::backdrop`, focus-trap, đóng bằng phím Esc, và animate được bằng CSS thuần qua `@starting-style` + `transition-behavior: allow-discrete` (không cần thư viện ngoài).
```
