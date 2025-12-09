---
title: "Đề xuất"
date: "2025-12-01"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Hệ thống Hỏi Đáp Tài liệu với AWS Bedrock

## Hệ thống Phân tích Tài liệu Thông minh sử dụng RAG

### 1. Tóm tắt Tổng quan

Hệ thống Document QA là một ứng dụng serverless được thiết kế để cách mạng hóa cách người dùng tương tác với tài liệu. Bằng cách tận dụng **AWS Bedrock** cho Generative AI và công nghệ **RAG (Retrieval Augmented Generation)**, nền tảng này cho phép người dùng tải lên tài liệu PDF/TXT và đặt câu hỏi bằng ngôn ngữ tự nhiên. Hệ thống cung cấp câu trả lời chính xác, nhận biết ngữ cảnh bằng cách truy xuất thông tin liên quan từ các tài liệu đã tải lên, giảm đáng kể thời gian tìm kiếm thủ công và cải thiện khả năng tiếp cận thông tin.

### 2. Phát biểu Vấn đề

### Vấn đề là gì?

Các phương pháp tìm kiếm tài liệu truyền thống (khớp từ khóa) thường không nắm bắt được ngữ cảnh hoặc ý nghĩa ngữ nghĩa. Việc xem xét tài liệu thủ công tốn thời gian, dễ xảy ra lỗi và không hiệu quả, đặc biệt đối với khối lượng văn bản lớn. Người dùng gặp khó khăn trong việc trích xuất thông tin chi tiết cụ thể một cách nhanh chóng, dẫn đến các điểm nghẽn về năng suất.

### Giải pháp

Chúng tôi đề xuất một **Chatbot dựa trên RAG Serverless** sử dụng **AWS Bedrock (Amazon Titan)**. Giải pháp bao gồm:

- **Tải lên & Xử lý**: Người dùng tải tài liệu lên S3; các hàm Lambda kích hoạt trích xuất văn bản và tạo embedding.
- **Tìm kiếm Vector**: Embeddings được lưu trữ và truy vấn để tìm các đoạn tài liệu liên quan.
- **Generative AI**: AWS Bedrock tạo ra các phản hồi ngôn ngữ tự nhiên dựa trên ngữ cảnh đã truy xuất.
- **Kiến trúc Serverless**: Được xây dựng trên AWS Lambda, API Gateway và DynamoDB để tự động mở rộng và hiệu quả chi phí.

### Lợi ích và Lợi tức Đầu tư

- **Hiệu quả**: Giảm thời gian phân tích tài liệu từ hàng giờ xuống còn vài giây.
- **Độ chính xác**: RAG đảm bảo câu trả lời được dựa trên tài liệu được cung cấp, giảm thiểu ảo giác (hallucinations).
- **Tiết kiệm Chi phí**: Mô hình serverless trả theo mức sử dụng (ước tính < $5/tháng cho mức sử dụng thấp).
- **Khả năng Mở rộng**: Tự động xử lý các tải khác nhau mà không cần quản lý cơ sở hạ tầng thủ công.

### 3. Kiến trúc Giải pháp

Nền tảng sử dụng kiến trúc serverless hiện đại để đảm bảo khả năng mở rộng, bảo mật và hiệu suất.

![Architecture](/images/image_proposal.png)

### Các Dịch vụ AWS Sử dụng

- **AWS Bedrock**: Cung cấp các Foundation Models (Amazon Titan) cho embeddings và tạo văn bản.
- **AWS Lambda**: Serverless compute để xử lý các yêu cầu API, xử lý tài liệu và điều phối.
- **Amazon API Gateway**: Quản lý các REST API endpoints cho frontend.
- **Amazon S3**: Lưu trữ tài liệu thô đã tải lên và các tài nguyên tĩnh của frontend.
- **Amazon DynamoDB**: Quản lý phiên người dùng và lịch sử trò chuyện.
- **Vector Store**: (Được triển khai qua Lambda/Local hoặc vector DB chuyên dụng) Lưu trữ document embeddings cho tìm kiếm ngữ nghĩa.

### Thiết kế Thành phần

- **Frontend**: Được lưu trữ trên S3 (hoặc Amplify), cung cấp giao diện trò chuyện thân thiện với người dùng.
- **API Layer**: API Gateway định tuyến các yêu cầu (`/upload`, `/ask`) đến các hàm Lambda.
- **Processing Layer**: Lambda xử lý trích xuất văn bản, gọi Bedrock để tạo embeddings và thực hiện tìm kiếm tương đồng vector.
- **AI Layer**: AWS Bedrock tạo ra các phản hồi sử dụng ngữ cảnh đã truy xuất và truy vấn của người dùng.

### 4. Triển khai Kỹ thuật

**Các Giai đoạn Triển khai**

- **Giai đoạn 1: Nền tảng (Tuần 1-4)**: Thiết lập môi trường AWS, quyền truy cập Bedrock và logic backend cơ bản.
- **Giai đoạn 2: API & Bảo mật (Tuần 5-7)**: Phát triển API Gateway, các hàm Lambda và triển khai CORS/Security.
- **Giai đoạn 3: Phát triển Frontend (Tuần 8-11)**: Xây dựng giao diện React/Next.js và tích hợp với APIs.
- **Giai đoạn 4: Kiểm thử & Triển khai (Tuần 12-14)**: Kiểm thử end-to-end, tối ưu hóa và triển khai cuối cùng.

**Yêu cầu Kỹ thuật**

- **AI Model**: Amazon Titan (qua Bedrock) cho Embeddings và Text Generation.
- **Backend**: Node.js/Python trên AWS Lambda.
- **Infrastructure as Code**: Serverless Framework hoặc AWS CDK.
- **Frontend**: React.js / Next.js.

