# Dự án Đặt Món Trưa Trực Tuyến

Đây là một ứng dụng web đơn giản cho phép người dùng đặt món trưa, hiển thị danh sách đơn hàng, thống kê và xuất dữ liệu ra file Excel. Ứng dụng hỗ trợ đa ngôn ngữ (Tiếng Việt và Tiếng Trung phồn thể).

## Tính năng chính

*   **Đặt món:**
    *   Nhập tên người dùng.
    *   Chọn đồ uống từ danh sách (có thể tùy chỉnh).
    *   Nhập số lượng đồ uống.
    *   Chọn size đồ uống (Nhỏ, Vừa, Lớn).
    *   Thêm món ăn tùy chọn.
    *   Tùy chọn "Lấy tẩy đá" (checkbox).
    *   Nút "Gửi Đơn Hàng".
*   **Danh sách đơn hàng:**
    *   Hiển thị danh sách các đơn hàng trong ngày.
    *   Thông tin bao gồm: Tên, Thức uống, Món ăn (nếu có), Số lượng, Size, Tẩy đá (có/không).
*   **Thống kê:**
    *   Thống kê số lượng từng loại đồ uống và món ăn đã được đặt.
*   **Xuất Excel:**
    *   Nút "Xuất Excel" để tải danh sách đơn hàng về dưới dạng file `.xlsx`.
* **Đa ngôn ngữ:**
    *   Hỗ trợ Tiếng Việt và Tiếng Trung (phồn thể).
    *   Nút chuyển đổi ngôn ngữ ở góc trên bên phải.
*   **Đếm ngược thời gian:**
    *   Đồng hồ đếm ngược đến thời gian chốt đơn (22:00).
    *   Ẩn form đặt món khi hết giờ.
*   **Xử lý trùng tên:**
     * Cảnh báo nếu người dùng nhập trùng tên với đơn hàng đã có.
    *   Tùy chọn ghi đè hoặc hủy.
* **Thông báo:**
    *   Sử dụng SweetAlert2 cho các thông báo (thành công, lỗi, cảnh báo).
*   **Hiển thị spinner:**
    *   Hiển thị loading spinner khi đang tải dữ liệu hoặc xử lý tác vụ.

## Công nghệ sử dụng

*   **HTML:** Cấu trúc trang web.
*   **Tailwind CSS:** Framework CSS để xây dựng giao diện nhanh chóng.
*   **JavaScript (Vanilla JS):** Xử lý logic, tương tác người dùng, gọi API.
*   **Cloudflare Workers:** Nền tảng serverless để triển khai API (`https://r2bucket.wmtan.workers.dev/api/orders`).  **Lưu ý:**  Bạn cần thay thế URL này bằng URL API thực tế của bạn.
*   **Thư viện:**
    *   **`tailwindcss`:** (đã đề cập ở trên)
    *   **`font-awesome`:** Cung cấp các icon.
    *   **`xlsx`:** Xử lý file Excel (đọc và ghi).
    *   **`sweetalert2`:** Hiển thị thông báo đẹp mắt.

## Cấu trúc code

*   **HTML (`index.html`):**
    *   `<head>`:
        *   Thiết lập charset, viewport.
        *   Liên kết đến các file CSS và JS (Tailwind, Font Awesome, xlsx, SweetAlert2).
        *   Định nghĩa các style tùy chỉnh (ngoài Tailwind).
    *   `<body>`:
        *   Nút chuyển đổi ngôn ngữ.
        *   Loading spinner (ẩn mặc định).
        *   Tiêu đề trang.
        *   Đồng hồ đếm ngược.
        *   Form đặt món (`<form id="orderForm">`).
            *   Các trường input: tên, đồ uống, số lượng, size, món ăn, checkbox tẩy đá.
            *   Nút "Gửi Đơn Hàng".
        *   Bảng danh sách đơn hàng (`<table id="orderList">`).
        *   Nút "Xuất Excel".
        *   Danh sách thống kê (`<ul id="summaryList">`).
        *   Footer.
    *   `<script>`:
        *   Định nghĩa `API_URL`.
        *   Đối tượng `translations` chứa các chuỗi ngôn ngữ.
        *   Các hàm:
            *   `changeLanguage(lang)`: Chuyển đổi ngôn ngữ.
            *   `loadOrders()`: Tải danh sách đơn hàng từ API.
            *   `loadSummary()`: Tải thống kê từ API.
            *   `exportToExcel()`: Xuất dữ liệu ra file Excel.
            *   `getTimeRemaining()`: Tính thời gian còn lại đến giờ chốt đơn.
            *   `updateCountdown()`: Cập nhật đồng hồ đếm ngược.
        *   Event listener cho form submit, nút xuất Excel, nút chuyển ngôn ngữ, nút toggle phần đồ ăn.
        *   Khởi tạo: Chuyển về ngôn ngữ mặc định, gọi các hàm load dữ liệu, cập nhật đồng hồ.

