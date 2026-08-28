## BÁO CÁO DỰ ÁN M3.BT2  
## Mục tiêu: Xây dựng mô hình RandomForest tự động phân loại nhóm hàng (NLT, NLR, NLG, NLS, HHB, HHN, NLC) cho các mặt hàng mới, dựa trên tên hàng và đơn vị tính.  
## Dữ liệu: danh_muc_vat_tu_loi.csv – danh mục vật tư có nhiều lỗi (trùng, thiếu nhóm, mã sai, đơn vị tính không chuẩn).  
1. TỔNG QUAN QUY TRÌNH  
  Quy trình được thực hiện theo 8 bước chính:
  - Bước 1: Phân tích và kiểm kê lỗi – Xác định số lượng và chi tiết từng loại lỗi.  
  - Bước 2: Làm sạch dữ liệu – Xóa trùng, khôi phục Nhom1, sửa mã, chuẩn hóa đơn vị tính.  
  - Bước 3: Xây dựng đặc trưng – Tạo 4 đặc trưng số từ tên hàng và đơn vị tính.  
  - Bước 4: Huấn luyện và đánh giá – RandomForest với hai cấu hình max_depth=None và max_depth=3.  
  - Bước 5: Thí nghiệm bỏ tiền tố mã – Đo ảnh hưởng của việc loại bỏ đặc trưng prefix.  
  - Bước 6: Vẽ biểu đồ – Confusion matrix cho cả ba mô hình.  
  - Bước 7: Dự đoán mặt hàng mới – Áp dụng mô hình tốt nhất cho ba mặt hàng chưa có trong danh mục.  
  - Bước 8: Tổng kết – So sánh hiệu suất và rút ra kết luận.
2. Phân tích và kiểm lỗi
  - Đọc dữ liệu ban đầu: dữ liệu gồm 123 dòng, 6 cột.
  - Kết quả kiểm kê lỗi như sau:
      * Kiểm tra dữ liệu trùng: Phát hiện 3 mã bị trùng
        + NLT0010: xuất hiện 3 lần
        + NLR0007: xuất hiện 3 lần
        + NLG0014: xuất hiện 3 lần
      * Kiểm tra dữ liệu thiếu Nhom1: Phát hiện 6 dòng có giá trị là 'Nhom1_missing'
        + NLS0012 - Mực nang
        + NLS0013 - Tu hài
        + NLT0016 - Cá diêu hồng
        + NLT0028 - Cua đồng
        + NLT0014 - Sườn heo
        + NLG0001 - Gạo nếp cái hoa vàng
      * Kiểm tra dữ liệu cột Mã: Phát hiện 3 mã sai chuẩn
        + nl g-0999 (có khoảng trắng, có dấu gạch ngang, chữ thường)
        + NLT 0580 (có khoảng trắng)
        + nlr-0123 (có dấu gạch ngang, chữ thường)
      * Kiểm tra dữ liệu cột ĐVT: có nhiều đơn vị tình khác nhau {gói, hộp, gam, thùng, kg, thùng 10kg, Hộp 500g}
3. Làm sạch dữ liệu
  - Với các dòng trùng theo mã: giữ lại dòng dữ liệu đầu tiên và xóa các dòng dữ liệu có mã trùng còn lại.
  - Chuẩn hóa cột Mã: Loại bỏ các dấu gạch ngang, khoảng trắng, các kí tự đặc biệt; in hoa toàn bộ dữ liệu ở cột Mã
    + NLT 0580 → NLT0580 (xóa khoảng trắng)
    + nlr-0123 → NLR0123 (in hoa, xóa dấu gạch ngang)
    + nl g-0999 → NLG0999 (in hoa, chuẩn hóa)
  - Với các dòng dữ liệu bị thiếu dữ liệu cột Nhom1: Thực hiện trích xuất tiền tố mã để khôi phục
    + NLS0012 - Mực nang → Tiền tố: NLS
    + NLS0013 - Tu hài → Tiền tố: NLS
    + NLT0016 - Cá diêu hồng → Tiền tố: NLT
    + NLT0028 - Cua đồng → Tiền tố: NLT
    + NLT0014 - Sườn heo → Tiền tố: NLT
    + NLG0001 - Gạo nếp cái hoa vàng → Tiền tố: NLG
  - Chuẩn hóa ĐVT về chỉ còn {kg, gam, gói, hộp, cái, thùng}.
4. Xây dựng đặc trưng
 - Từ dữ liệu đã làm sạch, trích xuất 4 đặc trưng:
