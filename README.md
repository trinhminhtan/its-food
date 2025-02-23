# Ứng Dụng Đặt Món Trưa Trực Tuyến

Ứng dụng web cho phép người dùng đặt món trưa, xem danh sách đơn hàng, thống kê và xuất dữ liệu ra file Excel. Ứng dụng hỗ trợ đa ngôn ngữ (Tiếng Việt và Tiếng Trung phồn thể).

## Tính Năng Chi Tiết

1.  **Đặt Món:**

    *   **Form Đặt Món:**
        *   **Tên Người Dùng:**  Trường nhập text (`<input type="text">`) bắt buộc, yêu cầu người dùng nhập tên. Có placeholder hướng dẫn.
        *   **Thức Uống:**
            *   Trường nhập text kết hợp với danh sách gợi ý (`<input type="text" list="drinkOptions">` và `<datalist id="drinkOptions">`).
            *   Danh sách gợi ý được tạo động bằng JavaScript, lấy từ đối tượng `translations` (hỗ trợ đa ngôn ngữ).
            *   Bắt buộc chọn đồ uống.
        *   **Số Lượng Đồ Uống:**
            *   Trường nhập số (`<input type="number">`).
            *   Giá trị mặc định là 1, giá trị tối thiểu là 1.
            *   Có kiểm tra lỗi: số lượng phải là số và lớn hơn 0.
        *   **Size Đồ Uống:**
            *   Dropdown (`<select>`) với các lựa chọn: Nhỏ, Vừa, Lớn.
            *   Các option trong dropdown có hỗ trợ đa ngôn ngữ (dùng `data-translate`).
        *   **Món Ăn (Tùy Chọn):**
            *   Có một nút "toggle" để hiển thị/ẩn phần nhập món ăn.
            *   Khi hiển thị, có một trường nhập text (`<input type="text">`) để nhập món ăn.
            *   Trường này không bắt buộc.
            *   Có placeholder hướng dẫn.
        *   **Lấy Tẩy Đá:**
            *   Checkbox (`<input type="checkbox">`).
            *   Có hiệu ứng thay đổi icon (fa-snowflake/fa-check-circle) và màu sắc khi check/uncheck.
        *   **Nút "Gửi Đơn Hàng":**
            *   Gửi dữ liệu form lên server (sử dụng `fetch` với phương thức POST).
            *   Có hiển thị loading spinner trong quá trình gửi.
            *   Kiểm tra lỗi trước khi gửi:
                *   Tên không được để trống.
                *   Phải chọn đồ uống.
                *   Số lượng phải hợp lệ.
            *   Xử lý trường hợp trùng tên:
                *   Kiểm tra xem đã có đơn hàng nào với tên nhập vào chưa.
                *   Nếu có, hiển thị cảnh báo (SweetAlert2) với lựa chọn ghi đè hoặc hủy.
            *   Nếu gửi thành công:
                *   Hiển thị thông báo thành công (SweetAlert2).
                *   Tải lại danh sách đơn hàng và thống kê.
                *   Reset form.
                *   Ẩn phần nhập món ăn (nếu đang hiển thị).
                *   Cuộn trang lên đầu.
            *   Nếu gửi thất bại:
                *   Hiển thị thông báo lỗi (SweetAlert2).

2.  **Danh Sách Đơn Hàng:**

    *   Hiển thị danh sách các đơn hàng trong ngày hiện tại.
    *   Dữ liệu được lấy từ API (sử dụng `fetch` với phương thức GET).
    *   Hiển thị dưới dạng bảng (`<table>`).
    *   Các cột: Tên, Thức uống, Món ăn (nếu có), Số lượng đồ uống, Size, Tẩy Đá (hiển thị icon ✅ hoặc ❌).
    *   Có xử lý trường hợp lỗi khi tải dữ liệu (hiển thị thông báo lỗi).
    *   Có loading spinner.

3.  **Thống Kê:**

    *   Hiển thị thống kê số lượng của từng loại đồ uống và món ăn đã được đặt.
    *   Dữ liệu được lấy từ API (giống như danh sách đơn hàng).
    *   Hiển thị dưới dạng danh sách (`<ul>`).
    *   Mỗi mục trong danh sách có dạng: `Tên món (Size)`: `Số lượng` (với đồ uống) hoặc `Tên món`: `Số lượng` (với đồ ăn).
    *   Có xử lý trường hợp lỗi khi tải dữ liệu.
     *   Có loading spinner.

