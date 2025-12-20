# CF_Quan22280073_Tuong22280102
# Giải thích: FINANCE_week06.ipynb

1. Mục tiêu Notebook
Notebook này thực hiện nhiệm vụ thiết kế và kiểm thử chiến lược giao dịch thuật toán dựa trên nguyên lý Mean Reversion (Hồi quy về trung bình), cụ thể là Pairs Trading (Giao dịch theo cặp):

Tìm kiếm cặp cổ phiếu: Xác định các cặp cổ phiếu có mối liên hệ mật thiết về giá trong dài hạn (Cointegration).

Thiết kế thuật toán: Xây dựng tín hiệu mua/bán dựa trên độ lệch (Spread) giữa hai cổ phiếu.

Backtest: Kiểm thử hiệu quả quá khứ của chiến lược.
 Giới thiệu Bộ Dữ liệu (Dataset Overview)
Dữ liệu đầu vào đóng vai trò quyết định đến hiệu suất của thuật toán Pairs Trading. Notebook này sử dụng dữ liệu lịch sử giá cổ phiếu của thị trường Việt Nam.

Nguồn dữ liệu: Các file .csv được lưu trữ cục bộ.

Tần suất (Frequency): Dữ liệu nội ngày (Intraday) khung thời gian 15 phút.

Lý do: Pairs Trading là chiến thuật khai thác sự chênh lệch giá ngắn hạn. Dữ liệu ngày (Daily) thường quá chậm để bắt được các nhịp hồi quy (mean reversion) diễn ra trong phiên.

Phạm vi (Universe): Tập trung vào nhóm cổ phiếu Ngành Ngân hàng (ví dụ: ACB, BID, CTG, MBB...).

Lý do: Các cổ phiếu cùng ngành thường chịu tác động bởi các yếu tố vĩ mô giống nhau (lãi suất, chính sách nhà nước...), do đó chúng có xu hướng di chuyển cùng chiều (tương quan cao) và dễ hình thành mối quan hệ đồng tích hợp (Cointegration) - điều kiện tiên quyết cho Pairs Trading.

!(close)

Cấu trúc dữ liệu (Features): Mỗi file CSV bao gồm các trường thông tin tiêu chuẩn (OHLCV):

time: Thời gian giao dịch (định dạng datetime).

open: Giá mở cửa của nến 15 phút.

high: Giá cao nhất trong 15 phút.

low: Giá thấp nhất trong 15 phút.

close: Giá đóng cửa của nến 15 phút (Dùng để tính toán tín hiệu chính).

volume: Khối lượng giao dịch.
## 2. Giải thích Chi tiết Các Hàm & Các Bước

### Bước 1: Đọc và Xử lý Dữ liệu (`load_intraday_stock`)

```python
def load_intraday_stock(csv_path: Path):
    # ... code đọc csv ...
    df["time"] = pd.to_datetime(df["time"])
    df = df.set_index("time").sort_index()
    # ...
```

**Tại sao cần hàm này?**
-   **Chuẩn hóa thời gian:** Dữ liệu gốc thường ở dạng chuỗi (string). Để tính toán tài chính (resample, time-series plotting), bắt buộc phải chuyển về dạng `datetime`.
-   **Time-Index:** Đặt cột `time` làm index giúp Pandas hiểu đây là dữ liệu chuỗi thời gian, cho phép dùng các hàm mạnh mẽ như `.resample()` (gom nhóm theo giờ/ngày/tháng) hay `.shift()` (lấy dữ liệu quá khứ/tương lai).
-   **Lọc nhiễu (NaN):** Dữ liệu tài chính thường có lỗi hoặc khoảng trống. Việc loại bỏ các dòng `NaN` (Not a Number) đảm bảo các phép tính thống kê không bị sai lệch.

### Bước 2: Tính Log-Returns (Lợi suất Logarit)

```python
log_returns = np.log(close_prices / close_prices.shift(1))
```

**Tại sao dùng Log-Returns thay vì Simple Returns (%)?**
1.  **Tính cộng (Additivity):** Lợi suất logarit của nhiều kỳ có thể cộng trực tiếp để ra lợi suất tổng. Ví dụ: Log-return năm = Tổng log-return 12 tháng. Simple returns không làm được điều này (phải nhân).
2.  **Phân phối chuẩn:** Log-returns thường có phân phối gần với phân phối chuẩn (Normal Distribution) hơn, giúp các mô hình thống kê hoạt động tốt hơn.
3.  **Xử lý số nhỏ:** Với các biến động giá nhỏ (intraday), log-return và simple-return gần như tương đương, nhưng log-return toán học "đẹp" hơn cho mô hình hóa.

