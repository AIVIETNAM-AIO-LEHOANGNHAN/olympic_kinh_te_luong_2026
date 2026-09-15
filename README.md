

# Đề tài : **Xây dựng chỉ số tâm lý tổng hợp nhà đầu tư và khả năng cải thiện giải thích lợi suất: Thực nghiệm tại thị trường chứng khoán Việt Nam**

## 1. Giới thiệu

Đề tài xây dựng một chỉ số tâm lý nhà đầu tư tổng hợp (**SI — Sentiment Index**) cho thị trường chứng khoán Việt Nam bằng cách kết hợp có căn cứ khoa học giữa:

- **MSI (Market Sentiment Index)** — chỉ số tâm lý suy luận từ dữ liệu hành vi giao dịch thị trường (thanh khoản, khối ngoại, số phiên trần/sàn...), xây dựng bằng PCA.
- **NSI (News Sentiment Index)** — chỉ số tâm lý suy luận từ dữ liệu văn bản tin tức tài chính tiếng Việt, xây dựng bằng mô hình NLP (PhoBERT).

Đề tài kiểm định liệu chỉ số tổng hợp SI có mang lại giá trị thông tin/dự báo lợi suất VN-Index cao hơn so với việc sử dụng riêng lẻ từng thành phần MSI hoặc NSI hay không.

## 2. Mục tiêu nghiên cứu

- Xây dựng một chỉ số tâm lý nhà đầu tư tổng hợp (SI) cho thị trường chứng khoán Việt Nam bằng cách kết hợp có căn cứ khoa học giữa MSI và NSI

- Kiểm định chỉ số này có mang lại giá trị thông tin/dự báo cao hơn so với sử dụng riêng lẻ từng thành phần hay không.

## 3. Câu hỏi nghiên cứu

 **1 : Làm sao xây dựng một chỉ số tâm lý tổng hợp (SI) đáng tin cậy từ dữ liệu thị trường và dữ liệu văn bản tại thị trường chứng khoán Việt Nam**

 **2 : Chỉ số này có mang lại giá trị thông tin/dự báo cao hơn so với từng thành phần riêng lẻ hay không?**

| Câu hỏi lớn | Câu hỏi nhỏ | Câu hỏi |
|---|---|---|
| 1 | RQ1 | MSI biến động ra sao? |
| 1 | RQ2 | NSI biến động ra sao? |
| 1 | RQ3 | Mức độ tương quan/đồng liên kết giữa MSI và NSI ra sao, có ổn định theo thời gian không? |
| 1 | RQ4 | Giữa MSI và NSI có quan hệ nhân quả không, ở độ trễ nào? |
| 1 | RQ5 | SI nên xây theo phương pháp nào (trọng số cố định, PCA lần hai, hay trọng số động)? |
| 2 | RQ6 | SI có giải thích/dự báo lợi suất VN-Index tốt hơn MSI, NSI riêng lẻ không? |
| * | RQ7 | SI có ổn định khi kiểm định ngoài mẫu (out-of-sample) không? |

## 4. Quy trình thực hiện

```
Giai đoạn 1: Xây dựng chỉ số tâm lý tổng hợp SI từ MSI và NSI
Giai đoạn 2: Kiểm định giá trị thông tin (dự báo)
```

<img width="1056" height="397" alt="image" src="https://github.com/user-attachments/assets/d85f8ae3-029e-4825-81c0-e5196e07420d" />

## 5. Cấu trúc thư mục

