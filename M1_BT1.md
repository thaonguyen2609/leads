1. Kiểm kê phân loại dữ liệu
<table border="1" cellpadding="8" cellspacing="0" style="border-collapse: collapse; width: 100%;">
  <thead>
    <tr>
      <th>STT</th>
      <th>Tài sản dữ liệu</th>
      <th>Loại</th>
      <th>Lý do phân loại</th>
      <th>Ứng dụng AI có thể làm được</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1</td>
      <td>Bảng giá 500 dòng trong Excel</td>
      <td>Có cấu trúc</td>
      <td>Dữ liệu được tổ chức rõ ràng thành hàng và cột; mỗi cột có kiểu dữ liệu cố định (số, văn bản, ngày tháng); dễ dàng truy vấn, lọc, sắp xếp và tính toán bằng Excel hoặc SQL.</td>
      <td>Dự báo xu hướng giá và phát hiện bất thường: Sử dụng các mô hình hồi quy (Linear Regression, Random Forest) để dự đoán giá tương lai dựa trên dữ liệu lịch sử, hoặc dùng Isolation Forest để phát hiện các mức giá bất thường (outlier).</td>
    </tr>
    <tr>
      <td>2</td>
      <td>File XML phiếu đặt hàng</td>
      <td>Bán cấu trúc</td>
      <td>Có cấu trúc dạng cây với các thẻ (tag) mở/đóng, nhưng số lượng và độ sâu của các thẻ có thể khác nhau giữa các phiếu. Dữ liệu có tổ chức nhưng không cứng nhắc như bảng.</td>
      <td>Tự động trích xuất và tổng hợp đơn hàng: Dùng parser (xml.etree, BeautifulSoup) để trích xuất các trường thông tin (mã đơn, ngày, danh sách sản phẩm, tổng tiền) từ nhiều file XML, sau đó hợp nhất vào một kho dữ liệu tập trung để phân tích báo cáo kinh doanh.</td>
    </tr>
    <tr>
      <td>3</td>
      <td>Hợp đồng PDF chụp bằng máy ảnh (scan)</td>
      <td>Phi cấu trúc</td>
      <td>File PDF này về bản chất là ảnh (raster) chụp từ bản giấy, máy tính chỉ thấy các điểm ảnh (pixel), không thể nhận dạng văn bản một cách trực tiếp. Cần kỹ thuật OCR để chuyển thành text trước khi xử lý.</td>
      <td>Trích xuất điều khoản và phân loại hợp đồng: Kết hợp OCR (Tesseract, PaddleOCR) để số hóa văn bản, sau đó dùng mô hình NLP (ví dụ: PhoBERT, NER) để trích xuất các thông tin then chốt (ngày ký, bên tham gia, số tiền, điều khoản chính) và tự động phân loại loại hợp đồng (mua bán, thuê, hợp tác...).</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Email than phiền của khách</td>
      <td>Phi cấu trúc</td>
      <td>Phần nội dung (body) là văn bản tự do, không có khuôn mẫu cố định. Mỗi khách hàng diễn đạt theo cách riêng, có thể viết tắt, sai chính tả, sử dụng ngôn ngữ cảm xúc. Dữ liệu này cần xử lý NLP để hiểu ý nghĩa.</td>
      <td>Phân tích cảm xúc và phân loại khiếu nại: Dùng các mô hình phân loại văn bản (Naive Bayes) để tự động phân loại email thành các nhóm (sản phẩm hỏng, giao hàng chậm, nhân viên phục vụ kém...) và đánh giá mức độ tức giận/tích cực; từ đó gợi ý phản hồi và ưu tiên xử lý.</td>
    </tr>
    <tr>
      <td>5</td>
      <td>File JSON trả về từ API</td>
      <td>Bán cấu trúc</td>
      <td>Dữ liệu ở dạng key-value, có thể lồng nhau và có các mảng. Tuy nhiên, cấu trúc không cố định hoàn toàn: các key có thể vắng mặt, kiểu dữ liệu có thể thay đổi giữa các lần gọi API. Cần parse và kiểm tra trước khi sử dụng.</td>
      <td>Phân tích dữ liệu real-time và dự đoán xu hướng: Nhận dữ liệu JSON từ API (ví dụ: thông tin giao dịch, hành vi người dùng), parse thành bảng, sau đó xây dựng mô hình dự báo để dự đoán xu hướng người dùng hoặc phát hiện sự kiện bất thường trong luồng dữ liệu.</td>
    </tr>
    <tr>
      <td>6</td>
      <td>Ảnh chụp biên bản nghiệm thu</td>
      <td>Phi cấu trúc</td>
      <td>Đơn thuần là file ảnh (JPEG, PNG). Máy tính chỉ nhìn thấy ma trận pixel, không có cấu trúc văn bản hay bảng biểu. Cần xử lý thị giác máy tính và OCR để khai thác.</td>
      <td>Nhận dạng và trích xuất thông tin biên bản: Sử dụng mô hình OCR (PaddleOCR, EasyOCR) kết hợp với các mô hình thị giác để xác định khu vực chữ ký, con dấu, ngày tháng; sau đó trích xuất các trường thông tin chính (mã biên bản, ngày kiểm tra, kết luận) và số hóa vào hệ thống quản lý.</td>
    </tr>
  </tbody>
