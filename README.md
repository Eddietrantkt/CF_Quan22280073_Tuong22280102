# CF_Quan22280073_Tuong22280102

## 📊 Financial Analysis & Trading Project - Phân Tích Tài Chính

A comprehensive project for analyzing Vietnamese banking stocks and implementing pair trading strategies using Python and financial data APIs.

Dự án phân tích cổ phiếu ngân hàng Việt Nam và triển khai các chiến lược giao dịch cặp sử dụng Python và các API dữ liệu tài chính.

---

## 🎯 Project Overview - Tổng Quan Dự Án

This project contains Jupyter notebooks for:
- Financial data collection from Vietnamese stock markets
- Technical and fundamental analysis of Vietnamese banking stocks
- Pair trading strategy implementation
- Financial modeling and forecasting

Dự án này bao gồm các notebook Jupyter cho:
- Thu thập dữ liệu tài chính từ thị trường chứng khoán Việt Nam
- Phân tích kỹ thuật và cơ bản của cổ phiếu ngân hàng Việt Nam
- Triển khai chiến lược giao dịch cặp
- Mô hình hóa và dự báo tài chính

---

## 📁 Repository Structure - Cấu Trúc Dự Án

```
CF_Quan22280073_Tuong22280102/
│
├── README.md                    # Project documentation
├── FINANCE.ipynb               # Main financial analysis notebook
├── FINANCE_week4.ipynb         # Week 4 financial exercises
├── FINANCE_week5.ipynb         # Week 5 financial exercises
├── FINANCE_week06.ipynb        # Week 6 financial exercises
└── vn_data_collector.ipynb     # Vietnamese stock data collector
```

---

## 📚 Notebooks Description - Mô Tả Các Notebook

### 1. **FINANCE.ipynb**
Main financial analysis notebook focusing on Vietnamese banking sector.
- Downloads price and volume data for top Vietnamese banks (VCB, TCB, ACB, MBB, BID, CTG, HDB, TPB, MSB, SHB)
- Performs fundamental analysis (P/E ratio, P/B ratio, dividend yield, etc.)
- Technical analysis and visualization
- Financial forecasting using Prophet

### 2. **vn_data_collector.ipynb**
Data collection tool for Vietnamese stocks using `vnstock` library (version 3.x).
- Collects historical stock data
- Prepares data for pair trading strategies
- Data preprocessing and cleaning

### 3. **FINANCE_week4.ipynb, FINANCE_week5.ipynb, FINANCE_week06.ipynb**
Weekly financial exercises and assignments covering various topics in financial analysis and modeling.

---

## 🛠️ Prerequisites - Yêu Cầu

- Python 3.7 or higher
- Jupyter Notebook or Google Colab
- Basic understanding of financial markets and trading

---

## 📦 Installation - Cài Đặt

### Required Python Libraries:

```bash
pip install yfinance pandas pandas-ta matplotlib mplfinance prophet vnstock
```

### Library Details:
- **yfinance**: Download financial data from Yahoo Finance
- **pandas**: Data manipulation and analysis
- **pandas-ta**: Technical analysis indicators
- **matplotlib & mplfinance**: Data visualization
- **prophet**: Time series forecasting
- **vnstock**: Vietnamese stock market data (version 3.x)

---

## 🚀 Usage - Cách Sử Dụng

### Option 1: Google Colab (Recommended)
Click the "Open in Colab" badge in each notebook to run directly in your browser.

### Option 2: Local Jupyter Notebook

1. Clone the repository:
```bash
git clone https://github.com/Eddietrantkt/CF_Quan22280073_Tuong22280102.git
cd CF_Quan22280073_Tuong22280102
```

2. Install dependencies:
```bash
pip install -r requirements.txt  # If available
# OR install libraries individually as shown above
```

3. Launch Jupyter Notebook:
```bash
jupyter notebook
```

4. Open any `.ipynb` file and run the cells sequentially.

---

## 📊 Analyzed Stocks - Cổ Phiếu Phân Tích

The project primarily focuses on top Vietnamese banking stocks:

| Ticker | Bank Name | Vietnamese Name |
|--------|-----------|-----------------|
| VCB.VN | Vietcombank | Ngân hàng TMCP Ngoại Thương Việt Nam |
| TCB.VN | Techcombank | Ngân hàng TMCP Kỹ Thương Việt Nam |
| ACB.VN | ACB | Ngân hàng TMCP Á Châu |
| MBB.VN | MBBank | Ngân hàng TMCP Quân Đội |
| BID.VN | BIDV | Ngân hàng TMCP Đầu tư và Phát triển Việt Nam |
| CTG.VN | VietinBank | Ngân hàng TMCP Công Thương Việt Nam |
| HDB.VN | HDBank | Ngân hàng TMCP Phát triển TP.HCM |
| TPB.VN | TPBank | Ngân hàng TMCP Tiên Phong |
| MSB.VN | MSB | Ngân hàng TMCP Hàng Hải Việt Nam |
| SHB.VN | SHB | Ngân hàng TMCP Sài Gòn - Hà Nội |

---

## 👥 Authors - Tác Giả

- **Quan** - Student ID: 22280073
- **Tuong** - Student ID: 22280102

---

## 📝 License - Giấy Phép

This project is available for educational purposes.

---

## 🤝 Contributing - Đóng Góp

Feel free to fork this repository and submit pull requests for improvements.

---

## 📧 Contact - Liên Hệ

For questions or suggestions, please open an issue on GitHub.

---

## ⚠️ Disclaimer - Tuyên Bố Miễn Trừ Trách Nhiệm

This project is for educational and research purposes only. The analysis and strategies presented should not be considered as financial advice. Always conduct your own research and consult with financial professionals before making investment decisions.

Dự án này chỉ phục vụ cho mục đích giáo dục và nghiên cứu. Các phân tích và chiến lược được trình bày không nên được coi là lời khuyên tài chính. Luôn tiến hành nghiên cứu riêng của bạn và tham khảo ý kiến chuyên gia tài chính trước khi đưa ra quyết định đầu tư.