*   **CSS (trong thẻ `<style>`):**
    *   Custom styles (không dùng Tailwind):
        *   Hiệu ứng hover cho nút.
        *   Ẩn các nút tăng/giảm của input number.
        *   Style cho đồng hồ đếm ngược.
        *   Style cho bảng đơn hàng.
        *   Style cho nút chuyển ngôn ngữ.
        *   Style focus cho các form element.
        *   Style cho nút "Gửi" và "Xuất Excel".
        *   Style cho tiêu đề.
        *   Style cho loading spinner.

## Các điểm cải tiến có thể có:

*   **Tách CSS:** Đưa phần CSS trong `<style>` ra một file `.css` riêng biệt.
*   **Tách JavaScript:** Tách JavaScript thành các module nhỏ hơn để dễ quản lý.
*   **Xác thực dữ liệu phía server:** Hiện tại, việc kiểm tra dữ liệu (ví dụ: số lượng > 0) chỉ thực hiện ở phía client.  Nên có thêm xác thực ở phía server (Cloudflare Workers) để đảm bảo an toàn.
*   **Lưu trữ dữ liệu:**  Hiện tại, dữ liệu đang được lưu trữ thông qua API `https://r2bucket.wmtan.workers.dev/api/orders`. Bạn cần triển khai backend để lưu trữ dữ liệu này (ví dụ: sử dụng Cloudflare R2, KV, D1, hoặc các cơ sở dữ liệu khác).
*   **Xử lý lỗi API:**  Cải thiện việc xử lý lỗi từ API.
*   **Responsive:** Mặc dù có sử dụng Tailwind, bạn có thể tối ưu hóa thêm cho các thiết bị có kích thước màn hình khác nhau.
*   **Accessibility:** Cải thiện khả năng truy cập (accessibility) cho người dùng sử dụng trình đọc màn hình (screen reader) hoặc các công nghệ hỗ trợ khác. Ví dụ:
    * Thêm `aria-label` cho các nút.
    * Đảm bảo cấu trúc HTML ngữ nghĩa.
    * Kiểm tra độ tương phản màu sắc.
* **Bảo mật:**
    * Nếu có thông tin nhạy cảm, cần mã hóa dữ liệu trước khi gửi lên server.
    * Cấu hình CORS (Cross-Origin Resource Sharing) trên Cloudflare Workers để chỉ cho phép trang web của bạn gọi API.
    * Sử dụng HTTPS.

## Hướng dẫn triển khai (tham khảo)

1.  **Tạo tài khoản Cloudflare:** Đăng ký tài khoản Cloudflare (nếu chưa có).
2.  **Tạo Worker:** Tạo một Cloudflare Worker mới.
3.  **Viết code API:** Viết code xử lý các request (GET, POST) trong Worker.  Bạn có thể tham khảo các ví dụ trong tài liệu của Cloudflare Workers.  Code này sẽ xử lý:
    *   Lấy danh sách đơn hàng (GET).
    *   Thêm/sửa đơn hàng (POST).
    *   (Tùy chọn) Kiểm tra trùng tên.
4.  **Lưu trữ dữ liệu:** Chọn một phương thức lưu trữ (R2, KV, D1, hoặc cơ sở dữ liệu bên ngoài) và kết nối Worker với cơ sở dữ liệu đó.
5.  **Triển khai Worker:** Deploy Worker của bạn.
6.  **Cập nhật `API_URL`:** Thay thế `API_URL` trong file `index.html` bằng URL của Worker bạn vừa triển khai.
7. **Upload code:** Tải code HTML, CSS, JS lên một hosting tĩnh (ví dụ: GitHub Pages, Netlify, Cloudflare Pages).

