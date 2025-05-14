# Forecasting-CPI-in-Viet-Nam
Phương pháp nghiên cứu khoa học

Dưới đây là nội dung báo cáo dựa trên mã Python đã cung cấp, được trình bày một cách ngắn gọn, rõ ràng và chuyên nghiệp để phù hợp với một báo cáo phân tích chuỗi thời gian về Chỉ số Giá tiêu dùng (CPI).

---

# Báo Cáo Phân Tích Chuỗi Thời Gian Chỉ Số Giá Tiêu Dùng (CPI)

## 1. Mục tiêu
Phân tích đặc tính chuỗi thời gian của Chỉ số Giá tiêu dùng (CPI) theo tháng (MoM) và theo năm (YoY), bao gồm xu hướng, tính mùa vụ, tính dừng, dị biệt, và mối quan hệ với các biến kinh tế như lãi suất, giá dầu, và giá vàng.

## 2. Dữ liệu và Tiền xử lý
- **Nguồn dữ liệu**: 
  - Dữ liệu CPI được đọc từ file `cpi.csv`, bao gồm cột thời gian (`t`) và CPI MoM (`cpi_mom`).
  - Dữ liệu ngoại sinh (lãi suất, giá dầu, giá vàng) được đọc từ file `exog_data.csv`. Nếu không có, lãi suất giả lập được tạo bằng phân phối chuẩn.
- **Tiền xử lý**:
  - Chuyển đổi thời gian sang định dạng `datetime` và đặt làm chỉ số với tần suất hàng tháng (`MS`).
  - Xử lý giá trị thiếu bằng phương pháp nội suy tuyến tính.
  - Tính CPI YoY từ CPI MoM bằng công thức tích lũy 12 tháng.
  - Làm mượt dữ liệu bằng trung bình trượt (window=3).
  - Chuyển đổi log và tính sai phân để phân tích thêm.
  - Gộp dữ liệu CPI với các biến ngoại sinh (lãi suất, giá dầu, giá vàng) và thêm cột tháng, năm để phân tích mùa vụ.

## 3. Phân tích Chuỗi Thời Gian

### 3.1. Phân tích Xu hướng và Mùa vụ
- **CPI MoM**:
  - **Xu hướng**: [Tăng/Giảm/Ổn định, dựa trên kết quả `trend_direction`].
  - **Mùa vụ**: [Có/Không rõ, dựa trên `seasonality_detected`].
  - Biểu đồ phân tích (Hình 1: `cpi_mom_decomposition.png`) cho thấy chuỗi CPI MoM có xu hướng dài hạn, thành phần mùa vụ, và phần dư.
- **CPI YoY**:
  - **Xu hướng**: [Tăng/Giảm/Ổn định].
  - **Mùa vụ**: [Có/Không rõ].
  - Biểu đồ phân tích (Hình 2: `cpi_yoy_decomposition.png`) xác định rõ xu hướng và mùa vụ của chuỗi YoY.

### 3.2. Kiểm định Tính Dừng
- **CPI MoM**:
  - Kết quả kiểm định ADF: p-value = [giá trị]. 
  - Kết luận: [Dừng/Không dừng]. Nếu không dừng, chuỗi dừng sau sai phân cấp [0/1].
- **CPI YoY**:
  - Kết quả kiểm định ADF: p-value = [giá trị].
  - Kết luận: [Dừng/Không dừng]. Nếu không dừng, chuỗi dừng sau sai phân cấp [0/1].
- Biểu đồ sai phân (Hình 3: `cpi_yoy_original_vs_diff.png`) thể hiện sự thay đổi của CPI YoY sau sai phân.

### 3.3. Phân tích Tự Tương Quan
- Biểu đồ ACF và PACF (Hình 4: `cpi_acf_pacf_plots.png`) cho thấy:
  - CPI MoM có các lag đáng kể tại [vị trí lag], gợi ý mô hình ARIMA tiềm năng.
  - CPI YoY có các lag đáng kể tại [vị trí lag], hỗ trợ việc lựa chọn tham số mô hình.

### 3.4. Phân tích Mùa vụ
- **Boxplot theo tháng** (Hình 5: `cpi_boxplot_by_month.png`):
  - CPI MoM và YoY có biến động khác nhau giữa các tháng, với [tháng cụ thể] thường có giá trị cao/thấp nhất.
- **Heatmap mùa vụ** (Hình 6: `cpi_seasonal_heatmap.png`):
  - Xác định các tháng có CPI cao/thấp trong các năm, với [mô hình mùa vụ cụ thể].

### 3.5. Phân tích Dị biệt
- Phương pháp phát hiện dị biệt: IQR.
- Kết quả:
  - CPI MoM: Phát hiện [số lượng] điểm dị biệt (Hình 7: `cpi_mom_anomalies_iqr.png`).
  - CPI YoY: Phát hiện [số lượng] điểm dị biệt (Hình 8: `cpi_yoy_anomalies_iqr.png`).
- Các điểm dị biệt thường xảy ra vào [thời điểm cụ thể], có thể liên quan đến các sự kiện kinh tế.

### 3.6. Phân tích Tương Quan
- **Ma trận tương quan** (Hình 9: `cpi_correlation_heatmap.png`):
  - CPI MoM có tương quan [mức độ] với lãi suất ([giá trị]), giá dầu ([giá trị]), và giá vàng ([giá trị]).
  - CPI YoY có tương quan [mức độ] với lãi suất ([giá trị]), giá dầu ([giá trị]), và giá vàng ([giá trị]).
- Biểu đồ so sánh (Hình 10-12: `cpi_vs_interest_rate.png`, `cpi_vs_oil_price.png`, `cpi_vs_gold_price.png`) cho thấy mối quan hệ trực quan giữa CPI và các biến kinh tế.

### 3.7. Phân tích Thống kê Trượt
- Biểu đồ thống kê trượt (Hình 13: `cpi_rolling_stats.png`) cho thấy:
  - Trung bình trượt và độ lệch chuẩn trượt của CPI MoM và YoY, xác định các giai đoạn biến động cao/thấp.

### 3.8. Phân tích Phân phối
- **Histogram** (Hình 14: `cpi_histogram.png`):
  - CPI MoM và YoY có phân phối [gần chuẩn/không chuẩn].
  - Dữ liệu log và sai phân có xu hướng [gần chuẩn hơn/khác].

