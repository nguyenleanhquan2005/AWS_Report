---
title: "Module 2: Xây dựng Knowledge Base"
date: "2025-09-09"
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

### Xây dựng Knowledge Base (Data Layer)

#### Bước 1: Đăng ký Amazon QuickSight
1. Đăng nhập vào AWS Management Console.
2. Tìm kiếm **QuickSight**.
3. Nếu bạn chưa đăng ký, bạn sẽ được nhắc đăng ký.
4. Chọn **Sign up for QuickSight**.
5. Chọn phiên bản **Enterprise** (có bản dùng thử miễn phí).
6. Làm theo hướng dẫn trên màn hình:
   - **Phương thức xác thực**: Sử dụng IAM federated identity & QuickSight-managed users (mặc định).
   - **Region**: Chọn **US East (N. Virginia)**.
   - **Tên tài khoản QuickSight**: Nhập tên duy nhất.
   - **Email thông báo**: Nhập email của bạn.
   - **Cho phép truy cập**: Bạn có thể để mặc định hoặc bỏ chọn nếu chúng ta chỉ tải lên file. Đối với workshop này, chúng ta không thực sự cần quyền truy cập S3, nhưng nên để S3 được chọn nếu bạn có kế hoạch sử dụng sau này.
7. Nhấp **Finish**.

#### Bước 2: Chuẩn bị Dữ liệu Mẫu

Chúng ta sẽ sử dụng file CSV đơn giản cho workshop này.

1. Sao chép dữ liệu sau và lưu thành file có tên `sales_data.csv` trên máy tính của bạn:

```csv
Date,Region,Product,Sales,Quantity
2023-01-01,North,Laptop,1200,2
2023-01-02,South,Phone,800,5
2023-01-03,East,Tablet,400,3
2023-01-04,West,Laptop,1200,1
2023-01-05,North,Phone,800,2
2023-01-06,South,Tablet,400,4
2023-01-07,East,Laptop,1200,3
2023-01-08,West,Phone,800,6
2023-01-09,North,Tablet,400,2
2023-01-10,South,Laptop,1200,4
```

Ngoài ra, bạn có thể sử dụng bất kỳ file CSV nào bạn có, nhưng các hướng dẫn sẽ theo cấu trúc này.

---

### Bước tiếp theo
Tiếp tục với [Module 3: Phát triển Backend Serverless](../5.3-s3-vpc/) để xây dựng các Lambda functions tự động hóa ingestion và xử lý chat queries.
