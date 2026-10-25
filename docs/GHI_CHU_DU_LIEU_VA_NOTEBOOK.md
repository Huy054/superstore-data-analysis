# Ghi chú kỹ thuật — Dữ liệu và notebook Python

## Nguồn dữ liệu

- Repo gốc: `Sample - Superstore.csv` (file gốc, bảng mã `windows-1252`/`cp1252`).
- Notebook cũ: `Data_Cleaning.ipynb` và `Cleaned_Superstore.csv`; bản cũ dùng `.round(2)` trên từng dòng `Sales`/`Profit`.
- Notebook mới: `analysis/Superstore_Analysis_Upgrade.ipynb`, xuất `Cleaned_Superstore_v2.csv` sau khi kiểm tra nguồn và giữ nguyên độ chính xác của Sales/Profit.
- Bảng trong Power BI vẫn đặt tên `Cleaned_Superstore`, **dù đường dẫn trỏ tới tệp CSV v2**. Không tự đổi tên bảng khi thay nguồn vì các visual và Measure phụ thuộc tên này.

## Kiểm tra chất lượng dữ liệu

Các kiểm tra đã quan sát từ notebook chạy trên Colab:

| Chỉ tiêu | Kết quả |
|---|---:|
| Số dòng | 9.994 |
| Row ID trùng | 0 |
| Thiếu giá trị ở cột bắt buộc | 0 |
| Quantity <= 0 | 0 |
| Sales âm | 0 |
| Discount ngoài [0,1] | 0 |

Chuẩn hóa `Order Date`, `Ship Date` theo định dạng tháng/ngày/năm (`%m/%d/%Y`); `Year`, `Month`, `Year-Month` là cột dẫn xuất. `Postal Code` lưu dưới dạng text để bảo toàn số 0 đầu nếu có. Không dùng `dropna()` trên mọi cột một cách mặc định.

## Notebook v2 khác gì notebook cũ?

1. **Không làm tròn từng dòng** trước khi cộng số; tránh làm sai kết quả tổng hợp do làm tròn quá sớm.
2. Có **kiểm tra dữ liệu**, ngừng chạy khi gặp khóa trùng, dòng thiếu trường bắt buộc hoặc kiểu số bất thường.
3. Phân tích thêm danh mục, Sub-Category, Segment, Region, Discount Band và 48 tháng.
4. Sử dụng `groupby(..., observed=False)` có chủ đích để giữ đầy đủ nhóm phân loại và tránh FutureWarning từ Pandas về thay đổi mặc định.
5. Xuất `Cleaned_Superstore_v2.csv` bằng `utf-8-sig` (Power BI đọc UTF-8). 

## Cách chạy và kiểm tra KPI

1. Đặt `Sample - Superstore.csv` cạnh thư mục làm việc notebook; khi chạy Colab nên upload trực tiếp vào `/content`.
2. Chạy lần lượt toàn bộ cell notebook.
3. Kiểm tra file `Cleaned_Superstore_v2.csv` xuất ra với **9.994 dòng**.
4. Đối chiếu `Total Sales ≈ 2.297.200,86`, `Total Profit ≈ 286.397,02`, `Total Orders = 5.009`, `Loss-making Lines = 1.871`.
5. Chuyển nguồn trong PBIX thông qua **Transform data → Source** và thiết lập vùng (locale) **English (United States)** cho việc chuyển `Sales`, `Profit`, `Discount` thành số khi cần; CSV chứa dấu chấm `.` làm dấu thập phân.

## Lưu ý về đơn vị

- Số đếm `COUNTROWS` là **dòng bán hàng**; `DISTINCTCOUNT(Order ID)` là **số đơn**.
- Biên lợi nhuận theo nhóm phải tính `SUM(Profit)/SUM(Sales)`, không phải `AVERAGE(Profit/Sales)`.
- Nhãn `nghìn`, `triệu` là cách hiển thị; không thay đổi giá trị Measure gốc.
- Các Measure kết hợp `FORMAT()` trả về **chuỗi** và chỉ phù hợp với KPI Card; giữ `Total Sales`/`Total Profit` dạng số cho phân tích.
