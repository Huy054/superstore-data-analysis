# DAX và cấu hình Power BI — Superstore

Tài liệu này ghi lại công thức sử dụng trong quá trình xây dựng và cấu hình mà báo cáo PBIX đang tham chiếu. **Lưu ý:** các tệp PBIX lưu DataModel ở định dạng nén; kiểm tra bên ngoài Power BI xác nhận được tên Measure/cột trong visual, nhưng không trích xuất trực tiếp được toàn bộ thân công thức từ DataModel. Nếu cần xác thực từng công thức đang lưu, mở file bằng Power BI Desktop và kiểm tra trong **Công cụ đo / Cột**.

## 1. KPI số — phải trả về số

```dax
Total Sales = SUM('Cleaned_Superstore'[Sales])
```

```dax
Total Profit = SUM('Cleaned_Superstore'[Profit])
```

```dax
Total Orders = DISTINCTCOUNT('Cleaned_Superstore'[Order ID])
```

```dax
Profit Margin % = DIVIDE([Total Profit], [Total Sales], 0)
```

```dax
Loss-making Lines =
CALCULATE(
    COUNTROWS('Cleaned_Superstore'),
    'Cleaned_Superstore'[Profit] < 0
)
```

```dax
Loss Rate % = DIVIDE(
    [Loss-making Lines],
    COUNTROWS('Cleaned_Superstore'),
    0
)
```

Định dạng `Profit Margin %` và `Loss Rate %` là phần trăm, hai chữ số thập phân.

## 2. Cột tính toán — Discount Band

**Đây là Cột mới (New column), không phải Measure.**

```dax
Discount Band =
VAR D = 'Cleaned_Superstore'[Discount]
RETURN
SWITCH(
    TRUE(),
    ISBLANK(D), "Không xác định",
    D = 0, "0%",
    D <= 0.1, "0–10%",
    D <= 0.2, "10–20%",
    D <= 0.3, "20–30%",
    D <= 0.4, "30–40%",
    ">40%"
)
```

```dax
Discount Band Sort =
VAR D = 'Cleaned_Superstore'[Discount]
RETURN
SWITCH(
    TRUE(),
    ISBLANK(D), 99,
    D = 0, 1,
    D <= 0.1, 2,
    D <= 0.2, 3,
    D <= 0.3, 4,
    D <= 0.4, 5,
    6
)
```

Sau khi tạo xong, chọn `Discount Band` → **Công cụ cột → Sắp xếp theo cột → Discount Band Sort**, để nhóm `>40%` không đứng đầu do sắp xếp theo ký tự.

## 3. Định dạng màu âm/dương

```dax
Discount Margin Color =
VAR M = [Profit Margin %]
RETURN
SWITCH(
    TRUE(),
    ISBLANK(M), "#94A3B8",
    M < 0, "#DC2626",
    "#0D9488"
)
```

```dax
Profit Sign Color =
VAR P = [Total Profit]
RETURN
SWITCH(
    TRUE(),
    ISBLANK(P), "#94A3B8",
    P < 0, "#DC2626",
    "#0D9488"
)
```

Trong định dạng visual: chọn **Màu → fx → Giá trị trường (Field value)** để gọi Measure màu. Màu chữ của nhãn dữ liệu có thể đặt độc lập với màu thanh/cột nếu bản Power BI hỗ trợ `fx` tại mục đó.

## 4. Card hiển thị đơn vị động

KPI Sales và Profit trên Dashboard sử dụng các Measure hiển thị riêng (`Sales KPI Display`, `Profit KPI Display`): tự đổi đơn vị `triệu`/`nghìn` khi lọc và hiển thị dấu âm `-` thay vì dấu ngoặc kế toán. Đây là chuỗi văn bản, **không dùng để vẽ biểu đồ**.

```dax
Sales KPI Display =
VAR S = [Total Sales]
VAR Amount = ABS(S)
VAR Sign = IF(S < 0, "-", "")
RETURN
IF(
    ISBLANK(S), BLANK(),
    Sign & "$" &
    SWITCH(
        TRUE(),
        Amount >= 1000000, FORMAT(Amount/1000000, "0.00", "vi-VN") & " triệu",
        Amount >= 1000, FORMAT(Amount/1000, "0.00", "vi-VN") & " nghìn",
        FORMAT(Amount, "#,##0.00", "vi-VN")
    )
)
```

```dax
Profit KPI Display =
VAR P = [Total Profit]
VAR Amount = ABS(P)
VAR Sign = IF(P < 0, "-", "")
RETURN
IF(
    ISBLANK(P), BLANK(),
    Sign & "$" &
    SWITCH(
        TRUE(),
        Amount >= 1000000, FORMAT(Amount/1000000, "0.00", "vi-VN") & " triệu",
        Amount >= 1000, FORMAT(Amount/1000, "0.00", "vi-VN") & " nghìn",
        FORMAT(Amount, "#,##0.00", "vi-VN")
    )
)
```

## 5. Báo cáo và bộ lọc

- **Business Performance:** 4 Card; `lineClusteredColumnComboChart` với `Year-Month`, `Total Sales` (cột), `Total Profit` (đường); `Profit Margin % by Category`; `Profit by Region`.
- **Profitability Deep Dive:** Card `Loss-making Lines`, `Loss Rate %`; `Profit Margin % by Discount Band`; `Profit by Sub-Category`.
- `Year` và `Region` là slicer dạng Dropdown, mỗi loại dùng **syncGroup cùng tên ở hai trang**. Các báo cáo thay đổi theo lựa chọn của bộ lọc.

