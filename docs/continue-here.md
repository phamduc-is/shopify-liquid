# 📍 Continue Here — Bug & việc còn treo (chờ review)

> Ghi lại lúc dừng phiên làm việc. Agent **không tự sửa** những mục dưới đây — chờ review thủ công.
> Nội dung cũ của file (link Figma, bảng tiến độ roadmap) lấy lại bằng: `git show b1e7d13:docs/continue-here.md`

---

## 🐞 Bug còn mở

### 1. `sections/testimonials.liquid` — nút prev/next chết trong Theme Editor

Block `{% javascript %}` chỉ chạy `document.querySelectorAll('.testimonials').forEach(...)` **một lần lúc page load**.

Khi merchant sửa setting trong Theme Editor, Shopify **render lại DOM của section mà không reload trang** → toàn bộ listener vừa gắn bị mất cùng DOM cũ → 2 nút mũi tên không phản hồi cho tới khi F5.

Hướng fix (chưa áp dụng):
- Tách thân `forEach` hiện tại thành hàm có tên (vd `initSection`) để tái dùng được.
- Thêm listener:
  ```js
  document.addEventListener('shopify:section:load', (event) => {
    event.target.querySelectorAll('.testimonials').forEach(initSection);
  });
  ```
- `event.target` là `<div class="shopify-section">` vừa render lại → chỉ init trong phạm vi đó, không init lại toàn trang (tránh gắn listener trùng).

**Tham chiếu**: `sections/slideshow.liquid` đã làm đúng pattern này (hàm `initSlideshow` + `shopify:section:load`) — copy cách tổ chức từ đó.

---

### 2. `sections/header.liquid` — cùng lỗi, nhưng nặng hơn

Block `{% javascript %}` của header hiện query thẳng từ `document` ở top-level, không bọc hàm, không có `shopify:section:load`. Sau khi section reload trong Theme Editor, các thứ sau **ngừng hoạt động**:

| Thành phần | Hành vi bị mất |
|---|---|
| `.announcement-bar__close` | Không đóng được announcement bar |
| `.header__menu-toggle` | Hamburger không mở `<dialog class="mobile-nav">` |
| `.mobile-nav__close` + click backdrop | Không đóng được mobile nav |
| `.header__icon-button--search` | Không mở được search popup |
| `.search-popup__close` + click backdrop | Không đóng được search popup |

Nặng hơn testimonials vì merchant chỉnh header rất thường xuyên trong editor (logo, menu, announcement text) → gần như chắc chắn gặp.

Hướng fix (chưa áp dụng): gom toàn bộ vào `function initHeader(root)`, query từ `root` thay vì `document`, rồi gọi ở cả page load và `shopify:section:load`.

> ⚠️ Lưu ý riêng cho header: `<dialog class="mobile-nav">` và `<dialog class="search-popup">` có thể nằm **ngoài** phạm vi `.shopify-section` của header (tuỳ nơi render) — kiểm tra DOM thật trước khi đổi sang query-từ-root, kẻo query không ra element.

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

## ✅ Đã fix trong phiên này (không cần làm lại)

Toàn bộ bug slideshow đã báo trước đó đều đã được xử lý — verify lại `src/scss/components/_slideshow.scss` và `blocks/slide.liquid`:

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
