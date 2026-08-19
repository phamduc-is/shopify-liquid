# 📍 Continue Here — Bug & việc còn treo (chờ review)

> Ghi lại lúc dừng phiên làm việc. Agent **không tự sửa** những mục dưới đây — chờ review thủ công.
> Nội dung cũ của file (link Figma, bảng tiến độ roadmap) lấy lại bằng: `git show b1e7d13:docs/continue-here.md`

---

## 🐞 Bug còn mở

### 1. `sections/header.liquid` — nút menu có cấp con là nút chết

Khi merchant tạo menu có sub-link, `{% if link.links != blank %}` render ra `<button class="header__nav-link header__nav-link--dropdown">` kèm icon chevron xuống — nhưng:

| Thiếu | Chi tiết |
|---|---|
| DOM | Không có `{% for %}` nào render `link.links` ra submenu |
| JS | Không có handler nào bắt `.header__nav-link--dropdown` |
| CSS | `_header.scss` không có selector `--dropdown` nào |

→ User thấy nút có mũi tên, bấm vào **không có gì xảy ra**. Xảy ra ở cả `.header__nav` (desktop) lẫn `.mobile-nav__links`. Đây là bug chức năng, không liên quan Theme Editor.

Hai hướng, chọn 1 (chưa quyết):
- **Làm dropdown thật** — render `link.links`; desktop dùng panel thả xuống, mobile dùng `<details>` accordion trong `.mobile-nav`; thêm `aria-expanded`. Là thêm feature, cần cả SCSS mới.
- **Hạ về link thường** — bỏ nhánh `if`, luôn render `<a href="{{ link.url }}">`, bỏ chevron. Hết nút chết ngay, đổi lại menu cấp con không vào được từ header.

---

### 2. Git identity — Mac mini chưa set, 28 commit cũ chưa attribute

MacBook Air **đã xong** (xem mục đã fix bên dưới). Hai việc còn lại:

**a. Mac mini chưa set.** `~/.gitconfig` nằm trong home của từng máy, không đồng bộ qua GitHub account — phải chạy lại y hệt trên Mac mini:

```bash
git config --global user.name  "Dawn"
git config --global user.email "298667006+phamduc-is@users.noreply.github.com"
git config --global user.useConfigOnly true
git var GIT_AUTHOR_IDENT   # verify: phải in ra đúng email noreply ở trên
```

Chưa chạy thì commit từ Mac mini vẫn ra `dawn@Dawns-Mac-mini.local` như cũ.

**b. 28 commit cũ vẫn mang author sai** — chỉ commit *từ đây trở đi* mới đúng:

| Author string cũ | Số commit | Nguồn gốc |
|---|---|---|
| `Dawn <dawn@Dawns-Mac-mini.local>` | 18 | hostname Mac mini |
| `Dawn <dawn@Macmini.bbrouter>` | 5 | cùng Mac mini, hostname do router cấp qua DHCP |
| `Dawn <dawn@Dawns-MacBook-Air.local>` | 5 | hostname MacBook Air |

Muốn 28 commit này cũng được attribute vào account thì phải rewrite history (`git filter-repo --mailmap`) rồi force-push — **toàn bộ SHA sẽ đổi**. Repo public nhưng một mình làm nên rủi ro thấp; vẫn chưa quyết, chưa làm.

---

## 💡 Enhancement đang cân nhắc (không phải bug)

### Kéo ngang bằng chuột cho `.testimonials__track`

Carousel hiện chạy bằng CSS `scroll-snap` — native, không cần JS. Trên desktop user cuộn được bằng:

- Shift + con lăn chuột ✅
- Trackpad 2 ngón vuốt ngang ✅
- 2 nút prev/next ✅
- Kéo scrollbar ❌ (đã tắt bằng `scrollbar-width: none`)
- **Click-kéo (grab) bằng chuột ❌ — browser không hỗ trợ native, cần JS**

Nếu làm, dùng Pointer Events (`pointerdown`/`pointermove`/`pointerup` + `setPointerCapture`) và bỏ qua `e.pointerType === 'touch'` (touch đã có native scroll).

**Hai điểm bắt buộc phải xử lý, nếu không carousel sẽ giật/trễ:**
1. Tạm set `scroll-snap-type: none` trong lúc kéo — snap `mandatory` sẽ liên tục kéo `scrollLeft` về điểm snap trong khi JS đang set thủ công. Bật lại ở `pointerup` để browser tự snap vào card gần nhất.
2. Tạm set `scroll-behavior: auto` trong lúc kéo — `_testimonials.scss` đang bật `smooth` (cho nút prev/next), mỗi lần set `scrollLeft` sẽ khởi động 1 animation → chuột đi trước, track chạy theo sau.

Ngoài ra nên có ngưỡng ~5px trước khi coi là "đang kéo", để click vào link trong card không bị hiểu sai thành drag.

---

## ✅ Đã fix — nhóm Theme Editor re-render

