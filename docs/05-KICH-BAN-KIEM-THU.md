# Kịch bản kiểm thử nghiệm thu — HRM Chấm công Tính lương

> Test case theo use case trong `03-USE-CASE.md`. Phần nền (đăng nhập, phân quyền, đặt lại dữ liệu) dùng TC-SYS-01 đến TC-SYS-10 trong `ZDOCS/03-KIEN-TRUC/USE-CASE-NEN.md` §8.

---

## 1. Cách chạy

| Mục | Nội dung |
| :--- | :--- |
| Môi trường | Chạy cả hai chế độ: dữ liệu mẫu (`mock`) và BE thật (`api`) |
| Dữ liệu | Đặt lại dữ liệu demo trước mỗi lượt test. Test case ghi "dữ liệu vừa đặt lại" thì chạy ngay sau khi đặt lại |
| Ngày chạy | Số liệu dưới đây tính cho ngày chạy 12/10/2026 (tháng trước là 09/2026). Chạy ngày khác thì ngày tháng dời theo, số tiền tính lại theo BR-21 đến BR-27 |
| Tài khoản | Theo cột "Vai trò" của từng test case, dùng "Đăng nhập nhanh" |
| Thiết bị | Test case vai trò Nhân viên chạy ở cửa sổ 390×844 |
| Đạt | Kết quả thực tế khớp toàn bộ cột "Kết quả mong đợi" |

Dữ liệu mẫu dùng trong test:

- Ca `HC` Hành chính: 08:00–17:00, nghỉ 12:00–13:00, 1 công, Thứ 2–Thứ 6, ân hạn 5 phút. Công chuẩn tháng 09/2026 và 10/2026 đều là 22. Ngày lễ mẫu 01/09/2026 và 02/09/2026.
- Tham số lương mẫu: bảo hiểm người lao động 10,5% (BHXH 8%, BHYT 1,5%, BHTN 1%), doanh nghiệp 21,5%; giảm trừ bản thân 15.500.000 ₫, người phụ thuộc 6.200.000 ₫; bậc thuế 1 đến 10.000.000 ₫ thuế suất 5%; ăn trưa 35.000 ₫/ngày (không chịu thuế); chuyên cần 500.000 ₫; lương tối thiểu vùng 5.310.000 ₫.
- `NV0025` Nguyễn Văn An (tài khoản `nhanvien`), phòng Kinh doanh, chính thức, vào làm 15/03/2024, ca `HC`, lương chính thức 11.000.000 ₫, lương đóng bảo hiểm 11.000.000 ₫, hưởng chuyên cần, 0 người phụ thuộc. Tháng 09/2026: 18 ngày đi làm đủ công (2 ngày đi muộn 20 phút và 35 phút), 2 ngày lễ, 1 ngày phép năm, 1 ngày nghỉ không lương. Phép năm 2026: đã tích lũy 10, đã dùng 6, còn 4, không có đơn chờ duyệt. Tháng 10/2026: đi làm đủ tới hôm qua, riêng Thứ 6 09/10/2026 chỉ có giờ vào 07:58. Hôm nay chưa chấm công.
- `NV0008` Lê Minh Quân (tài khoản `quanly`), Trưởng phòng Kinh doanh, lương chính thức và lương đóng bảo hiểm 30.000.000 ₫, phụ cấp trách nhiệm 2.000.000 ₫, 1 người phụ thuộc, không hưởng chuyên cần. Tháng 09/2026: 20 ngày đi làm đủ công, 2 ngày lễ.
- `NV0058` Vũ Thị Mai, phòng Kế toán, thử việc từ 25/08/2026, lương chính thức 10.000.000 ₫, tỉ lệ thử việc 85%, không hưởng chuyên cần. Tháng 09/2026: 20 ngày đi làm đủ công, 2 ngày lễ.
- `NV0031` Đỗ Văn Hùng, phòng Kho vận, hưởng chuyên cần; tháng 09/2026 đi muộn 4 lần.
- `NV0033` Hoàng Văn Nam, phòng Kho vận: 1 đơn giải trình ngày 29/09/2026 đang chờ duyệt.
- Nhân viên cuối cùng là `NV0060`. Tài khoản `admin` không gắn hồ sơ nhân viên.
- Bảng công 09/2026 "Đang mở", 4/6 phòng đã xác nhận (chưa có Kinh doanh, Kho vận); bảng công 08/2026 "Đã khóa"; bảng lương 07/2026 và 08/2026 "Đã chi trả".

