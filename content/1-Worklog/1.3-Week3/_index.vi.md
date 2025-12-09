---
title: "Nhật ký Tuần 3"
date: "2025-09-23"
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu Tuần 3:

* Nắm vững các dịch vụ backup và storage của AWS để bảo vệ dữ liệu và tích hợp hybrid cloud.
* Học quản lý đa tài khoản và quản trị với AWS Organizations và Control Tower.
* Hiểu về tự động hóa vận hành và infrastructure as code với Systems Manager và CloudFormation.
* Nắm vững các dịch vụ AWS cốt lõi: EC2, IAM, RDS, Route 53, Auto Scaling, ELB, CloudFront và S3.

### Các nhiệm vụ thực hiện trong tuần:

| Ngày | Nhiệm vụ | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
|:---:|------|:----------:|:---------------:|:-------------------|
| 2 | - Học AWS Backup để quản lý backup tập trung<br>- Nắm vững AWS Storage Gateway cho hybrid cloud storage<br>- Hiểu Amazon S3 Glacier cho lưu trữ archival<br>- Học AWS DataSync cho truyền dữ liệu tự động<br>- **Thực hành:**<br>&emsp; + Cấu hình backup plans và vaults<br>&emsp; + Triển khai File/Volume/Tape Gateway<br>&emsp; + Triển khai Glacier lifecycle policies<br>&emsp; + Thiết lập DataSync tasks | 09/29/2025 | 09/29/2025 | [AWS Backup](https://000102.awsstudygroup.com/)<br>[Storage Gateway](https://000032.awsstudygroup.com/)<br>[S3 Glacier](https://000074.awsstudygroup.com/)<br>[DataSync](https://000075.awsstudygroup.com/) |
| 3 | - Nắm vững AWS Control Tower cho môi trường đa tài khoản<br>- Hiểu AWS Organizations cho quản lý tập trung<br>- Học AWS Service Catalog cho provisioning chuẩn hóa<br>- **Thực hành:**<br>&emsp; + Thiết lập Control Tower landing zone<br>&emsp; + Cấu hình OUs và guardrails<br>&emsp; + Tạo Service Catalog portfolios | 09/30/2025 | 09/30/2025 | [Control Tower](https://000063.awsstudygroup.com/)<br>[Organizations](https://000064.awsstudygroup.com/)<br>[Service Catalog](https://000088.awsstudygroup.com/) |
| 4 | - Nắm vững AWS Systems Manager cho quản lý vận hành<br>- Hiểu AWS CloudFormation cho infrastructure as code<br>- **Thực hành:**<br>&emsp; + Cấu hình Session Manager và Run Command<br>&emsp; + Thiết lập Patch Manager và Parameter Store<br>&emsp; + Tạo CloudFormation stacks và StackSets | 10/01/2025 | 10/01/2025 | [Systems Manager](https://000089.awsstudygroup.com/)<br>[CloudFormation](https://000087.awsstudygroup.com/) |
| 5 | - Học kiến thức cơ bản về Amazon EC2 và compute services<br>- Nắm vững AWS IAM cho quản lý danh tính và truy cập<br>- Hiểu Amazon RDS cho managed databases<br>- Học Amazon Route 53 cho quản lý DNS<br>- **Thực hành:**<br>&emsp; + Khởi chạy EC2 instances với AMIs<br>&emsp; + Cấu hình IAM users, roles và policies<br>&emsp; + Triển khai RDS Multi-AZ instances<br>&emsp; + Thiết lập Route 53 hosted zones | 10/02/2025 | 10/02/2025 | [EC2](https://000012.awsstudygroup.com/)<br>[IAM](https://000030.awsstudygroup.com/)<br>[RDS](https://000044.awsstudygroup.com/)<br>[Route 53](https://000018.awsstudygroup.com/) |
| 6 | - Nắm vững AWS Auto Scaling cho quản lý capacity<br>- Hiểu Elastic Load Balancing cho phân phối traffic<br>- Học Amazon CloudFront cho content delivery<br>- Nắm vững Amazon S3 cho object storage<br>- **Thực hành:**<br>&emsp; + Tạo Auto Scaling groups với policies<br>&emsp; + Cấu hình ALB và NLB<br>&emsp; + Thiết lập CloudFront distributions<br>&emsp; + Triển khai S3 versioning và replication | 10/03/2025 | 10/03/2025 | [Auto Scaling](https://000111.awsstudygroup.com/)<br>[ELB](https://000026.awsstudygroup.com/)<br>[CloudFront](https://000033.awsstudygroup.com/)<br>[S3](https://000090.awsstudygroup.com/) |

### Thành tựu Tuần 3:

* Nắm vững thành công 17 dịch vụ AWS bao gồm backup/storage, quản trị đa tài khoản, tự động hóa vận hành và hạ tầng cốt lõi.
* Cấu hình AWS Backup với lịch trình tự động, backup cross-region và chiến lược disaster recovery.
* Triển khai Storage Gateway để tích hợp hybrid cloud với hạ tầng on-premises.
* Triển khai S3 Glacier lifecycle policies và Vault Lock để tuân thủ quy định.
* Thiết lập Control Tower landing zone với guardrails và Account Factory cho quản lý đa tài khoản.
* Cấu hình AWS Organizations với SCPs và consolidated billing cho quản trị tập trung.
* Nắm vững CloudFormation templates, nested stacks và StackSets cho infrastructure as code.
* Triển khai Systems Manager cho quản lý vận hành thống nhất bao gồm Session Manager, Run Command và Patch Manager.
* Có kinh nghiệm thực hành với EC2, IAM, RDS Multi-AZ, Route 53, Auto Scaling, ELB, CloudFront và S3.

### Ghi chú và Kiến thức Thu được trong Tuần 3:

#### Các Dịch vụ AWS Cốt lõi
* **EC2:** Dịch vụ compute với nhiều loại instance, AMIs, security groups, EBS volumes và giám sát CloudWatch.
* **IAM:** Quản lý danh tính và truy cập với users, groups, roles, policies, MFA và nguyên tắc least privilege.
* **RDS:** Managed relational databases với Multi-AZ cho high availability, read replicas cho scaling, automated backups và encryption.
* **Route 53:** Dịch vụ DNS với hosted zones, routing policies (simple, weighted, latency, failover), health checks và alias records.
* **Auto Scaling:** Quản lý capacity tự động với scaling policies (target tracking, step, scheduled), lifecycle hooks và tích hợp CloudWatch.
* **ELB:** Phân phối traffic với ALB (HTTP/HTTPS), NLB (TCP/UDP), target groups, health checks và SSL/TLS termination.
* **CloudFront:** Mạng phân phối nội dung toàn cầu với edge locations, cache behaviors, Lambda@Edge, origin failover và signed URLs.
* **S3:** Object storage với storage classes, versioning, lifecycle policies, replication (CRR/SRR), encryption và event notifications.
