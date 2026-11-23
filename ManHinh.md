# Danh sách màn hình và nghiệp vụ

## Tổng quan

- Có **18 module nghiệp vụ**.
- Có khoảng **42 màn hình giao diện thực tế**.
- Có khoảng **37 màn hình nghiệp vụ**, không tính đăng nhập, đổi mật khẩu, trang lỗi và trang giới thiệu.

## Nghiệp vụ bán hàng

Màn hình `Orders/Index` hỗ trợ:

- Xem sơ đồ bàn và trạng thái bàn.
- Mở bàn và nhập số khách.
- Gọi món, số lượng và ghi chú.
- Hủy món hoặc đổi món khi bếp chưa nhận.
- Chuyển bàn và gộp bàn.
- Theo dõi trạng thái món.
- Chuyển sang thanh toán.

## Hàng đợi bếp

Màn hình `Kitchen/Queue` hỗ trợ:

- Hiển thị món theo thứ tự gọi.
- Nhận chế biến.
- Đánh dấu hoàn thành.
- Hiển thị bàn, hóa đơn, số lượng, ghi chú và thời gian chờ.
- Phân quyền nhân viên bếp cập nhật; phục vụ và thu ngân chỉ xem.

## Thanh toán

Các màn hình:

- `Payments/Index` — Danh sách hóa đơn chờ thanh toán.
- `Payments/Checkout` — Tính tiền.
- `Payments/Receipt` — Biên nhận.

## Luồng demo chính

```text
Mở bàn
  → Gọi món
  → Bếp nhận món
  → Đánh dấu hoàn thành
  → Thanh toán
  → Đóng và giải phóng bàn
```

## Kết luận

Các nghiệp vụ cốt lõi đã có thể demo theo luồng bán hàng hoàn chỉnh:

**mở bàn → gọi món → bếp nhận món → hoàn thành → thanh toán → đóng bàn**.