### 5. Thời gian & Các Mốc quan trọng

- **Tháng 1**: Thiết kế Kiến trúc, Thiết lập AWS, Backend Core (Upload/Embeddings).
- **Tháng 2**: Triển khai RAG, Logic Tìm kiếm Vector, Phát triển API.
- **Tháng 3**: Tích hợp Frontend, Hoàn thiện UI/UX, Kiểm thử và Ra mắt.

### 6. Ước tính Ngân sách

**Chi phí Hàng tháng Ước tính (Mức Sử dụng Thấp-Trung bình)**

- **AWS Bedrock (Titan)**: ~$0 (Free Tier / Chi phí thấp mỗi 1k tokens)
- **AWS Lambda**: ~$0.20 mỗi 1M requests
- **Amazon S3**: ~$0.023 mỗi GB
- **Amazon DynamoDB**: ~$0.25 mỗi 1M requests
- **Amazon API Gateway**: ~$3.50 mỗi 1M requests

**Tổng Ước tính**: < **$5.00 / tháng**

### 7. Đánh giá Rủi ro

#### Ma trận Rủi ro

- **Hallucinations (Lỗi AI)**: Tác động Trung bình, Xác suất Trung bình.
- **Vượt Chi phí**: Tác động Trung bình, Xác suất Thấp (Serverless).
- **Rò rỉ Dữ liệu**: Tác động Cao, Xác suất Thấp.

#### Chiến lược Giảm thiểu

- **Hallucinations**: Triển khai RAG nghiêm ngặt (dựa câu trả lời vào ngữ cảnh).
- **Chi phí**: Thiết lập AWS Budget Alerts và hạn ngạch sử dụng.
- **Bảo mật**: Sử dụng Presigned URLs cho S3, IAM roles với quyền tối thiểu.

### 8. Kết quả Mong đợi

#### Cải tiến Kỹ thuật

- Pipeline phân tích tài liệu tự động hoàn toàn.
- Độ trễ truy xuất dưới một giây cho tìm kiếm vector.
- Kiến trúc có khả năng mở rộng hỗ trợ người dùng đồng thời.

#### Giá trị Dài hạn

- Một framework RAG có thể tái sử dụng cho các ứng dụng knowledge base trong tương lai.
- Tăng năng suất đáng kể cho người dùng cần truy xuất thông tin nhanh chóng.

### 9. Cấu trúc Nhóm và Trách nhiệm

| Tên | Mã sinh viên | Vai trò Chính | Email/Thông tin Liên hệ |
|-----|-------------|---------------|-------------------------|
| **Nguyễn Lê Anh Quân** | SE192307 | Trưởng nhóm/ Cloud Architect | nguyenleanhquan2005@gmail.com |
| **Đào Quang Vinh** | SE180012 | Project Manager/ Backend Developer (Bedrock, RAG) | its.vnhdq@gmail.com |
| **Nguyễn Thanh Liêm** | SE184163 | Backend Developer | liemntse184163@fpt.edu.vn |
| **Trần Đình Phong** | SE184217 | Frontend Developer/ UI/UX Designer | phongtdse184217@fpt.edu.vn |
| **Dương Nguyễn Gia Huy** | SE182202 | QA Engineer/Backend Developer (Bedrock, RAG) | huydngse182202@fpt.edu.vn |

#### Trách nhiệm Chi tiết theo Thành viên Nhóm

##### Nguyễn Lê Anh Quân - Cloud Architect/ Trưởng nhóm

**Trách nhiệm Chính:**
- Thiết kế kiến trúc AWS và lựa chọn dịch vụ
- Lập kế hoạch và tối ưu hóa cơ sở hạ tầng
- Kiến trúc bảo mật và IAM policies
- Tư vấn kỹ thuật và best practices

---

##### Đào Quang Vinh - Project Manager/Backend Developer

**Trách nhiệm Chính:**
- Quản lý dự án tổng thể và điều phối thời gian
- Điều phối nhóm và phân công nhiệm vụ
- Báo cáo tiến độ cho giảng viên/cố vấn
- Quản lý rủi ro và chiến lược giảm thiểu
- Giám sát tài liệu và đảm bảo chất lượng
- Xây dựng logic tìm kiếm và truy xuất vector
- Phát triển hàm Lambda xử lý chat/query

##### Dương Nguyễn Gia Huy - QA Engineer/Backend Developer

**Trách nhiệm Chính:**
- Phát triển logic backend cốt lõi
- Tích hợp Amazon Bedrock (Foundation Models)
- Triển khai pipeline RAG (Retrieval-Augmented Generation)
- Phát triển hàm Lambda cho document ingestion
- Tích hợp Amazon Bedrock Knowledge Bases
- Triển khai text chunking và tạo embedding

---

##### Nguyễn Thanh Liêm - Backend Developer

**Trách nhiệm Chính:**
- Cơ sở hạ tầng backend và quản lý dữ liệu
- Phát triển CI/CD pipeline
- Giám sát hệ thống và logging
- Tối ưu hóa hiệu suất
- Thiết kế và triển khai DynamoDB schema
- Logic lưu trữ lịch sử hội thoại

---

##### Trần Đình Phong - Frontend Developer

**Trách nhiệm Chính:**
- Thiết kế và phát triển giao diện người dùng
- Tích hợp frontend-backend
- Tối ưu hóa trải nghiệm người dùng
- Triển khai responsive design
- Triển khai giao diện tải file với drag-and-drop
- Kết nối frontend với API Gateway endpoints
- Xử lý API responses và error states
- Triển khai frontend lên S3 + CloudFront

---
