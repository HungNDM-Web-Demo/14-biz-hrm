# Tổng quan nghiệp vụ — HRM Chấm công Tính lương

> Mô tả demo này cho ai, giải quyết vấn đề gì. Cập nhật khi phạm vi thay đổi.

---

## 1. Thông tin chung

| Mục | Nội dung |
| :--- | :--- |
| Tên demo | HRM Chấm công Tính lương |
| Mã dự án | `biz-hrm` |
| Ngành / loại khách hàng | Doanh nghiệp 20–300 nhân viên |
| Loại sản phẩm | Web quản lý + giao diện điện thoại cho nhân viên |
| Ý tưởng gốc | 5.2 · HRM: chấm công & tính lương (`ZDOCS/02-KE-HOACH/DANH-SACH-Y-TUONG.md`) |
| Người phụ trách | |
| Trạng thái | Có tài liệu nghiệp vụ (SRS, use case, kịch bản demo, kịch bản kiểm thử), chưa code |

## 2. Vấn đề của khách hàng

- Cuối tháng tổng hợp công từ máy chấm công ra Excel mất nhiều ngày
- Đơn nghỉ phép bằng giấy
- Tính lương thủ công dễ sai, nhân viên thắc mắc

## 3. Giải pháp demo thể hiện

- Hồ sơ nhân viên, phòng ban, ca làm
- Chấm công trên web hoặc điện thoại
- Đơn nghỉ phép và duyệt
- Bảng công tháng, bảng lương (lương cơ bản, phụ cấp, khấu trừ, bảo hiểm)
- Phiếu lương từng người

## 4. Vai trò người dùng

| Vai trò | Mã | Dùng trên | Làm được gì |
| :--- | :--- | :--- | :--- |
| Quản trị viên | ADMIN | Desktop | Toàn quyền |
| Nhân sự | HR | Desktop | Hồ sơ, bảng công, bảng lương |
| Trưởng phòng | MANAGER | Desktop | Duyệt nghỉ phép, xem công phòng mình |
| Nhân viên | STAFF | Điện thoại | Chấm công, xin nghỉ, xem phiếu lương |

## 5. Phân hệ và chức năng

| Area | Chức năng | Màn hình (route FE dự kiến) | Ưu tiên |
| :--- | :--- | :--- | :---: |
| `System` | Đăng nhập, tài khoản, vai trò, cài đặt | Có sẵn | — |
| `Hr` | Tổng quan hôm nay; trang cá nhân nhân viên | `/dashboard` | P1 |
| `Hr` | Nhân viên, phòng ban | `/employees`, `/departments` | P1 |
| `Hr` | Ca làm | `/shifts` | P1 |
| `Hr` | Chấm công | `/attendance` | P1 |
| `Hr` | Nghỉ phép, duyệt | `/leaves` | P1 |
| `Hr` | Bảng công tháng | `/timesheets` | P1 |
| `Hr` | Bảng lương | `/payrolls` | P1 |
| `Hr` | Phiếu lương | `/payslips` | P1 |
| `Hr` | Báo cáo nhân sự | `/reports` | P2 |

P1 = bắt buộc có để demo · P2 = nên có · P3 = làm thêm nếu còn thời gian.

## 6. Luồng nghiệp vụ chính

```mermaid
flowchart LR
    S0["Nhân viên chấm công vào, ra"] --> S1["Xin nghỉ phép"] --> S2["Trưởng phòng duyệt"] --> S3["Nhân sự chốt bảng công"] --> S4["Tính lương"] --> S5["Nhân viên xem phiếu lương"]
```

## 7. Phạm vi

**Có trong demo:**

- Chấm công, nghỉ phép, bảng công, bảng lương, phiếu lương

**Không làm (nói rõ với khách nếu được hỏi):**

- Kết nối máy chấm công thật
- Khai báo bảo hiểm, thuế điện tử
- Các kết nối bên ngoài khác chỉ giả lập

## 8. Dữ liệu mẫu cần chuẩn bị

| Dữ liệu | Số lượng | Ghi chú |
| :--- | :---: | :--- |
| Phòng ban | 6 |  |
| Nhân viên | 60 |  |
| Chấm công | 3 tháng | Có đi muộn, về sớm, quên chấm |
| Đơn nghỉ phép | 80 |  |
| Bảng lương | 3 tháng |  |
