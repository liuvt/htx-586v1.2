# HTX586CONTRACT – Enterprise Light UI Redesign

## Phạm vi đã cập nhật

- Design system light mới cho MudBlazor 9.5.0.
- Main layout: topbar glass, sidebar enterprise, brand HTX586, trạng thái hệ thống, user menu.
- Navigation phân nhóm riêng cho Owner, Admin và Driver.
- PageHeader dùng chung cho toàn bộ trang.
- StatCard mới với màu ngữ nghĩa, badge, mô tả và hiệu ứng chiều sâu.
- Dashboard Owner: command center toàn hệ thống, KPI, sức khỏe vận hành, quick actions.
- Dashboard Admin: điều phối theo Công ty/Văn phòng được gán.
- Dashboard Driver: doanh thu theo ngày, phương tiện, hợp đồng và timeline thông báo.
- Chuẩn hóa giao diện MudTable, form controls, button, chip, alert, tab và paper trên toàn hệ thống.
- Auth layout mới, đồng nhất nhận diện với khu vực quản trị.
- Responsive cho desktop, tablet và mobile; giữ mobile bottom navigation của Driver.

## Các file chính

- `src/HTX586CONTRACT.Web/Themes/AppMudTheme.cs`
- `src/HTX586CONTRACT.Web/wwwroot/css/app.css`
- `src/HTX586CONTRACT.Web/Components/Layout/MainLayout.razor`
- `src/HTX586CONTRACT.Web/Components/Layout/NavMenu.razor`
- `src/HTX586CONTRACT.Web/Components/Layout/UserMenu.razor`
- `src/HTX586CONTRACT.Web/Components/Layout/AuthLayout.razor`
- `src/HTX586CONTRACT.Web/Components/Shared/PageHeader.razor`
- `src/HTX586CONTRACT.Web/Components/Shared/StatCard.razor`
- ba file Dashboard của Owner, Admin và Driver.

## Kiểm tra khi chạy local

```powershell
dotnet restore
dotnet build HTX586CONTRACT.slnx
dotnet run --project src/HTX586CONTRACT.Web
```

Môi trường tạo bản chỉnh sửa không có .NET SDK, vì vậy cần chạy ba lệnh trên tại máy phát triển để xác nhận build và xem giao diện bằng dữ liệu thật.
