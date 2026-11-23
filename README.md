# He thong quan ly quan an

Do an xay dung ung dung quan ly nha hang bang Windows Forms. Du lieu va cac quy tac nghiệp vụ được lưu trên Microsoft SQL Server 2022 chạy trong Docker.

## Cong nghe

- Windows Forms trên .NET 10
- Entity Framework Core
- Microsoft SQL Server 2022 trong Docker
- .NET User Secrets cho chuoi ket noi

## Yeu cau

- Windows 10/11
- Docker Desktop dang chạy Linux containers
- .NET 10 SDK
- Visual Studio 2022/2025 hoac PowerShell

## Goi nộp

Chọn toàn bộ thư mục source hoặc nén thành ZIP, nhưng không nộp:

- `docker/.env`
- `bin/`, `obj/`, `.vs/`
- mật khẩu cá nhân
- dữ liệu hoặc file sao lưu thật

Gói nộp cần giữ:

```text
database/
docker/
docs/
src/
README.md
.gitignore
```

## Khoi tao SQL Server

Tại thư mục gốc:

```powershell
Copy-Item .\docker\.env.example .\docker\.env
notepad .\docker\.env
```

Đặt mật khẩu SQL mạnh trong `docker/.env`:

```text
MSSQL_SA_PASSWORD=REPLACE_WITH_A_STRONG_PASSWORD
MSSQL_PORT=1433
```

Khởi chạy database:

```powershell
docker compose --env-file .\docker\.env -f .\docker\docker-compose.yml up -d --build
docker compose --env-file .\docker\.env -f .\docker\docker-compose.yml ps
docker compose --env-file .\docker\.env -f .\docker\docker-compose.yml logs --tail=50 sqlserver
```

Đợi container ở trạng thái `healthy`. Data được lưu trong Docker volume `restaurant-mssql-data`.

Script SQL được chạy theo thứ tự từ `database/01_schema.sql` đến `database/06_branch_management.sql` khi volume mới được tạo.

## Cau hinh WinForms

Thay `YOUR_SQL_PASSWORD` bằng mật khẩu đã đặt trong `docker/.env`:

```powershell
dotnet user-secrets set "ConnectionStrings:DefaultConnection" "Server=localhost,1433;Database=RestaurantManagement;User Id=sa;Password=YOUR_SQL_PASSWORD;TrustServerCertificate=True;MultipleActiveResultSets=True" --project .\src\RestaurantManagement.WinForms
```

Nếu đổi `MSSQL_PORT`, thay `1433` bằng đúng cổng đã cấu hình.

Không ghi mật khẩu thật vào `appsettings.json` hoặc commit `docker/.env`.

## Chay bang Visual Studio

1. Mở `src\RestaurantManagement.slnx`.
2. Chọn `RestaurantManagement.WinForms` làm Startup Project.
3. Chọn cấu hình `Debug` rồi nhấn `F5`.

## Chay bang PowerShell

```powershell
dotnet restore .\src\RestaurantManagement.slnx
dotnet build .\src\RestaurantManagement.slnx
dotnet run --project .\src\RestaurantManagement.WinForms\RestaurantManagement.WinForms.csproj
```

## Tai khoan demo

| Tai khoan | Mat khau | Vai tro |
|---|---|---|
| `admin` | `Admin@123` | Quan tri |
| `manager` | `Admin@123` | Quan ly |
| `sale` | `Admin@123` | Phuc vu |
| `thungan` | `Admin@123` | Thu ngan |
| `bep` | `Admin@123` | Bep |
| `nv901` | `Admin@123` | Phuc vu chi nhanh CN002 |
| `thungan2` | `Admin@123` | Thu ngan chi nhanh CN002 |

Luồng kiểm tra cơ bản:

1. Đăng nhập `sale` hoặc `admin`, mở bàn và gọi món.
2. Đăng nhập `bep`, nhận món và đánh dấu hoàn thành.
3. Đăng nhập `thungan` hoặc `manager`, thanh toán hóa đơn.
4. Đăng nhập `admin`, kiểm tra báo cáo, chi nhánh và nhật ký thao tác.

## Cau hinh SQLite/Access

Dự án không dùng MySQL hay Microsoft Access. SQL Server 2022 là cơ sở dữ liệu được thiết kế, seed và kiểm thử trong repo.

## Dừng và khởi động lại

Dừng container nhưng giữ dữ liệu:

```powershell
docker compose --env-file .\docker\.env -f .\docker\docker-compose.yml stop
```

Khởi động lại:

```powershell
docker compose --env-file .\docker\.env -f .\docker\docker-compose.yml start
```

Reset database chỉ chạy khi chấp nhận xóa toàn bộ dữ liệu hiện có:

```powershell
docker compose --env-file .\docker\.env -f .\docker\docker-compose.yml down -v
docker compose --env-file .\docker\.env -f .\docker\docker-compose.yml up -d --build
```

## Tai lieu

- `docs/1_SubmissionRunGuide_WindowsForms.md` — hướng dẫn nộp và chạy lại đồ án trên Windows.
- `docs/2_UserGuide_OpenTableAndOrder_WindowsForms.md` — hướng dẫn mở bàn và gọi món trong Windows Forms.
- `docs/3_DemoPresentation_WindowsForms.md` — kịch bản demo Windows Forms.
- `docs/11_DatabaseSchemaDesign.md` — thiết kế cơ sở dữ liệu.
- `docker/README.md` — vận hành SQL Server Docker.
- `database/README.md` — thứ tự và mục đích các script SQL.
