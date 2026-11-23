# 3. Kịch bản demo Windows Forms

**Đối tượng:** Giảng viên
**Thời lượng gợi ý:** 8-10 phút

## Chuẩn bị

- Docker Desktop đang chạy.
- SQL Server container ở trạng thái `healthy`.
- Visual Studio đã mở `src\RestaurantManagement.slnx`.
- `RestaurantManagement.WinForms` được chọn làm Startup Project.
- Chuỗi kết nối được lưu bằng .NET User Secrets.

## Phần 1: Giới thiệu

Trình bày mục tiêu quản lý nhà hàng, các vai trò và luồng nghiệp vụ từ mở bàn đến thanh toán.

## Phần 2: Mở bàn và gọi món

1. Đăng nhập bằng tài khoản `sale` hoặc `manager`.
2. Mở màn hình bán hàng/POS.
3. Chọn bàn trống và nhập số khách hợp lệ.
4. Gọi một hoặc vài món.
5. Chỉ ra hóa đơn đang mở và trạng thái bàn.

## Phần 3: Bếp

1. Đăng nhập bằng `bep`.
2. Mở hàng đợi món.
3. Chuyển một món từ Chờ sang Đang chế biến rồi sang Hoàn thành.
4. Quay lại màn hình bán hàng và chỉ ra trạng thái món đã cập nhật.

## Phần 4: Thanh toán

1. Đăng nhập bằng `thungan` hoặc `manager`.
2. Mở hóa đơn đang phục vụ.
3. Chọn hình thức thanh toán.
4. Xác nhận thanh toán.
5. Mở biên nhận và xác nhận bàn được giải phóng.

## Phần 5: Quản lý

Đăng nhập bằng `admin` để minh họa:

- Quản lý chi nhánh.
- Nhân viên và tài khoản.
- Danh sách bàn và thực đơn.
- Báo cáo doanh thu, món bán và lương.
- Nhật ký thao tác.

## Phương án dự phòng

- Nếu Docker chưa sẵn sàng, chờ container báo `healthy` trước khi mở Windows Forms.
- Nếu database rỗng, kiểm tra scripts đã chạy đầy đủ hoặc khởi tạo lại volume theo hướng dẫn trong README.
- Không xóa Docker volume trong quá trình demo nếu không cần reset toàn bộ dữ liệu.
