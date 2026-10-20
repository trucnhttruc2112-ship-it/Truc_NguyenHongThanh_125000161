# NHẬT KÝ PROMPT ENGINEERING (VIBE CODING LOG)

- **Họ và tên:** [CẦN ĐIỀN: Họ tên đầy đủ]
- **MSSV:** [CẦN ĐIỀN: MSSV]
- **Tên đề tài Website:** Khám phá các vùng đất Việt Nam (website du lịch giới thiệu các vùng miền)
- **Công cụ AI đã sử dụng:**
  - Google Antigravity (bản miễn phí): sinh và chỉnh sửa mã nguồn `index.html`, `style.css` theo prompt.
  - Claude: hỗ trợ lập kế hoạch, soạn và tối ưu nội dung các prompt gửi cho Antigravity.
  - Visual Studio Code + Live Server: xem trước, kiểm thử responsive, commit và đẩy mã lên GitHub.
- **Repository:** [CẦN ĐIỀN: link repo] | **Live Preview:** [CẦN ĐIỀN: link GitHub Pages]

## Tổng quan quy trình

Tôi chia dự án thành 10 prompt, mỗi prompt xử lý đúng một mục tiêu để dễ kiểm tra và dễ sửa lỗi:

| Prompt | Mục tiêu | Thuộc |
|---|---|---|
| 1 | Dựng khung Semantic HTML5, CSS Variables, Flexbox/Grid | Lần 1 |
| 2 | Sửa lỗi: giảm `<div>`, thay giá trị hard-code bằng CSS Variables | Lần 1 |
| 3 | Gắn hình ảnh, alt, lazy loading, object-fit | Lần 2 |
| 4 | Hoàn thiện UI, hover/focus, font Roboto, menu hamburger CSS thuần | Lần 3 |
| 5 | Tích hợp Animate.css cho các phần tử chính ở màn hình đầu | Lần 4 |
| 6 | Hiệu ứng xuất hiện khi cuộn bằng CSS thuần (thay AOS) | Lần 5 |
| 7 | Sửa lỗi: hiệu ứng card kết thúc quá sớm, xung đột với hover | Lần 5 |
| 8 | Lật thẻ card, hiệu ứng cuộn (hero mờ dần, lệch nhịp card), hover nổi bật | Lần 6 |
| 9 | Đơn giản hóa hiệu ứng cuộn về một class dùng chung, dễ kiểm soát | Lần 7 |
| 10 | Sửa lỗi tràn chữ ở phần Liên hệ và footer trên mobile | Lần 8 |

## 1. Yêu cầu ban đầu (System/Initial Prompt)

> *Prompt tổng quan dùng để khởi tạo khung website (Prompt 1):*

```text
Bạn là chuyên gia front-end. Hãy tạo website du lịch "Khám phá các vùng đất Việt Nam" gồm index.html, style.css và thư mục assets/images/ (để trống).

RÀNG BUỘC BẮT BUỘC:
- Chỉ HTML5 + CSS thuần. Chưa dùng thư viện nào ở bước này.
- Semantic HTML5: <header> chứa logo và <nav>; <main> chứa nhiều <section> (hero, giới thiệu, vùng miền, điểm đến nổi bật, liên hệ); <article> cho từng card vùng đất; <footer> có thông tin liên hệ và copyright. Mỗi section có <h2>. Hạn chế <div>, chỉ dùng khi thật sự không có thẻ ngữ nghĩa phù hợp. Có <figure>/<figcaption> cho ảnh.
- Điều hướng dùng link neo (#gioi-thieu, #vung-mien...) trỏ tới id của section.
- CSS Variables trong :root cho: màu (--primary-color, --secondary-color, --accent-color, --bg-color, --text-color), font (--font-heading, --font-body), spacing (--space-sm/md/lg/xl), bo góc, bóng đổ. Mọi giá trị trong style.css phải dùng var(), không hard-code màu lặp lại.
- Layout dùng Flexbox cho header/nav, CSS Grid cho lưới card (auto-fit, minmax). Responsive bằng media query cho mobile, tablet.
- Tuyệt đối KHÔNG viết JavaScript, không dùng thẻ <script>.
- Bảng màu lấy cảm hứng xanh biển và xanh lá Việt Nam, font Google Fonts hợp du lịch.
- Nội dung tiếng Việt thật, có ý nghĩa (không dùng lorem ipsum).
- Thêm lang="vi", meta viewport, thẻ <title> và meta description.
```