### 3.9. So sánh Dữ liệu Gốc, Làm mượt, và Log
- Biểu đồ so sánh (Hình 15-16: `cpi_original_vs_smoothed.png`, `cpi_original_vs_log.png`):
  - Dữ liệu làm mượt giảm nhiễu, giữ nguyên xu hướng chính.
  - Dữ liệu log giúp ổn định phương sai, phù hợp cho mô hình hóa.

## 4. Kết luận
- Chuỗi CPI MoM và YoY có [xu hướng cụ thể], với [mức độ mùa vụ] rõ rệt vào các tháng [tháng cụ thể].
- CPI có tương quan đáng chú ý với [biến kinh tế cụ thể], gợi ý tác động từ các yếu tố ngoại sinh.
- Phát hiện [số lượng] điểm dị biệt, cần xem xét thêm bối cảnh kinh tế.
- Dữ liệu đã được làm mượt, chuyển đổi log, và sai phân, sẵn sàng cho mô hình hóa (ví dụ: ARIMA, SARIMA).

## 5. Đề xuất
- Tiến hành mô hình hóa chuỗi thời gian (ARIMA/SARIMA) dựa trên kết quả ACF/PACF.
- Phân tích sâu hơn các điểm dị biệt bằng dữ liệu sự kiện kinh tế.
- Cập nhật dữ liệu ngoại sinh thường xuyên để cải thiện độ chính xác của phân tích tương quan.

## 6. Kết quả Đầu ra
- Dữ liệu phân tích được lưu tại `data/analyzed_time_series.csv`.
- Các biểu đồ được lưu trong thư mục `img/` với độ phân giải cao (300 DPI).

---

**Lưu ý**: Các giá trị cụ thể (p-value, xu hướng, mùa vụ, tương quan, số điểm dị biệt) cần được cập nhật từ kết quả thực tế của mã. Báo cáo này chỉ cung cấp khung nội dung dựa trên logic của mã.

Dưới đây là báo cáo phân tích dựa trên hai đoạn mã Python đã cung cấp, tập trung vào việc dự báo Chỉ số Giá tiêu dùng (CPI) MoM và YoY bằng các mô hình chuỗi thời gian cổ điển và Auto ARIMA. Báo cáo được trình bày ngắn gọn, rõ ràng, và chuyên nghiệp để phù hợp với mục đích phân tích và dự báo.

---

# Báo Cáo Dự Báo Chỉ Số Giá Tiêu Dùng (CPI) Bằng Các Mô Hình Chuỗi Thời Gian

## 1. Mục tiêu
Dự báo CPI MoM và CPI YoY trong 12 tháng tiếp theo sử dụng các mô hình chuỗi thời gian cổ điển (Exponential Smoothing, ARIMA, SARIMA, SARIMAX, Prophet) và Auto ARIMA, đồng thời đánh giá hiệu suất dự báo thông qua các chỉ số RMSE, MAE, MAPE, sMAPE.

## 2. Dữ liệu và Tiền xử lý
- **Nguồn dữ liệu**: 
  - Dữ liệu được đọc từ file `data/analyzed_time_series.csv`, bao gồm CPI MoM, CPI YoY, giá dầu (`oil_price`), và các biến khác.
- **Tiền xử lý**:
  - Chuyển đổi cột `time` sang định dạng `datetime` và đặt làm chỉ số với tần suất hàng tháng (`MS`).
  - Kiểm tra và nội suy giá trị thiếu bằng phương pháp tuyến tính hoặc thời gian.
  - Loại bỏ giá trị vô cực và đảm bảo dữ liệu số hợp lệ.
  - Chia dữ liệu thành tập huấn luyện (train) và kiểm tra (test), với 12 tháng cuối làm tập test.
  - Sử dụng giá dầu làm biến ngoại sinh cho SARIMAX và Prophet.
- **Kiểm tra tính dừng**:
  - CPI MoM: Tính dừng sau bậc sai phân `d_mom` = [kết quả từ `check_stationarity`].
  - CPI YoY: Tính dừng sau bậc sai phân `d_yoy` = [kết quả từ `check_stationarity`].

## 3. Phương pháp Dự báo

### 3.1. Mô hình Chuỗi Thời Gian Cổ điển
- **Mô hình sử dụng**:
  - **Exponential Smoothing**: Mô hình Holt-Winters với thành phần xu hướng và mùa vụ cộng tính, chu kỳ mùa vụ 12 tháng.
  - **ARIMA**: Sử dụng tham số `(p, d, q)` = `(1, d_mom/yoy, 1)` dựa trên tính dừng.
  - **SARIMA**: Kết hợp ARIMA với thành phần mùa vụ `(P, D, Q, m)` = `(1, 0, 1, 12)`.
  - **SARIMAX**: Tương tự SARIMA, bổ sung biến ngoại sinh (giá dầu).
  - **Prophet**: Mô hình dự báo của Facebook với mùa vụ hàng năm và biến ngoại sinh (giá dầu).
- **Quy trình**:
  - Huấn luyện mô hình trên tập train, dự báo 12 tháng tiếp theo.
  - Tính khoảng tin cậy 95% cho dự báo (trừ Prophet tự động cung cấp).
  - Đánh giá hiệu suất trên tập test bằng RMSE, MAE, MAPE, sMAPE.
  - Vẽ biểu đồ lịch sử, thực tế, và dự báo (Hình 1-10: `cpi_[mom/yoy]_[model]_forecast.png`).
  - Vẽ ACF phần dư (Hình 11-20: `residual_acf_[model]_[mom/yoy].png`) để kiểm tra tính ngẫu nhiên.

### 3.2. Mô hình Auto ARIMA
- **Mô hình sử dụng**:
  - Auto ARIMA từ `pmdarima` tự động chọn tham số `(p, d, q)` và `(P, D, Q, m)` tối ưu dựa trên tiêu chí AIC.
  - Chu kỳ mùa vụ: 12 tháng (dữ liệu hàng tháng).
- **Quy trình**:
  - Huấn luyện trên tập train, dự báo 12 tháng cuối (test) và 12 tháng tiếp theo.
  - Đánh giá hiệu suất trên tập test bằng MSE, RMSE, MAE.
  - Vẽ biểu đồ chuỗi thời gian (Hình 21-22: `[cpi_mom/yoy]_time_series.png`), so sánh thực tế-dự báo (Hình 23-24: `auto_arima_predictions_[cpi_mom/yoy].png`), và dự báo tương lai (Hình 25-26: `auto_arima_forecast_[cpi_mom/yoy].png`).

## 4. Kết quả Phân tích

