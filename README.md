Phân tích Hiệu quả Kinh doanh Cửa hàng Bán lẻ (Superstore Sales Analysis)

📌 Tổng quan dự án (Project Overview)

Dự án này là một Mini-project ứng dụng quy trình Data Analytics thực tế để phân tích bộ dữ liệu bán hàng của một hệ thống siêu thị (Superstore). Dự án sử dụng phương pháp kết hợp: dùng Python để tiền xử lý, làm sạch dữ liệu (ETL) và dùng Power BI để xây dựng Dashboard trực quan hóa tương tác.

📊 Mô tả dữ liệu (Dataset)

Nguồn dữ liệu: Superstore Sales Dataset (từ Kaggle).

Nội dung: Chứa thông tin chi tiết về các giao dịch bán hàng bao gồm ngày tháng, phân khúc khách hàng (Segment), danh mục sản phẩm (Category/Sub-Category), doanh thu (Sales) và lợi nhuận (Profit).

🛠️ Công nghệ sử dụng (Tech Stack)

Data Cleaning & ETL (Python): Sử dụng thư viện pandas trên môi trường Google Colab để xử lý dữ liệu bị thiếu, chuyển đổi kiểu dữ liệu (Datetime) và trích xuất các đặc trưng mới (Year, Month).

Data Visualization (Power BI): Nhập file dữ liệu đã làm sạch để xây dựng Interactive Dashboard. Tích hợp ngôn ngữ DAX để tạo các thước đo (Measures) tính toán Biên lợi nhuận (Profit Margin).

💡 Kết quả phân tích chính (Key Insights)

Sau khi trực quan hóa dữ liệu trên Power BI, chúng ta rút ra được các kết luận chẩn đoán kinh doanh sau:

Về Sản phẩm: Nhóm mặt hàng Tables và Bookcases mang lại doanh thu cao nhưng biên lợi nhuận (Profit Margin) đang bị âm. Nguyên nhân chính do lạm dụng chiến lược giảm giá (Discount) quá sâu ở các mặt hàng này.

Về Khách hàng: Phân khúc Consumer (Người tiêu dùng cá nhân) là nhóm khách hàng cốt lõi, đóng góp phần lớn vào tổng lợi nhuận của toàn hệ thống, vượt trội hơn so với nhóm Corporate.

Về Thời gian: Doanh thu có xu hướng tăng mạnh vào các tháng cuối năm (Quý 4), đặc biệt là tháng 11 và 12, cho thấy tính mùa vụ rõ rệt trong hành vi mua sắm.


🚀 Cấu trúc thư mục dự án

Sample - Superstore.csv : Dataset mẫu

Data_Cleaning.ipynb: Mã nguồn Python dùng để làm sạch dữ liệu thô ban đầu.

Cleaned_Superstore.csv: Tập dữ liệu chuẩn sau khi đã được Python xử lý và làm sạch.

Superstore_Dashboard.pbix: File Power BI chứa Dashboard hoàn chỉnh và các hàm tính toán DAX.