---

## 2. Test case

### 2.1. Tổ chức và hồ sơ nhân viên

| Mã | UC | Tình huống | Vai trò | Các bước | Kết quả mong đợi |
| :--- | :--- | :--- | :--- | :--- | :--- |
| TC-01 | UC-01 | Thêm nhân viên thử việc | Nhân sự | 1. Nhân viên → Thêm nhân viên 2. Nhập Phạm Thu Trang, CCCD 079196001234, phòng Kế toán, vào làm hôm nay, Thử việc, ca `HC` 3. Tab Lương: lương chính thức 10.000.000 ₫, tỉ lệ thử việc 85%, lương đóng bảo hiểm 10.000.000 ₫ 4. Lưu | Mã `NV0061`; lương cơ bản tính lương hiển thị 8.500.000 ₫; nhân viên có trong danh sách phòng Kế toán |
| TC-02 | UC-01 | Tỉ lệ thử việc dưới 85% | Nhân sự | 1. Như TC-01, nhập tỉ lệ thử việc 80% 2. Lưu | Báo "Lương thử việc tối thiểu 85% lương chính thức", không lưu |
| TC-03 | UC-01 | Lương đóng bảo hiểm dưới lương tối thiểu vùng | Nhân sự | 1. Như TC-01, nhập lương đóng bảo hiểm 5.000.000 ₫ 2. Lưu | Báo "Lương đóng bảo hiểm không thấp hơn 5.310.000 ₫", không lưu |
| TC-04 | UC-01 | Trưởng phòng xem nhân viên | Trưởng phòng | 1. Mở Nhân viên 2. Mở hồ sơ `NV0025` 3. Gõ thẳng URL hồ sơ một nhân viên phòng Kế toán | Bước 1 chỉ có nhân viên phòng Kinh doanh; bước 2 không có tab Lương và phụ cấp, không có nút Sửa; bước 3 chuyển `/forbidden` |
| TC-05 | UC-01 | Ngừng hoạt động phòng còn nhân viên | Nhân sự | 1. Phòng ban → Kinh doanh 2. Tắt "Hoạt động" | Từ chối, nêu số nhân viên đang làm việc của phòng |

### 2.2. Ca làm

| Mã | UC | Tình huống | Vai trò | Các bước | Kết quả mong đợi |
| :--- | :--- | :--- | :--- | :--- | :--- |
| TC-06 | UC-02 | Thêm ca chiều | Nhân sự | 1. Ca làm → Thêm ca 2. Mã `CA2`, "Ca chiều kho", 14:00–22:30, nghỉ 18:00–18:30, 1 công, Thứ 2–Thứ 7, ân hạn 5 phút 3. Lưu | Giờ chuẩn 8 giờ; công chuẩn tháng 10/2026 hiển thị 27; ca có trong danh sách, 0 nhân viên |
| TC-07 | UC-02 | Giờ nghỉ nằm ngoài ca | Nhân sự | 1. Thêm ca 08:00–17:00, nghỉ 17:30–18:00 2. Lưu | Báo "Giờ nghỉ phải nằm trong ca", không lưu |
| TC-08 | UC-02 | Gán ca hàng loạt theo phòng | Nhân sự | 1. Sau TC-06, chọn `CA2` → Gán cho nhân viên 2. Chọn phòng Kho vận, áp dụng từ 01/11/2026 3. Lưu 4. Gán lại với ngày áp dụng 01/08/2026 | Bước 3: hồ sơ nhân viên Kho vận có ca `CA2` từ 01/11/2026, bảng công 10/2026 không đổi; bước 4 báo "Tháng đã chốt công, chọn ngày từ tháng chưa chốt" |

### 2.3. Chấm công