### 4.1. Hiệu suất Mô hình (Dựa trên tập test)
- **CPI MoM**:
  - [Mô hình tốt nhất]: RMSE = [giá trị thấp nhất], MAE = [giá trị], MAPE = [giá trị], sMAPE = [giá trị].
  - [Mô hình kém nhất]: RMSE = [giá trị cao nhất].
  - Kết quả chi tiết trong file `classical_model_results.csv` và biểu đồ so sánh (Hình 1, 3, 5, 7, 9).
- **CPI YoY**:
  - [Mô hình tốt nhất]: RMSE = [giá trị thấp nhất], MAE = [giá trị], MAPE = [giá trị], sMAPE = [giá trị].
  - [Mô hình kém nhất]: RMSE = [giá trị cao nhất].
  - Kết quả chi tiết trong file `classical_model_results.csv` và biểu đồ so sánh (Hình 2, 4, 6, 8, 10).
- **Auto ARIMA**:
  - CPI MoM: MSE = [giá trị], RMSE = [giá trị], MAE = [giá trị].
  - CPI YoY: MSE = [giá trị], RMSE = [giá trị], MAE = [giá trị].
  - Tham số tốt nhất:
    - CPI MoM: `(p, d, q)` = [kết quả], `(P, D, Q, m)` = [kết quả].
    - CPI YoY: `(p, d, q)` = [kết quả], `(P, D, Q, m)` = [kết quả].

### 4.2. Dự báo Tương Lai (12 tháng tiếp theo)
- **CPI MoM**:
  - Dự báo được lưu tại `classical_model_forecasts.csv` và `auto_arima_forecast_cpi_mom.csv`.
  - Biểu đồ dự báo (Hình 1, 3, 5, 7, 9, 25) cho thấy [xu hướng tăng/giảm/ổn định].
- **CPI YoY**:
  - Dự báo được lưu tại `classical_model_forecasts.csv` và `auto_arima_forecast_cpi_yoy.csv`.
  - Biểu đồ dự báo (Hình 2, 4, 6, 8, 10, 26) cho thấy [xu hướng tăng/giảm/ổn định].
- **Dự báo kết hợp**: Lưu tại `combined_arima_forecast.csv`.

### 4.3. Phân tích Phần dư
- ACF phần dư (Hình 11-20) cho thấy:
  - [Mô hình cụ thể]: Phần dư gần ngẫu nhiên, không có tự tương quan đáng kể tại các lag.
  - [Mô hình cụ thể]: Phần dư có tự tương quan tại lag [giá trị], gợi ý cần cải thiện mô hình.

### 4.4. Vai trò Biến Ngoại sinh
- Giá dầu (`oil_price`) được sử dụng trong SARIMAX và Prophet, có tác động [mức độ] đến dự báo CPI MoM và YoY (dựa trên kết quả mô hình).

## 5. Kết luận
- **Hiệu suất mô hình**:
  - [Mô hình tốt nhất] cho CPI MoM và YoY dựa trên RMSE thấp nhất.
  - Auto ARIMA cung cấp hiệu suất cạnh tranh nhờ tự động hóa lựa chọn tham số.
- **Dự báo tương lai**:
  - CPI MoM dự kiến [tăng/giảm/ổn định] trong 12 tháng tới.
  - CPI YoY dự kiến [tăng/giảm/ổn định], với [mô hình cụ thể] cho kết quả đáng tin cậy nhất.
- **Hạn chế**:
  - Một số mô hình (ví dụ: Prophet) có thể nhạy cảm với giá trị ngoại sinh thiếu chính xác.
  - Phần dư của [mô hình cụ thể] cho thấy tự tương quan, cần điều chỉnh tham số hoặc bổ sung biến.

## 6. Đề xuất
- Tinh chỉnh tham số mô hình (đặc biệt là SARIMA/SARIMAX) bằng cách thử nghiệm thêm các giá trị `(p, d, q)` và `(P, D, Q, m)`.
- Bổ sung các biến ngoại sinh khác (ví dụ: lãi suất, giá vàng) để cải thiện SARIMAX và Prophet.
- Kiểm tra dự báo định kỳ với dữ liệu mới để đảm bảo tính chính xác.
- Sử dụng các mô hình học máy (LSTM, XGBoost) để so sánh với các mô hình cổ điển.

## 7. Kết quả Đầu ra
- **File kết quả**:
  - `classical_model_results.csv`: Hiệu suất các mô hình.
  - `classical_model_forecasts.csv`: Dự báo từ các mô hình cổ điển.
  - `auto_arima_forecast_[cpi_mom/yoy].csv`: Dự báo từ Auto ARIMA.
  - `combined_arima_forecast.csv`: Dự báo kết hợp từ Auto ARIMA.
- **Biểu đồ**:
  - Lưu trong thư mục `plots/` với các tên file như `cpi_[mom/yoy]_[model]_forecast.png`, `auto_arima_forecast_[cpi_mom/yoy].png`.
- **Log**:
  - Nhật ký chi tiết lưu tại `classical_models_log.txt`.

---

**Lưu ý**: Các giá trị cụ thể (RMSE, MAE, tham số mô hình, xu hướng dự báo) cần được cập nhật từ kết quả thực tế của mã. Báo cáo này cung cấp khung nội dung dựa trên logic và đầu ra của mã.

Dưới đây là báo cáo phân tích dựa trên các đoạn mã Python được cung cấp, tập trung vào việc dự báo Chỉ số Giá tiêu dùng (CPI) MoM và YoY bằng các mô hình hồi quy tuyến tính (Linear, Ridge, Lasso, ElasticNet), Random Forest, XGBoost, LightGBM và Support Vector Regression (SVR). Báo cáo được trình bày ngắn gọn, rõ ràng, và chuyên nghiệp, phù hợp với mục đích phân tích và dự báo.

---

# Báo Cáo Dự Báo Chỉ Số Giá Tiêu Dùng (CPI) Bằng Các Mô Hình Hồi Quy và Học Máy

## 1. Mục tiêu
Dự báo CPI MoM và CPI YoY trong 12 tháng tiếp theo sử dụng các mô hình hồi quy tuyến tính (Linear, Ridge, Lasso, ElasticNet), Random Forest, XGBoost, LightGBM và SVR, đồng thời đánh giá hiệu suất dự báo thông qua các chỉ số RMSE, MAE và MAPE.

## 2. Dữ liệu và Tiền xử lý
- **Nguồn dữ liệu**: 
  - Dữ liệu được đọc từ file `data/analyzed_time_series.csv`, bao gồm CPI MoM, CPI YoY và các biến khác, với cột `time` làm chỉ số.
