# Báo cáo module 2:
## Yêu cầu của module: đọc 2 file danh_muc_vat_tu_loi.csv và quy_trinh_mo_phong.md, kiểm tra đánh giá 2 file trên. Phân tích cấu trúc rồi đưa ra kỹ năng xử lý. Viết hàm Python đánh giá trả điểm theo luật BM.02.02/NCC trong quy_trinh_mo_phong.md.  
1. Đoc và kiểm tra ban đầu
- file danh_muc_vat_tu_loi.csv
  * Gồm 124 dòng, 6 cột [Ma_vat_tu, Ten_vat_tu, Don_vi_tinh, Nhom1, Nhom2, Trang_thai] và 6491 ký tự.
  * Kiểm tra ban đầu:
      + ma_trung: ['NLT0010', 'NLR0007', 'NLT0010', 'NLR0007', 'NLT0010', 'NLG0014', 'NLR0007', 'NLG0014', 'NLG0014']
      + gia_tri_thieu: {'Nhom2': 1}
      + nhom1_khong_hop_le: ['Nhom1_missing', 'Nhom1_missing', 'Nhom1_missing', 'Nhom1_missing', 'Nhom1_missing', 'Nhom1_missing']
      + nhom2_ky_la: ['NCC', 'XXX', '???']
- file quy_trinh_mo_phong.md
  * Gồm 2608 kí tự, dạng văn bản thuần, loại phi cấu trúc dạng chuỗi.
2.  Sự khác biệt giữa .csv và .md
    - Bản chất:
      * CSV (Comma‑Separated Values): Là định dạng tệp văn bản đơn giản, trong đó dữ liệu được tổ chức thành bảng với các hàng và cột. Mỗi dòng là một bản ghi, các trường được phân cách bằng dấu phẩy (hoặc dấu khác như tab, dấu chấm phẩy). CSV tập trung vào dữ liệu có cấu trúc, dễ dàng nhập/xuất cho các ứng dụng bảng tính hoặc cơ sở dữ liệu.
      * Markdown: Là ngôn ngữ đánh dấu nhẹ, dùng để định dạng văn bản với cú pháp đơn giản như # cho tiêu đề, * cho danh sách, | cho bảng. Mục đích chính là tạo tài liệu dễ đọc cho con người và dễ dàng chuyển đổi sang HTML. Markdown không có cấu trúc cứng, nội dung chủ yếu là văn bản tự nhiên.
    - Cách xử lý và kỹ năng tương ứng:
      * Đối với CSV:
         + Cần xác định mã hóa (UTF‑8, ANSI…), dấu phân cách, dấu ngoặc kép.
         + Sử dụng thư viện như pandas, csv module, hoặc Excel để đọc/ghi.
         + Yêu cầu kỹ năng làm sạch dữ liệu: xử lý giá trị thiếu, chuẩn hóa kiểu,
           phát hiện ngoại lệ, loại bỏ khoảng trắng thừa.
         + Phân tích chủ yếu là thống kê, tổng hợp, nối ghép, lọc.
      * Đối với Markdown:
         + Phân tích cú pháp bằng biểu thức chính quy hoặc dùng thư viện parser (ví dụ: markdown‑it, python‑markdown) để xây dựng cây cú pháp.
         + Cần hiểu cấu trúc tài liệu để trích xuất thông tin, như các mục, bước, bảng biểu, biểu mẫu.
         + Yêu cầu kỹ năng xử lý văn bản tự nhiên và đọc hiểu tài liệu kỹ thuật.
         + Phân tích chủ yếu là kiểm tra tính đầy đủ, tính nhất quán, phát hiện phần thiếu sót hoặc không rõ ràng.
    - Ứng dụng thực tế:
      * CSV thường được dùng trong kho dữ liệu (data warehouse), báo cáo bán hàng, danh mục hàng hóa, dữ liệu giao dịch. Công cụ phân tích chủ yếu là pandas, SQL, Power BI, Tableau.
      * Markdown thường được dùng để viết hướng dẫn sử dụng, tài liệu dự án, quy trình sổ tay kỹ thuật. Việc xử lý tự động thường là kiểm tra định dạng, trích xuất thông tin để tạo báo cáo hoặc đồng bộ với hệ thống quản lý.
3. Bài học rút ra
  - Hiểu đinh dạng dữu liệu để chọn công cụ xử lý phù hợp
    * Dữ liệu có thể đến dưới nhiều hình thức khác nhau. Việc nhận diện đúng định dạng (CSV, JSON, XML, văn bản thuần, Markdown…) giúp chọn thư viện và phương pháp xử lý tối ưu, tránh mất thời gian và sai sót.
    * Áp dụng: Khi nhận file từ các bộ phận, hãy hỏi rõ cấu trúc hoặc mở thử để biết cần xử lý như thế nào.
  - Quy tăc nghiệp vụ cần được mã hóa rõ ràng và kiểm thử
    * Một quy tắc như chấm điểm giá nằm trong văn bản quy trình có thể dễ dàng chuyển thành hàm Python. Điều này giúp tự động hóa, giảm thiểu sai sót và dễ dàng kiểm tra với nhiều đầu vào.
    * Áp dụng: Khi xây dựng các công cụ phân tích, hãy viết các hàm nhỏ cho từng quy tắc, kiểm thử với các trường hợp biên để đảm bảo chính xác.
  - Kiểm tra chất lượng dữ liệu là bước bắt buộc
    * Dữ liệu đầu vào luôn có lỗi: thiếu, sai, trùng, không chuẩn. Việc phát hiện và ghi lại lỗi là quan trọng hơn là vội vàng sửa chữa. Cần đưa ra quyết định xử lý dựa trên ngữ cảnh nghiệp vụ.
    * Áp dụng: Trước khi phân tích hoặc xây dựng báo cáo, hãy dành thời gian để "nhìn sơ bộ" dữ liệu (số dòng, cột, giá trị thiếu, phân phối…). Ghi lại các vấn đề và tham khảo ý kiến chuyên môn trước khi chỉnh sửa.