**Lý do thiết kế prompt như vậy:**
- Nêu vai trò ("chuyên gia front-end") để AI trả lời đúng chuyên môn.
- Liệt kê cụ thể từng thẻ ngữ nghĩa bắt buộc và yêu cầu hạn chế `<div>`, nhằm đáp ứng tiêu chí Semantic HTML5.
- Yêu cầu CSS Variables và cấm hard-code màu ngay từ đầu để mã nguồn nhất quán.
- Cấm JavaScript ngay từ đầu để AI không tự sinh thêm mã ngoài phạm vi tôi cần.
- Chưa dùng thư viện ở bước này để tách rõ từng giai đoạn, dễ kiểm tra lỗi.

## 2. Quá trình tinh chỉnh & Giải quyết sự cố (Iteration)

### Lần 1: Dựng khung Semantic HTML & CSS Variable
- **Prompt đã dùng:** Prompt 1 (ở mục 1) để dựng khung, sau đó Prompt 2 (ở phần Debug prompt bên dưới) để sửa lỗi.
- **Kết quả AI tạo ra:** [CẦN ĐIỀN: mô tả ngắn kết quả thật: các section đã có (hero, giới thiệu, vùng miền, điểm đến, liên hệ), các thẻ ngữ nghĩa đã dùng, các biến trong :root]
- **Nhận xét & Tinh chỉnh của SV:** Sau khi kiểm tra mã, tôi phát hiện AI còn dùng [CẦN ĐIỀN: số lượng] thẻ `<div>` không cần thiết và còn một số giá trị màu, khoảng cách viết trực tiếp thay vì dùng biến. Tôi nhắc AI thay `<div>` bằng thẻ ngữ nghĩa phù hợp (`<section>`, `<article>`, `<figure>`, `<ul>/<li>`) và đưa toàn bộ giá trị hard-code về CSS Variables, đồng thời yêu cầu giữ nguyên giao diện.
- **Thách thức gặp phải:** AI có xu hướng dùng `<div>` làm khung chung và hard-code giá trị lặp lại dù prompt đã nêu ràng buộc.
- **Cách sửa prompt (Debug prompt):** (Prompt 2)

```text
Rà soát lại index.html: hiện có [n] thẻ <div> không cần thiết. Hãy thay bằng thẻ ngữ nghĩa phù hợp (<section>, <article>, <figure>, <ul>/<li>...) và giữ nguyên giao diện. Trong style.css, thay toàn bộ giá trị màu/khoảng cách hard-code bằng CSS Variables đã khai báo. Chỉ nêu danh sách thay đổi, rồi sửa trực tiếp trong file.
```

> Ghi chú: [CẦN ĐIỀN: thay `[n]` bằng số thẻ `<div>` thật mà bạn đã đếm khi gửi prompt]

### Lần 2: Gắn hình ảnh vào website
- **Prompt đã dùng:** (Prompt 3)

```text
Trong thư mục assets/images/ hiện có các file: [dán danh sách tên file].
Hãy gắn ảnh vào website theo các quy tắc sau, và sửa trực tiếp trong index.html và style.css:
1. Đường dẫn dùng đường dẫn TƯƠNG ĐỐI và khớp chính xác tên file, đúng chữ hoa/thường (ví dụ assets/images/ha-long.jpg). Trong style.css, đường dẫn url() tính từ vị trí file style.css.
2. Ảnh hero dùng background-image trong CSS; ảnh card và điểm đến dùng thẻ <img> đặt trong <figure> kèm <figcaption>.
3. Mỗi <img> có alt tiếng Việt mô tả cụ thể (ví dụ: "Vịnh Hạ Long với những hòn đảo đá vôi lúc hoàng hôn"), có width và height để tránh giật layout, và loading="lazy" cho ảnh bên dưới màn hình đầu (không áp dụng cho ảnh hero).
4. CSS cho ảnh: object-fit: cover, tỉ lệ khung đồng nhất bằng aspect-ratio, bo góc dùng CSS Variables đã có.
5. Không thêm JavaScript.
6. Cuối cùng liệt kê ảnh nào gắn vào phần tử nào, và file nào trong thư mục chưa được dùng.
```

> Ghi chú: [CẦN ĐIỀN: ghi danh sách tên file ảnh thật bạn đã dán vào prompt]

