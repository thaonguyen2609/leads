BÁO CÁO MODULE 5: HỒI QUY TUYẾN TÍNH DỰ ĐOÁN GIÁ NHÀ  
Yêu cầu module: Đầu vào cho trước diện tích và giá của 5 căn hộ. Yêu cầu dự đoán giá cho căn hộ có diện tích 80m2, tính R2 để đánh giá mức độ phù hợp của mô hình kèm biểu đồ Scatter.  
1. Dữ liệu  
Dữ liệu có 5 mẫu căn hộ gồm diện tích và giá tương ứng  
|---------------------------------------------------------|  
| Điểm căn hộ | Diện tích (m²) | Giá thực tế (Triệu đồng) |  
|-------------|----------------|--------------------------|  
| Căn 1       | 45             | 1800                     |  
| Căn 2       | 60             | 2500                     |  
| Căn 3       | 72             | 3100                     |  
| Căn 4       | 85             | 3600                     |  
| Căn 5       | 95             | 4200                     |    
|---------------------------------------------------------|  
=> Đánh giá dữ liệu: Số lượng dữ liệu đầy đủ không bị thiếu, không có giá trị bất thường, không cần chỉnh sửa. Trong quá trình tính toán sẽ chuyển toàn bộ sang kiểu số thực float để tránh sai số làm tròn.
2. Cơ sở lý thuyết
- Không có đường thẳng nào đi qua hoàn hảo cả 5 điểm thực tế. Sẽ luôn có độ lệnh (sai số) giữa giá thực tế Yi và giá dự đoán Y'i. Hồi quy tuyến tính dẽ tìm đường thẳng sao cho tổng bình phương sai số là nhỏ nhất:  
  $$\text{Minimize } \sum (y_i - \hat{y}_i)^2$$ với $$\hat{y}_i$$ = a $$\{x_i}$$ + b
- Hệ số góc a: Với mỗi khi diện tích tăng 1m2, giá nhà trung bình sẽ tăng thêm a triệu đồng  
  $a = \frac{\text{Cov}(x, y)}{\text{Var}(x)}$ = $$\frac{\sum (x_i - \bar{x})(y_i - \bar{y})}{\sum (x_i - \bar{x})^2} $$  
  Với  $\bar{x}$, $\bar{y}$ lần lượt là các giá trị trung bình của diện tích (x) và giá thực tế (y)
- Chỉ số $R^2$:
    * Baseline (Mô hình gốc đơn giản nhất): Nếu không có máy học, cách đoán đơn giản nhất khi gặp nhà mới là lấy trung bình cộng giá của 5 căn nhà cũ ($\bar{y}$). Khi đó sai số chính là tổng biến thiên toàn phần ($SS_{tot}$).
    * $R^2$ đo lường xem mô hình đường thẳng $y = ax + b$ đã giúp giảm được bao nhiêu % sai số so với việc chỉ đoán mò bằng số trung bình.
    * $R^2$ là hệ số xác định:  
      $$R^2 = 1 - \frac{SS_{res}}{SS_{tot}}$$  
      trong đó: \
        + $SS_{tot} = \sum (y_i - \bar{y})^2$: tổng bình phương độ lệch của giá trị thực tế so với giá trị trung bình.
        + $SS_{res} = \sum (y_i - \hat{y}_i)^2$: tổng bình phương phần dư ( sai số của mô hình).  
      
3. Các bước tính chi tiết  
- Tính giá trị trung bình x và y  
  $\bar{x}$ = (45+60+72+85+95)/5 = 71.4m2  
  $\bar{y}$ = (1800+2500+3100+3600+4200)/5 = 3040.0 triệu  
- Lập bảng tính sai lệch, phương sai và hiệp phương sai    
  | $x_i$ | $y_i$ | $x_i - \bar{x}$ | $y_i - \bar{y}$ | $(x_i - \bar{x})^2$ | $(x_i - \bar{x})(y_i - \bar{y})$ |  
