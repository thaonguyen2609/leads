- PHASE 1 — Khảo sát & phân loại (READ-ONLY)
Mục đích: hiểu file trước khi can thiệp.

Đọc từng sheet ở 2 chế độ song song: công thức gốc và giá trị đã tính.

Nhận diện tất cả các bảng dữ liệu trong sheet (dò dòng tiêu đề, dòng header).

Phân loại từng dòng thành DÒNG DỮ LIỆU hoặc DÒNG TỔNG KẾT dựa trên mật độ ô điền, vị trí số liệu, keyword (cộng/tổng/VAT…).

Phát hiện lỗi: ô trống, ô = 0, ô N/A, lỗi công thức, công thức chưa tính.

Lưu lại kết quả phân loại để các phase sau tái sử dụng — không phải phân tích lại.

→ Kết quả: báo cáo đầy đủ + bản đồ dòng DATA / SUMMARY của mọi sheet.

- PHASE 2 — Chuẩn hóa header
Mục đích: đảm bảo các cột có tên đúng chuẩn để Phase 3 nhận diện được.

Chỉ chạy trên các sheet ngày trong tuần (Thứ 2 → Chủ nhật).

Nhận diện dòng header linh hoạt (chấp nhận sai chính tả, thiếu dấu).

So khớp với bộ header chuẩn 10 cột: Cơ cấu, Món ăn, Mã hàng, Nguyên liệu, Định lượng sống, Số suất, Gọi hàng, Đơn giá, Thành tiền, Tổng.

Cột nào sai → sửa thẳng về tên chuẩn (trong bộ nhớ).

Ghi log từng cột đã sửa.

→ Kết quả: mọi sheet có header chuẩn, sẵn sàng tra cứu.

- PHASE 3 — Điền Mã hàng
Mục đích: dùng sheet "Báo giá" để tra và điền mã hàng cho từng nguyên liệu.

Chuẩn bị:

Đọc sheet "Báo giá" → dựng 2 bảng tra:

(Tên NL + Đơn giá) → Mã hàng (tra chính xác).

Tên NL → danh sách (giá, mã) (tra dự phòng).

Resolve VLOOKUP:

Quét các ô chứa công thức VLOOKUP trỏ về sheet "Báo giá".

Tự thực thi VLOOKUP (không cần Excel mở, không cần cache) → ghi kết quả vào ô.

Chạy nhiều vòng để xử lý VLOOKUP lồng nhau.

Điền mã hàng — chỉ trên DÒNG DỮ LIỆU (bỏ qua dòng tổng kết):

Bước 1: Dòng có đủ NL + ĐG → tra chính xác (Tên + Giá). Khớp thì điền mã, không khớp thì để trống (không fallback).

Bước 2: Dòng thiếu ĐG → tra theo tên, lấy giá thấp nhất trong Báo giá, điền cả mã lẫn giá.

Bước 3: Cảnh báo các dòng bất thường (NL trống nhưng cột khác có dữ liệu), gắn nhãn DATA / SUMMARY để dễ đối chiếu.

→ Kết quả: mỗi nguyên liệu có Mã hàng (nếu tìm được trong Báo giá).

- PHASE 4 — Dọn dẹp dòng rác
Mục đích: xóa các dòng dữ liệu không có nguyên liệu (dòng trống/rác) mà không phá cấu trúc file.

Chỉ xóa dòng nằm trong tập DATA do Phase 1 phân loại — tuyệt đối không đụng header, tiêu đề, dòng tổng kết.

Ba thao tác bảo vệ cấu trúc:

Cứu cột "Món ăn" bị merge:

Trước khi xóa, ghi nhớ toàn bộ vùng ô gộp.

Nếu ô neo (anchor) của vùng gộp sắp bị xóa → chuyển giá trị xuống dòng sống kế tiếp.

→ Tên món ăn không bị mất khỏi các dòng nguyên liệu cùng nhóm.

Re-merge với tọa độ đã dịch:

Sau khi xóa dòng, tạo lại tất cả vùng merge với tọa độ đã trừ đi số dòng bị xóa phía trên.

→ Cấu trúc ô của các cột khác giữ nguyên như trước.

Điều chỉnh công thức:

Excel tự làm việc này, nhưng thư viện Python thì không — nên tool phải tự làm.

Quét mọi ô công thức, dịch chuyển số dòng trong các tham chiếu (A5, $B$10, range A5:B20…).

Tham chiếu vào dòng đã xóa → đánh dấu #REF! (giống Excel).

→ Số liệu, công thức của các cột khác không bị lệch.

→ Kết quả: file sạch dòng rác, cấu trúc nguyên vẹn.

📤 KẾT THÚC
Ghi ra file output duy nhất (PO VC26 tuần 03.08_fill.xlsx).

File gốc giữ nguyên không đổi.

In tổng kết: số bảng, số dòng data/summary, số ô lỗi từng loại, số mã hàng đã điền, số dòng đã xóa, số formula đã điều chỉnh.