| Mã | UC | Tình huống | Vai trò | Các bước | Kết quả mong đợi |
| :--- | :--- | :--- | :--- | :--- | :--- |
| TC-09 | UC-03 | Chấm vào, chấm ra | Nhân viên | 1. Chạy vào ngày làm việc, trong khung 06:00–21:00 2. Bấm "Chấm vào" 3. Bấm "Chấm ra" | Bước 2: hiện giờ vào theo giờ máy chủ, nhãn "Đúng giờ" nếu không sau 08:05, ngược lại "Đi muộn N phút" tính từ 08:00; nút đổi thành "Chấm ra". Bước 3: hiện giờ ra, giờ làm thực tế, công theo BR-09 |
| TC-10 | UC-03 | Tài khoản không gắn hồ sơ nhân viên | Quản trị viên | 1. Mở Chấm công của tôi | Không có nút chấm, hiện "Tài khoản chưa gắn hồ sơ nhân viên" |
| TC-11 | UC-03 | Lịch công tháng của tôi | Nhân viên | 1. Chấm công → Lịch công tháng 2. Chọn tháng 09/2026 3. Chọn tháng 10/2026 | Tháng 09: 18 ngày `X` (2 ngày có chấm cam), `L` ngày 01 và 02, 1 ngày `P`, 1 ngày `KL`; tháng 10: ngày 09/10 hiện `?`, 0 công, có nút "Giải trình" |
| TC-12 | UC-04 | Nhập file có dòng lỗi | Nhân sự | 1. Nhập file chấm công → Tải file mẫu 2. Chọn lại chính file đó | Xem trước: 10 dòng hợp lệ, 2 dòng lỗi ("Mã nhân viên NV9999 không tồn tại", "Ngày 31/09/2026 không hợp lệ"); bấm "Nhập dòng hợp lệ" báo đã nhập 10 dòng |
| TC-13 | UC-04 | Nhập dòng thuộc tháng đã khóa | Nhân sự | 1. Sửa file mẫu, thêm dòng `NV0040`, 14/08/2026, 08:00 2. Nhập | Dòng đó báo "Tháng 08/2026 đã chốt công", các dòng khác vẫn nhập được |
| TC-14 | UC-05 | Đi muộn trong và ngoài ân hạn | Nhân sự | 1. Dữ liệu chấm công → ngày 05/10/2026 → dòng `NV0025` → Sửa 2. Giờ vào 08:04, giờ ra 17:00, lý do "Kiểm thử", lưu 3. Sửa giờ vào 08:12, lưu | Bước 2: "Đúng giờ", giờ làm 8 giờ, 1 công. Bước 3: "Đi muộn 12 phút", giờ làm 7 giờ 48 phút, 1 công |
| TC-15 | UC-05 | Nửa công và không công | Nhân sự | 1. Sửa ngày 06/10/2026 của `NV0025`: 08:00–15:30 2. Sửa lại 08:00–11:00 | Bước 1: "Về sớm 90 phút", giờ làm 6 giờ 30 phút, 0,5 công. Bước 2: "Về sớm 360 phút", giờ làm 3 giờ, 0 công |
| TC-16 | UC-05 | Sửa công không nhập lý do | Nhân sự | 1. Mở Sửa một dòng, đổi giờ, để trống lý do | Nút "Lưu" bị khóa, ô lý do báo đỏ |
| TC-17 | UC-05 | Trưởng phòng xem dữ liệu chấm công | Trưởng phòng | 1. Mở Dữ liệu chấm công hôm nay | Chỉ có nhân viên phòng Kinh doanh; không có nút "Sửa" |
| TC-18 | UC-06 | Giải trình quên chấm ra được duyệt | Nhân viên, Trưởng phòng | 1. Lịch công → ngày 09/10/2026 → Giải trình, giờ ra 17:05, lý do "Quên chấm ra" 2. Đổi sang Trưởng phòng, Đơn giải trình → Chờ tôi duyệt → Duyệt 3. Đổi sang Nhân viên, xem lại ngày 09/10 | Bước 1 tạo đơn `GT261012nnn` "Chờ duyệt"; bước 3 ngày 09/10 có giờ vào 07:58, giờ ra 17:05, 1 công, ký hiệu `X` |
| TC-19 | UC-06 | Giải trình quá hạn 3 ngày làm việc | Nhân viên | 1. Tạo giải trình cho ngày 05/10/2026 | Báo "Chỉ giải trình trong 3 ngày làm việc gần nhất", không tạo đơn |

### 2.4. Nghỉ phép

