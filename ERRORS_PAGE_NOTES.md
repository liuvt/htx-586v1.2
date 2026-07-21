# ErrorsPage / Access Denied flow

Đã bổ sung luồng lỗi thống nhất cho source `htx-586v1.2`.

## URL hỗ trợ

- `/account/access-denied?ReturnUrl=...` → 403
- `/error/401`
- `/error/403`
- `/error/404`
- `/error/408`
- `/error/500`
- `/error/503`

## Hành vi

- Giữ nguyên `MainLayout`: topbar, sidebar, user menu và bottom navigation.
- Người chưa đăng nhập được đưa đến `/account/login` và giữ `returnUrl`.
- Người đã đăng nhập nhưng sai quyền được đưa đến ErrorsPage 403.
- Nút `Về khu vực của tôi` tự xác định dashboard theo vai trò:
  - Owner: `/owner/dashboard`
  - Admin: `/admin/dashboard`
  - Driver: `/driver/dashboard`
- Route không tồn tại hiển thị ErrorsPage 404 thay vì nội dung trống hoặc trang lỗi trình duyệt.
- Lỗi render component được bắt bằng `ErrorBoundary` trong `MainLayout`.
- HTTP status 401/403/404/408/500/503 của request HTML được chuyển về ErrorsPage.
- File tĩnh, SignalR và endpoint dữ liệu không bị đổi sang trang HTML vì middleware chỉ xử lý GET/HEAD có `Accept: text/html`.

## File đã thay đổi

- `src/HTX586CONTRACT.Web/Components/Pages/Errors/ErrorsPage.razor`
- `src/HTX586CONTRACT.Web/Components/Routes.razor`
- `src/HTX586CONTRACT.Web/Components/Layout/MainLayout.razor`
- `src/HTX586CONTRACT.Web/Components/Shared/RedirectToRequiredPage.razor`
- `src/HTX586CONTRACT.Web/Components/_Imports.razor`
- `src/HTX586CONTRACT.Web/Program.cs`
- `src/HTX586CONTRACT.Web/wwwroot/css/app.css`

## Kiểm tra sau khi giải nén

```bash
dotnet restore
dotnet build
```

Kiểm tra trực tiếp:

1. Đăng nhập Owner rồi mở `/admin/dashboard` → hiện ErrorsPage 403, giữ layout và ReturnUrl.
2. Mở `/duong-dan-khong-ton-tai` → hiện ErrorsPage 404.
3. Mở `/error/500?ReturnUrl=%2Fowner%2Fdashboard` → hiện ErrorsPage 500 và có nút tải lại.
4. Đăng xuất rồi mở route có `[Authorize]` → chuyển login và giữ returnUrl.
