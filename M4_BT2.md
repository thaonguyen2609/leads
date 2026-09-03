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
  - Đối với dữ liệu là trống/chữ thì xóa điền NaN: số lượng Nan sau khi làm sạch là 14 
  - Với giá có định dạng khác (kiểu "12.500", "12.500,5"): Duyệt chuỗi số của từng ô xem dấu chấm có chức năng phân cách nghìn hay là dấu thập phân.
    + Nếu dấu chấm '.' có chức năng phân cách phần nghìn thì xóa bỏ
    + Nếu dấu chấm '.' là dấu thập phân thì để nguyên
    + Với số chứa các dấu phẩy ',' thì thay dấu phẩy ',' thành dấu '.'
    + Ép toàn bộ giá trị của cột giá sang kiểu float.
      => Ép định dạng chung cho dữ liệu cột giá như mẫu "200000.0" hoặc "12500.5" (dấu chấm '.' chỉ mang vai trò là dấu thập phân)
  - Loại bỏ 15 dòng trùng: số dòng sau khi bỏ trùng là 648 
  - Đối với các outlier chuyển các giá trị vào file outlier_quarantine.csv và loại bỏ khỏi tập sạch: Giá trị quá chênh lệch (gấp ~10 lần mặt bằng), có khả năng nhập sai số 0, không đại diện cho quan hệ giá thực tế.
3. Thực nghiệm trên KNN
  - Xây dựng mô hình dự đoán giá T5 từ giá T4 và độ dài tên bằng thuật toàn KNN.
  - Mô hình sẽ được thực nghiệm trên 4 trường hợp:
    * Bản bẩn chưa loại outlier không scaling: huẩn luyên trên file df gốc 'bao_gia_ncc_loi.csv'. Nó chứa tất cả các lỗi, giá trị trống, định dạng sai, giá trị âm, trùng lặp và outlier. Và không có scale
    * Bản bẩn chưa loại outlier Có scale: huẩn luyên trên file df gốc 'bao_gia_ncc_loi.csv'. Nó chứa tất cả các lỗi, giá trị trống, định dạng sai, giá trị âm, trùng lặp và outlier. Và có scale
    * Bản sạch không scaling: huấn luyện trên file df_clean sạch, không có outlier, không có giá trị âm (vì đã loại bỏ trong clean_price và df_clean chỉ giữ các giá trị dương hợp lệ). Và huấn luyện không có scale
    * Bản sạch có scaling: huấn luyện trên file df_clean sạch, không có outlier, không có giá trị âm (vì đã loại bỏ trong clean_price và df_clean chỉ giữ các giá trị dương hợp lệ). Và huấn luyện có scale
  - Kết quả thu được sau khi huấn luyện mô hình (RMSE trên tập test)
|-----------------------------------------------|
| Dữ liệu	  | Scaling	            | RMSE        |
| Bẩn       |	Không	              | 2081.1036   |
| Bẩn       |	Có (StandardScaler)	| 3571.4413   |
| Sạch      |	Không	              | 2478.4118   |
| Sạch      |	Có (StandardScaler)	| 2840.6010   |
|-----------------------------------------------|
4. Thí nghiệm điền 0
  - Thay thế các giá trị Nan bằng 0: trung vị giá trừ 48250 -> 47000
    * Giá trung vị bị lệch lớn, không phản ảnh đúng thực tế (vì không có giá sản phẩm nào bằng 0).
    * Các chỉ số khác nhưu trung bình, ohaan vị cũng bị ảnh hưởng tương tự.
    * Nếu dùng tập dữ liệu này để huấn luyện (ví dụ KNN), các mẫu có giá 0 sẽ tạo ra các điểm dữ liệu giả, làm nhiễu không gian đặc trưng, khiến mô hình học sai quy luật (đặc biệt là KNN – vì khoảng cách Euclidean bị kéo về 0).
    * Hệ quả: RMSE có thể tăng lên, độ chính xác giảm.
    * Nếu dùng trung vị này để ước tính giá trung bình hoặc làm cơ sở đàm phán, sẽ đưa ra con số thấp hơn thực tế, gây thiệt hại cho nhà cung cấp hoặc người mua.