| Mã | UC | Tình huống | Vai trò | Các bước | Kết quả mong đợi |
| :--- | :--- | :--- | :--- | :--- | :--- |
| TC-20 | UC-07 | Đơn phép năm qua cuối tuần | Nhân viên | 1. Đơn nghỉ → Tạo đơn 2. Phép năm, từ Thứ 6 16/10/2026 đến Thứ 2 19/10/2026, cả ngày, lý do "Về quê" 3. Gửi | Trước khi gửi hiện "Đơn 2 ngày, còn 4 ngày phép, sau khi gửi còn 2"; đơn mã `NP261012nnn` "Chờ duyệt"; trang cá nhân hiện phép còn lại 2 |
| TC-21 | UC-07 | Vượt phép khả dụng | Nhân viên | 1. Dữ liệu vừa đặt lại 2. Tạo đơn phép năm Thứ 2 19/10/2026 đến Thứ 6 23/10/2026 | Hiện "Đơn 5 ngày"; báo "Vượt số phép còn lại (4 ngày)", không gửi được |
| TC-22 | UC-07 | Đơn nửa ngày | Nhân viên | 1. Tạo đơn phép năm Thứ 4 21/10/2026, nửa ngày sáng, chưa gửi | Hiện "Đơn 0,5 ngày"; đóng hộp thoại không tạo đơn |
| TC-23 | UC-07 | Trùng ngày với đơn khác | Nhân viên | 1. Sau TC-20, tạo đơn nghỉ không lương Thứ 2 19/10/2026 | Báo trùng với đơn của TC-20, không gửi được |
| TC-24 | UC-07 | Hạn gửi đơn | Nhân viên | 1. Tạo đơn phép năm bắt đầu hôm nay 12/10/2026 2. Tạo đơn nghỉ ốm ngày 07/10/2026 3. Tạo đơn nghỉ ốm ngày 06/10/2026 | Bước 1 báo "Phép năm phải gửi trước ít nhất 1 ngày"; bước 2 gửi được; bước 3 báo quá hạn 3 ngày làm việc |
| TC-25 | UC-08 | Trưởng phòng duyệt đơn | Trưởng phòng | 1. Sau TC-20, mở Đơn nghỉ → Chờ tôi duyệt 2. Mở đơn của `NV0025`, xem lịch nghỉ của phòng 3. Duyệt | Đơn "Đã duyệt"; quỹ phép `NV0025`: đã dùng 8, chờ duyệt 0, còn lại 2; bảng công 10/2026 ô 16/10 và 19/10 là `P` |
| TC-26 | UC-08 | Từ chối đơn | Nhân viên, Trưởng phòng | 1. Nhân viên tạo đơn phép năm Thứ 3 20/10/2026 2. Trưởng phòng bấm Từ chối, để trống lý do 3. Nhập lý do "Phòng đã có 2 người nghỉ", xác nhận | Bước 2 nút xác nhận bị khóa; bước 3 đơn "Từ chối"; phép còn lại của nhân viên trở về như trước bước 1 |
| TC-27 | UC-08 | Đơn của trưởng phòng do nhân sự duyệt | Trưởng phòng, Nhân sự | 1. Trưởng phòng tạo đơn phép năm Thứ 5 22/10/2026 2. Mở tab Chờ tôi duyệt 3. Đổi sang Nhân sự, mở tab Chờ tôi duyệt, duyệt | Bước 2 không có đơn vừa tạo; bước 3 đơn có trong danh sách và duyệt được |
| TC-28 | UC-07 | Hủy đơn đã duyệt chưa tới ngày | Nhân viên | 1. Sau TC-25, mở đơn 16/10–19/10 2. Hủy đơn, xác nhận | Đơn "Đã hủy"; phép còn lại 4; ô 16/10, 19/10 trên bảng công không còn `P` |

### 2.5. Bảng công

| Mã | UC | Tình huống | Vai trò | Các bước | Kết quả mong đợi |
| :--- | :--- | :--- | :--- | :--- | :--- |
| TC-29 | UC-10 | Tổng hợp công tháng | Nhân sự | 1. Bảng công → 09/2026 2. Xem dòng `NV0025` 3. Bấm ô ngày đi muộn 35 phút | Công chuẩn 22, công thực tế 18, `P` 1, `L` 2, `KL` 1, đi muộn 2 lần 55 phút, ngày công tính lương 21; ô 01/09 và 02/09 là `L`; bước 3 hiện giờ vào 08:35, giờ ra, 1 công |
| TC-30 | UC-10 | Trưởng phòng xác nhận công phòng | Trưởng phòng | 1. Bảng công → 09/2026 2. Bấm "Xác nhận công phòng" | Chỉ thấy phòng Kinh doanh; phòng chuyển "Đã xác nhận"; không có nút "Chốt bảng công" |
| TC-31 | UC-10 | Chốt khi còn đơn chờ duyệt | Nhân sự | 1. Bảng công 09/2026 → Chốt bảng công 2. Duyệt đơn giải trình của `NV0033` ngay trong danh sách 3. Bấm Chốt lại 4. Bấm "Vẫn chốt" | Bước 1 từ chối, liệt kê đơn giải trình ngày 29/09/2026 của `NV0033`; bước 3 hiện cảnh báo phòng Kho vận chưa xác nhận; bước 4 bảng công "Đã chốt", ghi người chốt và thời điểm |
| TC-32 | UC-10 | Dữ liệu tháng đã chốt bị khóa | Nhân sự | 1. Sau TC-31, Dữ liệu chấm công ngày 15/09/2026 2. Nhập file có dòng `NV0040`, 15/09/2026, 08:00 3. Bấm "Mở lại" bảng công 09/2026, nhập lý do 4. Chốt lại | Bước 1 không có nút "Sửa", chú thích "Tháng đã chốt công"; bước 2 dòng đó báo "Tháng 09/2026 đã chốt công"; bước 3 bảng công về "Đang mở" (chưa có bảng lương); bước 4 "Đã chốt" |

