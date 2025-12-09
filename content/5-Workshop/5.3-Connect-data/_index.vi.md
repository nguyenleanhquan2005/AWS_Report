---
title: "Module 3: Phát triển Backend Serverless"
date: "2025-09-09"
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

## KẾT NỐI DỮ LIỆU

#### Bước 1: Dataset Mới

1. Trong QuickSight console, chọn **Datasets** từ navigation pane bên trái.
2. Chọn **New dataset**.
3. Trong phần **FROM NEW DATA SOURCES**, chọn **Upload a file**.
4. Chọn file `sales_data.csv` mà bạn đã tạo ở bước trước.

#### Bước 2: Xem trước và Chỉnh sửa

1. Sau khi file được tải lên, QuickSight sẽ hiển thị bản xem trước.
2. Chọn **Edit settings and prepare data**.
3. Ở đây bạn có thể thấy các cột: `Date`, `Region`, `Product`, `Sales`, `Quantity`.
4. Đảm bảo `Sales` và `Quantity` được phát hiện là số (Integers hoặc Decimals).
5. Đảm bảo `Date` được phát hiện là kiểu Date.
6. Chọn **Save & Publish** ở góc trên bên phải.
7. Chọn **Cancel** để quay lại màn hình chính, hoặc click trực tiếp **Visualize** nếu được nhắc.

---

### Bước tiếp theo
Tiếp tục với [Module 4: API & Bảo mật](../5.4-s3-onprem/) để expose các functions này qua API Gateway với authentication.