4.  **Xuất Excel:**
    *   Nút "Xuất Excel" cho phép tải dữ liệu về dưới dạng file `.xlsx`.
    *   Sử dụng thư viện `xlsx` để tạo file Excel.
    *    File excel có 2 sheet
        * Sheet 1 (Orders) : Chứa thông tin chi tiết
        * Sheet 2 (Summary): chứa thông tin thống kê
    *   Có loading spinner.
    *   Có xử lý lỗi.
    *   Tên file có dạng: `danh_sach_don_hang_<ngày>.xlsx`.

5.  **Đa Ngôn Ngữ:**

    *   Hỗ trợ Tiếng Việt và Tiếng Trung (phồn thể).
    *   Có các nút chuyển đổi ngôn ngữ ở góc trên bên phải màn hình.
    *   Khi click vào nút, ngôn ngữ của toàn bộ trang sẽ thay đổi.
    *   Các chuỗi ngôn ngữ được lưu trong đối tượng `translations`.
    *   Sử dụng thuộc tính `data-translate` để đánh dấu các phần tử cần dịch.
    *   Thay đổi ngôn ngữ của thuộc tính `lang` của thẻ `<html>` (cho accessibility).

6.  **Đồng Hồ Đếm Ngược:**

    *   Hiển thị thời gian còn lại đến 22:00 (giờ chốt đơn).
    *   Định dạng: `HH:mm:ss`.
    *   Cập nhật mỗi giây (sử dụng `setInterval`).
    *   Khi hết giờ:
        *   Hiển thị thông báo "Đã hết thời gian đặt hàng!".
        *   Ẩn form đặt món.
        *   Đổi màu chữ thành màu đỏ.

7.  **Loading Spinner:**

    *   Hiển thị spinner (vòng tròn xoay) khi đang tải dữ liệu hoặc xử lý tác vụ (gửi đơn, xuất Excel).
    *   Spinner được ẩn mặc định.
    *   Sử dụng class `hidden` để ẩn/hiện spinner.

8.  **API:**
    *   Sử dụng `fetch` để gọi API.
    *   API URL được lưu trong biến `API_URL` (**Cần thay đổi URL này thành URL thực tế**).
    *   Phương thức:
        *   `GET`: Lấy danh sách đơn hàng và thống kê.
        *   `POST`: Gửi đơn hàng mới.
    *   Định dạng dữ liệu: JSON.

## Công Nghệ

*   **HTML:** Cấu trúc trang web.
*   **Tailwind CSS:** Framework CSS để xây dựng giao diện.  Sử dụng các class utility của Tailwind để style nhanh.
*   **JavaScript (Vanilla JS):** Xử lý logic, tương tác người dùng, gọi API, không sử dụng framework/library nào khác (ngoài các thư viện được liệt kê bên dưới).
*   **Cloudflare Workers:** Nền tảng serverless để triển khai API.
*   **Thư viện:**
    *   **`tailwindcss`:** (đã đề cập)
    *   **`font-awesome`:** Cung cấp các icon (ví dụ: fa-utensils, fa-coffee, ...).
    *   **`xlsx`:** Xử lý file Excel (đọc và ghi).
    *   **`sweetalert2`:** Hiển thị thông báo đẹp mắt (thay thế cho `alert`, `confirm`).

## Hướng Dẫn Triển Khai (tham khảo)

1.  **Tạo tài khoản Cloudflare:** Đăng ký tài khoản Cloudflare (nếu chưa có).
2.  **Tạo Worker:** Tạo một Cloudflare Worker mới.
3.  **Viết code API:** Viết code xử lý các request (GET, POST) trong Worker.  Code này sẽ xử lý:
    *   Lấy danh sách đơn hàng (GET).  Trả về dữ liệu dạng JSON.
    *   Thêm/sửa đơn hàng (POST).  Nhận dữ liệu JSON từ client, lưu vào cơ sở dữ liệu, trả về kết quả (thành công/lỗi).
    *   Kiểm tra trùng tên (có thể thực hiện trong cùng hàm POST hoặc tách riêng).
4.  **Lưu trữ dữ liệu:** Chọn một phương thức lưu trữ dữ liệu trên Cloudflare (R2, KV, D1) hoặc sử dụng cơ sở dữ liệu bên ngoài (nếu cần) và kết nối Worker của bạn với cơ sở dữ liệu đó.
5.  **Triển khai Worker:** Deploy Worker của bạn lên Cloudflare.  Bạn sẽ nhận được một URL của Worker.
6.  **Cập nhật `API_URL`:** Thay thế giá trị của biến `API_URL` trong file `index.html` bằng URL của Worker bạn vừa triển khai.
7.  **Upload code:** Tải code HTML, CSS (nếu tách riêng), và JavaScript lên một dịch vụ hosting tĩnh (ví dụ: GitHub Pages, Netlify, Cloudflare Pages).
8.  **Test:** Mở trang web và kiểm tra các chức năng.