| -------------------------------------------------------------------------|
| Đặc trưng	   | Mô tả	                                       | Loại      |
---------------------------------------------------------------------------|
| prefix	     | 3 ký tự đầu của mã vật tư (one‑hot encoding)  | Rời rạc   |
| unit_clean	 | Đơn vị tính đã chuẩn hóa (one‑hot encoding)	 | Rời rạc   |
| ten_length 	 | Độ dài tên hàng (số ký tự, bỏ khoảng trắng)	 | Liên tục  |
| ten_digits	 | Số chữ số xuất hiện trong tên hàng	           | Liên tục  |
|--------------------------------------------------------------------------|
  - Thống kê đặc trưng:
    + Số lượng prefix duy nhất: 7 (NLT, NLR, NLG, NLS, HHB, HHN, NLC)
    + Số lượng ĐVT duy nhất: 4 (kg, gam, gói, hộp, cái, thùng)
    + Độ dài tên trung bình: 7.63 ký tự
    + Số chữ số trung bình trong tên: 0.01
  - Phân phối nhóm hàng sau làm sạch:
|---------------------------------------------|
| Nhóm	| Tên nhóm	              | Số lượng  |
|---------------------------------------------|
| NLT	  | Thực phẩm tươi sống	    | 30        |
| NLR	  | Rau củ quả	            | 20        |
| NLG	  | Gạo và ngũ cốc	        | 15        |
| NLS	  | Thủy hải sản	          | 15        |
| HHN	  | Hàng khô gia vị	        | 15        |
| HHB	  | Bánh và chế phẩm	      | 12        |
| NLC	  | Nguyên liệu chế biến	  | 10        |
|---------------------------------------------|
5. Huấn luyện và đánh giá mô hình
  - Chia tệp dữ liệu thành: 80% huấn luyện & 20% kiểm tra.
  - Sử dụng Randomforest với max_depth=None & max_depth=3 để huấn luyện mô hình
  - Kết quả sau khi huấn luyện
    + Mô hình 1 – max_depth=None
  |------------------------------------|
  | Chỉ số           |	Giá trị        |
  |------------------------------------|
  | Train Accuracy	 | 0.9785          |
  | Test Accuracy    | 1.0000          |
  | Chênh lệch	     | -0.0215         |
  | CV (5‑fold)	     | 0.9743 ± 0.0210 |
  |------------------------------------|
  Classification Report (test):
                precision    recall  f1-score   support
         HHB       1.00      1.00      1.00         3
         HHN       1.00      1.00      1.00         3
         NLC       1.00      1.00      1.00         2
         NLG       1.00      1.00      1.00         3
         NLR       1.00      1.00      1.00         4
         NLS       1.00      1.00      1.00         3
         NLT       1.00      1.00      1.00         6
    accuracy                           1.00        24
   macro avg       1.00      1.00      1.00        24
weighted avg       1.00      1.00      1.00        24
<img width="2770" height="2364" alt="image" src="https://github.com/user-attachments/assets/2d5dd4bf-123a-4aae-9b12-4dfa0843d9f7" />

    => Nhận xét:Mô hình đạt độ chính xác 100% trên tập kiểm tra. Tuy nhiên, tập kiểm tra chỉ có 24 mẫu, kết quả này có thể không đại diện cho tổng thể.
    
    + Mô hình 2 – max_depth=3
  |------------------------------------|
  | Chỉ số           |	Giá trị        |
  |------------------------------------|
  | Train Accuracy	 | 0.9677          |
  | Test Accuracy    | 1.0000          |
  | Chênh lệch	     | -0.0323         |
  | CV (5‑fold)	     | 0.9743 ± 0.0210 |
  |------------------------------------|
  Classification Report (test):
              precision    recall  f1-score   support
         HHB       1.00      1.00      1.00         3
         HHN       1.00      1.00      1.00         3
         NLC       1.00      1.00      1.00         2
         NLG       1.00      1.00      1.00         3
         NLR       1.00      1.00      1.00         4
         NLS       1.00      1.00      1.00         3
         NLT       1.00      1.00      1.00         6
    accuracy                           1.00        24
   macro avg       1.00      1.00      1.00        24
weighted avg       1.00      1.00      1.00        24
<img width="2770" height="2364" alt="image" src="https://github.com/user-attachments/assets/3154c346-c201-4213-8312-024df134d394" />

  => Nhận xét: Cả hai mô hình đều đạt 100% trên tập test. Tuy nhiên, do tập test nhỏ, cần đánh giá thêm qua cross-validation.
