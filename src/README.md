# Source - Restaurant Management

Solution Windows Forms/Clean Architecture sử dụng Microsoft SQL Server 2022 trong Docker.

## Cấu trúc

```text
src/
├── RestaurantManagement.slnx
├── RestaurantManagement.Domain/        # Mô hình miền
├── RestaurantManagement.Application/   # DTO và service contracts
├── RestaurantManagement.Infrastructure/# EF Core và triển khai dịch vụ
├── RestaurantManagement.Tests/         # Kiểm thử nghiệp vụ
└── RestaurantManagement.WinForms/      # Giao diện Windows Forms
```

## Cấu hình

Sau khi khởi tạo SQL Server bằng Docker Compose, cấu hình chuỗi kết nối bằng User Secrets:

```powershell
dotnet user-secrets set "ConnectionStrings:DefaultConnection" "Server=localhost,1433;Database=RestaurantManagement;User Id=sa;Password=<MSSQL_SA_PASSWORD>;TrustServerCertificate=True;MultipleActiveResultSets=True" --project .\RestaurantManagement.WinForms
```

Không ghi mật khẩu thật vào `appsettings.json`.

## Chạy

```powershell
dotnet restore .\RestaurantManagement.slnx
dotnet build .\RestaurantManagement.slnx
dotnet run --project .\RestaurantManagement.WinForms\RestaurantManagement.WinForms.csproj
```

Có thể mở `RestaurantManagement.slnx` bằng Visual Studio, chọn `RestaurantManagement.WinForms` làm Startup Project và nhấn `F5`.

Hướng dẫn đầy đủ: `../README.md` và `../docs/1_SubmissionRunGuide_WindowsForms.md`.