- **Kết quả AI tạo ra:** [CẦN ĐIỀN: ảnh hero, ảnh card, ảnh điểm đến đã được gắn ở đâu; AI có báo file nào chưa dùng không]
- **Nhận xét & Tinh chỉnh của SV:** Vì AI không biết trước các file trong thư mục `assets/images/`, tôi đã liệt kê tên file thật vào prompt và yêu cầu khớp chính xác chữ hoa/thường, vì GitHub Pages phân biệt hoa thường. Tôi cũng yêu cầu `alt` mô tả cụ thể, `width`/`height` để tránh giật layout và `loading="lazy"` cho ảnh bên dưới màn hình đầu để tải nhanh hơn.
- **Thách thức gặp phải:** [CẦN ĐIỀN: lỗi ảnh thật bạn gặp nếu có, ví dụ ảnh không hiện do sai đường dẫn, ảnh quá nặng, ảnh bị méo; nếu không có lỗi, ghi cách bạn đã kiểm tra bằng tab Network trong DevTools]
- **Cách sửa prompt (Debug prompt):** [CẦN ĐIỀN: prompt sửa nếu có, hoặc ghi "Không phát sinh lỗi ở bước này"]

### Lần 3: Hoàn thiện UI, hover/focus, font chữ và menu hamburger CSS thuần
- **Prompt đã dùng:** (Prompt 4)

```text
Nâng cấp style.css cho giao diện đẹp và hoàn thiện, vẫn không dùng JavaScript:
- Hero section: ảnh nền toàn màn hình (background-image + overlay gradient), chữ căn giữa, nút CTA nổi bật.

- Nút bấm: viết tay :hover (nâng nhẹ bằng transform, đổi màu, đổ bóng) và :focus-visible (outline rõ ràng, đạt chuẩn truy cập), transition mượt.
- font chữ roboto
- làm menu hamburger trên giao diện mobile toàn bộ bằng css không dùng javascript (Sử dụng kỹ thuật input[type="checkbox"] ẩn kết hợp với nhãn (<label>) làm nút hamburger.

Sử dụng CSS selector :checked để khi người nhận vào nút, menu vùng miền sẽ trượt xuống hoặc hiển thị mượt mà trên màn hình mobile.

Đảm bảo giao diện responsive bằng CSS Flexbox/Grid."*)
- Card vùng đất: :hover phóng nhẹ ảnh (overflow hidden + transform scale), nâng card bằng box-shadow; :focus-within cho card.
- Menu nav: gạch chân chạy khi hover bằng ::after, header dính trên cùng (position: sticky) có nền mờ.
- Section xen kẽ màu nền, tiêu đề section có đường nhấn nhỏ bằng ::after.
- Responsive: menu xếp gọn ở màn hình nhỏ bằng Flexbox (wrap), KHÔNG dùng JS hay hamburger JS.
- Mọi giá trị vẫn dùng CSS Variables; thêm --transition, --shadow-hover.
- Thêm scroll-behavior: smooth cho html.
```

- **Kết quả AI tạo ra:** [CẦN ĐIỀN: hero toàn màn hình, nút CTA, menu sticky, hamburger trên mobile, hover/focus của nút và card]
- **Nhận xét & Tinh chỉnh của SV:** Ở bước này tôi gộp nhiều yêu cầu giao diện vào một prompt: hero, nút bấm, font Roboto, hover/focus cho card, menu, màu xen kẽ giữa các section. Tôi bổ sung yêu cầu menu hamburger hoàn toàn bằng CSS (ô `input[type="checkbox"]` ẩn kết hợp `<label>` làm nút hamburger, dùng `:checked` để mở menu) vì ràng buộc của dự án là không dùng JavaScript. Tôi cũng yêu cầu `:focus-visible` cho nút để đảm bảo khả năng truy cập bằng bàn phím.
- **Thách thức gặp phải:** [CẦN ĐIỀN: lỗi thật nếu có, ví dụ menu hamburger không mở/đóng đúng, hoặc menu che nội dung, hoặc font Roboto chưa áp dụng]
- **Cách sửa prompt (Debug prompt):** [CẦN ĐIỀN: prompt sửa nếu có]

### Lần 4: Tích hợp thư viện Animate.css
- **Prompt đã dùng:** (Prompt 5)

