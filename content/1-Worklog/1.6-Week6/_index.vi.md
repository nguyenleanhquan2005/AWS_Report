---
title: "Worklog Tuần 6"
date: "2025-10-13"
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6:

* Làm chủ cơ sở dữ liệu quan hệ và NoSQL trên AWS.
* Học các chiến lược caching với ElastiCache.
* Hiểu về migration cơ sở dữ liệu và disaster recovery.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
|:---:|------|:----------:|:---------------:|:-------------------|
|  2  | - Relational DB: <br>&emsp; + Khởi chạy RDS Instance (MySQL/PostgreSQL) <br>&emsp; + Lưu trữ thông tin xác thực trong Secrets Manager <br>&emsp; + Cấu hình Multi-AZ và Read Replicas | 13/10/2025 | 13/10/2025 | <https://cloudjourney.awsstudygroup.com/> |
|  3  | - NoSQL & Caching: <br>&emsp; + Tạo DynamoDB Tables <br>&emsp; + Ghi Items qua CLI/SDK <br>&emsp; + Khởi chạy ElastiCache (Redis) Cluster | 14/10/2025 | 14/10/2025 | [DynamoDB Developer Guide](https://docs.aws.amazon.com/dynamodb/) |
|  4  | - Advanced DB: <br>&emsp; + Hoàn thành PostgreSQL Workshop (Phần 1) <br>&emsp; + Khám phá SQL Server HA trên AWS <br>&emsp; + Hiểu về RDS Proxy | 15/10/2025 | 15/10/2025 | <https://cloudjourney.awsstudygroup.com/> |
|  5  | - Migration Tools: <br>&emsp; + Nghiên cứu Database Migration Service (DMS) <br>&emsp; + Hiểu về Schema Conversion Tool (SCT) <br>&emsp; + Xem xét Server Migration Service | 16/10/2025 | 16/10/2025 | [AWS DMS](https://docs.aws.amazon.com/dms/) |
|  6  | - Disaster Recovery: <br>&emsp; + Cấu hình AWS Elastic Disaster Recovery (DRS) <br>&emsp; + Thực hành: Failover RDS sang Standby Region | 17/10/2025 | 17/10/2025 | [AWS DRS](https://docs.aws.amazon.com/drs/) |

### Kết quả đạt được tuần 6:

* Khởi chạy thành công các RDS instances với MySQL và PostgreSQL engines.
* Lưu trữ thông tin xác thực cơ sở dữ liệu an toàn trong AWS Secrets Manager.
* Cấu hình Multi-AZ deployments để đảm bảo tính khả dụng cao.
* Thiết lập read replicas cho việc mở rộng đọc và disaster recovery.
* Tạo DynamoDB tables với partition và sort keys.
* Thực hiện các thao tác CRUD trên DynamoDB qua CLI và SDK.
* Khởi chạy ElastiCache Redis clusters cho application caching.
* Hoàn thành PostgreSQL workshop và học các tính năng nâng cao.
* Khám phá các mẫu high availability của SQL Server trên AWS.
* Hiểu về RDS Proxy cho connection pooling và failover.
* Nghiên cứu AWS DMS cho database migration với downtime tối thiểu.
* Học Schema Conversion Tool cho heterogeneous migrations.
* Cấu hình AWS Elastic Disaster Recovery cho khả năng phục hồi ứng dụng.
* Thực hành RDS failover sang standby regions.