### 2.6. Bảng lương

| Mã | UC | Tình huống | Vai trò | Các bước | Kết quả mong đợi |
| :--- | :--- | :--- | :--- | :--- | :--- |
| TC-33 | UC-12 | Tạo bảng lương khi bảng công chưa chốt | Nhân sự | 1. Dữ liệu vừa đặt lại 2. Bảng lương → Tạo bảng lương 09/2026 | Báo "Bảng công tháng 09/2026 chưa chốt", có liên kết tới Bảng công |
| TC-34 | UC-12 | Tính lương nhân viên không có thuế | Nhân sự | 1. Sau TC-31, tạo bảng lương 09/2026 2. Mở dòng `NV0025` | Mã `BL2609`, "Nháp". Dòng `NV0025`: công 21/22; lương theo công 10.500.000 ₫; ăn trưa 630.000 ₫; chuyên cần 500.000 ₫; tổng thu nhập 11.630.000 ₫; bảo hiểm 1.155.000 ₫ (880.000 + 165.000 + 110.000); thu nhập chịu thuế 11.000.000 ₫; thu nhập tính thuế 0 ₫; thuế 0 ₫; thực lĩnh 10.475.000 ₫; bảo hiểm phần doanh nghiệp 2.365.000 ₫ |
| TC-35 | UC-12 | Tính lương có thuế và người phụ thuộc | Nhân sự | 1. Mở dòng `NV0008` | Công 22/22; lương theo công 30.000.000 ₫; ăn trưa 700.000 ₫; trách nhiệm 2.000.000 ₫; tổng thu nhập 32.700.000 ₫; bảo hiểm 3.150.000 ₫; thu nhập chịu thuế 32.000.000 ₫; giảm trừ 21.700.000 ₫; thu nhập tính thuế 7.150.000 ₫; thuế 357.500 ₫; thực lĩnh 29.192.500 ₫ |
| TC-36 | UC-12 | Nhân viên thử việc không tính bảo hiểm | Nhân sự | 1. Mở dòng `NV0058` | Lương cơ bản tính lương 8.500.000 ₫; công 22/22; lương theo công 8.500.000 ₫; ăn trưa 700.000 ₫; tổng thu nhập 9.200.000 ₫; bảo hiểm 0 ₫; thuế 0 ₫; thực lĩnh 9.200.000 ₫ |
| TC-37 | UC-12 | Mất chuyên cần | Nhân sự | 1. Mở dòng `NV0031`, bấm vào ô chuyên cần | Chuyên cần 0 ₫; công thức ghi "Đi muộn, về sớm 4 lần, vượt 3 lần" |
| TC-38 | UC-12 | Khoản cộng và xem công thức | Nhân sự | 1. Dòng `NV0025` → Khoản cộng, trừ → thêm "Thưởng doanh số" 2.000.000 ₫, chịu thuế 2. Bấm vào ô lương theo công | Tổng thu nhập 13.630.000 ₫; thu nhập chịu thuế 13.000.000 ₫; thuế 0 ₫; thực lĩnh 12.475.000 ₫; bước 2 hiện "11.000.000 × 21 ÷ 22 = 10.500.000" |
| TC-39 | UC-12 | Thực lĩnh âm | Nhân sự | 1. Dòng `NV0058` → thêm khoản trừ "Tạm ứng" 15.000.000 ₫ 2. Xóa khoản trừ đó | Bước 1: thực lĩnh −5.800.000 ₫, dòng đỏ, nút "Gửi duyệt" bị khóa; bước 2 dòng trở lại 9.200.000 ₫, mở khóa "Gửi duyệt" |
| TC-40 | UC-13 | Gửi duyệt | Nhân sự | 1. Bảng lương `BL2609` "Nháp", không còn dòng lỗi → Gửi duyệt 2. Thử sửa khoản cộng | Bước 1 "Chờ duyệt"; bước 2 không sửa được; nhân sự không thấy nút "Duyệt" |
| TC-41 | UC-13 | Trả lại bảng lương | Quản trị viên | 1. Sau TC-40, mở `BL2609` → Trả lại, để trống lý do 2. Nhập lý do "Kiểm tra lại thưởng phòng Kinh doanh", xác nhận | Bước 1 nút xác nhận bị khóa; bước 2 bảng lương về "Nháp", nhân sự thấy lý do trên đầu trang |
| TC-42 | UC-13 | Duyệt bảng lương | Nhân sự, Quản trị viên | 1. Nhân sự gửi duyệt lại 2. Quản trị viên mở `BL2609`, xem thẻ tổng, bấm Duyệt 3. Đổi sang Nhân sự, mở Bảng công 09/2026 | Bước 2 thẻ tổng có số nhân viên, tổng thu nhập, bảo hiểm phần người lao động và doanh nghiệp, thuế, thực lĩnh, chênh lệch so với 08/2026; tổng thực lĩnh bằng tổng các dòng; sau duyệt "Đã duyệt". Bước 3 bảng công "Đã khóa", nút "Mở lại" bị vô hiệu |
| TC-43 | UC-13 | Đánh dấu đã chi trả | Nhân sự | 1. Sau TC-42, bấm "Đã chi trả", chọn ngày 15/10/2026 | Bảng lương "Đã chi trả", hiện ngày chi trả 15/10/2026 |

