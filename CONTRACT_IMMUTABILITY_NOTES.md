# Contract immutability update

## Mục tiêu

Hợp đồng sau khi kết thúc phải là hồ sơ lịch sử bất biến. Việc đổi xe được gán cho tài xế, chuyển tài xế sang Công ty/Văn phòng khác, sửa hồ sơ khách hàng, xe, chủ xe hoặc chữ ký master không được làm thay đổi hợp đồng cũ.

## Cách triển khai

- Dùng cột `Contracts.ContractDataJson` hiện có để lưu snapshot đầy đủ của:
  - Công ty/Văn phòng và người đại diện.
  - Tài xế và GPLX.
  - Khách hàng/người thuê.
  - Xe, số chỗ và chủ sở hữu.
  - URL/hash/thời gian của ba chữ ký master.
- Snapshot được tạo khi lập hợp đồng và được cập nhật lại khi Owner/Admin còn được phép sửa hợp đồng.
- Khi khách hàng ký, snapshot được chốt trong cùng transaction với chữ ký và trạng thái `Completed`.
- Các trạng thái `Completed`, `Cancelled`, `Expired`, `Invalidated` bị khóa ở tầng service; không thể sửa, xóa, ký thêm hoặc hủy lại.
- Trang lịch sử Owner/Admin và trang Driver đọc snapshot, không đọc hồ sơ danh mục hiện tại.
- PDF đọc snapshot và PDF chính thức đã sinh sẽ được tái sử dụng, không tự render lại từ dữ liệu mới.
- Báo cáo Excel hợp đồng dùng snapshot cho Công ty/Văn phòng và số chỗ xe.

## Database

Không cần migration mới vì dùng lại cột `ContractDataJson` kiểu `nvarchar(max)` đã có.

Khi ứng dụng khởi động, hợp đồng cũ chưa có JSON snapshot sẽ được backfill một lần, ưu tiên các cột snapshot cũ (`CompanyNameSnapshot`, `DriverNameSnapshot`, `VehiclePlateSnapshot`, ...), sau đó mới dùng dữ liệu danh mục hiện tại cho các trường trước đây chưa được lưu.

## Lưu ý với dữ liệu cũ

Nếu một hợp đồng cũ chưa có snapshot đầy đủ và hồ sơ danh mục đã bị thay đổi trước khi cài bản sửa này, hệ thống không thể tự suy ra chính xác giá trị lịch sử của những trường chưa từng được lưu. PDF chính thức đã sinh trước đó vẫn được giữ nguyên và nên được xem là tài liệu gốc cho trường hợp này.

## Kiểm tra đề nghị

1. Tạo hợp đồng và hoàn tất bằng chữ ký khách hàng.
2. Ghi lại Công ty/Văn phòng, tài xế, xe, số chỗ, chủ xe và chữ ký đang hiển thị.
3. Đổi xe của tài xế hoặc chuyển tài xế sang đơn vị khác.
4. Sửa tên Công ty/Văn phòng, thông tin xe/chủ xe và chữ ký master.
5. Mở lại hợp đồng từ Owner, Admin và Driver; dữ liệu phải giữ nguyên.
6. Mở PDF lần thứ hai; URL/hash và nội dung PDF phải không đổi.
