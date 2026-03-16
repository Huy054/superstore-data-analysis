# Phân tích Hiệu quả Kinh doanh Cửa hàng Bán lẻ (Superstore Sales Analysis)

## 📌 Tổng quan dự án (Project Overview)
Dự án này là một Mini-project ứng dụng quy trình **Data Analytics** thực tế để phân tích bộ dữ liệu bán hàng của một hệ thống siêu thị (Superstore). Dự án sử dụng phương pháp kết hợp: dùng **Python** để tiền xử lý, làm sạch dữ liệu (ETL) và dùng **Power BI** để xây dựng Dashboard trực quan hóa tương tác.

## 📊 Mô tả dữ liệu (Dataset)
* **Nguồn dữ liệu:** Superstore Sales Dataset (từ Kaggle).
* **Nội dung:** Chứa thông tin chi tiết về các giao dịch bán hàng bao gồm ngày tháng, phân khúc khách hàng (Segment), danh mục sản phẩm (Category/Sub-Category), doanh thu (Sales) và lợi nhuận (Profit).

## 🛠️ Công nghệ sử dụng (Tech Stack)
* **Data Cleaning & ETL (Python):** `pandas` được sử dụng để xử lý missing values, chuyển đổi kiểu dữ liệu (Datetime) và tạo các feature mới (Year, Month).
* **Data Visualization (Power BI):** Nhập file dữ liệu đã làm sạch để xây dựng Interactive Dashboard, thiết lập các Measure (như Profit Margin bằng DAX).

## 💡 Kết quả phân tích chính (Key Insights)
Sau khi trực quan hóa dữ liệu trên Power BI, chúng ta rút ra được các kết luận sau:
1. **Về Sản phẩm:** Nhóm mặt hàng `Tables` và `Bookcases` mang lại doanh thu cao nhưng lợi nhuận đang bị âm do lạm dụng chiến lược giảm giá (Discount) quá sâu.
2. **Về Khách hàng:** Phân khúc `Consumer` (Người tiêu dùng cá nhân) đóng góp phần lớn vào tổng lợi nhuận của toàn hệ thống.
3. **Về Thời gian:** Doanh thu có xu hướng tăng mạnh vào các tháng cuối năm (Quý 4).

## 🚀 Cấu trúc thư mục
* `Data_Cleaning.ipynb`: Mã nguồn Python dùng để làm sạch dữ liệu thô ban đầu.
* `Cleaned_Superstore.csv`: Tập dữ liệu sau khi đã được Python làm sạch và chuẩn hóa.
* `Superstore_Dashboard.pbix`: File Power BI chứa Dashboard hoàn chỉnh. 
