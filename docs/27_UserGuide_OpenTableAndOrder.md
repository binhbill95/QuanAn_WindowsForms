# Hướng dẫn mở bàn và gọi món trên Windows Forms

## Vai trò

Tài khoản có quyền gọi món gồm `ADMIN`, `OWNER`, `MANAGER`, `WAITER` và `CASHIER`.

Tài khoản cần được gắn hồ sơ nhân viên đang hoạt động để ghi nhận người thực hiện. Bàn phải đang hoạt động và chưa có hóa đơn phục vụ khác.

## Quy trình

1. Đăng nhập ứng dụng Windows Forms.
2. Mở chức năng **Bán hàng/POS**.
3. Chọn bàn đang ở trạng thái **Trống**.
4. Nhập số lượng khách, không thấp hơn 1 và không vượt sức chứa của bàn.
5. Xác nhận mở bàn để tạo hóa đơn phục vụ.
6. Chọn món còn bán, nhập số lượng và ghi chú nếu cần.
7. Thêm món vào hóa đơn.

## Kiểm tra kết quả

- Bàn chuyển từ **Trống** sang **Có khách**.
- Hóa đơn hiển thị món, số lượng, đơn giá và thành tiền.
- Món xuất hiện trong hàng đợi bếp theo thứ tự gọi.
- Giá món được lưu tại thời điểm gọi.

## Xử lý tình huống

| Tình huống | Cách xử lý |
|---|---|
| Tài khoản chưa gắn nhân viên | Quản trị viên gắn tài khoản với nhân viên đang hoạt động |
| Bàn đã có khách | Mở hóa đơn hiện tại thay vì tạo hóa đơn mới |
| Số khách vượt sức chứa | Chọn bàn đủ chỗ hoặc điều chỉnh số khách |
| Món hết hoặc ngừng bán | Chọn món khác hoặc nhờ quản lý kiểm tra thực đơn |
| Món đã được bếp nhận | Không hủy hoặc đổi theo luồng món đang chờ |

## Luồng tóm tắt

```text
Khách đến
  -> Chọn bàn trống
  -> Nhập số khách
  -> Mở bàn và tạo hóa đơn
  -> Gọi món
  -> Bếp nhận và hoàn thành
  -> Thu ngân thanh toán
  -> Giải phóng bàn
```
