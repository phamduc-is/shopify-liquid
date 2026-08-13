
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
