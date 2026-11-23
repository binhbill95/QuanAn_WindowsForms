# 1. Hướng dẫn nộp và chạy đồ án Windows Forms

## Công nghệ

- Windows Forms trên .NET 10.
- SQL Server 2022 chạy trong Docker.
- Entity Framework Core truy cập SQL Server.
- .NET User Secrets lưu chuỗi kết nối và mật khẩu cục bộ.

SQL Server, không phải MySQL hoặc Microsoft Access, là cơ sở dữ liệu chính của đồ án.

## Chuẩn bị máy giảng viên

- Windows 10/11.
- Docker Desktop đang chạy ở chế độ Linux containers.
- .NET 10 SDK.
- Visual Studio 2022/2025 hoặc PowerShell.

## Gói nộp

Nộp nguyên thư mục source hoặc một file ZIP chứa `database/`, `docker/`, `docs/`, `src/`, `README.md` và `.gitignore`.

Không nộp `docker/.env`, `bin/`, `obj/`, `.vs/` hoặc mật khẩu thật.

## 1. Tạo cấu hình Docker

Mở PowerShell tại thư mục gốc repo:

```powershell
Copy-Item .\docker\.env.example .\docker\.env
notepad .\docker\.env
```

Đặt mật khẩu SQL mạnh trong `docker/.env`:

```text
MSSQL_SA_PASSWORD=REPLACE_WITH_A_STRONG_PASSWORD
MSSQL_PORT=1433
```

## 2. Khởi tạo SQL Server

```powershell
docker compose --env-file .\docker\.env -f .\docker\docker-compose.yml up -d --build
docker compose --env-file .\docker\.env -f .\docker\docker-compose.yml ps
docker compose --env-file .\docker\.env -f .\docker\docker-compose.yml logs --tail=50 sqlserver
```

Chờ container ở trạng thái `healthy`. Lần chạy đầu tiên, các script trong `database/` được chạy theo thứ tự `01_schema.sql` đến `06_branch_management.sql`. Docker volume `restaurant-mssql-data` giữ dữ liệu giữa các lần chạy.

Nếu cổng `1433` đã được sử dụng, đổi `MSSQL_PORT` trong `.env` và dùng cùng cổng trong chuỗi kết nối.

## 3. Cấu hình User Secrets

Từ thư mục gốc, thay `YOUR_SQL_PASSWORD` bằng mật khẩu trong `docker/.env`:

```powershell
dotnet user-secrets set "ConnectionStrings:DefaultConnection" "Server=localhost,1433;Database=RestaurantManagement;User Id=sa;Password=YOUR_SQL_PASSWORD;TrustServerCertificate=True;MultipleActiveResultSets=True" --project .\src\RestaurantManagement.WinForms
```

Kiểm tra thiết lập mà không in ra mật khẩu:

```powershell
dotnet user-secrets list --project .\src\RestaurantManagement.WinForms
```

Không commit mật khẩu vào `appsettings.json`, User Secrets hoặc file `.env`.

## 4. Chạy ứng dụng Windows Forms

### Visual Studio

1. Mở `src\RestaurantManagement.slnx`.
2. Chọn `RestaurantManagement.WinForms` làm Startup Project.
3. Chọn cấu hình `Debug`.
4. Nhấn `F5`.

### PowerShell

```powershell
dotnet restore .\src\RestaurantManagement.slnx
dotnet build .\src\RestaurantManagement.slnx
dotnet run --project .\src\RestaurantManagement.WinForms\RestaurantManagement.WinForms.csproj
```

## Tài khoản demo

| Tài khoản | Mật khẩu | Vai trò |
|---|---|---|
| `admin` | `Admin@123` | Quản trị |
| `manager` | `Admin@123` | Quản lý |
| `sale` | `Admin@123` | Phục vụ |
| `thungan` | `Admin@123` | Thu ngân |
| `bep` | `Admin@123` | Bếp |

Luồng kiểm tra: mở bàn → gọi món → bếp nhận và hoàn thành món → thu ngân thanh toán → kiểm tra bàn được giải phóng và dữ liệu báo cáo.

## Dừng và reset

Dừng container nhưng giữ dữ liệu:

```powershell
docker compose --env-file .\docker\.env -f .\docker\docker-compose.yml stop
```

Khởi động lại:

```powershell
docker compose --env-file .\docker\.env -f .\docker\docker-compose.yml start
```

Chỉ reset database khi được phép xóa toàn bộ dữ liệu:

```powershell
docker compose --env-file .\docker\.env -f .\docker\docker-compose.yml down -v
docker compose --env-file .\docker\.env -f .\docker\docker-compose.yml up -d --build
```