Theme Editor render lại DOM của section mà **không reload trang**, nên JS chạy 1 lần lúc page load sẽ mất hết listener cùng DOM cũ. Đã xử lý theo pattern của `sections/slideshow.liquid` (IIFE + `init(root)` + `shopify:section:load`).

| Bug cũ | Trạng thái |
|---|---|
| `testimonials.liquid` — 2 nút prev/next chết sau khi sửa setting trong editor | ✅ tách `initSection`, thêm `shopify:section:load` |
| `header.liquid` — đóng announcement bar, hamburger, mobile nav, search popup đều chết sau khi sửa setting | ✅ gom `initHeader(root)`, query từ `root` thay vì `document` |
| Listener `resize` của testimonials tích luỹ mỗi lần section re-render, closure giữ tham chiếu DOM đã gỡ | ✅ 1 listener `resize` duy nhất ở scope module, query lại `document` mỗi lần thay vì đóng gói tham chiếu |
| `header.liquid` khai báo 7 `const` ở top-level — các javascript tag của theme bị Shopify gộp chung 1 file nên dùng chung scope, trùng tên là `SyntaxError` chết cả bundle | ✅ bọc IIFE như slideshow |

Ghi chú đã xác minh khi fix:
- `<dialog class="mobile-nav">` và `<dialog class="search-popup">` **nằm trong** `<header class="header">` → vẫn thuộc `.shopify-section` của header, query từ `root` an toàn. (Cảnh báo cũ trong file này về việc dialog có thể nằm ngoài scope — đã loại trừ.)
- `.announcement-bar` là **anh em** của `<header>`, không phải con → `root` phải là `.shopify-section` chứ không phải `.header`. Lấy bằng `header.closest('.shopify-section')` vì id section trong `header-group` do Shopify sinh, không hardcode được.

---

## ✅ Đã fix — git identity trên MacBook Air

`user.name` / `user.email` trước đây **chưa set ở cả local lẫn global**, git tự suy ra `$(whoami)@$(hostname)` → mỗi máy (và mỗi lần router đổi hostname) đẻ ra một identity khác nhau. Email `.local` không tồn tại trên GitHub nên commit không link được vào account: không avatar, không tính contribution graph.

Lưu ý: **đăng nhập GitHub và commit author là hai lớp tách rời**. Token/SSH chỉ trả lời "có quyền push không"; author name/email được đóng băng vào commit object lúc `git commit` và GitHub **không ghi đè** nó — chỉ tra ngược email xem thuộc account nào. Nên login cùng account trên 2 máy không hề đồng bộ identity.

Đã set global trên MacBook Air:

| Key | Giá trị | Lý do |
|---|---|---|
| `user.name` | `Dawn` | |
| `user.email` | `298667006+phamduc-is@users.noreply.github.com` | Repo public → dùng noreply thay email thật, vì email trong commit là **public vĩnh viễn**, ai clone cũng đọc được bằng `git log`. Dạng `<ID>+<username>@users.noreply.github.com` vẫn link đúng vào account. |
| `user.useConfigOnly` | `true` | Chặn git tự đoán từ hostname. Git sẽ **báo lỗi và từ chối commit** thay vì âm thầm bịa email rác — net an toàn cho máy mới / container / CI sau này. Mặc định của git là `false`. |

---

## ✅ Đã fix — nhóm slideshow (phiên trước)

Verify lại `src/scss/components/_slideshow.scss` và `blocks/slide.liquid`:

| Bug cũ | Trạng thái |
|---|---|
| `.slideshow__image { z-index: -1 }` làm ảnh bị `background-color` của slide che | ✅ đổi sang `z-index: 0`, có comment giải thích stacking context |
| Ảnh đè lên chữ sau khi bỏ z-index âm | ✅ `.slideshow__content` có `position: relative; z-index: 1` |
| `.slideshow__slide { flex: 0 0 100% }` vô hiệu vì `content_for 'blocks'` bọc block trong `<div class="shopify-block">` → slide chỉ rộng 544px | ✅ đổi selector sang `.slideshow__slides > *` + `min-width: 100%` |
| `.slideshow__content` bị `margin-inline: auto` của `.container` căn giữa | ✅ giữ `.container` nguyên vẹn, chuyển giới hạn bề ngang xuống heading/text |
| Dots vô hình (`rgb(255 255 255 / 50%)` trên nền sáng) | ✅ dùng `--color-foreground` + `opacity` 0.3 / 1 |
| Ảnh dọc bị crop mạnh khi trải full-bleed | ✅ ảnh chiếm nửa phải (`width: 50%`) + `object-position: top center` |
| Nhiều `<h1>` khi có nhiều slide | ✅ slide đầu `<h1>`, các slide sau `<h2>` |
| `loading: 'lazy'` trên hero above-the-fold | ✅ slide đầu `eager` + `fetchpriority: high`, slide sau `lazy` |