- **Tiền xử lý**:
  - Chuyển đổi cột `time` sang định dạng `datetime` và đặt làm chỉ số.
  - Chuẩn hóa dữ liệu CPI MoM và YoY bằng `StandardScaler` để đảm bảo tính đồng nhất.
  - Tạo các đặc trưng trễ (lags) từ 1 đến 12 tháng (`lag_1` đến `lag_12`) để sử dụng làm biến đầu vào.
  - Loại bỏ các hàng chứa giá trị thiếu sau khi tạo lags.
  - Chia dữ liệu thành tập huấn luyện (train) và kiểm tra (test), với 12 tháng cuối làm tập test.
- **Đặc trưng**:
  - Các đặc trưng đầu vào bao gồm 12 giá trị CPI MoM hoặc YoY tại các thời điểm trễ (lag features).

## 3. Phương pháp Dự báo

### 3.1. Mô hình Hồi quy Tuyến tính
- **Mô hình sử dụng**:
  - **Linear Regression**: Hồi quy tuyến tính cơ bản.
  - **Ridge Regression**: Hồi quy với chuẩn hóa L2, tối ưu tham số `alpha` bằng GridSearchCV.
  - **Lasso Regression**: Hồi quy với chuẩn hóa L1, tối ưu tham số `alpha` bằng GridSearchCV.
  - **ElasticNet**: Kết hợp L1 và L2, tối ưu `alpha` và `l1_ratio` bằng GridSearchCV.
- **Quy trình**:
  - Huấn luyện mô hình trên tập train với các đặc trưng lag.
  - Dự đoán trên tập test và dự báo 12 tháng tiếp theo bằng cách sử dụng các dự đoán trước đó làm đầu vào.
  - Đánh giá hiệu suất trên tập test bằng RMSE, MAE, MAPE.
  - Vẽ biểu đồ so sánh giá trị thực tế, dự đoán và dự báo (Hình 1-8: `[model_type]_forecast.png`).

### 3.2. Mô hình Random Forest
- **Mô hình sử dụng**:
  - Random Forest Regressor với tối ưu tham số (`n_estimators`, `max_depth`, `min_samples_split`, `min_samples_leaf`) bằng RandomizedSearchCV.
- **Quy trình**:
  - Tương tự hồi quy tuyến tính, sử dụng các đặc trưng lag để huấn luyện và dự báo.
  - Đánh giá hiệu suất và vẽ biểu đồ so sánh (Hình 9-10).

### 3.3. Mô hình XGBoost
- **Mô hình sử dụng**:
  - XGBoost Regressor với tối ưu tham số (`n_estimators`, `max_depth`, `learning_rate`, `subsample`, `colsample_bytree`, `gamma`) bằng RandomizedSearchCV.
- **Quy trình**:
  - Tương tự Random Forest, tập trung vào khả năng xử lý phi tuyến tính của XGBoost.
  - Đánh giá và vẽ biểu đồ (Hình 11-12).

### 3.4. Mô hình LightGBM
- **Mô hình sử dụng**:
  - LightGBM Regressor, với hai phiên bản: không tối ưu tham số và tối ưu tham số (`n_estimators`, `max_depth`, `learning_rate`, `subsample`, `colsample_bytree`) bằng RandomizedSearchCV.
- **Quy trình**:
  - Tương tự các mô hình trước, tập trung vào hiệu suất cao và tốc độ xử lý của LightGBM.
  - Đánh giá và vẽ biểu đồ (Hình 13-16).

### 3.5. Mô hình SVR
- **Mô hình sử dụng**:
  - Support Vector Regression, với hai phiên bản: mặc định và tối ưu tham số (`C`, `epsilon`, `kernel`, `gamma`) bằng RandomizedSearchCV.
- **Quy trình**:
  - Huấn luyện và dự báo tương tự, tập trung vào khả năng xử lý phi tuyến tính của SVR với kernel RBF, linear hoặc poly.
  - Đánh giá và vẽ biểu đồ (Hình 17-20).

## 4. Kết quả Phân tích

### 4.1. Hiệu suất Mô hình (Dựa trên tập test)
- **CPI MoM**:
  - **Mô hình tốt nhất**: [Mô hình cụ thể, ví dụ: XGBoost] với RMSE = [giá trị thấp nhất], MAE = [giá trị], MAPE = [giá trị].
  - **Mô hình kém nhất**: [Mô hình cụ thể, ví dụ: Linear Regression] với RMSE = [giá trị cao nhất].
  - **Tham số tối ưu** (nếu có): [Ví dụ: Ridge `alpha`, XGBoost `n_estimators`, SVR `C`, v.v.].
  - Kết quả chi tiết từ console output và biểu đồ so sánh (Hình 2, 4, 6, 8, 10, 12, 14, 16, 18, 20).
- **CPI YoY**:
  - **Mô hình tốt nhất**: [Mô hình cụ thể] với RMSE = [giá trị thấp nhất], MAE = [giá trị], MAPE = [giá trị].
  - **Mô hình kém nhất**: [Mô hình cụ thể] với RMSE = [giá trị cao nhất].
  - **Tham số tối ưu** (nếu có): [Ví dụ: Lasso `alpha`, Random Forest `max_depth`, LightGBM `learning_rate`, v.v.].
  - Kết quả chi tiết từ console output và biểu đồ so sánh (Hình 1, 3, 5, 7, 9, 11, 13, 15, 17, 19).

### 4.2. Dự báo Tương Lai (12 tháng tiếp theo)
- **CPI MoM**:
  - Dự báo từ các mô hình cho thấy [xu hướng tăng/giảm/ổn định], với [mô hình cụ thể] có hiệu suất tốt nhất.
  - Biểu đồ dự báo (Hình 2, 4, 6, 8, 10, 12, 14, 16, 18, 20) minh họa giá trị thực tế, dự đoán và dự báo.
- **CPI YoY**:
  - Dự báo cho thấy [xu hướng tăng/giảm/ổn định], với [mô hình cụ thể] đáng tin cậy nhất.
  - Biểu đồ dự báo (Hình 1, 3, 5, 7, 9, 11, 13, 15, 17, 19) minh họa rõ ràng.