### 2.7. Phiếu lương và tổng quan

| Mã | UC | Tình huống | Vai trò | Các bước | Kết quả mong đợi |
| :--- | :--- | :--- | :--- | :--- | :--- |
| TC-44 | UC-14 | Xem phiếu lương sau khi duyệt | Nhân viên | 1. Sau TC-42, mở Phiếu lương 2. Mở phiếu tháng 09/2026 3. Bấm "Lưu PDF" | Có phiếu `PL2609NV0025`; số liệu khớp dòng lương lúc duyệt (TC-34, hoặc TC-38 nếu đã thêm thưởng); bản in khổ A5 đủ từng khoản |
| TC-45 | UC-14 | Phiếu chưa phát hành và phiếu người khác | Nhân viên | 1. Dữ liệu vừa đặt lại, mở Phiếu lương 2. Gõ URL phiếu lương tháng 08/2026 của `NV0008` | Bước 1 có phiếu 07/2026, 08/2026, không có 09/2026; bước 2 chuyển `/forbidden` |
| TC-46 | UC-15 | Tổng quan cập nhật sau khi chấm công | Nhân sự, Nhân viên | 1. Nhân sự ghi lại số "Đã chấm vào" 2. Nhân viên chấm vào (TC-09) 3. Nhân sự tải lại Tổng quan | "Đã chấm vào" tăng 1; `NV0025` không còn trong danh sách chưa chấm công |
| TC-47 | UC-15 | Tổng quan của trưởng phòng | Trưởng phòng | 1. Đăng nhập nhanh vai trò Trưởng phòng | Số liệu và danh sách chỉ của phòng Kinh doanh; có đơn chờ duyệt của phòng |
| TC-48 | UC-15 | Trang cá nhân nhân viên | Nhân viên | 1. Dữ liệu vừa đặt lại, đăng nhập nhanh vai trò Nhân viên | Có nút "Chấm vào", phép còn lại 4, phiếu lương mới nhất 08/2026; không có thẻ số liệu toàn công ty |
| TC-49 | UC-16 | Báo cáo chuyên cần | Nhân sự | 1. Báo cáo → Chuyên cần, tháng 09/2026, phòng Kinh doanh | `NV0025` có 2 lần đi muộn, 55 phút; tổng phòng bằng tổng các dòng |

---

## 3. Biên bản chạy thử

| Ngày | Người test | Chế độ | Số TC đạt / tổng | Lỗi ghi nhận |
| :--- | :--- | :--- | :---: | :--- |
| | | | | |
