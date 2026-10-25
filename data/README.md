# Data — Dữ liệu đang sử dụng

| Tệp | Vai trò | Trạng thái |
|---|---|---|
| `Sample - Superstore.csv` | CSV **nguồn gốc**, dùng để chạy notebook Python; mã hóa Windows-1252 (`cp1252`) | **Đang ở thư mục gốc repo cũ**; hãy chuyển vào `data/` |
| `Cleaned_Superstore_v2.csv` | CSV **sạch mới** do notebook nâng cấp tạo; Power BI sử dụng | **Chưa kèm trong ZIP**; sẽ upload từ Google Colab vào `data/` |

**Không đặt `Cleaned_Superstore.csv` cũ vào đây**; chuyển bản cũ đó sang `archive/`. Không đổi nội dung CSV khi di chuyển.

Nếu chạy notebook nâng cấp trên Colab, tải tệp nguồn từ `data/` vào `/content` dưới tên `Sample - Superstore.csv`. Nếu chạy notebook từ thư mục gốc của repo trên máy, có thể đổi biến `INPUT_FILE` thành `Path("data/Sample - Superstore.csv")`. Sau khi chạy, chuyển `Cleaned_Superstore_v2.csv` vào `data/` và trỏ Power BI tới đường dẫn mới.
