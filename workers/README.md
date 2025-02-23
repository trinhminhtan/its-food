## Cloudflare Worker API cho Ứng Dụng Đặt Món

Đây là code JavaScript cho một Cloudflare Worker, đóng vai trò là backend API cho ứng dụng đặt món trưa. Worker này xử lý các request liên quan đến việc lưu trữ và truy xuất dữ liệu đơn hàng.

**Chức Năng Chính:**

1.  **Xử Lý Request (Hàm `fetch`):**

    *   Nhận `request` (thông tin về request HTTP) và `env` (biến môi trường, chứa thông tin về bucket R2).
    *   Phân tích URL của request (`request.url`).
    *   Gọi các hàm xử lý tương ứng dựa trên phương thức (method) và đường dẫn (pathname) của request.
    *   Hàm sanitize để chống lại tấn công XSS

2.  **Xử Lý Đặt Hàng (POST `/api/orders`):**

    *   **Nhận Dữ Liệu:** Đọc dữ liệu JSON từ body của request (`request.json()`).
    *   **Kiểm Tra Dữ Liệu:**
        *   Đảm bảo các trường bắt buộc (`name`, `drink`, `quantity`) đều có dữ liệu.
        *   Nếu thiếu, trả về lỗi 400 (Bad Request) kèm thông báo lỗi.
    *   **Làm Sạch Dữ Liệu:**
        *   Sử dụng hàm `sanitizeInput` để loại bỏ các ký tự đặc biệt ( `<>"'(){}]`) có thể gây ra tấn công XSS.
        *   Áp dụng `sanitizeInput` cho các trường `name`, `drink`, `food`.
        *   Chuyển `quantity` sang kiểu string.
    *   **Lấy Dữ Liệu Hiện Tại:**
        *   Lấy file `orders.json` từ Cloudflare R2 bucket (`env.ORDERS_BUCKET`).
        *   Nếu file tồn tại, parse nội dung JSON thành đối tượng JavaScript.  Nếu không, khởi tạo một đối tượng rỗng `{}`.
    *   **Cập Nhật/Thêm Đơn Hàng:**
        *   Tạo/cập nhật mảng đơn hàng cho ngày hiện tại (`today`).
        *   *Quan trọng:* Lọc bỏ các đơn hàng cũ có cùng tên (`order.name !== data.name`), để nếu có đơn hàng trùng tên, đơn hàng mới sẽ ghi đè.
        *   Thêm đơn hàng mới vào mảng.
    *   **Lưu Dữ Liệu:** Ghi đối tượng `orders` (đã cập nhật) trở lại file `orders.json` trong R2 bucket.
    *   **Trả Về Phản Hồi:** Trả về response JSON với thông báo thành công (status code 200).  Thiết lập các header CORS để cho phép truy cập từ các domain khác.
    *   **Xử Lý Lỗi:** Nếu có lỗi trong quá trình xử lý (ví dụ: lỗi parse JSON, lỗi kết nối R2), trả về lỗi 500 (Internal Server Error) kèm thông báo lỗi.

3.  **Lấy Danh Sách Đơn Hàng (GET `/api/orders`):**

    *   **Lấy Dữ Liệu:** Lấy file `orders.json` từ R2 bucket.
    *   **Parse JSON:** Parse nội dung JSON thành đối tượng JavaScript. Nếu file không tồn tại, trả về một đối tượng rỗng.
    *   **Trả Về Dữ Liệu:** Trả về response JSON chứa dữ liệu đơn hàng (status code 200).  Thiết lập header CORS.
    *   **Xử Lý Lỗi:** Nếu có lỗi (ví dụ: lỗi kết nối R2), trả về lỗi 500 kèm thông báo.

4.  **Xử Lý OPTIONS Request (CORS Preflight):**

    *   Nếu phương thức là `OPTIONS`, trả về response 204 (No Content) với các header CORS cần thiết để trình duyệt cho phép các request `GET` và `POST` từ các domain khác.

5.  **Not Found (404):**

    *   Nếu không có route nào khớp với request, trả về lỗi 404 (Not Found).

**Chi tiết quan trọng:**

*   **Cloudflare R2:** Dữ liệu đơn hàng được lưu trữ trong một Cloudflare R2 bucket.  Tên bucket được lưu trong biến môi trường `ORDERS_BUCKET`.  Bạn cần tạo bucket này và cấu hình Worker để sử dụng nó.
*   **`orders.json`:**  Tất cả dữ liệu được lưu trong một file JSON duy nhất tên là `orders.json`.  Cấu trúc của file này là một đối tượng, với các key là ngày (định dạng `YYYY-MM-DD`), và giá trị là mảng các đơn hàng của ngày đó.
*   **Sanitize Input:** Hàm `sanitizeInput` rất quan trọng để ngăn chặn các tấn công XSS (Cross-Site Scripting).
*    **CORS:** Các header `Access-Control-Allow-Origin`, `Access-Control-Allow-Methods`, `Access-Control-Allow-Headers` được thiết lập để cho phép truy cập API từ các domain khác (ví dụ: từ trang web của bạn chạy trên GitHub Pages).  `Access-Control-Allow-Origin: *` cho phép tất cả các domain, bạn có thể giới hạn lại nếu cần.
* **Error Handling:** Code có xử lý lỗi cơ bản (try-catch), trả về các mã lỗi HTTP phù hợp (400, 500).

**Cách triển khai:**

1.  **Tạo R2 Bucket:** Trong dashboard Cloudflare, tạo một R2 bucket.  Ghi nhớ tên bucket.
2.  **Tạo Worker:** Tạo một Cloudflare Worker.
3.  **Copy Code:** Copy code JavaScript trên vào Worker.
4.  **Cấu Hình:**
    *   Trong phần Settings của Worker, vào mục Variables.
    *   Thêm một biến môi trường tên là `ORDERS_BUCKET`, giá trị là tên R2 bucket bạn đã tạo.
    *   Trong phần Settings, vào mục Triggers chọn route, thêm route `*/*` (hoặc route cụ thể hơn nếu bạn muốn).
5.  **Deploy:** Deploy Worker.  Bạn sẽ nhận được một URL của Worker (ví dụ: `https://<your-worker-name>.<your-subdomain>.workers.dev`).  Đây chính là `API_URL` mà bạn cần dùng trong code HTML/JavaScript của ứng dụng frontend.

Code này cung cấp một backend API đơn giản, an toàn (với `sanitizeInput`) và có khả năng mở rộng (nhờ Cloudflare Workers và R2).