```text
Tích hợp Animate.css bản 4.1.1 qua CDN (cdnjs) trong <head>, đặt TRƯỚC style.css. Áp dụng hiệu ứng bằng class:
- Tiêu đề h1 ở hero: animate__animated animate__fadeInDown
- Đoạn mô tả hero: animate__animated animate__fadeInUp kèm animate__delay-1s
- Nút CTA hero: animate__animated animate__pulse animate__infinite animate__slow (hoặc animate__heartBeat), ưu tiên nhẹ nhàng
- Logo ở header: animate__animated animate__fadeInLeft
Không lạm dụng: chỉ dùng ở các phần tử chính trên màn hình đầu. Thêm @media (prefers-reduced-motion: reduce) trong style.css để tắt animation cho người dùng cần giảm chuyển động. Không viết JavaScript.
```

- **Kết quả AI tạo ra:** [CẦN ĐIỀN: Animate.css đã được nhúng chưa, các class đã gắn ở phần tử nào]
- **Nhận xét & Tinh chỉnh của SV:** Tôi chỉ định rõ phiên bản (4.1.1), nguồn CDN và thứ tự nhúng (đặt trước `style.css` để CSS của tôi có thể ghi đè khi cần). Tôi giới hạn hiệu ứng ở các phần tử chính ở màn hình đầu (logo, tiêu đề, mô tả, nút CTA) để tránh lạm dụng gây rối mắt, và thêm `prefers-reduced-motion` để tắt chuyển động cho người dùng cần giảm chuyển động.
- **Thách thức gặp phải:** [CẦN ĐIỀN: lỗi thật nếu có, ví dụ hiệu ứng không chạy do sai thứ tự nhúng hoặc sai tên class, hoặc nút CTA nhấp nháy quá mạnh]
- **Cách sửa prompt (Debug prompt):** [CẦN ĐIỀN: prompt sửa nếu có]

### Lần 5: Hiệu ứng xuất hiện khi cuộn trang bằng CSS thuần (thay thế AOS)
- **Bối cảnh & quyết định:** AOS (Animate On Scroll) là thư viện JavaScript, cần dòng khởi tạo `AOS.init()` để hoạt động. Nếu chỉ nhúng `aos.css` mà không có JavaScript thì các phần tử gắn `data-aos` sẽ bị ẩn. Vì yêu cầu của tôi cho dự án này là **không dùng JavaScript**, tôi quyết định dùng giải pháp tương đương về chức năng: hiệu ứng cuộn viết bằng CSS thuần (CSS scroll-driven animations với `animation-timeline: view()`). [CẦN ĐIỀN (nếu có): Giảng viên đã xác nhận cho phép thay thế: dán nội dung trả lời.]
- **Prompt đã dùng:** (Prompt 6)

```text
Không dùng JavaScript, không thẻ <script>, không dùng thư viện AOS. Tạo hiệu ứng "xuất hiện khi cuộn trang" bằng CSS thuần (scroll-driven animations) trong style.css.

Yêu cầu:
1. Khai báo CSS Variables mới trong :root: --reveal-distance: 40px; --reveal-duration-range: 35%.
2. Định nghĩa các @keyframes riêng (tên rõ nghĩa, không trùng tên keyframes của Animate.css): reveal-up (opacity 0 + translateY(var(--reveal-distance)) → hiện), reveal-left (translateX(-40px)), reveal-right (translateX(40px)), reveal-zoom (scale(0.92)).
3. Tạo các class tiện ích: .reveal-up, .reveal-left, .reveal-right, .reveal-zoom. QUAN TRỌNG: toàn bộ khai báo animation phải nằm TRONG:
   @supports (animation-timeline: view()) { ... }
   để trình duyệt không hỗ trợ vẫn hiển thị nội dung bình thường (TUYỆT ĐỐI không để opacity: 0 ngoài @supports).
   Mỗi class dùng: animation-name tương ứng; animation-timing-function: ease-out; animation-fill-mode: both; animation-timeline: view(); animation-range: entry 0% entry var(--reveal-duration-range);
4. Hiệu ứng lệch nhau cho card trong lưới: dùng :nth-child(2) / (3) để đổi animation-range (vd entry 10% entry 45%, entry 20% entry 55%), không dùng animation-delay.
5. Gắn class cho HTML: tiêu đề h2 của mỗi section → .reveal-up; card vùng đất (article) → .reveal-up hoặc .reveal-zoom; khối giới thiệu xen kẽ .reveal-left / .reveal-right; ảnh → .reveal-zoom. KHÔNG gắn lên hero (hero đã dùng Animate.css), header, footer.
6. Tránh tràn ngang khi dùng reveal-left/right: thêm overflow-x: clip cho <main>.
7. Trong @media (prefers-reduced-motion: reduce) tắt hết hiệu ứng reveal (animation: none; opacity: 1; transform: none).
8. Chỉ chọn hiệu ứng nhất quán, mượt, không gây rối mắt. Sau khi sửa, liệt kê những class đã gắn vào phần tử nào.
```