</table>

2. Đánh giá mức độ phức tạp và công sức làm sạch
<table><thead><tr><th><span class="">Tiêu chí</span></th><th><span class="">Có cấu trúc (Excel)</span></th><th><span class="">Bán cấu trúc (XML/JSON)</span></th><th><span class="">Phi cấu trúc (PDF scan, Email, Ảnh)</span></th></tr></thead><tbody><tr><td><span class="">% thời gian ước tính cho làm sạch</span></td><td><span class="">20%</span></td><td><span class="">45-50%</span></td><td><span class="">70-80%</span></td></tr><tr><td><span class="">Công cụ/công nghệ cần thiết</span></td><td><span class="">Excel, Pandas, OpenRefine</span></td><td><span class="">xml.etree, json, BeautifulSoup</span></td><td><span class="">OCR (Tesseract, PaddleOCR), OpenCV, NLP</span></td></tr><tr><td><span class="">Rủi ro chính</span></td><td><span class="">Sai định dạng, ô trống, trùng</span></td><td><span class="">Thiếu tag/key, cú pháp sai, kiểu không nhất quán</span></td><td><span class="">OCR sai, chất lượng ảnh kém, văn bản phức tạp</span></td></tr><tr><td><span class="">Phương pháp xử lý điển hình</span></td><td><span class="">- Kiểm tra null, điền mean/mode</span><br><span class="">- Chuẩn hóa kiểu</span><br><span class="">- Xóa trùng</span><br><span class="">- Regex</span></td><td><span class="">- Parse cẩn thận bắt lỗi</span><br><span class="">- Kiểm tra key tồn tại</span><br><span class="">- Validation schema</span><br><span class="">- Gán default</span></td><td><span class="">- Tiền xử lý ảnh (grayscale, threshold, xoay)</span><br><span class="">- OCR + hậu xử lý (sửa chính tả)</span><br><span class="">- Tokenization, bỏ stopwords</span></td></tr></tbody></table>  

3. Bản Đồ Vòng Đời Dự Án AI (8 Bước Tiêu Chuẩn)

- Bước 1: Xác định mục tiêu (Problem Formulation)**
   * Xác định rõ bài toán kinh doanh (ví dụ: tự động hóa đọc hợp đồng scan để giảm 90% thời gian nhập liệu thủ công).
- Bước 2: Thu thập & Tích hợp dữ liệu (Data Acquisition)**
   * Gom cả 6 nguồn dữ liệu (Excel, XML, PDF, Email, JSON, Ảnh) vào kho lưu trữ an toàn (Data Lake / Cloud Storage).
- Bước 3: Khám phá & Kiểm kê dữ liệu (EDA - Exploratory Data Analysis)**
   * Quét toàn bộ dữ liệu để đếm số lượng lỗi, kiểm tra độ phân bố và lập bảng thống kê trước khi can thiệp.
- Bước 4: Tiền xử lý & Làm sạch (Data Preprocessing & Cleaning)**
   * *Excel/JSON/XML:* Điền giá trị khuyết hợp lý, chuyển dạng phẳng.
   * *Ảnh/Scan:* Khử nhiễu, xoay thẳng ảnh, cắt viền, trích xuất text qua OCR.
   * *Email:* Tách từ, chuẩn hóa chính tả, loại bỏ từ rác.
- Bước 5: Trích xuất đặc trưng (Feature Engineering)**
   * Biến đổi dữ liệu sạch thành các ma trận số/vector đặc trưng mà thuật toán học máy có thể tính toán được.
- Bước 6: Huấn luyện mô hình (Model Training)**
   * Đưa dữ liệu vào huấn luyện các mô hình phù hợp: Hồi quy (dự đoán giá), NLP (phân loại email), Computer Vision (soi con dấu).
- Bước 7: Đánh giá & Kiểm thử độc lập (Model Evaluation)**
   * Kiểm tra độ chính xác của mô hình trên tập dữ liệu chưa từng thấy để tránh hiện tượng "học vẹt" (Overfitting).
- Bước 8: Triển khai & Giám sát (Deployment & Monitoring)**
   * Đóng gói mô hình thành dịch vụ API để ứng dụng thực tế gọi vào; theo dõi liên tục để phát hiện khi chất lượng ảnh/văn bản đời thực thay đổi (Data Drift).