### 4.3. So sánh Mô hình
- **Hồi quy tuyến tính**: Phù hợp với dữ liệu có quan hệ tuyến tính, nhưng hiệu suất kém nếu dữ liệu phức tạp (phi tuyến).
- **Random Forest và XGBoost**: Hiệu quả trong việc xử lý phi tuyến tính, với XGBoost thường vượt trội nhờ tối ưu gradient boosting.
- **LightGBM**: Tốc độ nhanh, hiệu suất cao, đặc biệt khi tối ưu tham số.
- **SVR**: Hiệu quả với kernel phi tuyến (RBF), nhưng nhạy cảm với tham số và dữ liệu lớn.
- **Tối ưu tham số**: Các mô hình sử dụng RandomizedSearchCV (Ridge, Lasso, ElasticNet, Random Forest, XGBoost, LightGBM, SVR) thường cải thiện hiệu suất so với phiên bản mặc định.

## 5. Kết luận
- **Hiệu suất mô hình**:
  - [Mô hình cụ thể, ví dụ: XGBoost hoặc LightGBM với RandomizedSearchCV] cho hiệu suất tốt nhất trên cả CPI MoM và YoY, với RMSE thấp nhất.
  - Hồi quy tuyến tính đơn giản (Linear Regression) thường kém hiệu quả nhất do hạn chế trong việc xử lý phi tuyến tính.
- **Dự báo tương lai**:
  - CPI MoM dự kiến [tăng/giảm/ổn định] trong 12 tháng tới.
  - CPI YoY dự kiến [tăng/giảm/ổn định], với [mô hình cụ thể] cung cấp dự báo đáng tin cậy nhất.
- **Hạn chế**:
  - Các mô hình chỉ sử dụng đặc trưng lag, chưa khai thác biến ngoại sinh (ví dụ: giá dầu, lãi suất).
  - Dự báo dài hạn (12 tháng) có thể giảm độ chính xác do tích lũy sai số từ các dự đoán trước đó.
  - SVR và ElasticNet nhạy cảm với tham số, cần tối ưu kỹ lưỡng.

## 6. Đề xuất
- Bổ sung các biến ngoại sinh (giá dầu, lãi suất, giá vàng) để cải thiện độ chính xác, đặc biệt với XGBoost và LightGBM.
- Kết hợp các mô hình (ensemble) để tận dụng ưu điểm của từng mô hình.
- Thử nghiệm thêm các mô hình học sâu (LSTM, GRU) để so sánh với các mô hình học máy.
- Cập nhật dữ liệu định kỳ và kiểm tra lại dự báo để điều chỉnh mô hình.
- Tăng số lần lặp (`n_iter`) trong RandomizedSearchCV để tìm tham số tối ưu hơn.

## 7. Kết quả Đầu ra
- **Kết quả hiệu suất**:
  - In trực tiếp trên console với các chỉ số RMSE, MAE, MAPE cho tập test và dự báo.
  - Tham số tối ưu (nếu có) được hiển thị cho các mô hình sử dụng GridSearchCV hoặc RandomizedSearchCV.
- **Biểu đồ**:
  - Lưu tại thư mục hiện tại với tên `[model_type]_forecast.png` (hồi quy tuyến tính) hoặc hiển thị trực tiếp (Random Forest, XGBoost, LightGBM, SVR).
  - Minh họa giá trị thực tế, dự đoán trên tập test, và dự báo 12 tháng tiếp theo.
- **Dữ liệu dự báo**:
  - Không lưu trực tiếp vào file CSV, nhưng có thể trích xuất từ `predictions` và `forecasts` trong mã.

---

**Lưu ý**: Các giá trị cụ thể (RMSE, MAE, MAPE, tham số tối ưu, xu hướng dự báo) cần được cập nhật từ kết quả thực tế của mã. Báo cáo này cung cấp khung nội dung dựa trên logic và đầu ra của mã.

Dưới đây là báo cáo phân tích dựa trên các đoạn mã Python được cung cấp, tập trung vào việc dự báo Chỉ số Giá tiêu dùng (CPI) MoM và YoY bằng các mô hình học sâu (LSTM, GRU, Transformer, MLP, CNN-LSTM). Báo cáo được trình bày ngắn gọn, rõ ràng, và chuyên nghiệp, phù hợp với mục đích phân tích và dự báo.

---

# Báo Cáo Dự Báo Chỉ Số Giá Tiêu Dùng (CPI) Bằng Các Mô Hình Học Sâu

## 1. Mục tiêu
Dự báo CPI MoM và CPI YoY trong 12 tháng tiếp theo sử dụng các mô hình học sâu (LSTM, GRU, Transformer, MLP, CNN-LSTM), đồng thời đánh giá hiệu suất dự báo thông qua các chỉ số RMSE, MSE, MAE và MAPE.

## 2. Dữ liệu và Tiền xử lý
- **Nguồn dữ liệu**: 
  - Dữ liệu được đọc từ file `data/analyzed_time_series.csv`, bao gồm CPI MoM, CPI YoY, với cột `time` làm chỉ số.
- **Tiền xử lý**:
  - Chuyển đổi cột `time` sang định dạng `datetime` và đặt làm chỉ số.
  - Chuẩn hóa dữ liệu CPI MoM và YoY bằng `StandardScaler` để đảm bảo tính đồng nhất.
  - Tạo các đặc trưng trễ (lags) từ 1 đến 12 tháng (`lag_1` đến `lag_12`) làm biến đầu vào.
  - Loại bỏ các hàng chứa giá trị thiếu sau khi tạo lags.
  - Dữ liệu được định dạng thành các chuỗi thời gian với số bước thời gian (`timesteps=3`) để phù hợp với kiến trúc học sâu.
  - Chia dữ liệu thành tập huấn luyện (train) và kiểm tra (test), với 12 tháng cuối làm tập test.
- **Đặc trưng**:
  - Đầu vào là các chuỗi 3 bước thời gian, mỗi bước chứa 12 giá trị lag của CPI MoM hoặc YoY.
  - Dữ liệu được định dạng thành ma trận 3D (samples, timesteps, features) cho LSTM, GRU, Transformer, MLP, và 4D (samples, timesteps, features, channels) cho CNN-LSTM.

## 3. Phương pháp Dự báo

### 3.1. Mô hình LSTM
- **Kiến trúc**:
  - Một lớp LSTM (64 units, activation=`swish`) và một lớp Dense để dự đoán giá trị tiếp theo.
  - Tối ưu bằng Adam (learning rate=0.01), hàm mất mát MSE.
- **Quy trình**:
  - Huấn luyện trên tập train với 50 epochs, batch size=16.
  - Dự đoán trên tập test và dự báo 12 tháng tiếp theo bằng cách sử dụng các dự đoán trước đó làm đầu vào.
  - Đánh giá hiệu suất và vẽ biểu đồ (Hình 1-2).

