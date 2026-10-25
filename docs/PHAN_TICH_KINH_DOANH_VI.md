# Phân tích kinh doanh và đề xuất — Superstore (2014–2017)

## 1. Phạm vi và phương pháp

Bộ dữ liệu Sample Superstore gồm **9.994 dòng bán hàng**, **5.009 đơn hàng duy nhất**, **793 khách hàng**, trải từ năm 2014 tới 2017. Tính toán được người thực hiện chạy trên CSV gốc bằng notebook Python/Pandas nâng cấp; báo cáo Power BI dùng tệp CSV sạch v2 và hai trang `Business Performance`, `Profitability Deep Dive`. Giá trị tiền tính bằng USD, số tổng được làm tròn khi trình bày.

**Định nghĩa:** `Total Sales = SUM(Sales)`, `Total Profit = SUM(Profit)`, `Profit Margin = Total Profit / Total Sales`, `Loss Rate = COUNTROWS(Profit < 0) / COUNTROWS`. Số dòng không phải số đơn, và các phép tính tỷ lệ được tính trên số tổng, không phải trung bình tỷ lệ từng dòng.

## 2. Bức tranh chung

| Chỉ số | Giá trị |
|---|---:|
| Sales | **$2.297.200,86** |
| Profit | **$286.397,02** |
| Profit Margin | **12,47%** |
| Orders | **5.009** |
| Units sold | **37.873** |
| Loss-making Lines | **1.871 / 9.994** |
| Loss Rate | **18,72%** |

Nhìn riêng doanh thu có thể bỏ qua thực tế gần một phần năm các dòng bán hàng bị lỗ. Vì thế dashboard tách riêng **hiệu quả tổng thể** và **phân tích rủi ro lợi nhuận**.

## 3. Insight 1 — Furniture có doanh thu lớn nhưng biên lợi nhuận thấp

| Category | Sales (USD) | Profit (USD) | Profit Margin |
|---|---:|---:|---:|
| Technology | 836.154,03 | 145.454,95 | 17,40% |
| Furniture | 741.999,80 | 18.451,27 | **2,49%** |
| Office Supplies | 719.047,03 | 122.490,80 | 17,04% |

**Hàm ý:** Furniture không yếu về doanh thu nhưng yếu về khả năng chuyển doanh thu thành lợi nhuận. Cần kiểm tra sâu những Sub-Category có thể kéo biên lợi nhuận đi xuống. Không nên suy diễn toàn bộ Furniture đang lỗ vì tổng Profit của nhóm vẫn dương.

**Khuyến nghị có thể thử nghiệm:** kiểm tra cơ cấu giảm giá theo Sub-Category, khu vực và thời gian; xác định các SKU mang tỷ trọng doanh thu lớn nhưng biên lợi nhuận âm; thử nghiệm chính sách chiết khấu có kiểm soát và đánh giá trước/sau.

## 4. Insight 2 — Tables, Bookcases, Supplies có tổng lợi nhuận âm

| Sub-Category | Profit (USD, xấp xỉ) | Nhận xét |
|---|---:|---|
| Tables | **−17.725,48** | Khoản lỗ tổng hợp lớn nhất |
| Bookcases | **−3.472,56** | Cần rà soát tiếp |
| Supplies | **−1.189,10** | Quy mô nhỏ hơn |
| Copiers | **+55,6 nghìn** | Đóng góp lợi nhuận cao |
| Phones | **+44,5 nghìn** | Đóng góp lợi nhuận cao |

**Hàm ý:** cần ưu tiên vấn đề theo **tổng lỗ tuyệt đối** và khả năng sinh lời, không chỉ theo doanh thu. Tables đáng được phân tích trước, nhưng không thể đề xuất ngừng bán nếu chưa xét tác động gián tiếp, tệp khách hàng và giỏ hàng.

## 5. Insight 3 — Mức giảm giá cao đi kèm rủi ro lỗ

