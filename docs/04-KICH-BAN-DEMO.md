# Kịch bản demo — HRM Chấm công Tính lương

> Dùng khi trình bày cho chủ doanh nghiệp, trưởng phòng nhân sự công ty 20–300 người. Thời lượng 12–15 phút. Đi theo một vòng công việc: nhân viên chấm công, xin nghỉ, trưởng phòng duyệt, nhân sự chốt công, tính lương, giám đốc duyệt, nhân viên nhận phiếu lương.

---

## 1. Chuẩn bị trước buổi demo

- [ ] Mở sẵn link demo, thử "Đăng nhập nhanh" cả 4 vai trò
- [ ] Đặt lại dữ liệu demo (thanh công cụ demo → Đặt lại dữ liệu) để có 3 tháng chấm công tính tới hôm qua, bảng công tháng trước "Đang mở", tài khoản `nhanvien` chưa chấm công hôm nay
- [ ] Đổi tên công ty, logo, màu theo thương hiệu khách (Cài đặt → Thương hiệu) nếu đã biết
- [ ] Mở hai cửa sổ: một cửa sổ thu nhỏ 390×844 (hoặc điện thoại thật) cho vai trò Nhân viên, một cửa sổ desktop cho Nhân sự, Trưởng phòng, Giám đốc
- [ ] Tải sẵn file chấm công mẫu (Chấm công → Nhập file → "Tải file mẫu"); file có 2 dòng lỗi cố ý để demo
- [ ] Ghi nhớ số liệu của Nguyễn Văn An (`NV0025`) tháng trước: công 21/22, thực lĩnh 10.475.000 ₫
- [ ] Nhắc mình: tỉ lệ bảo hiểm, giảm trừ gia cảnh, biểu thuế trong demo là giá trị mẫu, cấu hình được

| Link | Ghi chú |
| :--- | :--- |
| FE | `http://localhost:3114` hoặc link online |
| Swagger | `https://localhost:7114/swagger` (khi khách hỏi về tích hợp) |

| Tài khoản | Vai trò | Dùng để cho xem |
| :--- | :--- | :--- |
| `admin` | Quản trị viên (đóng vai Giám đốc) | Duyệt bảng lương, phân quyền, thương hiệu |
| `nhansu` | Nhân sự | Hồ sơ, dữ liệu chấm công, nhập file, bảng công, bảng lương, báo cáo |
| `quanly` | Trưởng phòng Kinh doanh | Tổng quan phòng, duyệt đơn nghỉ, duyệt giải trình, xác nhận công |
| `nhanvien` | Nhân viên Kinh doanh (Nguyễn Văn An) | Chấm công điện thoại, xin nghỉ, giải trình, xem phiếu lương |

Mật khẩu: giá trị `DEMO_PASSWORD` trong `.env`, hoặc bấm "Đăng nhập nhanh".

---

## 2. Câu chuyện demo

> Mở đầu: "Anh/chị hình dung đây là một ngày đầu tháng ở công ty mình: nhân viên đi làm, có người xin nghỉ, còn phòng nhân sự phải chốt công và làm lương tháng trước. Em đi lần lượt từng người để anh/chị thấy phần mềm bớt được việc gì."

