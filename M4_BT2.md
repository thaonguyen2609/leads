## Báo cáo dự án M4.BT2 — Pipeline làm sạch bao_gia_ncc_loi.csv + thực nghiệm bẩn vs sạch
## Dữ liệu: Báo giá nguyên vật liệu từ 3 nhà cung cấp (Hiền Nam, Hải Anh, Tâm Nhung) trong 2 tháng (4 và 5) là đầu vào cho mọi bài toán phân tích giá. Dữ liệu thô thường chứa nhiều lỗi: giá trống, sai định dạng, trùng lặp, giá bất thường (outlier).
## Mục tiêu: Xây dựng một pipeline làm sạch tự động, chứng minh lợi ích của việc làm sạch thông qua thực nghiệm học máy. Đồng thời, chỉ ra tác hại của việc xử lý thiếu sót (điền 0) lên thống kê.
Các công việc cần thực hiện:
- Kiểm kê chi tiết các loại lỗi trước khi sửa.
- Xây dựng hàm chuẩn hóa giá clean_price xử lý 5 định dạng khác nhau.
- Loại bỏ dòng trùng và cách ly outlier (3 giá trị).
- Đánh giá ảnh hưởng của việc làm sạch và scaling lên độ chính xác dự đoán (KNN).
- Thí nghiệm điền 0 vào ô trống và so sánh thống kê.
1. Đọc file dữ liệu và phân tích ban đầu
  - File bao gồm 663 dòng x 6 cột [ Ma_hang, Ten_hang, Don_vi_tinh,	Thang	NCC	Gia ]
  - Phân tích, kiểm kê lỗi file: Duyệt từng giá trị trong cột Gia
    + Giá trống (ô bị để trống hoặc chỉ toàn khoảng trắng ): có 8 giá trị trống
    + Giá chứa chữ cái: có 4 giá trị giá chứa kí tự chữ
    + Giá có định dạng khác (kiểu "12.500", "12.500,5"): có 83 giá trị có dạng khác
    + Giá mang giá trị âm (không hợp lệ với dữ liệu là giá bán): có 2 giá trị âm
    + Giá bất thường (mức giá cao chênh lệch bất thường với mặt bằng chung): có 3 giá trị bất thường
    + Số dòng bị trùng theo bộ [Ma_hang + NCC + Thang]: có 30 dòng (bao gồm 15 bộ dữ liệu theo bị lặp lại 2 lần)
2. Chuẩn hóa dữ liệu Giá
  - Đối với dữ liệu là trống/chữ thì xóa điền NaN
  - Với giá có định dạng khác (kiểu "12.500", "12.500,5"): Duyệt chuỗi số của từng ô xem dấu chấm có chức năng phân cách nghìn hay là dấu thập phân.
    + Nếu dấu chấm '.' có chức năng phân cách phần nghìn thì xóa bỏ
    + Nếu dấu chấm '.' là dấu thập phân thì để nguyên
    + Với số chứa các dấu phẩy ',' thì thay dấu phẩy ',' thành dấu '.'
    + Ép toàn bộ giá trị của cột giá sang kiểu float.
      => Ép định dạng chung cho dữ liệu cột giá như mẫu "200000.0" hoặc "12500.5" (dấu chấm '.' chỉ mang vai trò là dấu thập phân)
  - 
