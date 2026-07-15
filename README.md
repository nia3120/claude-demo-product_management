# Livotec Product Management Dashboard

Dashboard quản trị sản phẩm (Product Management) cho Livotec — chạy dưới dạng 1 file HTML tĩnh, dữ liệu đọc/ghi qua Google Apps Script kết nối tới Google Sheet gốc, publish qua GitHub Pages.

## File đi kèm
- `dashboard.html` — toàn bộ giao diện + logic, mở trực tiếp bằng trình duyệt hoặc host tĩnh.
- `Code.gs` — mã Google Apps Script, publish thành Web App để làm API đọc/ghi Google Sheet.

## Các mục trong dashboard
1. **Tổng quan** — KPI toàn công ty, biểu đồ doanh thu vs kế hoạch, cảnh báo nổi bật.
2. **Kinh doanh & tồn kho** — xem theo tháng / quý / Year-to-Date / tiến độ kế hoạch năm / theo từng nhóm sản phẩm (quạt điện, bếp từ, nồi cơm điện, bình nước nóng, máy sưởi, máy hút ẩm, máy hút mùi, máy hút bụi).
3. **Danh mục sản phẩm (BCG)** — review SKU theo 4 vùng BCG (Invest & Grow / Turn into Star / Maintain & Milk / Killed), lọc theo kênh GT/MT.
4. **Quản trị dự án PTSPM** — danh sách dự án phát triển sản phẩm mới, gồm cả hàng OEM và tự sản xuất, có thể thêm/sửa/xóa dự án.
5. **Cảnh báo & công việc** — tự động cảnh báo tồn kho bất thường, dự án chậm/rủi ro, kế hoạch năm khó đạt; kèm danh sách việc cần làm có thể tick/thêm/xóa.
6. **Cài đặt kết nối** — nhập URL Web App Apps Script, kiểm tra kết nối, bật/tắt chế độ chỉnh sửa (phân quyền xem cho lãnh đạo vs. chỉnh sửa cho PM).

## Bước 1 — Chuẩn bị Google Sheet
Tạo 1 Google Sheet gốc với 4 tab (sheet) theo đúng tên và cột mô tả trong đầu file `Code.gs`:
`KinhDoanh`, `DanhMuc`, `PTSPM`, `Todo`.

## Bước 2 — Publish Apps Script làm Web App
1. Trong Google Sheet: **Tiện ích mở rộng → Apps Script**.
2. Dán nội dung `Code.gs` vào, lưu lại.
3. **Deploy → New deployment → Web app**.
   - Execute as: **Me**
   - Who has access: **Anyone** (để dashboard public gọi được; nếu cần bảo mật hơn, giới hạn theo tổ chức và cân nhắc thêm xác thực token).
4. Copy URL dạng `.../exec`.

## Bước 3 — Kết nối dashboard với Sheet
- Cách nhanh: mở `dashboard.html`, vào mục **Cài đặt kết nối**, dán URL vào ô "Web App URL", bấm **Kiểm tra kết nối** rồi **Tải dữ liệu từ Sheet**.
- Cách cố định cho bản publish: mở `dashboard.html`, tìm dòng
  ```js
  const APPS_SCRIPT_URL_DEFAULT = "";
  ```
  và điền sẵn URL `.../exec` vào giữa hai dấu ngoặc kép trước khi đưa lên GitHub.

> Lưu ý: nếu chưa kết nối, dashboard tự chạy với **dữ liệu mẫu (demo)** để xem giao diện — không lỗi, không trắng trang.

## Bước 4 — Publish qua GitHub Pages
1. Tạo repository mới trên GitHub, đẩy (push) `dashboard.html` lên (có thể đổi tên thành `index.html` để URL gọn).
2. Vào **Settings → Pages** của repo → chọn nhánh (branch) chứa file → Save.
3. GitHub sẽ cấp URL dạng `https://<username>.github.io/<repo>/` để chia sẻ cho lãnh đạo/PM xem.

## Phân quyền xem/sửa
- Toggle **"Chế độ chỉnh sửa"** ở góc trên hoặc trong mục Cài đặt: bật khi PM cần nhập số liệu thực tế/tiến độ dự án (ghi ngược về Sheet), tắt khi trình chiếu cho lãnh đạo để tránh sửa nhầm số liệu.

## Mở rộng thêm
- Có thể bổ sung cột `DoanhThu` thực tế vào sheet `KinhDoanh` nếu muốn số doanh thu chính xác tuyệt đối thay vì tính theo đơn giá bình quân của từng nhóm hàng.
- Có thể thêm xác thực (ví dụ một mã token đơn giản gửi kèm mỗi request) trong `Code.gs` nếu Web App public nhưng muốn hạn chế ghi dữ liệu từ bên ngoài.
