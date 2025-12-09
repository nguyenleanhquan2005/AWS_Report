---
title: "Module 4: API & Bảo mật"
date: "2025-09-09"
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

## TẠO BIỂU ĐỒ

#### Bước 1: Tạo Analysis

1. Nếu bạn chưa ở trong chế độ xem analysis, hãy vào **Datasets**, chọn dataset của bạn và chọn **Create analysis**.
2. Bạn sẽ thấy một workspace trống được gọi là "Sheet".

#### Bước 2: Biểu đồ 1 - Doanh số theo Sản phẩm (Donut Chart)

1. Trong khung **Visual types** (góc dưới bên trái), chọn biểu tượng **Donut chart**.
2. Từ **Fields list** (bên trái), kéo `Product` vào ô **Group/Color**.
3. Kéo `Sales` vào ô **Value**.
4. Bạn sẽ thấy biểu đồ donut hiển thị phân bổ doanh số theo sản phẩm.

#### Bước 3: Biểu đồ 2 - Xu hướng Doanh số theo Thời gian (Line Chart)

1. Chọn **Add > Add visual** từ thanh menu trên cùng.
2. Chọn biểu tượng **Line chart**.
3. Kéo `Date` vào ô **X axis**.
4. Kéo `Sales` vào ô **Value**.
5. Bạn sẽ thấy biểu đồ đường hiển thị doanh số theo thời gian.

#### Bước 4: Biểu đồ 3 - Doanh số theo Khu vực (Bar Chart)

1. Chọn **Add > Add visual**.
2. Chọn biểu tượng **Vertical bar chart**.
3. Kéo `Region` vào ô **X axis**.
4. Kéo `Sales` vào ô **Value**.
5. Bây giờ bạn có thể thấy khu vực nào hoạt động tốt nhất.

---

### Bước tiếp theo
Tiếp tục với [Module 5: Tích hợp Frontend & Kiểm thử](../5.5-policy/) để xây dựng và deploy ứng dụng React.