- **Kết quả AI tạo ra:** [CẦN ĐIỀN: các @keyframes và class .reveal-* AI đã tạo, các phần tử đã được gắn class]
- **Nhận xét & Tinh chỉnh của SV:** Tôi bắt buộc đặt mọi khai báo animation trong `@supports` để trình duyệt chưa hỗ trợ (ví dụ Firefox) vẫn hiển thị đủ nội dung thay vì bị ẩn. Tôi cũng cấm dùng `animation-delay` cho hiệu ứng lệch nhịp của card (vì animation chạy theo vị trí cuộn chứ không theo thời gian) và yêu cầu dùng `animation-range` khác nhau theo `:nth-child`. Tôi thêm `overflow-x: clip` cho `<main>` để hiệu ứng trượt trái/phải không gây thanh cuộn ngang.
- **Thách thức gặp phải:** Khi thử, tôi thấy hiệu ứng xuất hiện của card kết thúc khi card chưa vào hết khung nhìn nên trông giật, và hiệu ứng `transform` của animation có nguy cơ xung đột với `transform` dùng cho hover của card.
- **Cách sửa prompt (Debug prompt):** (Prompt 7)

```text
Hiệu ứng xuất hiện của card kết thúc khi card chưa vào hết khung nhìn nên trông giật. Điều chỉnh animation-range thành entry 5% entry 50% cho .reveal-up, giữ phần lệch theo :nth-child, và kiểm tra không xung đột với transition hover của card (hover dùng transform thì tách phần tử: animate trên <article>, hover trên ảnh bên trong để không đè nhau).
```

### Lần 6: Lật thẻ card, hiệu ứng cuộn nâng cao và hover nổi bật
- **Prompt đã dùng:** (Prompt 8)