6. Thí nghiệm bỏ tiền tố mã
  - Loại bỏ đặc trưng prefix, chỉ giữ lại unit_clean, ten_length, ten_digits. Huấn luyện lại RandomForest với max_depth=None (cùng tham số).
  - Kết quả:
    + Test Accuracy: 0.2500
    + Giảm so với mô hình đầy đủ: -75.00%
  - Classification Report (không prefix):
                  precision    recall  f1-score   support
         HHB       0.50      0.33      0.40         3
         HHN       0.00      0.00      0.00         3
         NLC       1.00      0.50      0.67         2
         NLG       0.17      0.33      0.22         3
         NLR       0.00      0.00      0.00         4
         NLS       0.33      0.33      0.33         3
         NLT       0.20      0.33      0.25         6
    accuracy                           0.25        24
   macro avg       0.31      0.26      0.27        24
weighted avg       0.26      0.25      0.24        24
<img width="2809" height="2364" alt="image" src="https://github.com/user-attachments/assets/aa6fc017-1463-4573-a69b-c441e6fcdc56" />

  - Nhận xét:
    + Mô hình chỉ dựa vào tên và đơn vị tính đạt 25.00% độ chính xác.
    + Các nhóm có đặc điểm tên riêng biệt (NLR, NLS) vẫn giữ được độ chính xác tương đối, trong khi các nhóm có tên mơ hồ (HHB, HHN, NLG) bị ảnh hưởng nhiều.
    + Kết luận: Tiền tố mã đóng góp rất lớn vào hiệu suất. Tên hàng và ĐVT không đủ để phân loại chính xác.
7. Dự đoán mặt hàng mới
  Ba mặt hàng chưa có trong danh mục được đưa vào dự đoán bằng mô hình tốt nhất (max_depth=3) và mô hình không prefix để so sánh.
|-------------------------------------------------------------------------------------------------------------|
| STT	| Tên mặt hàng	        | ĐVT	 | Dự đoán (có prefix)        	| Dự đoán (không prefix)	   | Kết luận   |
|-------------------------------------------------------------------------------------------------------------|
| 1	  | Cá diêu hồng sơ chế   |	kg	 | NLC (Nguyên liệu chế biến)   |	NLG (Gạo và ngũ cốc)	     |     Khác   |
| 2	  | Bánh mỳ ruốc	        | cái	 | HHB (Bánh và chế phẩm)	      | NLT (Thực phẩm tươi sống)	 |     Khác   |
| 3	  | Sữa chua uống         | hộp  | NLG (Gạo và ngũ cốc)         |	NLG (Gạo và ngũ cốc)	     |     Giống  |
|-------------------------------------------------------------------------------------------------------------|
  - Giải thích:
    + Cá diêu hồng sơ chế: Có từ khóa "Cá" nhưng cả hai mô hình đều không dự đoán vào NLS. Điều này cho thấy mô hình chưa học được từ khóa đặc trưng hoặc do thiếu dữ liệu.
    + Bánh mỳ ruốc: Từ "Bánh" giúp mô hình có prefix dự đoán vào HHB, nhưng mô hình không prefix lại dự đoán vào NLT, cho thấy prefix đóng vai trò quan trọng.
    + Sữa chua uống: Cả hai mô hình đều dự đoán vào NLG (Gạo và ngũ cốc), điều này không hợp lý vì sữa chua không thuộc nhóm này. Có thể do thiếu dữ liệu mẫu và đặc trưng chưa đủ.
8. Kết luận
  - Mô hình RandomForest có khả năng học được quy luật phân loại nhóm hàng từ tên và đơn vị tính, nhưng phụ thuộc rất lớn vào prefix. Vì vậy việc bỏ đi 1 cột dữ liệu là điều không thể và cũng như không nên thực hiện.
    + Với đầy đủ 4 đặc trưng (có prefix), độ chính xác đạt 100% trên tập test (tuy nhiên tập test chỉ có 24 mẫu).
    + Khi bỏ prefix, độ chính xác giảm mạnh còn 25%, cho thấy mã vật tư mang thông tin phân loại chủ yếu.
  - Dự đoán mặt hàng mới cho thấy mô hình có thể gán nhóm hợp lý cho các mặt hàng có tên chứa từ khóa đặc trưng. Đối với những mặt hàng mơ hồ (Sữa chua uống), mô hình chưa thể đưa ra dự đoán chính xác.
  - Hạn chế chính: Tập dữ liệu nhỏ (117 mẫu) và tập test rất nhỏ (24 mẫu), dẫn đến kết quả 100% có thể không phản ánh đúng thực tế. Mô hình cần được thử nghiệm trên tập dữ liệu lớn hơn để đánh giá chính xác hơn.