### 3.2. Mô hình GRU
- **Kiến trúc**:
  - Một lớp GRU (64 units, activation=`relu`) và một lớp Dense.
  - Tối ưu bằng Adam (learning rate=0.01), hàm mất mát MSE.
- **Quy trình**:
  - Tương tự LSTM, tập trung vào khả năng xử lý chuỗi nhanh hơn của GRU.
  - Đánh giá và vẽ biểu đồ (Hình 3-4).

### 3.3. Mô hình Transformer
- **Kiến trúc**:
  - Một lớp MultiHeadAttention (4 heads, key_dim=64), LayerNormalization, và hai lớp Dense (64 units với `relu`, 1 unit đầu ra).
  - Tối ưu bằng Adam (learning rate=0.001), hàm mất mát MSE.
- **Quy trình**:
  - Huấn luyện và dự báo tương tự, tận dụng cơ chế attention để nắm bắt phụ thuộc dài hạn.
  - Đánh giá và vẽ biểu đồ (Hình 5-6).

### 3.4. Mô hình MLP
- **Kiến trúc**:
  - Một lớp Flatten để làm phẳng chuỗi, một lớp Dense (64 units, activation=`relu`), và một lớp Dense đầu ra.
  - Tối ưu bằng Adam (learning rate=0.001), hàm mất mát MSE.
- **Quy trình**:
  - Huấn luyện và dự báo tương tự, đơn giản hơn các mô hình chuỗi.
  - Đánh giá và vẽ biểu đồ (Hình 7-8).

### 3.5. Mô hình CNN-LSTM
- **Kiến trúc**:
  - Một lớp TimeDistributed Conv1D (64 filters, kernel_size=1, activation=`relu`), TimeDistributed Flatten, một lớp LSTM (50 units, activation=`relu`), và một lớp Dense.
  - Tối ưu bằng Adam (learning rate=0.001), hàm mất mát MSE.
- **Quy trình**:
  - Kết hợp CNN để trích xuất đặc trưng cục bộ và LSTM để xử lý chuỗi.
  - Đánh giá và vẽ biểu đồ (Hình 9-10).

## 4. Kết quả Phân tích

### 4.1. Hiệu suất Mô hình (Dựa trên tập test)
- **CPI MoM**:
  - **Mô hình tốt nhất**: [Ví dụ: LSTM hoặc CNN-LSTM] với RMSE = [giá trị thấp nhất], MAE = [giá trị], MAPE = [giá trị].
  - **Mô hình kém nhất**: [Ví dụ: MLP] với RMSE = [giá trị cao nhất].
  - Kết quả chi tiết từ console output và biểu đồ so sánh (Hình 2, 4, 6, 8, 10).
- **CPI YoY**:
  - **Mô hình tốt nhất**: [Ví dụ: Transformer] với RMSE = [giá trị thấp nhất], MAE = [giá trị], MAPE = [giá trị].
  - **Mô hình kém nhất**: [Ví dụ: MLP] với RMSE = [giá trị cao nhất].
  - Kết quả chi tiết từ console output và biểu đồ so sánh (Hình 1, 3, 5, 7, 9).

### 4.2. Dự báo Tương Lai (12 tháng tiếp theo)
- **CPI MoM**:
  - Dự báo cho thấy [xu hướng tăng/giảm/ổn định], với [mô hình cụ thể] có hiệu suất tốt nhất.
  - Biểu đồ dự báo (Hình 2, 4, 6, 8, 10) minh họa giá trị thực tế, dự đoán và dự báo.
- **CPI YoY**:
  - Dự báo cho thấy [xu hướng tăng/giảm/ổn định], với [mô hình cụ thể] đáng tin cậy nhất.
  - Biểu đồ dự báo (Hình 1, 3, 5, 7, 9) minh họa rõ ràng.

### 4.3. So sánh Mô hình
- **LSTM và GRU**: Hiệu quả trong việc nắm bắt phụ thuộc chuỗi dài, với GRU nhanh hơn nhưng có thể kém hơn về độ chính xác trong một số trường hợp.
- **Transformer**: Tốt trong việc xử lý phụ thuộc dài hạn nhờ cơ chế attention, nhưng yêu cầu cấu hình phức tạp hơn.
- **MLP**: Đơn giản, nhưng kém hiệu quả trong việc xử lý dữ liệu chuỗi thời gian so với các mô hình chuỗi.
- **CNN-LSTM**: Kết hợp trích xuất đặc trưng cục bộ (CNN) và phụ thuộc chuỗi (LSTM), thường đạt hiệu suất cao nhưng phức tạp hơn.

## 5. Kết luận
- **Hiệu suất mô hình**:
  - [Mô hình cụ thể, ví dụ: CNN-LSTM hoặc Transformer] cho hiệu suất tốt nhất trên cả CPI MoM và YoY, với RMSE thấp nhất.
  - MLP thường kém hiệu quả nhất do không tận dụng được cấu trúc chuỗi thời gian.
- **Dự báo tương lai**:
  - CPI MoM dự kiến [tăng/giảm/ổn định] trong 12 tháng tới.
  - CPI YoY dự kiến [tăng/giảm/ổn định], với [mô hình cụ thể] cung cấp dự báo đáng tin cậy nhất.
- **Hạn chế**:
  - Các mô hình chỉ sử dụng đặc trưng lag, chưa khai thác biến ngoại sinh (giá dầu, lãi suất).
  - Dự báo dài hạn (12 tháng) có thể tích lũy sai số do sử dụng các dự đoán trước đó.
  - Transformer và CNN-LSTM yêu cầu tài nguyên tính toán cao và cấu hình tham số cẩn thận.

## 6. Đề xuất
- Bổ sung các biến ngoại sinh (giá dầu, lãi suất, giá vàng) để cải thiện độ chính xác.
- Tinh chỉnh kiến trúc mô hình (số units, số lớp, learning rate) và tăng số epochs để tối ưu hiệu suất.
- Thử nghiệm các biến thể Transformer (như Temporal Fusion Transformer) để cải thiện xử lý chuỗi thời gian.
- Kết hợp các mô hình học sâu với các mô hình cổ điển (ARIMA, SARIMA) trong một hệ thống ensemble.
- Cập nhật dữ liệu định kỳ và kiểm tra lại dự báo để điều chỉnh mô hình.

## 7. Kết quả Đầu ra
- **Kết quả hiệu suất**:
  - In trực tiếp trên console với các chỉ số RMSE, MSE, MAE, MAPE cho tập test và dự báo.