```text
Nâng cấp website hiện tại, chỉ sửa index.html và style.css. TUYỆT ĐỐI không viết JavaScript, không thẻ <script>, không dùng AOS hay thư viện JS nào. Giữ nguyên Semantic HTML5, CSS Variables, Flexbox/Grid và Animate.css đã có ở hero.

PHẦN A. HIỆU ỨNG LẬT THẺ CHO CARD VÙNG ĐẤT (CSS thuần)
1. Đổi mỗi card vùng đất thành cấu trúc sau (giữ <article> làm thẻ ngoài, thêm tabindex="0"):
<article class="card reveal-up" tabindex="0">
  <div class="card-inner">
    <figure class="card-front"> <img ...> <figcaption><h3>Tên</h3><p>Vùng miền</p></figcaption> </figure>
    <section class="card-back"> <h3>Tên</h3> <p>Mô tả 2-3 câu</p> <a class="btn" href="#lien-he">Tìm hiểu thêm</a> </section>
  </div>
</article>
Mặt trước: ảnh + tên. Mặt sau: mô tả, thời điểm đẹp nhất, món nên thử, nút CTA. Nội dung tiếng Việt thật.
2. CSS: .card có perspective: 1000px và aspect-ratio đồng nhất; .card-inner có position: relative, width/height 100%, transform-style: preserve-3d, transition: transform 0.7s; .card-front và .card-back có position: absolute, inset: 0, backface-visibility: hidden (kèm -webkit-backface-visibility); .card-back có transform: rotateY(180deg).
3. Lật khi .card:hover .card-inner và .card:focus-within .card-inner { transform: rotateY(180deg); }. Thêm :focus-visible cho .card với outline rõ ràng dùng CSS Variables.
4. Với @media (hover: none): giữ lật bằng :focus-within (chạm vào card để lật).
5. @media (prefers-reduced-motion: reduce): bỏ xoay 3D, thay bằng hiệu ứng mờ đổi mặt (opacity), không dùng transform xoay.

PHẦN B. HIỆU ỨNG CUỘN (CSS scroll-driven animations, thay cho AOS)
1. Thêm vào :root: --reveal-distance: 40px.
2. Toàn bộ khai báo animation-timeline phải nằm TRONG @supports (animation-timeline: view()) { ... }. Ngoài @supports tuyệt đối không đặt opacity: 0 hay transform ban đầu, để trình duyệt không hỗ trợ vẫn hiển thị đầy đủ nội dung.
3. Keyframes riêng, tên không trùng Animate.css: reveal-up (opacity 0→1, translateY(var(--reveal-distance))→0), reveal-left, reveal-right (translateX ±40px), reveal-zoom (scale 0.92→1).
4. Class .reveal-up/.reveal-left/.reveal-right/.reveal-zoom: animation-name tương ứng; animation-timing-function: ease-out; animation-fill-mode: both; animation-timeline: view(); animation-range: entry 5% entry 45%.
5. Card xuất hiện lệch nhau: .card:nth-child(3n+2) dùng animation-range: entry 12% entry 52%; .card:nth-child(3n) dùng entry 20% entry 60%. KHÔNG dùng animation-delay.
6. Hiệu ứng "mờ dần khi cuộn đi" cho hero: thêm class .hero-fade vào khối bao ngoài nội dung hero (không gắn lên h1, đoạn mô tả hay nút đang dùng Animate.css). Keyframes hero-out: opacity 1→0.2 và translateY(0)→-30px; animation-timeline: view(); animation-range: exit 10% exit 80%; animation-fill-mode: both.
7. (Tùy chọn, thêm nếu gọn) thanh tiến độ cuộn: một phần tử trang trí fixed ở đầu trang, cao 4px, nền var(--accent-color), transform-origin: left, animation: progress linear; animation-timeline: scroll(root); keyframes scaleX(0)→scaleX(1). Phần tử này dùng aria-hidden="true", đặt ngay đầu <body>.
8. Gắn class: h2 của mỗi section → .reveal-up; khối giới thiệu xen kẽ .reveal-left / .reveal-right; ảnh lớn → .reveal-zoom; card vùng đất → .reveal-up. KHÔNG gắn lên header, footer, hero chính (ngoài .hero-fade).
9. Thêm overflow-x: clip cho <main> để reveal-left/right không gây thanh cuộn ngang.
10. @media (prefers-reduced-motion: reduce): tắt mọi animation cuộn (animation: none; opacity: 1; transform: none) và ẩn thanh tiến độ.

PHẦN C. HOVER / FOCUS NỔI BẬT (tự viết, dùng CSS Variables)
1. Card: .card:hover { translate: 0 -8px; box-shadow: var(--shadow-hover); } — dùng thuộc tính "translate" riêng, KHÔNG dùng transform, để không bị đè bởi animation cuộn. Thêm transition cho translate và box-shadow.
2. Ảnh mặt trước card: khi hover, filter: brightness(1.08) saturate(1.1) nhẹ.
3. Section: section:hover nhẹ nhàng nổi bật bằng cách làm đường nhấn ::after dưới h2 dài ra (width từ 60px lên 120px) và đổi màu sang var(--accent-color); không đổi nền cả section.
4. Nút .btn: :hover nâng nhẹ + đổi màu + đổ bóng; :focus-visible có outline 3px offset 3px.
5. Link menu: gạch chân chạy bằng ::after khi :hover và :focus-visible.
6. Mọi transition dùng biến --transition. Không lạm dụng: tối đa 1 hiệu ứng nhấn mỗi phần tử.

Sau khi sửa: (a) liệt kê class đã gắn ở phần tử nào, (b) xác nhận không còn <script> nào trong dự án, (c) xác nhận mọi opacity: 0 chỉ nằm trong @keyframes hoặc @supports.
```

- **Kết quả AI tạo ra:** [CẦN ĐIỀN: card lật được chưa, hiệu ứng hero mờ dần, thanh tiến độ (nếu có), hover của card và section]
- **Nhận xét & Tinh chỉnh của SV:** Tôi cung cấp sẵn cấu trúc HTML mẫu của card để AI không tự đoán cấu trúc. Tôi tách rõ vai trò từng lớp để các hiệu ứng không đè lên nhau: hiệu ứng cuộn dùng `transform` trên thẻ ngoài `<article>`, hiệu ứng lật dùng `transform` trên `.card-inner`, còn hiệu ứng nâng card khi hover dùng thuộc tính riêng `translate` (không dùng `transform`) nên không bị animation cuộn ghi đè. Tôi thêm `tabindex="0"` (thuộc tính HTML, không phải JavaScript) và `:focus-within` để card lật được bằng bàn phím và cảm ứng, và thêm `prefers-reduced-motion` để thay xoay 3D bằng hiệu ứng mờ đổi mặt.
- **Giải thích kiến trúc:** Card dùng `<article>` (thẻ ngoài, chịu hiệu ứng cuộn). Thẻ `<div class="card-inner">` là cần thiết về mặt kỹ thuật cho hiệu ứng lật 3D (`transform-style: preserve-3d`) nên đây không phải `<div>` vô nghĩa. Mặt trước dùng `<figure>`/`<figcaption>`, mặt sau dùng `<section>`.
- **Thách thức gặp phải:** [CẦN ĐIỀN: lỗi thật nếu có, ví dụ mặt sau hiện ngược chữ, nội dung mặt sau bị tràn, hover và lật xung đột, hiệu ứng quá nhiều gây rối]
- **Cách sửa prompt (Debug prompt):** [CẦN ĐIỀN: prompt sửa nếu có]