```
.
├── README.md
├── requirements.txt
├── .gitignore
│
├── docs/                              # Tài liệu đề cương, tổng hợp khung nghiên cứu
│   ├── de_cuong_nghien_cuu.docx
│   ├── tong_hop_khung_nghien_cuu.docx
│   └── figures/                       # Sơ đồ, biểu đồ minh họa
│
├── data/
│   ├── raw/                           # Dữ liệu thô, chưa xử lý
│   │   ├── market/                    # Giá, khối lượng, khối ngoại theo mã/ngày
│   │   ├── news/                      # Tin tức crawl từ CafeF (theo mã, thị trường chung)
│   │   └── macro/                     # CPI, lãi suất, tỷ giá
│   ├── interim/                       # Dữ liệu trung gian (đã làm sạch, chưa tổng hợp)
│   └── processed/                     # MSI, NSI, SI đã tính xong — sẵn sàng cho mô hình
│
├── src/
│   ├── crawling/
│   │   ├── crawl_tin_theo_ma.py
│   │   ├── crawl_thi_truong_chung.py
│   │   └── crawl_vi_mo.py
│   │
│   ├── preprocessing/
│   │   ├── clean_news_data.py         # Làm sạch, loại trùng, đồng bộ ngày giao dịch
│   │   └── clean_market_data.py
│   │
│   ├── sentiment/
│   │   ├── phobert_sentiment.py       # Gán nhãn cảm xúc bằng PhoBERT
│   │   └── attention_index.py         # Tính HHI, Attention Index
│   │
│   ├── indices/
│   │   ├── build_msi.py               # PCA → MSI
│   │   ├── build_nsi.py               # Tổng hợp NSI theo mã/thị trường
│   │   └── build_si.py                # Kết hợp MSI + NSI → SI
│   │
│   ├── validation/
│   │   ├── correlation_tests.py       # Pearson/Spearman, DCC-GARCH
│   │   ├── cointegration_granger.py   # Đồng liên kết Johansen, nhân quả Granger
│   │   └── copula_tail_dependence.py
│   │
│   ├── evaluation/
│   │   ├── explanatory_power.py       # So sánh R²/AIC/BIC giữa MSI, NSI, SI (H4)
│   │   └── out_of_sample_test.py      # Kiểm định ngoài mẫu (H5)
│   │
│   └── tests/
│       └── test_data_quality.py       # Bộ test kiểm tra chất lượng dữ liệu trước NLP
│
├── notebooks/                         # Notebook khám phá dữ liệu, trực quan hóa kết quả
│   ├── 01_eda_market_data.ipynb
│   ├── 02_eda_news_data.ipynb
│   └── 03_ket_qua_kiem_dinh.ipynb
│
└── outputs/
    ├── figures/                       # Biểu đồ kết quả (MSI, NSI, SI theo thời gian...)
    └── tables/                        # Bảng kết quả kiểm định xuất ra báo cáo
```

## 6. Cài đặt

```bash
git clone <repo-url>
cd <repo-name>
pip install -r requirements.txt
```

## 7. Giai đoạn nghiên cứu

| Nội dung | Chi tiết |
|---|---|
| Giai đoạn nghiên cứu | **2021 – 2025** |

## 8. Các mã chứng khoán nghiên cứu

| Rổ chỉ số | Cách chọn mã |
|---|---|
| **VN30** | 10 mã cổ phiếu : BID, BVH, CTG, FPT, GAS, VHM, VIC, VJC, VNM, VPB, VRE |
| **VNMidcap** | 5 mã cổ phiếu : AAA, AGG, ANV, ASM, AST |
| **VNSmallcap** | 5 mã cổ phiếu : ABS, ACC, ACL, ADS, AGM |
| **Tổng cộng** | **20 mã** |

### 9. Danh sách chỉ số và cách lấy dữ liệu

| Chỉ số | Cách lấy |
|---|---|
| Giá mở cửa trong ngày | Lấy theo ngày ở mỗi mã (20 mã) |
| Giá đóng cửa trong ngày | Lấy theo ngày ở mỗi mã (20 mã) |
| Giá cao nhất trong phiên | Lấy cả 2 phiên sáng và chiều ở mỗi mã (20 mã) |
| Giá thấp nhất trong phiên | Lấy cả 2 phiên sáng và chiều ở mỗi mã (20 mã) |
| Giá trần | Lấy cả 2 phiên theo ngày ở mỗi mã (20 mã) |
| Giá sàn | Lấy cả 2 phiên theo ngày ở mỗi mã (20 mã) |
| Số mã tăng trần | Lấy theo ngày ở mỗi mã (20 mã) |
| Số mã giảm sàn | Lấy theo ngày ở mỗi mã (20 mã) |
| Khối lượng khớp lệnh mua bán trong ngày | Lấy theo ngày ở mỗi mã (20 mã) |
| Giá trị giao dịch theo ngày | Lấy theo ngày ở mỗi mã (20 mã) |
| Số lượng cổ phiếu đang lưu hành | Lấy theo quý (3 tháng 1 lần) ở mỗi mã |
| Tổng giá trị giao dịch mua của nhà đầu tư nước ngoài | Lấy theo ngày ở mỗi mã (20 mã) |
| Tổng giá trị giao dịch bán của nhà đầu tư nước ngoài | Lấy theo ngày ở mỗi mã (20 mã) |
| Khối lượng cổ phiếu mà nhà đầu tư nước ngoài còn được phép mua trong ngày (foreign room) | Lấy theo ngày ở mỗi mã (20 mã) |
| Chỉ số giá tiêu dùng CPI | Lấy từ Tổng cục Thống kê theo tháng |
| Lãi suất điều hành của NHNNVN | Lấy từ Ngân hàng Nhà nước VN theo lần điều chỉnh |
| Lãi suất trái phiếu Chính phủ kỳ hạn 10 năm | Lấy trên sàn HNX theo ngày |
| Tỷ giá USD/VND | Lấy từ Ngân hàng Nhà nước VN theo ngày |


