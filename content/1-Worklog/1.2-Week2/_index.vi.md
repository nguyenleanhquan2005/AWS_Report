---
title: "Nhật ký Tuần 2"
date: "2025-09-15"
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Mục tiêu Tuần 2:

* Nắm vững VPCs, kết nối hybrid và edge networking.
* Hiểu về giám sát mạng, bảo mật và phân phối nội dung.
* Học các kịch bản networking nâng cao bao gồm VPC Peering, Transit Gateway và Site-to-Site VPN.

### Các nhiệm vụ thực hiện trong tuần:
| Ngày | Nhiệm vụ | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
|:---:|------|:----------:|:---------------:|:-------------------|
|  2  | - VPC Cơ bản: <br>&emsp; + Tạo VPC, Subnets (Public/Private) <br>&emsp; + Cấu hình Route Tables và Internet Gateways <br>&emsp; + Workshop: Networking trên AWS (Phần 1) | 15/09/2025 | 15/09/2025 | [AWS VPC Workshop](https://000003.awsstudygroup.com/) |
|  3  | - Giám sát & Bảo mật Mạng: <br>&emsp; + Bật VPC Flow Logs <br>&emsp; + Cấu hình AWS WAF (Web Application Firewall) <br>&emsp; + Thiết lập VPC Endpoints (Gateway & Interface) | 16/09/2025 | 16/09/2025 | [VPC Flow Logs](https://000092.awsstudygroup.com/)<br>[AWS WAF](https://000074.awsstudygroup.com/)<br>[VPC Endpoints](https://000026.awsstudygroup.com/) |
|  4  | - Kết nối: <br>&emsp; + Cấu hình VPC Peering <br>&emsp; + Khám phá AWS Transit Gateway <br>&emsp; + Quản lý DNS với Amazon Route 53 | 17/09/2025 | 17/09/2025 | [VPC Peering](https://000019.awsstudygroup.com/)<br>[Transit Gateway](https://000020.awsstudygroup.com/)<br>[Route 53 Hybrid DNS](https://000010.awsstudygroup.com/) |
|  5  | - Phân phối Nội dung: <br>&emsp; + Tạo CloudFront Distribution <br>&emsp; + Triển khai Lambda@Edge cho dynamic routing <br>&emsp; + Cấu hình Route 53 Geolocation Routing | 18/09/2025 | 18/09/2025 | <https://000130.awsstudygroup.com/> |
|  6  | - Thực hành: Kịch bản VPC Nâng cao <br>&emsp; + Mô phỏng kết nối Site-to-Site VPN | 19/09/2025 | 19/09/2025 | [Networking Workshop](https://000003.awsstudygroup.com/) |

### Thành tựu Tuần 2:

* Tạo thành công VPCs với public và private subnets, route tables và internet gateways.
* Cấu hình VPC Flow Logs để giám sát và khắc phục sự cố traffic mạng.
* Triển khai AWS WAF để bảo vệ ứng dụng web khỏi các lỗ hổng phổ biến.
* Thiết lập VPC Endpoints (Gateway và Interface) cho kết nối riêng tư đến dịch vụ AWS.
* Cấu hình VPC Peering cho giao tiếp an toàn giữa các VPCs.
* Khám phá AWS Transit Gateway cho kết nối mạng tập trung trên nhiều VPCs.
* Nắm vững quản lý DNS Route 53 bao gồm geolocation routing policies.
* Tạo CloudFront distributions cho phân phối nội dung toàn cầu với độ trễ thấp.
* Triển khai Lambda@Edge cho định tuyến nội dung động tại edge locations.
* Thực hành các kịch bản VPC nâng cao và mô phỏng kết nối Site-to-Site VPN.

### Ghi chú và Kiến thức Thu được trong Tuần 2:

#### 1. VPC Cơ bản
* **Thành phần VPC:**
    * **Subnets:** Public subnets có routes đến Internet Gateway, private subnets thì không.
    * **Route Tables:** Kiểm soát định tuyến traffic trong VPC và đến mạng bên ngoài.
    * **Internet Gateway:** Cho phép giao tiếp giữa VPC và internet.
    * **NAT Gateway:** Cho phép instances trong private subnet truy cập internet mà không bị lộ.
* **Lập kế hoạch CIDR:**
    * Chọn CIDR blocks phù hợp để tránh xung đột IP với mạng on-premises.
    * Lập kế hoạch cho tăng trưởng tương lai khi phân bổ subnet ranges.
* **Best Practices:**
    * Sử dụng private subnets cho databases và backend servers.
    * Đặt load balancers và bastion hosts trong public subnets.
    * Triển khai nhiều availability zones cho high availability.