### Lần 7: Đơn giản hóa hiệu ứng cuộn về một class dùng chung
- **Prompt đã dùng:** (Prompt 9)

```text
Chỉ sửa style.css và index.html, không dùng JavaScript hay thẻ <script>.
Thêm hiệu ứng "hiện dần khi cuộn tới" bằng CSS thuần:

1. Thêm keyframes:
@keyframes hien-len { from { opacity: 0; transform: translateY(60px); } to { opacity: 1; transform: translateY(0); } }

2. Thêm quy tắc, bắt buộc nằm trong @supports và dòng animation-timeline đặt SAU dòng animation:
@supports (animation-timeline: view()) {
  .hieu-ung-cuon {
    animation: hien-len linear both;
    animation-timeline: view();
    animation-range: entry 0% entry 60%;
  }
}

3. Tuyệt đối không đặt opacity: 0 hay transform ban đầu ở ngoài @supports hoặc ngoài @keyframes.

4. Thêm class hieu-ung-cuon vào: tiêu đề h2 của mọi section, mỗi <article class="card">, và ảnh lớn. Không thêm vào header, footer và khối hero.

5. Thêm @media (prefers-reduced-motion: reduce) { .hieu-ung-cuon { animation: none; } }

6. Cuối cùng liệt kê phần tử nào đã được gắn class, và xác nhận không có <script> nào trong dự án.
```

- **Kết quả AI tạo ra:** [CẦN ĐIỀN: hiệu ứng hiện dần có chạy không, các phần tử đã gắn class `hieu-ung-cuon`]
- **Nhận xét & Tinh chỉnh của SV:** Sau khi thử, tôi thấy bộ hiệu ứng cuộn ở Prompt 6 và Prompt 8 (nhiều class, lệch nhịp theo `:nth-child`, hero mờ dần, thanh tiến độ) còn phức tạp và khó kiểm soát lỗi. Tôi quyết định đưa về một hiệu ứng duy nhất, một class `hieu-ung-cuon` dùng chung, để dễ kiểm tra, ít xung đột và không gây rối mắt. Tôi đặc biệt lưu ý thứ tự khai báo: `animation-timeline` phải đặt sau dòng `animation`, vì dòng `animation` viết tắt sẽ đặt lại giá trị timeline nếu đứng sau.
- **Thách thức gặp phải:** [CẦN ĐIỀN: lý do cụ thể khiến bạn đơn giản hóa, ví dụ hiệu ứng chưa rõ, bị giật, xung đột với hover hoặc lật thẻ]
- **Cách sửa prompt (Debug prompt):** Chính Prompt 9 ở trên là bản sửa, tôi viết sẵn từng dòng CSS cần có thay vì mô tả chung chung.

### Lần 8: Sửa lỗi tràn chữ ở phần Liên hệ và footer trên mobile
- **Prompt đã dùng:** (Prompt 10, đồng thời là Debug prompt)
- **Kết quả AI tạo ra:** [CẦN ĐIỀN: AI đã sửa những gì ở phần Liên hệ và footer]
- **Nhận xét & Tinh chỉnh của SV:** Khi kiểm tra bằng chế độ giả lập thiết bị của trình duyệt ở 360px và 390px, tôi thấy chữ và nội dung ở phần "Liên hệ với chúng tôi" và `<footer>` bị tràn ra ngoài khung. Tôi xác định phần tử gây tràn rồi viết prompt nêu rõ từng nguyên nhân có thể có: chữ dài không tự xuống dòng (`overflow-wrap`), lưới có cột rộng cố định (`minmax(min(100%, 240px), 1fr)`), flex không `wrap`, chiều rộng cố định bằng px, `white-space: nowrap`, và cỡ chữ tiêu đề quá lớn (`clamp()`). Tôi yêu cầu giữ nguyên giao diện desktop, Semantic HTML5, CSS Variables và các hiệu ứng đã có.
- **Thách thức gặp phải:** Nội dung phần Liên hệ và footer (email, liên kết, địa chỉ) có chuỗi dài không có điểm ngắt, nên tràn khung trên màn hình hẹp. [CẦN ĐIỀN: phần tử cụ thể gây tràn mà bạn tìm ra]
- **Cách sửa prompt (Debug prompt):**

