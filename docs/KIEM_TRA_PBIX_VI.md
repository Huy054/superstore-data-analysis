# Kiểm tra PBIX — Superstore_Dashboard_New.pbix

## Phạm vi kiểm tra

Đọc cấu trúc ZIP của PBIX (định dạng Power BI Desktop), file `Report/Layout` và hình ảnh từ **PDF hai trang cuối cùng**. Không chạy Power BI Desktop trong môi trường kiểm tra; mô hình `DataModel` sử dụng định dạng nén nên không trích xuất và thực thi độc lập toàn bộ Measure.

## Cấu trúc báo cáo đã xác nhận

| Trang trong PBIX | Số đối tượng | Trạng thái |
|---|---:|---|
| `Trang 1` | 6 | Trang cũ, giữ để đối chiếu; không xuất trong PDF cuối |
| `Business Performance` | 10 | 4 KPI, 3 biểu đồ, 2 slicer, tiêu đề |
| `Profitability Deep Dive` | 7 | 2 KPI, 2 biểu đồ, 2 slicer, tiêu đề |

### Trường được dùng bởi từng đối tượng

- `Business Performance`: `Sales KPI Display`, `Profit KPI Display`, `Profit Margin %`, `Total Orders`; combo `Year-Month × Total Sales × Total Profit`; cột `Category × Profit Margin %`; thanh `Region × Total Profit`; slicers `Year`, `Region`.
- `Profitability Deep Dive`: cột `Discount Band × Profit Margin %`; thanh `Sub-Category × Total Profit`; Card `Loss-making Lines`, `Loss Rate %`; slicers `Year`, `Region`.
- **Cả 4 slicer** cùng trường tương ứng sử dụng `syncGroup` có `fieldChanges: true`, `filterChanges: true` để đồng bộ giữa hai trang.

## Đối chiếu với PDF đã xuất

- PDF cuối có đúng **2 trang**, không chứa `Trang 1` cũ.
- Tất cả bộ lọc trên PDF hiện **Tất cả**; KPI tổng thể Sales **$2,30 triệu**, Profit **$286,40 nghìn**, Margin **12,47%**, Orders **5.009**.
- Trang 2 có **1.871 dòng lỗ**, Loss Rate **18,72%**; nhóm Discount `>40%` có Margin **−77,40%**.
- Không thấy trục, thẻ và biểu đồ bị cắt ở hai ảnh kết xuất PDF.
- Lỗi chữ **`OREDERS`** ở phiên bản trước đã được đổi thành **`ORDERS`** trên PDF cuối.

## Cần kiểm tra thêm trên Power BI Desktop

1. **Đường dẫn nguồn:** máy khác có thể không tìm được CSV v2 vì nguồn Power Query trỏ theo đường dẫn tuyệt đối trên Windows. Chọn **Transform data → Source** và đổi sang CSV v2 trên máy hiện tại.
2. **Độ chính xác Measure:** đối chiếu kết quả tổng trên Desktop với notebook CSV v2; file PBIX nhúng DataModel không đủ để tái chạy mô hình trong công cụ kiểm tra.
3. **Lọc và đồng bộ:** đã có bằng chứng qua ảnh dùng thử Central/2017; để đảm bảo trạng thái sau khi mở lại PBIX, thử lọc và xóa chọn trực tiếp.
4. **Kiểu dữ liệu:** kiểm tra cột `Order Date`, `Ship Date`, `Sales`, `Profit`, `Discount`, `Quantity`, `Year`, `Month` đúng kiểu. Cần dùng locale `English (United States)` khi parse CSV có dấu chấm thập phân.
5. **Chữ giao diện ngoài báo cáo:** bản PDF có xuất hiện chữ nhỏ `Power BI Desktop` phía trên ở một số khu vực. Nếu là một thành phần chèn ngoài ý muốn, có thể xóa hoặc điều chỉnh trên Desktop và xuất lại; không ảnh hưởng KPI.

