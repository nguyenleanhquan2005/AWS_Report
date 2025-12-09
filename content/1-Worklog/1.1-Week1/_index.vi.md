---
title: "Nhật ký Tuần 1"
date: "2025-09-09"
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---


### Mục tiêu Tuần 1:

* Kết nối và làm quen với các thành viên First Cloud Journey.
* Hiểu về các dịch vụ AWS cơ bản, cách sử dụng console & CLI.

### Các nhiệm vụ thực hiện trong tuần:
| Ngày | Nhiệm vụ                                                                                                                                                                                                | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo                        |
|:---:|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:----------:|:---------------:|:------------------------------------------|
|  2  | - Làm quen với các thành viên FCJ <br> - Đọc và ghi chú quy định của đơn vị thực tập                                                                                                 | 08/09/2025 |   08/09/2025    |                                           |
|  3  | - Tìm hiểu về AWS và các loại dịch vụ <br>&emsp; + Compute <br>&emsp; + Storage <br>&emsp; + Networking <br>&emsp; + Database <br>&emsp; + ...                                                      | 09/09/2025 |   09/09/2025    | <https://cloudjourney.awsstudygroup.com/> |
|  3  | - Kiến thức cơ bản về IAM: <br>&emsp; + Tạo Users, Groups và Roles <br>&emsp; + Viết IAM Policies với Conditions <br>&emsp; + Thiết lập Permission Boundaries                                                  | 09/09/2025 |   09/09/2025    | [AWS IAM Docs](https://docs.aws.amazon.com/iam/) |
|  4  | - Tạo tài khoản AWS Free Tier <br> - Tìm hiểu về AWS Console & AWS CLI <br> - **Thực hành:** <br>&emsp; + Tạo tài khoản AWS <br>&emsp; + Cài đặt & cấu hình AWS CLI <br> &emsp; + Cách sử dụng AWS CLI | 10/09/2025 |   10/09/2025    | <https://000001.awsstudygroup.com//> |
|  5  | - Quản lý Chi phí: <br>&emsp; + Thiết lập AWS Budgets và Alerts <br>&emsp; + Định nghĩa Cost Allocation Tags <br>&emsp; + Yêu cầu tăng Quota qua Service Quotas                                          | 11/09/2025 |   11/09/2025    | [AWS Cost Management](https://aws.amazon.com/aws-cost-management/) |
|  6  | - Ôn tập & Thực hành: <br>&emsp; + Ôn tập cấu trúc IAM JSON Policy <br>&emsp; + Lab: Cấu hình Cross-Account Access sử dụng Roles                                                                         | 12/09/2025 |   12/09/2025    | <https://cloudjourney.awsstudygroup.com/> |

### Thành tựu Tuần 1:

* Hiểu được AWS là gì và nắm vững các nhóm dịch vụ cơ bản: Compute, Storage, Networking, Database, ...
* Tạo và cấu hình thành công tài khoản AWS Free Tier.
* Làm quen với AWS Management Console và học cách tìm kiếm, truy cập và sử dụng các dịch vụ qua giao diện web.
* Nắm vững kiến thức cơ bản về IAM bao gồm users, groups, roles và tạo policies với conditions.
* Cấu hình thành công permission boundaries để giới hạn quyền tối đa cho các IAM entities.
* Tìm hiểu AWS Single Sign-On (IAM Identity Center) để quản lý danh tính tập trung trên nhiều tài khoản AWS.
* Triển khai chiến lược tagging tài nguyên để tổ chức tốt hơn và phân bổ chi phí.
* Thiết lập AWS Budgets với alerts để giám sát và kiểm soát chi tiêu AWS.
* Định nghĩa cost allocation tags để theo dõi và báo cáo chi phí chi tiết.
* Học cách yêu cầu tăng quota qua Service Quotas để mở rộng tài nguyên.
* Ôn tập cấu trúc và cú pháp IAM JSON policy để viết custom policies.
* Cấu hình thành công cross-account access sử dụng IAM roles để chia sẻ tài nguyên an toàn.

### Ghi chú và Kiến thức Thu được trong Tuần 1:

#### 1. Tổng quan về Cloud Computing
* **Định nghĩa:** Cung cấp tài nguyên IT theo yêu cầu qua internet, thay vì mua và quản lý cơ sở hạ tầng vật lý.
* **Lợi ích chính:**
    * **Trả theo mức sử dụng:** Chỉ trả tiền cho những gì sử dụng, giúp tối ưu chi phí (ví dụ: tắt server vào ban đêm).
    * **Phát triển nhanh:** Các tính năng tự động hóa giúp triển khai ứng dụng nhanh hơn.
    * **Khả năng mở rộng linh hoạt (Elasticity):** Dễ dàng tăng/giảm tài nguyên (CPU, RAM) theo nhu cầu tức thời.
    * **Triển khai toàn cầu:** Tận dụng cơ sở hạ tầng của nhà cung cấp để phục vụ người dùng trên toàn thế giới trong vài phút.

#### 2. Thị trường thực tế và Yêu cầu của Nhà tuyển dụng
* **Thị trường Việt Nam:** Nhu cầu nhân lực cloud rất cao ở cả công ty công nghệ và ngành truyền thống (ngân hàng). Việc áp dụng công nghệ nhanh, không còn chậm so với thế giới.
* **Yêu cầu của Nhà tuyển dụng:**
    * **Kinh nghiệm thực tế:** Chứng chỉ chỉ là điểm khởi đầu. Quan trọng nhất là khả năng xây dựng và khắc phục sự cố độc lập.
    * **Portfolio quan trọng hơn CV:** Dự án cá nhân, blog kỹ thuật, mã nguồn Github có giá trị hơn CV chỉ liệt kê.
    * **Kiến thức nền tảng:** Phải có kiến thức vững về networking và hệ điều hành.
    * **Kỹ năng mềm:** Giao tiếp, làm việc nhóm và tiếng Anh là yếu tố thiết yếu.

#### 3. Phương pháp Học tập Hiệu quả và Sức mạnh Cộng đồng
* **Phương pháp Học tập:**
    * **Hành trình dài:** Cần kiên trì, không thể thành thạo trong vài tháng, phải học liên tục.
    * **Giải quyết vấn đề chủ động:** Phát triển thói quen tự nghiên cứu trước khi hỏi.
    * **Học để "Xây dựng":** Mục tiêu cuối cùng là tạo ra sản phẩm và giải pháp cụ thể, không chỉ làm theo lab có sẵn.
    * **Đừng sợ thất bại và Đầu tư:** Chấp nhận đầu tư thời gian, công sức và chi phí nhỏ để thực hành.
* **Sức mạnh Cộng đồng:**
    * **Môi trường hỗ trợ:** Nơi học hỏi và chia sẻ kiến thức không sợ bị phán xét.
    * **Networking:** Tham dự sự kiện là cơ hội gặp gỡ chuyên gia và nhà tuyển dụng tiềm năng.