```text
Trên giao diện mobile (360px, 390px), phần "Liên hệ với chúng tôi" và <footer> bị tràn chữ/nội dung ra ngoài khung. Chỉ sửa style.css và index.html, không dùng JavaScript, không đổi giao diện trên desktop.

Hãy rà soát và sửa:
1. Mọi chữ dài trong phần liên hệ và footer (email, link, số điện thoại, địa chỉ, dòng copyright): thêm overflow-wrap: anywhere; và word-break: normal.
2. Mọi lưới (Grid) trong hai phần này: không dùng cột rộng cố định; dùng grid-template-columns: repeat(auto-fit, minmax(min(100%, 240px), 1fr)); trên mobile chuyển thành 1 cột.
3. Mọi flex container: thêm flex-wrap: wrap; mọi flex item có chữ dài thêm min-width: 0.
4. Loại bỏ width cố định bằng px; thay bằng width: 100%; max-width: ...; box-sizing: border-box cho input, textarea, nút, ảnh.
5. Xóa white-space: nowrap nếu có (trừ logo).
6. Dùng padding-inline: var(--space-md) cho khối bao ngoài, kiểm tra không có phần tử nào rộng hơn 100% màn hình.
7. Cỡ chữ tiêu đề của hai phần dùng clamp() để không tràn ở 360px.
8. Thêm vào cuối <html>/<body> kiểm tra: không có thanh cuộn ngang ở 320px, 360px, 390px, 768px.
9. Giữ nguyên Semantic HTML5 (<footer>, <address> cho thông tin liên hệ nếu hợp lý), CSS Variables và các hiệu ứng đã có.

Cuối cùng liệt kê từng thay đổi và phần tử nào gây tràn.
```

## 3. Đánh giá cá nhân về quá trình Vibe Coding

- **AI đã giúp bạn tiết kiệm thời gian ở phần nào nhất?**
  Dựng khung HTML với nhiều section, viết CSS Variables, lưới Grid, hiệu ứng lật thẻ 3D. Nên ước lượng thời gian tiết kiệm, ví dụ "viết tay khoảng X phút, dùng AI còn Y phút".

- **Bài học rút ra khi đặt câu hỏi (Prompt) cho AI trong thiết kế web?**
  - Chia nhỏ yêu cầu, mỗi prompt một mục tiêu (khung HTML, hình ảnh, giao diện, thư viện, hiệu ứng, sửa lỗi responsive) để dễ kiểm tra và sửa lỗi.
  - Nêu ràng buộc rõ ngay từ đầu (không JavaScript, thẻ ngữ nghĩa bắt buộc, dùng CSS Variables) để AI không tự ý làm khác.
  - Cung cấp thông tin AI không thể tự biết (danh sách file ảnh thật, cấu trúc HTML mẫu của card) thay vì để AI đoán.
  - Khi sửa lỗi, mô tả cụ thể triệu chứng, kích thước màn hình bị lỗi và nguyên nhân nghi ngờ, thay vì chỉ nói "bị lỗi".
  - Luôn tự kiểm tra kết quả: đếm thẻ `<div>`, xem trên giả lập mobile 360px, xem trên trình duyệt khác. AI không tự phát hiện hết lỗi.


- **AI đã làm sai điều gì và bạn đã chỉ đạo lại ra sao?**
  AI dùng quá nhiều `<div>` và hard-code màu (Lần 1), hiệu ứng card kết thúc quá sớm gây giật (Lần 5), tràn chữ ở mobile (Lần 8).

- **Hạn chế còn lại của sản phẩm:**
  Hiệu ứng cuộn bằng `animation-timeline` chỉ hoạt động trên các trình duyệt hỗ trợ tính năng này; trình duyệt chưa hỗ trợ vẫn hiển thị đủ nội dung nhờ khối `@supports`. 