| Nhóm Discount | Margin trên tổng Sales |
|---|---:|
| 0% | +29,51% |
| (0%, 10%] | +16,61% |
| (10%, 20%] | +11,58% |
| (20%, 30%] | −10,05% |
| (30%, 40%] | −19,44% |
| > 40% | **−77,40%** |

Dữ liệu ghi nhận **933/933 dòng có Discount > 40% bị lỗ**, tổng `Profit` khoảng **−$99,56 nghìn** trong nhóm đó. Khi thử lọc `Region = Central` và `Year = 2017`, dashboard hiển thị **247 dòng lỗ**, **Loss Rate 31,75%**, và mức Discount >40% có Margin **−136,65%**. Đây là ví dụ chứng minh dashboard có thể hỗ trợ khoanh vùng vấn đề theo ngữ cảnh.

**Giới hạn nhân quả:** giảm giá cao có tương quan với lợi nhuận âm; dữ liệu không đủ để kết luận giảm giá là nguyên nhân duy nhất. Nhóm sản phẩm, khu vực, chi phí và chính sách thương mại có thể là biến gây nhiễu.

**Khuyến nghị:** (1) theo dõi lợi nhuận theo Discount Band × Category × Region; (2) rà soát giao dịch giảm giá lớn và SKU liên quan; (3) thiết lập thí nghiệm thay đổi ngưỡng giảm giá trước khi áp dụng rộng rãi. Không coi ngưỡng 20% là ngưỡng chính sách tối ưu nếu chưa thử nghiệm.

## 6. Insight 4 — Hiệu quả khác biệt theo khu vực

| Region | Profit (USD, xấp xỉ) |
|---|---:|
| West | 108.418 |
| East | 91.523 |
| South | 46.749 |
| Central | 39.706 |

Central có tổng lợi nhuận thấp nhất trong bốn khu vực; điều này không tự chứng minh Central kém hiệu quả nhất trên mọi tiêu chí. Để phân tích cần thêm **Profit Margin**, số đơn và cơ cấu sản phẩm tương ứng theo cùng bộ lọc. Khi lọc Central trong năm 2017, Profit Margin trang 1 còn **5,13%**, minh họa việc phân tích theo ngữ cảnh có thể cho thấy vấn đề không lộ rõ ở tổng thể.

## 7. Insight 5 — Doanh thu có xu hướng tăng nhưng dao động theo tháng

Doanh thu giai đoạn năm 2017 là khoảng **$733,2 nghìn**, cao nhất trong bốn năm; biểu đồ **48 tháng** cho thấy dao động đáng kể và các tháng 11–12 thường cao. Không đủ cơ sở chứng minh yếu tố mùa vụ do lễ hội nếu thiếu thông tin chiến dịch, thời tiết, hoặc biến vĩ mô.

## 8. Đề xuất triển khai cho quản lý

1. **Ưu tiên phân tích Tables** cùng các dòng giao dịch Discount cao để xác định sản phẩm và khu vực chịu lỗ chính.
2. **Theo dõi đồng thời Sales, Profit, Margin, Loss Rate**, thay vì chỉ quản lý chỉ tiêu doanh thu.
3. **Phân nhóm và thử nghiệm chính sách giảm giá** cho nhóm có rủi ro, đo lường với nhóm đối chứng hoặc giai đoạn phù hợp.
4. **Phân tích Region và Year dưới cùng định nghĩa KPI** để tránh so sánh sai giữa các phân khúc.

## 9. Giới hạn và tính minh bạch

- Số liệu là **Sample Superstore** dùng cho mục tiêu học tập, không phải số liệu thực của một công ty cụ thể.
- Tổng thể có giá trị dương nhưng từng nhóm hoặc giao dịch có thể âm; không nên ngoại suy các kết luận ra ngoài bộ dữ liệu.
- `Profit` không mặc nhiên là lợi nhuận ròng kế toán.
- `Loss Rate` là tỷ lệ **dòng bán hàng**, không phải tỷ lệ đơn hàng thua lỗ hay tỷ lệ khách hàng.