|:---:|:---:|:---:|:---:|:---:|:---:|  
| 45 | 1800 | -26.4 | -1240 | 696.96 | 32,736 |  
| 60 | 2500 | -11.4 | -540 | 129.96 | 6,156 |  
| 72 | 3100 | 0.6 | 60 | 0.36 | 36 |  
| 85 | 3600 | 13.6 | 560 | 184.96 | 7,616 |  
| 95 | 4200 | 23.6 | 1160 | 556.96 | 27,376 |  
| **Tổng ($\sum$)** | — | **0** | **0** | **1,569.20** | **73,920.00** |
- Tính hệ số góc a:  
  $$a = \frac{\sum (x_i - \bar{x})(y_i - \bar{y})}{\sum (x_i - \bar{x})^2} = \frac{73920}{1569.2} \approx 47.1068\text{ (triệu đồng/m}^2\text{)}$$
- Tính hệ số chặn b:  
  $$b = \bar{y} - a \cdot \bar{x} = 3040 - (47.1068 \times 71.4) = 3040 - 3363.4259 \approx -323.4259\text{ (triệu)}$$  
  => Phương trình hồi quy tìm được: y = 47.1068*x - 323.4259  
4. Dự đoán giá cho diện tích 80m2  
  y = 47.1068*80 - 323.4259 = 3445.12 triệu  
5. Đánh giá sai số và tính $R^2$  
  - Tổng biến thiên toàn phần (Sai số của Baseline khi đoán bằng $\bar{y}$):  
    $$SS_{tot} = \sum (y_i - \bar{y})^2 = (-1240)^2 + (-540)^2 + 60^2 + 560^2 + 1160^2 = 3,492,000$$    
  - Tổng bình phương sai số còn lại của mô hình hồi quy ($SS_{res}$):  
      * Căn 1 ($45\text{ m}^2$): $\hat{y}_1 = 1796.38 \rightarrow e_1 = 1800 - 1796.38 = 3.62 \rightarrow e_1^2 \approx 13.10$
      * Căn 2 ($60\text{ m}^2$): $\hat{y}_2 = 2502.98 \rightarrow e_2 = 2500 - 2502.98 = -2.98 \rightarrow e_2^2 \approx 8.90$
      * Căn 3 ($72\text{ m}^2$): $\hat{y}_3 = 3068.26 \rightarrow e_3 = 3100 - 3068.26 = 31.74 \rightarrow e_3^2 \approx 1007.17$
      * Căn 4 ($85\text{ m}^2$): $\hat{y}_4 = 3680.65 \rightarrow e_4 = 3600 - 3680.65 = -80.65 \rightarrow e_4^2 \approx 6504.84$   
      * Căn 5 ($95\text{ m}^2$): $\hat{y}_5 = 4151.72 \rightarrow e_5 = 4200 - 4151.72 = 48.28 \rightarrow e_5^2 \approx 2330.90$
        
  => $SS_{res} = \sum e_i^2 = 13.10 + 8.90 + 1007.17 + 6504.84 + 2330.90 \approx 9,864.91$  
  - Hệ số xác định $R^2$:
    $$R^2 = 1 - \frac{SS_{res}}{SS_{tot}} = 1 - \frac{9864.91}{3492000} = 1 - 0.002825 = 0.9972$$ (tương đương 99.72%)  
=> Đánh giá:
      * Với R2 = 0.9972 có nghĩa 99.72% sự biến động của giá được giả thích bởi diện tích. Điều này chứng tỏ mối quan hệ giữa diện tích và giá trong dữ liệu này gần như tuyến tính hoàn hảo. 
      * Mặt khác: Mô hình được đánh giá trên mô hình qua ít dữ liệu, hệ số chặn b âm không có ý nghĩa thực tế (khi diện tích = 0 thì giá âm -> phi lý). Ngoài ra, trong thực tế, giá nhà còn phụ thuộc vào các yếu tố khác như vị trí, tình trạng nhà, tiện ích xung quanh. Vì vậy, cần phải thẩm định thêm về cỡ mẫu, tính hợp lý của hệ số góc/chặn và kiểm thử trên tập dữ liệu độc lập trước khi đưa vào sản xuất.
 