- **Biểu đồ**:
  - Hiển thị trực tiếp, minh họa giá trị thực tế, dự đoán trên tập test, và dự báo 12 tháng tiếp theo (Hình 1-10).
- **Dữ liệu dự báo**:
  - Không lưu trực tiếp vào file CSV, nhưng có thể trích xuất từ `predictions` và `forecasts` trong mã.

---

**Lưu ý**: Các giá trị cụ thể (RMSE, MAE, MAPE, xu hướng dự báo) cần được cập nhật từ kết quả thực tế của mã. Báo cáo này cung cấp khung nội dung dựa trên logic và đầu ra của mã.

Dưới đây là báo cáo phân tích dựa trên các đoạn mã Python được cung cấp, tập trung vào việc dự báo Chỉ số Giá tiêu dùng (CPI) MoM và YoY bằng các mô hình kết hợp (hybrid models) sử dụng SARIMA/SARIMAX kết hợp với các mô hình học máy (Random Forest, Ridge, XGBoost, LightGBM, SVR, MLP, CatBoost) và một mô hình ensemble. Báo cáo được trình bày ngắn gọn, rõ ràng, và chuyên nghiệp, phù hợp với mục đích phân tích và dự báo.

---

# Báo Cáo Dự Báo Chỉ Số Giá Tiêu Dùng (CPI) Bằng Các Mô Hình Kết Hợp

## 1. Mục tiêu
Dự báo CPI MoM và CPI YoY trong 12 tháng tiếp theo sử dụng các mô hình kết hợp giữa SARIMA/SARIMAX và các mô hình học máy (Random Forest, Ridge, XGBoost, LightGBM, SVR, MLP, CatBoost), đồng thời áp dụng một mô hình ensemble để cải thiện hiệu suất. Đánh giá hiệu suất dự báo thông qua các chỉ số RMSE, MAE và MAPE.

## 2. Dữ liệu và Tiền xử lý
- **Nguồn dữ liệu**: 
  - Dữ liệu được đọc từ file `data/analyzed_time_series.csv`, bao gồm CPI MoM, CPI YoY, và biến ngoại sinh (`oil_price` nếu có), với cột `time` làm chỉ số.
- **Tiền xử lý**:
  - Chuyển đổi cột `time` sang định dạng `datetime` và đặt làm chỉ số.
  - Chia dữ liệu thành tập huấn luyện (train) và kiểm tra (test), với 12 tháng cuối làm tập test.
  - Tạo các đặc trưng trễ (lags) từ 1 đến 12 tháng (`lag_1` đến `lag_12`) cho phần dư (residuals) của SARIMA/SARIMAX.
  - Chuẩn hóa dữ liệu phần dư bằng `StandardScaler` cho các mô hình yêu cầu (SVR, LSTM).
- **Đặc trưng**:
  - CPI MoM: Sử dụng SARIMA và các đặc trưng lag của phần dư.
  - CPI YoY: Sử dụng SARIMAX với biến ngoại sinh `oil_price` (nếu có) và các đặc trưng lag của phần dư.
  - Phần dư được dự đoán bằng các mô hình học máy để cải thiện dự báo SARIMA/SARIMAX.

## 3. Phương pháp Dự báo

### 3.1. Mô hình Kết hợp SARIMA/SARIMAX với Học Máy
- **Cấu trúc chung**:
  - **SARIMA/SARIMAX**: Dự đoán thành phần tuyến tính và mùa vụ của chuỗi thời gian.
    - CPI MoM: SARIMA với mùa vụ (`m=12`), tự động chọn tham số bằng `auto_arima`.
    - CPI YoY: SARIMAX với biến ngoại sinh `oil_price` (nếu có) và mùa vụ (`m=12`).
  - **Học máy**: Dự đoán phần dư của SARIMA/SARIMAX để cải thiện độ chính xác.
    - Tạo đặc trưng lag (12 tháng) từ phần dư.
    - Huấn luyện các mô hình học máy trên phần dư, sau đó kết hợp với dự báo SARIMA/SARIMAX.
- **Các mô hình học máy**:
  - **Random Forest + Ridge**:
    - CPI MoM: Random Forest (`n_estimators=100`).
    - CPI YoY: Ridge (`alpha=1.0`).
  - **XGBoost**:
    - Cả CPI MoM và YoY: XGBoost (`n_estimators=100`, `objective='reg:squarederror'`).
  - **LightGBM**:
    - Cả CPI MoM và YoY: LightGBM (`n_estimators=100`).
  - **SVR**:
    - Cả CPI MoM và YoY: SVR với `kernel='rbf'`, tối ưu tham số (`C`, `epsilon`, `gamma`) bằng GridSearchCV.
  - **MLP**:
    - Cả CPI MoM và YoY: MLPRegressor (`hidden_layer_sizes=(50, 50)`, `max_iter=1000`).
  - **CatBoost**:
    - Cả CPI MoM và YoY: CatBoost (`n_estimators=100`, `verbose=0`).
- **Quy trình**:
  - Huấn luyện SARIMA/SARIMAX trên tập train để dự đoán thành phần chính.
  - Tính phần dư (residuals) giữa giá trị thực tế và dự đoán SARIMA/SARIMAX.
  - Huấn luyện mô hình học máy trên các đặc trưng lag của phần dư.
  - Dự đoán trên tập test (sử dụng phần dư thực tế) và dự báo 12 tháng tiếp theo (sử dụng phần dư dự đoán).
  - Đánh giá hiệu suất và vẽ biểu đồ (Hình 1-16: `[cpi_mom/yoy]_test_period_plot_[model].png`).
  - Lưu kết quả vào file CSV (`cpi_forecast_results_[model].csv`).

### 3.2. Mô hình Ensemble
- **Cấu trúc**:
  - Kết hợp dự đoán và dự báo từ các mô hình SVR_Tuned, LightGBM, và CatBoost bằng cách lấy trung bình (average ensemble).
- **Quy trình**:
  - Tính trung bình các dự đoán và dự báo từ các mô hình riêng lẻ.
  - Đánh giá hiệu suất ensemble và vẽ biểu đồ (Hình 17-18: `[cpi_mom/yoy]_test_period_plot_ensemble.png`).
  - Lưu kết quả vào file CSV (`cpi_forecast_results_ensemble.csv`).

## 4. Kết quả Phân tích