### Bước 3: Phân tích Biến động (Volatility)

```python
# Rolling Standard Deviation * Căn bậc 2 của số phiên
vol = log_returns.rolling(WINDOW).std() * np.sqrt(TRADING_DAYS)
```

**Tại sao làm bước này?**
-   **Đo lường rủi ro:** Trong tài chính, rủi ro thường được định nghĩa là "độ biến động" (Volatility). Giá dao động càng mạnh, độ lệch chuẩn càng cao -> rủi ro càng lớn.
-   **Rolling Window (Cửa sổ trượt):** Thị trường thay đổi liên tục. Volatility của năm 2023 khác năm 2024. Dùng cửa sổ trượt (ví dụ: 30 ngày gần nhất) để quan sát rủi ro **tại thời điểm hiện tại**, thay vì một con số cố định cho toàn lịch sử.
-   **Annualized (Theo năm):** Nhân vởi `sqrt(252)` (252 là số ngày giao dịch trung bình/năm) để quy đổi độ biến động về **năm**. Điều này giúp so sánh rủi ro giữa các tài sản khác nhau hoặc các khung thời gian khác nhau một cách chuẩn mực.
-   Ngoài các lý do về tính cộng và phân phối chuẩn, trong Pairs Trading, log-returns giúp chuẩn hóa quy mô giá. Một cổ phiếu giá 100.000 VND và một cổ phiếu giá 20.000 VND khó so sánh trực tiếp biến động tuyệt đối. Log-returns đưa chúng về cùng một thước đo phần trăm biến động để dễ dàng tìm kiếm tương quan.

### Bước 4: Chiến lược Momentum (`momentum_long_short_backtest`)

Đây là phần cốt lõi của bài tập.

#### Lý do (Logic chiến lược):
-   **Hiệu ứng Momentum (Đà):** Các nghiên cứu chỉ ra rằng các cổ phiếu đã tăng giá mạnh trong quá khứ gần (Winners) có xu hướng tiếp tục tăng trong ngắn hạn, và ngược lại (Losers tiếp tục giảm).
-   **Chiến lược Long-Short:**
    -   **Long (Mua):** Mua nhóm cổ phiếu tốt nhất (Top N Winners).
    -   **Short (Bán khống):** Bán nhóm cổ phiếu tệ nhất (Bottom N Losers) để kiếm lời từ việc giá giảm hoặc để phòng vệ (hedge) rủi ro thị trường chung.

#### Giải thích các bước trong hàm:

1.  **Resample (`build_periodic_close_and_return`):**
    -   Chuyển dữ liệu từ tần suất cao (15 phút/ngày) sang tần suất thấp hơn (Tháng/Quý) để giảm nhiễu và tập trung vào xu hướng lớn.

2.  **Ranking (Xếp hạng):**
    -   Tại mỗi cuối tháng $t$, so sánh lợi suất của tất cả cổ phiếu.
    -   Chọn ra Top $N$ cổ phiếu tăng mạnh nhất và Bottom $N$ cổ phiếu giảm mạnh nhất.

3.  **Tín hiệu (Signal Generation):**
    -   `signals_long = True`: Đánh dấu sẽ giữ cổ phiếu này trong tháng tới.
    -   Lưu ý: Tín hiệu được tạo ra từ dữ liệu **quá khứ** (tháng $t$), để áp dụng cho tháng **tương lai** ($t+1$).

4.  **Tính Lợi nhuận (`future_ret`):**
    -   `future_ret = periodic_ret.shift(-1)`: Lấy lợi suất của tháng sau ($t+1$) đẩy lùi về hàng của tháng hiện tại ($t$).
    -   Mục đích: Để nhân Tín hiệu (có tại cuối tháng $t$) với Lợi nhuận thực tế đạt được (trong suốt tháng $t+1$). Đây là cách backtest chính xác để tránh "nhìn trước tương lai" (look-ahead bias).

5.  **Equity Curve (Đường vốn):**
    -   `(1 + strategy_ret).cumprod()`: Tính giá trị tích lũy của tài khoản theo thời gian. Nếu bắt đầu với 1 đồng, sau n kỳ sẽ có bao nhiêu đồng.

## 3. Kết luận
Notebook này không chỉ tính toán con số, mà mô phỏng lại quy trình thực tế của một Quant Trader:
1.  Làm sạch dữ liệu.
2.  Hiểu đặc tính dữ liệu (Volatility).
3.  Xây dựng giả thuyết đầu tư (Momentum).
4.  Kiểm chứng giả thuyết bằng dữ liệu quá khứ (Backtest).
