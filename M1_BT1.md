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
<table><thead><tr><th><span class="">Tiêu chí</span></th><th><span class="">Có cấu trúc (Excel)</span></th><th><span class="">Bán cấu trúc (XML/JSON)</span></th><th><span class="">Phi cấu trúc (PDF scan, Email, Ảnh)</span></th></tr></thead><tbody><tr><td><span class="">% thời gian ước tính cho làm sạch</span></td><td><span class="">20%</span></td><td><span class="">45-50%</span></td><td><span class="">70-80%</span></td></tr><tr><td><span class="">Công cụ/công nghệ cần thiết</span></td><td><span class="">Excel, Pandas</span></td><td><span class="">xml.etree, json</span></td><td><span class="">OCR, OpenCV, NLP</span></td></tr><tr><td><span class="">Rủi ro chính</span></td><td><span class="">Sai định dạng, ô trống, trùng</span></td><td><span class="">Thiếu tag/key, cú pháp sai, kiểu không nhất quán</span></td><td><span class="">OCR sai, chất lượng ảnh kém, văn bản phức tạp</span></td></tr><tr><td><span class="">Phương pháp xử lý điển hình</span></td><td><span class="">- Kiểm tra null, điền mean/mode</span><br><span class="">- Chuẩn hóa kiểu</span><br><span class="">- Xóa trùng</span><br><span class="">- Regex</span></td><td><span class="">- Parse cẩn thận bắt lỗi</span><br><span class="">- Kiểm tra key tồn tại</span><br><span class="">- Validation schema</span><br><span class="">- Gán default</span></td><td><span class="">- Tiền xử lý ảnh (grayscale, threshold, xoay)</span><br><span class="">- OCR + hậu xử lý (sửa chính tả)</span><br><span class="">- Tokenization, bỏ stopwords</span></td></tr></tbody></table>  

3. Bản Đồ Vòng Đời Dự Án AI (8 Bước Tiêu Chuẩn)

- Bước 1: Xác định mực tiêu kinh doanh  
  * Xác định rõ bài toán cụ thể: Ví dụ: "Xây dựng hệ thống tự động trích xuất thông tin từ các nguồn dữ liệu hỗn hợp để tạo báo cáo tổng hợp, giảm 90% thời gian nhập liệu thủ công."
  * Xác định tiêu chí thành công (KPI): Độ chính xác tối thiểu 85% trên tập kiểm tra, thời gian xử lý dưới 2 giây/record.
- Bước 2: Thu thập dữ liệu  
  * Gom tất cả nguồn dữ liệu từ các phòng ban vào một kho lưu trữ tập trung (ví dụ: thư mục dự án trên máy chủ hoặc trên Cloud).
  * Thực hiện quản lý phiên bản dữ liệu (Data Versioning): Ghi rõ nhật ký (log) cho từng file bao gồm: ngày lấy, nguồn gốc, dung lượng, checksum (mã băm để kiểm tra file có bị sửa đổi sau này không). Ví dụ: bang_gia_20260827_v1.xlsx.
  * Đảm bảo tuân thủ bảo mật và quyền riêng tư (ẩn danh thông tin khách hàng nếu có) trước khi đưa vào phân tích.
- Bước 3: Khám phá dữ liệu & Kiểm kê dữ liệu  
  * Sử dụng thư viện (Pandas, Plotly) để thực hiện thống kê mô tả toàn diện. Tính toán số lượng bản ghi, tỷ lệ giá trị thiếu (missing), tỷ lệ outlier.  
  * Với văn bản và ảnh: Đếm số lượng file bị lỗi (không mở được, file rỗng, XML/JSON invalid).
  * Lập bảng kiểm kê lỗi trước khi xử lý.
- Bước 4: Tiền xử lý % Làm sạch dữ liệu  
  * Đối với Excel (có cấu trúc):  
    * Xử lý ô trống: Điền giá trị trung bình (mean) hoặc trung vị (median) nếu tỷ lệ thiếu < 5%. Nếu > 20%, cân nhắc xóa cột đó.
    * Chuẩn hóa định dạng: Chuyển cột ngày tháng về chuẩn YYYY-MM-DD; chuyển cột giá về kiểu số, loại bỏ đơn vị tiền tệ (ví dụ: "10 triệu" -> 10000000).
    * Xóa bản ghi trùng lặp dựa trên mã sản phẩm hoặc khóa chính.
  * Đối với XML và JSON (bán cấu trúc):
    * Parse file, bắt lỗi ngoại lệ (exception) cho các file sai cú pháp.
    * Làm phẳng (flatten) dữ liệu lồng nhau: Chuyển các object lồng thành các cột riêng (ví dụ: customer.name -> cột customer_name).
    * Xử lý key thiếu: Kiểm tra sự tồn tại của key trước khi truy cập; nếu thiếu, gán giá trị mặc định rỗng hoặc 0 và ghi log để kiểm tra sau.
  * Đối với Ảnh và PDF Scan (phi cấu trúc):
    * Tiền xử lý ảnh (OpenCV): Xoay ảnh thẳng (deskew), chuyển sang ảnh xám (grayscale), tăng độ tương phản (threshold), khử nhiễu để tối ưu cho OCR.
    * Áp dụng OCR (Tesseract hoặc PaddleOCR) để chuyển ảnh thành văn bản.
    * Hậu xử lý OCR: Sử dụng từ điển tiếng Việt hoặc mô hình sửa lỗi chính tả (VD: pySpellChecker) để sửa các từ bị đọc sai (ví dụ: "san pharn" -> "sản phẩm").
  * Đối với Email (phi cấu trúc):
    * Trích xuất phần body (nội dung), loại bỏ HTML tags, chữ ký email.
    * Chuẩn hóa văn bản: Chuyển về chữ thường, loại bỏ ký tự đặc biệt, emoji, URL.
    * Chuẩn hóa chính tả và viết tắt (ví dụ: "k" -> "không", "dc" -> "được").  