### 4.1. Hiệu suất Mô hình (Dựa trên tập test)
- **CPI MoM**:
  - **Mô hình tốt nhất**: [Ví dụ: Ensemble hoặc CatBoost] với RMSE = [giá trị thấp nhất], MAE = [giá trị], MAPE = [giá trị].
  - **Mô hình kém nhất**: [Ví dụ: MLP] với RMSE = [giá trị cao nhất].
  - **Tham số tối ưu (SVR)**: [Ví dụ: `C`, `epsilon`, `gamma` từ `GridSearchCV`].
  - Kết quả chi tiết từ console output, file `model_comparison_results_improved.csv`, và biểu đồ so sánh (Hình 2, 4, 6, 8, 10, 12, 17).
- **CPI YoY**:
  - **Mô hình tốt nhất**: [Ví dụ: Ensemble hoặc LightGBM] với RMSE = [giá trị thấp nhất], MAE = [giá trị], MAPE = [giá trị].
  - **Mô hình kém nhất**: [Ví dụ: SVR] với RMSE = [giá trị cao nhất].
  - **Tham số tối ưu (SVR)**: [Ví dụ: `C`, `epsilon`, `gamma` từ `GridSearchCV`].
  - Kết quả chi tiết từ console output, file `model_comparison_results_improved.csv`, và biểu đồ so sánh (Hình 1, 3, 5, 7, 9, 11, 18).

### 4.2. Dự báo Tương Lai (12 tháng tiếp theo)
- **CPI MoM**:
  - Dự báo cho thấy [xu hướng tăng/giảm/ổn định], với [mô hình cụ thể, ví dụ: Ensemble] có hiệu suất tốt nhất.
  - Biểu đồ dự báo (Hình 2, 4, 6, 8, 10, 12, 17) minh họa giá trị thực tế, dự đoán, và dự báo.
- **CPI YoY**:
  - Dự báo cho thấy [xu hướng tăng/giảm/ổn định], với [mô hình cụ thể, ví dụ: CatBoost] đáng tin cậy nhất.
  - Biểu đồ dự báo (Hình 1, 3, 5, 7, 9, 11, 18) minh họa rõ ràng.

### 4.3. So sánh Mô hình
- **Random Forest + Ridge**: Hiệu quả tốt cho dữ liệu có tính tuyến tính, nhưng kém hơn trong các trường hợp phức tạp.
- **XGBoost và CatBoost**: Xuất sắc trong việc xử lý phi tuyến tính, với CatBoost thường vượt trội nhờ khả năng xử lý dữ liệu nhỏ hiệu quả.
- **LightGBM**: Nhanh và chính xác, đặc biệt khi không cần chuẩn hóa dữ liệu.
- **SVR_Tuned**: Hiệu quả khi tối ưu tham số, nhưng nhạy cảm với dữ liệu và yêu cầu chuẩn hóa.
- **MLP**: Kém hiệu quả hơn các mô hình gradient boosting, có thể do cấu hình đơn giản.
- **Ensemble**: Kết hợp ưu điểm của SVR_Tuned, LightGBM, và CatBoost, thường đạt hiệu suất cao nhất nhờ giảm sai số tổng quát.
- **Tham số tối ưu**: SVR_Tuned cải thiện hiệu suất nhờ GridSearchCV, trong khi các mô hình khác sử dụng cấu hình mặc định.

## 5. Kết luận
- **Hiệu suất mô hình**:
  - [Mô hình cụ thể, ví dụ: Ensemble hoặc CatBoost] cho hiệu suất tốt nhất trên cả CPI MoM và YoY, với RMSE thấp nhất.
  - MLP thường kém hiệu quả nhất do hạn chế trong xử lý dữ liệu chuỗi thời gian với cấu hình hiện tại.
- **Dự báo tương lai**:
  - CPI MoM dự kiến [tăng/giảm/ổn định] trong 12 tháng tới.
  - CPI YoY dự kiến [tăng/giảm/ổn định], với [mô hình cụ thể, ví dụ: Ensemble] cung cấp dự báo đáng tin cậy nhất.
- **Hạn chế**:
  - Chỉ sử dụng `oil_price` làm biến ngoại sinh cho CPI YoY, chưa khai thác các biến khác (lãi suất, giá vàng).
  - Dự báo dài hạn (12 tháng) có thể tích lũy sai số do sử dụng phần dư dự đoán.
  - Một số mô hình (MLP, SVR) yêu cầu tối ưu tham số kỹ lưỡng hơn để cạnh tranh với gradient boosting.

## 6. Đề xuất
- Bổ sung thêm biến ngoại sinh (lãi suất, giá vàng, tỷ giá hối đoái) để cải thiện dự báo SARIMAX.
- Tối ưu tham số cho tất cả các mô hình học máy (Random Forest, XGBoost, LightGBM, MLP) bằng GridSearchCV hoặc RandomizedSearchCV.
- Thử nghiệm các mô hình ensemble phức tạp hơn (weighted average, stacking) để tận dụng ưu điểm của từng mô hình.
- Kết hợp mô hình học sâu (LSTM, GRU) với SARIMA/SARIMAX để so sánh với các mô hình học máy.
- Cập nhật dữ liệu định kỳ và kiểm tra lại dự báo để điều chỉnh mô hình.

## 7. Kết quả Đầu ra
- **Kết quả hiệu suất**:
  - In trực tiếp trên console với các chỉ số RMSE, MAE, MAPE cho tập test và dự báo.
  - So sánh hiệu suất các mô hình lưu trong file `model_comparison_results_improved.csv`.
  - Tham số tối ưu cho SVR_Tuned được hiển thị (nếu áp dụng).
- **Biểu đồ**:
  - Lưu tại thư mục hiện tại với tên `[cpi_mom/yoy]_test_period_plot_[model].png` (bao gồm ensemble).
  - Minh họa giá trị thực tế, dự đoán trên tập test, và dự báo 12 tháng tiếp theo.
- **Dữ liệu dự báo**:
  - Lưu vào file CSV (`cpi_forecast_results_[model].csv` và `cpi_forecast_results_ensemble.csv`) với các cột: `time`, `cpi_mom_actual`, `cpi_mom_predicted`, `cpi_mom_forecasted`, `cpi_yoy_actual`, `cpi_yoy_predicted`, `cpi_yoy_forecasted`.

---

**Lưu ý**: Các giá trị cụ thể (RMSE, MAE, MAPE, xu hướng dự báo, tham số tối ưu) cần được cập nhật từ kết quả thực tế của mã. Báo cáo này cung cấp khung nội dung dựa trên logic và đầu ra của mã.