| # | Vai trò | Thao tác | Điểm cần nhấn mạnh |
| :---: | :--- | :--- | :--- |
| 1 | Nhân viên | Trên cửa sổ điện thoại, đăng nhập `nhanvien`; trang cá nhân hiện ca Hành chính 08:00–17:00; bấm "Chấm vào" | Một chạm là xong, giờ lấy theo máy chủ nên không chỉnh giờ điện thoại để gian lận được; đi muộn hiện ngay số phút |
| 2 | Nhân viên | Mở Lịch công tháng: ngày đi muộn chấm cam, một ngày quên chấm ra có dấu `?`; bấm "Giải trình", nhập giờ ra 17:05, lý do "Quên chấm ra" | Nhân viên tự thấy công của mình hằng ngày, không đợi cuối tháng mới thắc mắc |
| 3 | Nhân viên | Tạo đơn phép năm từ Thứ 6 tuần sau đến Thứ 2 kế tiếp; hệ thống tính 2 ngày (bỏ Thứ 7, Chủ nhật), báo còn 4 ngày phép, sau khi gửi còn 2 | Hết đơn giấy; số ngày nghỉ và phép còn lại tự tính đúng luật 12 ngày/năm |
| 4 | Trưởng phòng | Đổi vai trò sang Trưởng phòng; Tổng quan phòng Kinh doanh: ai đi muộn, ai nghỉ, đơn chờ duyệt; mở đơn của An, xem lịch nghỉ cả phòng rồi duyệt; duyệt luôn đơn giải trình | Duyệt ngay trên điện thoại; thấy được ngày đó phòng còn bao nhiêu người trước khi duyệt |
| 5 | Nhân sự | Đổi sang Nhân sự; Tổng quan toàn công ty; vào Nhập file chấm công, chọn file máy vân tay của kho: xem trước báo 2 dòng lỗi (mã nhân viên sai, ngày sai), nhập phần còn lại | Bộ phận dùng máy chấm công vẫn gom về chung một chỗ, không phải gõ lại Excel |
| 6 | Nhân sự | Mở Bảng công tháng trước: lưới 60 người × 30 ngày, ký hiệu `X`, `P`, `L`, `V`, `?`; bấm một ô xem giờ chấm gốc và lịch sử sửa; bấm "Chốt bảng công": hệ thống chặn vì còn 1 đơn giải trình chờ duyệt ở phòng Kho, duyệt ngay rồi chốt | Cuối tháng không còn ngồi tổng hợp nhiều ngày; hệ thống không cho chốt khi còn đơn treo |
| 7 | Nhân sự | Tạo bảng lương tháng trước: 60 dòng tính trong vài giây; mở dòng An: 21/22 công, lương theo công 10.500.000 ₫, ăn trưa 630.000 ₫, chuyên cần 500.000 ₫, bảo hiểm 1.155.000 ₫, thuế 0 ₫, thực lĩnh 10.475.000 ₫; bấm vào từng số xem công thức | Công thức minh bạch, ai hỏi cũng giải thích được ngay; không còn sai do kéo công thức Excel |
| 8 | Nhân sự | Thêm khoản cộng "Thưởng doanh số" 2.000.000 ₫ cho An, thực lĩnh lên 12.475.000 ₫; mở dòng Trưởng phòng Lê Minh Quân để xem cách tính thuế TNCN có người phụ thuộc; bấm "Gửi duyệt" | Thưởng, tạm ứng nhập tay vẫn tính lại bảo hiểm, thuế tự động |
| 9 | Giám đốc | Đổi sang Quản trị viên; mở bảng lương chờ duyệt: tổng quỹ lương, bảo hiểm phần doanh nghiệp, so với tháng trước theo phòng ban; bấm "Duyệt" | Giám đốc duyệt trên một màn hình, biết ngay quỹ lương tăng ở phòng nào |
| 10 | Nhân viên | Quay lại cửa sổ điện thoại: thẻ "Phiếu lương mới"; mở phiếu tháng trước xem từng khoản, lưu PDF | Nhân viên tự xem lương trên điện thoại, mỗi người chỉ thấy phiếu của mình |
| 11 | Nhân sự | Đánh dấu bảng lương "Đã chi trả"; mở Báo cáo: chuyên cần theo phòng (ai đi muộn nhiều), quỹ lương 3 tháng | Có số liệu để xét chuyên cần, khen thưởng, kiểm soát chi phí lương |
| 12 | Quản trị viên | Mở Tham số lương: tỉ lệ bảo hiểm, giảm trừ gia cảnh, biểu thuế có ngày hiệu lực; mở Vai trò, cho thấy Trưởng phòng không có quyền xem lương | Luật đổi thì nhân sự tự cập nhật tham số, không phải sửa phần mềm; phân quyền tới từng nút |

> Kết: "Số liệu anh/chị vừa thấy là dữ liệu mẫu, tham số bảo hiểm và thuế cũng là giá trị mẫu. Bản triển khai sẽ nhập nhân viên, ca làm, cách tính lương riêng của công ty mình và đối chiếu quy định hiện hành."

---

## 3. Câu hỏi khách hay hỏi

| Câu hỏi | Trả lời gợi ý |
| :--- | :--- |
| Có kết nối trực tiếp máy chấm công vân tay, khuôn mặt không? | Demo nhập file Excel xuất từ máy. Bản triển khai kết nối trực tiếp qua SDK hoặc API của hãng máy (ZKTeco, Hikvision…), cần khảo sát đúng model máy. |
| Chấm công điện thoại có chấm hộ, chấm ở nhà được không? | Demo lưu vị trí để tham khảo. Bản triển khai có thể giới hạn theo wifi công ty, bán kính GPS, hoặc chụp ảnh khuôn mặt khi chấm. |
| Luật bảo hiểm, thuế thay đổi thì sao? | Tỉ lệ bảo hiểm, mức trần, giảm trừ gia cảnh, biểu thuế là tham số có ngày hiệu lực; nhân sự tự cập nhật. Giá trị trong demo là mẫu, khi triển khai sẽ đối chiếu quy định hiện hành. |
| Có tính làm thêm giờ, ca đêm, lương sản phẩm, hoa hồng không? | Demo tính lương theo ngày công, thưởng nhập tay. Bản triển khai bổ sung làm thêm giờ 150%, 200%, 300%, phụ cấp ca đêm, công thức hoa hồng theo doanh số. |
| Có xuất file nộp BHXH, khai thuế, chi lương ngân hàng không? | Demo chưa có. Bản triển khai xuất được file theo mẫu cơ quan BHXH và file chi lương theo mẫu ngân hàng công ty dùng; nộp điện tử vẫn qua phần mềm chuyên dụng. |
| Nhân viên có xem được lương người khác không? | Không. Mỗi người chỉ thấy phiếu của mình; trưởng phòng xem công phòng mình nhưng không thấy lương. |
| Công ty có nhiều ca xoay, nhiều chi nhánh thì sao? | Demo một ca mặc định mỗi người. Bản triển khai thêm xếp ca theo tuần, nhiều địa điểm chấm công, phân quyền theo chi nhánh. |
| Đang có file Excel nhân viên và lương, chuyển sang được không? | Được, khi triển khai sẽ nhập danh sách nhân viên, lương, phép tồn từ Excel. |

---

## 4. Sau buổi demo

- [ ] Ghi lại yêu cầu khách nêu thêm vào `01-TONG-QUAN-NGHIEP-VU.md`
- [ ] Ghi lại số nhân viên, số phòng ban, loại máy chấm công đang dùng, các ca làm
- [ ] Ghi lại cấu trúc lương (các khoản phụ cấp, thưởng), quy chế phép riêng, ngày chốt công và ngày trả lương
- [ ] Gửi link demo và tài khoản `nhanvien`, `quanly`, `nhansu` cho khách tự trải nghiệm (nếu đồng ý)