Tất cả các bước sửa lỗi đều phải ghi log chi tiết (lỗi gì, sửa thành gì) để có thể đối chiếu và tái lập kết quả.
- Bước 5: Trích xuất đặc trưng & Mã hóa dữ liệu  
  Mục tiêu là chuyển đổi dữ liệu sạch thành các vector số mà thuật toán học máy có thể hiểu được.  
  Chia dữ liệu thành các loại đặc trưng:  
  * Đặc trưng định lượng (số): Chuẩn hóa về cùng tỷ lệ bằng StandardScaler hoặc MinMaxScaler (đặc biệt quan trọng cho các thuật toán như SVM, KNN, Neural Network).
  * Đặc trưng định tính (phân loại): Mã hóa bằng One-Hot Encoding nếu ít giá trị, hoặc Label Encoding nếu là thứ bậc.
  * Với dữ liệu văn bản (Email, OCR text): Áp dụng các kỹ thuật biểu diễn văn bản: Nếu dùng mô hình truyền thống, dùng TF-IDF hoặc Bag of Words. Nếu dùng mô hình deep learning, dùng PhoBERT hoặc các embedding đã được huấn luyện sẵn.
  * Với dữ liệu ảnh (nếu cần nhận dạng khuôn dấu/chữ ký):
  * Sử dụng các mô hình CNN pre-trained (ResNet, EfficientNet) để trích xuất vector đặc trưng của ảnh.
- Bước 6: Chia dữ liệu & Huấn luyện mô hình  
  * Chia dữ liệu: Tách tập dữ liệu thành 3 phần rõ ràng:
    * Tập Huấn luyện (Train): 70% - dùng để học.
    * Tập Xác thực (Validation): 15% - dùng để điều chỉnh siêu tham số.
    * Tập Kiểm tra (Test): 15% - dùng để đánh giá cuối cùng (chỉ dùng 1 lần duy nhất).
  * Huấn luyện mô hình cơ sở (Baseline): Bắt đầu với các mô hình đơn giản như Linear Regression, Logistic Regression, Decision Tree để có điểm tham chiếu.
  * Tối ưu siêu tham số (Hyperparameter Tuning): Sử dụng Grid Search hoặc Bayesian Optimization trên tập Validation để tìm ra bộ tham số tốt nhất cho các mô hình phức tạp hơn (XGBoost, Random Forest, hoặc Fine-tune PhoBERT).  
  Lưu ý: Không được chạm vào tập Test trong bước này.
- Bước 7: Đánh giá độc lập & Phân tích sai số  
  * Đánh giá mô hình cuối cùng trên tập Test (chưa từng thấy) để có các chỉ số khách quan: Accuracy, Precision, Recall, F1-Score, và ma trận nhầm lẫn (Confusion Matrix).
  * Phân tích sai số (Error Analysis): Xem những trường hợp nào mô hình dự đoán sai. Ví dụ: OCR đọc sai tên sản phẩm dẫn đến phân loại sai; hoặc email viết tắt quá nhiều làm mất nghĩa.
  * Vòng lặp cải tiến: Dựa trên phân tích sai số, quay lại Bước 4 (Làm sạch) để xử lý kỹ hơn những lỗi cụ thể đó, hoặc quay lại Bước 5 (Feature Engineering) để thêm đặc trưng mới. Quá trình này lặp lại cho đến khi đạt KPI đề ra.
- Bước 8: Triển khai & Giám sát  
  * Triển khai: Đóng gói pipeline (từ Bước 1 đến Bước 7) thành một dịch vụ API sử dụng FastAPI hoặc Flask. Người dùng có thể gửi file Excel/PDF/Email lên và nhận về kết quả phân tích.
  * Giám sát thực tế 
    * Theo dõi Data Drift: Kiểm tra xem phân phối dữ liệu đầu vào (ví dụ: giá trị đơn hàng, độ dài email) có thay đổi theo thời gian không. Nếu có, mô hình sẽ kém chính xác.
    * Theo dõi Concept Drift: Quan hệ giữa đầu vào và đầu ra có thay đổi không.
    * Theo dõi hiệu năng (latency, thời gian phản hồi) và độ chính xác dự đoán thực tế.


