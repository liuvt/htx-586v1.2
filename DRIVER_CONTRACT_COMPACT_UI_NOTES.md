# Driver Contract Compact UI

## Trang áp dụng

- `/driver/contracts/create`
- `/driver/contracts/{id}`

## Thay đổi hiển thị

- Chỉ hiển thị Công ty/Văn phòng và biển số xe dưới dạng thông tin tóm tắt, không dùng input disabled.
- Ẩn khỏi giao diện Driver: tài xế, khu vực, nguồn tạo, hãng/loại xe, số chỗ, chủ xe, CCCD chủ xe và các chữ ký cố định.
- Các dữ liệu bị ẩn vẫn được lấy từ database và snapshot bởi `ContractService`, không ảnh hưởng dữ liệu PDF.
- Hợp đồng đã lưu chỉ hiển thị khách hàng theo một dòng tên và số điện thoại.
- Khi tạo mới, Driver vẫn có thể chọn khách hàng cũ hoặc nhập khách hàng mới.
- Loại bỏ các alert hướng dẫn dài, trường ghi chú hợp đồng và cột ghi chú hành khách khỏi giao diện Driver.
- Dữ liệu ghi chú đã tồn tại vẫn nằm trong model khi tải/lưu lại; thay đổi này chỉ ẩn phần nhập trên giao diện.
- Khu vực ký chỉ hiển thị chữ ký khách hàng. Chữ ký Công ty, chủ xe và tài xế tiếp tục lấy tự động khi xuất hợp đồng.

## File thay đổi

- `src/HTX586CONTRACT.Web/Components/Pages/Driver/Contracts/Create.razor`
- `src/HTX586CONTRACT.Web/wwwroot/css/app.css`

## Kiểm tra đề xuất

1. Đăng nhập Driver và mở `/driver/contracts/create`.
2. Kiểm tra chỉ thấy Công ty/Văn phòng và biển số xe trong nhóm thông tin cố định.
3. Tạo hợp đồng với khách hàng mới và khách hàng đã có.
4. Mở hợp đồng được Admin/Owner phát xuống và kiểm tra khách hàng chỉ hiển thị dạng tóm tắt.
5. Cho khách ký, hoàn tất và xuất PDF.
6. Kiểm tra PDF vẫn có đầy đủ Công ty, đại diện, xe, số chỗ, chủ xe, CCCD và các chữ ký cố định.
