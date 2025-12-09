---
title: "Nhật ký Tuần 4"
date: "2025-09-29"
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu Tuần 4:
* Thành thạo S3, Block Storage, chiến lược Backup và Hybrid storage.
* Hiểu về bảo vệ dữ liệu, mã hóa và tuân thủ.
* Học tối ưu hóa hiệu suất lưu trữ và sao chép đa vùng.

### Các nhiệm vụ thực hiện trong tuần này:
| Ngày | Nhiệm vụ | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
|:----:|----------|:------------:|:---------------:|:-------------------|
|  2   | - Object Storage: <br>&emsp; + Lưu trữ Website tĩnh trên S3 <br>&emsp; + Cấu hình S3 Bucket Policies & Versioning <br>&emsp; + Triển khai Lifecycle Rules | 29/09/2025 | 29/09/2025 | <https://cloudjourney.awsstudygroup.com/> |
|  3   | - Block Storage (EBS): <br>&emsp; + Tự động hóa Snapshots với Data Lifecycle Manager <br>&emsp; + Kiểm tra EBS Multi-Attach <br>&emsp; + Xem xét các loại EBS Volume | 30/09/2025 | 30/09/2025 | [Amazon EBS](https://docs.aws.amazon.com/ebs/) |
|  4   | - File & Hybrid Storage: <br>&emsp; + Tạo Amazon FSx cho Windows File Server <br>&emsp; + Mount EFS trên Linux EC2 <br>&emsp; + Cấu hình Storage Gateway (File Gateway) | 01/10/2025 | 01/10/2025 | <https://cloudjourney.awsstudygroup.com/> |
|  5   | - Bảo vệ Dữ liệu: <br>&emsp; + Cấu hình AWS Backup Plans <br>&emsp; + Quét S3 với Amazon Macie <br>&emsp; + Tạo/Quản lý Keys trong AWS KMS | 02/10/2025 | 02/10/2025 | [AWS Backup](https://docs.aws.amazon.com/backup/) |
|  6   | - Hiệu suất: <br>&emsp; + Hoàn thành Storage Performance Workshop <br>&emsp; + Thực hành: Thiết lập S3 Cross-Region Replication | 03/10/2025 | 03/10/2025 | <https://cloudjourney.awsstudygroup.com/> |

### Thành tựu Tuần 4:
* Lưu trữ thành công website tĩnh trên S3 với tên miền tùy chỉnh và HTTPS.
* Cấu hình S3 bucket policies để kiểm soát truy cập an toàn và versioning để bảo vệ dữ liệu.
* Triển khai S3 lifecycle rules để tự động chuyển đổi lớp lưu trữ và tối ưu chi phí.
* Tự động hóa EBS snapshots sử dụng Data Lifecycle Manager để tự động sao lưu.
* Kiểm tra tính năng EBS Multi-Attach để chia sẻ block storage trên nhiều instances.
* Xem xét và lựa chọn các loại EBS volume phù hợp dựa trên yêu cầu hiệu suất.
* Tạo Amazon FSx cho Windows File Server để quản lý đầy đủ hệ thống file Windows.
* Mount Amazon EFS trên các Linux EC2 instances để chia sẻ file storage.
* Cấu hình AWS Storage Gateway (File Gateway) để tích hợp hybrid cloud storage.
* Thiết lập AWS Backup plans để quản lý sao lưu tập trung trên các dịch vụ AWS.
* Quét S3 buckets với Amazon Macie để phát hiện và bảo vệ dữ liệu nhạy cảm.
* Tạo và quản lý encryption keys trong AWS KMS để mã hóa dữ liệu khi lưu trữ.
* Hoàn thành workshop về hiệu suất lưu trữ và tối ưu hóa cấu hình storage.
* Cấu hình S3 Cross-Region Replication để khôi phục thảm họa và tuân thủ.

### Ghi chú và Kiến thức Thu được trong Tuần 4:

#### 1. Object Storage với Amazon S3
* **S3 Static Website Hosting:**
    * Kích hoạt lưu trữ website tĩnh trên S3 buckets.
    * Cấu hình index và error documents.
    * Sử dụng CloudFront cho HTTPS và tên miền tùy chỉnh.
    * Triển khai bucket policies cho quyền truy cập đọc công khai.
* **S3 Bucket Policies & Versioning:**
    * Policies dựa trên JSON để kiểm soát truy cập chi tiết.
    * Versioning bảo vệ chống xóa và ghi đè ngẫu nhiên.
    * MFA Delete để bảo vệ bổ sung trên versioned buckets.
    * Kết hợp với IAM policies để bảo mật toàn diện.
* **S3 Lifecycle Rules:**
    * Tự động chuyển đổi giữa các lớp lưu trữ (Standard → IA → Glacier).
    * Thiết lập quy tắc hết hạn để tự động xóa object.
    * Giảm chi phí lưu trữ bằng cách di chuyển dữ liệu ít truy cập.
    * Áp dụng quy tắc cho phiên bản hiện tại và trước đó.

#### 2. Block Storage với Amazon EBS
* **EBS Snapshots & Data Lifecycle Manager:**
    * Sao lưu tăng dần được lưu trữ trong S3.
    * Tự động hóa tạo và giữ lại snapshot với DLM.
    * Sao chép snapshot đa vùng để khôi phục thảm họa.
    * Fast snapshot restore để tạo volume nhanh chóng.
* **EBS Multi-Attach:**
    * Gắn một EBS volume vào nhiều EC2 instances.
    * Chỉ hỗ trợ trên Provisioned IOPS SSD (io1/io2) volumes.
    * Yêu cầu hệ thống file nhận biết cluster (không phải hệ thống file tiêu chuẩn).
    * Trường hợp sử dụng: cơ sở dữ liệu cluster, ứng dụng lưu trữ chia sẻ.
* **Các loại EBS Volume:**
    * **gp3/gp2 (General Purpose SSD):** Cân bằng giá/hiệu suất, boot volumes.
    * **io2/io1 (Provisioned IOPS SSD):** Hiệu suất cao, cơ sở dữ liệu, workloads quan trọng.
    * **st1 (Throughput Optimized HDD):** Big data, data warehouses, xử lý log.
    * **sc1 (Cold HDD):** Dữ liệu ít truy cập, chi phí thấp nhất.

#### 3. File & Hybrid Storage
* **Amazon FSx cho Windows File Server:**
    * Hệ thống file Windows được quản lý đầy đủ với giao thức SMB.
    * Tích hợp Active Directory để xác thực.
    * Triển khai Multi-AZ để có tính khả dụng cao.
    * Hỗ trợ DFS namespaces và replication.
* **Amazon EFS (Elastic File System):**
    * Hệ thống file NFS được quản lý đầy đủ cho Linux workloads.
    * Tự động mở rộng dung lượng lưu trữ.
    * Nhiều EC2 instances có thể truy cập đồng thời.
    * Các lớp lưu trữ: Standard và Infrequent Access.
* **AWS Storage Gateway (File Gateway):**
    * Hybrid cloud storage kết nối on-premises với S3.
    * Trình bày S3 dưới dạng NFS/SMB file shares.
    * Caching cục bộ để truy cập độ trễ thấp.
    * Tự động truyền dữ liệu đến S3 để đảm bảo độ bền.

#### 4. Bảo vệ Dữ liệu & Tuân thủ
* **AWS Backup:**
    * Quản lý sao lưu tập trung trên các dịch vụ AWS.
    * Backup plans với lịch trình và chính sách giữ lại.
    * Sao chép sao lưu đa vùng và đa tài khoản.
    * Backup vault lock để tuân thủ (WORM).
* **Amazon Macie:**
    * Tự động phát hiện dữ liệu nhạy cảm trong S3.
    * Sử dụng machine learning để xác định PII, thông tin xác thực, dữ liệu tài chính.
    * Tạo findings và cảnh báo cho nhóm bảo mật.
    * Tích hợp với Security Hub và EventBridge.
* **AWS KMS (Key Management Service):**
    * Dịch vụ được quản lý để tạo và kiểm soát encryption key.
    * Customer Master Keys (CMKs) để mã hóa dữ liệu.
    * Tự động xoay key để tăng cường bảo mật.
    * Tích hợp với hầu hết các dịch vụ AWS để mã hóa khi lưu trữ.
    * CloudTrail logging để kiểm toán việc sử dụng key.

#### 5. Hiệu suất Lưu trữ & Sao chép
* **Tối ưu hóa Hiệu suất Lưu trữ:**
    * Chọn loại lưu trữ phù hợp dựa trên yêu cầu IOPS và throughput.
    * Sử dụng EBS-optimized instances để có hiệu suất ổn định.
    * Triển khai S3 Transfer Acceleration để tải lên nhanh hơn.
    * Sử dụng multipart upload cho các object lớn.
* **S3 Cross-Region Replication (CRR):**
    * Sao chép tự động, không đồng bộ sang vùng khác.
    * Yêu cầu versioning được kích hoạt trên cả hai buckets.
    * Trường hợp sử dụng: tuân thủ, khôi phục thảm họa, giảm độ trễ.
    * Replication Time Control (RTC) để thời gian sao chép có thể dự đoán.
* **S3 Same-Region Replication (SRR):**
    * Sao chép trong cùng vùng cho các trường hợp sử dụng khác nhau.
    * Tổng hợp log, chủ quyền dữ liệu, môi trường test/dev.
    * Độ trễ thấp hơn so với sao chép đa vùng.
