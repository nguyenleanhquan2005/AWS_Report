---
title: "Blog 1"
date: "2025-09-09"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---
## [Artificial Intelligence](https://aws.amazon.com/blogs/machine-learning/)

# Tạo prompt chính xác với Dịch vụ Hình ảnh của Stability AI trên Amazon Bedrock

Bởi Suleman Patel, Isha Dua, Fabio Branco, and Maxfield Hulker vào ngày 18 tháng 9 năm 2025 trong các mục [Amazon Bedrock](https://aws.amazon.com/blogs/machine-learning/category/artificial-intelligence/amazon-machine-learning/amazon-bedrock/), [Amazon SageMaker](https://aws.amazon.com/blogs/machine-learning/category/artificial-intelligence/sagemaker/), [Announcements](https://aws.amazon.com/blogs/machine-learning/category/post-types/announcements/), [Artificial Intelligence](https://aws.amazon.com/blogs/machine-learning/category/artificial-intelligence/), [Foundation models](https://aws.amazon.com/blogs/machine-learning/category/artificial-intelligence/generative-ai/foundation-models/), [Generative AI](https://aws.amazon.com/blogs/machine-learning/category/artificial-intelligence/generative-ai/), [Launch](https://aws.amazon.com/blogs/machine-learning/category/news/launch/) [Permalink](https://aws.amazon.com/blogs/machine-learning/prompting-for-precision-with-stability-ai-image-services-in-amazon-bedrock/)  [Comments](https://aws.amazon.com/blogs/machine-learning/prompting-for-precision-with-stability-ai-image-services-in-amazon-bedrock/#Comments)  [Share](https://aws.amazon.com/blogs/machine-learning/prompting-for-precision-with-stability-ai-image-services-in-amazon-bedrock/#)

[Amazon Bedrock](https://aws.amazon.com/bedrock/) hiện đang cung cấp các Dịch vụ Hình ảnh của Stability AI: 9 công cụ giúp các doanh nghiệp cải thiện cách tạo và chỉnh sửa hình ảnh. Công nghệ này mở rộng các mô hình Stable Diffusion và Stable Image để cho bạn quyền kiểm soát chính xác việc tạo và chỉnh sửa hình ảnh. Các prompt rõ ràng là rất quan trọng—chúng cung cấp định hướng nghệ thuật cho hệ thống AI. Các prompt mạnh mẽ kiểm soát các yếu tố cụ thể như tông màu, kết cấu, ánh sáng và bố cục để tạo ra kết quả hình ảnh mong muốn. Khả năng này phục vụ các nhu cầu chuyên nghiệp trong lĩnh vực nhiếp ảnh sản phẩm, ý tưởng và các chiến dịch marketing. 

Trong bài viết này, chúng tôi sẽ mở rộng bài viết "[Hiểu về kỹ thuật prompt: Mở khóa tiềm năng sáng tạo của các mô hình Stability AI trên AWS](https://aws.amazon.com/blogs/machine-learning/understanding-prompt-engineering-unlock-the-creative-potential-of-stability-ai-models-on-aws/)". Chúng tôi sẽ chỉ ra cách sử dụng hiệu quả các kỹ thuật prompting nâng cao để tối đa hóa chất lượng và độ chính xác khi tạo hình ảnh cho các ứng dụng doanh nghiệp bằng Dịch vụ Hình ảnh của Stability AI trên Amazon Bedrock.

## **Tổng quan giải pháp**

Các Dịch vụ Hình ảnh của Stability AI có sẵn dưới dạng API trong Amazon Bedrock, bao gồm các khả năng như in-painting, chuyển đổi phong cách, đổi màu, xóa nền, xóa đối tượng, style guide, và nhiều hơn nữa.

Trong các phần sau, trước tiên chúng tôi sẽ thảo luận về cấu trúc prompt để kiểm soát tối đa việc tạo hình ảnh, sau đó chúng tôi sẽ cung cấp các kỹ thuật prompting nâng cao để định hướng phong cách. Các mẫu mã nguồn có thể được tìm thấy trong kho [GitHub repository](https://github.com/aws-samples/stabilityai-sample-notebooks/tree/main/stability-ai-image-services) sau.

## **Điều kiện tiên quyết**

Để bắt đầu với Dịch vụ Hình ảnh của Stability AI trong Amazon Bedrock, hãy làm theo hướng dẫn trong "Bắt đầu với API" để hoàn thành các điều kiện tiên quyết sau:

1. Thiết lập tài khoản AWS của bạn.  
2. Nhận thông tin xác thực để cấp quyền truy cập theo chương trình.  
3. Gắn quyền Amazon Bedrock cho một người dùng hoặc vai trò [AWS Identity and Access Management](https://aws.amazon.com/iam/) (IAM).  
4. Yêu cầu quyền truy cập vào các mô hình Amazon Bedrock. 

## **Cấu trúc prompt để tối đa hóa quyền kiểm soát**

Để tối đa hóa các khả năng chi tiết của Dịch vụ Hình ảnh của Stability AI trong Amazon Bedrock, bạn phải xây dựng các prompt cho phép kiểm soát một cách tinh vi. 

Phần này nêu ra các phương pháp tốt nhất để xây dựng các prompt hiệu quả nhằm tạo ra kết quả mong muốn. Chúng tôi sẽ minh họa cách cấu trúc prompt ảnh hưởng đến kết quả và tại sao các prompt chi tiết hơn thường mang lại kết quả nhất quán và dễ kiểm soát hơn.

**Chọn đúng loại prompt cho trường hợp sử dụng của bạn**

Việc chọn đúng định dạng prompt giúp mô hình hiểu rõ hơn ý định của bạn. Ba định dạng prompt chính mang lại các mức độ kiểm soát và khả năng đọc khác nhau:

* **Ngôn ngữ tự nhiên** tối đa hóa khả năng đọc và phù hợp nhất cho việc sử dụng thông thường.  
* **Định dạng dựa trên thẻ (tag-based)** cho phép kiểm soát cấu trúc chính xác và lý tưởng cho các ứng dụng kỹ thuật.  
* **Định dạng lai (hybrid)** kết hợp ngôn ngữ tự nhiên và các yếu tố cấu trúc của thẻ để cung cấp nhiều quyền kiểm soát hơn nữa. 

Bảng sau đây cung cấp các ví dụ về ba cách phổ biến để diễn đạt prompt của bạn. Mỗi định dạng prompt đều có thế mạnh riêng tùy thuộc vào mục tiêu hoặc giao diện bạn đang sử dụng. .

| Loại Prompt  | Prompt ví dụ | Hình ảnh được tạo bằng Stable Image Ultra trong Amazon Bedrock | Description and use case |
| :---- | :---- | :---- | :---- |
| Prompt cơ bản (Natural Language) | “Một bức ảnh đơn giản về chai nước hoa được đặt trên bàn cẩm thạch” | ![Your profile picture](/images/image1.png) | Dễ đọc và trực quan. Tuyệt vời để khám phá, các công cụ đàm thoại, và một số loại mô hình. Stable Diffusion 3.5 phản hồi tốt nhất với phong cách này. |
| Tag-Based Prompt | “chai nước hoa, bề mặt đá cẩm thạch, ánh sáng dịu, chất lượng cao, ảnh sản phẩm” | ![][image2] | Được sử dụng trong nhiều giao diện người dùng tạo hình(Generation UIs) hoặc với các mô hình được huấn luyện trên các bộ dữ liệu như LAION hoặc Danbooru. Gọn nhẹ và tốt cho việc xếp chồng các chi tiết. |
| Hybrid Prompt | “chai nước hoa trên bàn đá cẩm thạch, ánh sáng studio dịu, lấy nét sắc, khẩu độ f/2.8” | ![][image3] | Kết hợp tốt nhất của cả hai phương pháp. Thêm sự nhấn mạnh bằng cú pháp trọng số để ảnh hưởng đến các ưu tiên của mô hình. |

### **Xây dựng prompt theo module**

Prompt theo module nâng cao hiệu quả tạo hình AI. Cách tiếp cận này chia các prompt thành các thành phần riêng biệt, mỗi thành phần chỉ rõ cần vẽ gì và đối tượng đó nên xuất hiện như thế nào. Cấu trúc module mang lại một số lợi ích nhất định: chúng giúp ngăn chặn các hướng dẫn mâu thuẫn hoặc khó hiểu, cho phép kiểm soát đầu ra chính xác, và đơn giản hóa việc gỡ lỗi prompt. Bằng cách cô lập các yếu tố riêng lẻ, bạn có thể nhanh chóng xác định và điều chỉnh các phần hiệu quả hoặc không hiệu quả của prompt. Phương pháp này cuối cùng dẫn đến các hình ảnh do AI tạo ra tinh tế và có chủ đích hơn.

Bảng dưới đây cung cấp các ví dụ về các mô-đun prompt. Hãy thử nghiệm với các trình tự prompt khác nhau để đạt được kết quả mong muốn; ví dụ, việc đặt phần phong cách trước chủ đề sẽ cho nó một "trọng lượng hình ảnh"(visual weight) lớn hơn

Xin lỗi vì sự thiếu sót trước đó. Dưới đây là bảng đã được cập nhật với cột "Ví dụ" và các ví dụ phù hợp cho từng hạng mục:

| Module | Ví dụ | Mô tả |
| :---- | :---- | :---- |
| Tiền tố | “Chân dung biên tập thời trang của” | Thiết lập tông màu và mục đích cho một bức chân dung được tạo kiểu theo phong cách thời trang cao cấp |
| Chủ thể	 | “người phụ nữ có làn da nâu trung bình và mái tóc xoăn ngắn” | Cung cấp đặc điểm ngoại hình và chi tiết bề mặt của người mẫu để hướng dẫn tạo ra các đường nét khuôn mặt. |
| Bổ ngữ | “mặc áo lưới đen bất đối xứng, trang sức kim loại” | Thêm quần áo và phụ kiện được tạo kiểu để tăng sự thu hút về mặt thị giác. |
| Hành động | “ngồi với vai xoay góc, mắt nhìn thẳng vào máy ảnh, một cánh tay đưa lên” | Mô tả ngôn ngữ cơ thể và dáng đứng để tạo ra một bố cục năng động. |
| Môi trường | “những chùm ánh sáng định hướng mạnh giao nhau qua các thanh cửa sổ” | Thêm ngữ cảnh cho sự tương tác ánh sáng kịch tính và bầu không khí. |
| Phong cách | “ánh sáng chiaroscuro tương phản cao, điêu khắc và trừu tượng” | Thông báo về thẩm mỹ và tâm trạng (thiên về bóng tối, u buồn, kiến trúc) |
| Máy ảnh/Ánh sáng	 | “chụp bằng ống kính 85mm, bố cục trong studio, các lớp bóng đổ và ánh sáng phủ lên mặt và cơ thể” | Thêm độ chính xác kỹ thuật và giúp kiểm soát mức độ chân thực và sự sắc nét. |

Ví dụ sau minh họa cách sử dụng một prompt module để tạo ra kết quả mong muốn

| Modular Prompt | Hình ảnh được tạo bằng Stable Image Ultra trong Amazon Bedrock |
| :---- | :---- |
| “Một bức chân dung thời trang cao cấp về một người phụ nữ có làn da nâu trung bình và mái tóc xoăn ngắn. Cô mặc một chiếc áo lưới đen bất đối xứng, điểm xuyến những món trang sức kim loại. Trong tư thế ngồi với vai xoay góc và một cánh tay nâng lên, ánh mắt cô hướng thẳng vào ống kính. Toàn bộ khung hình được “tô” thêm những tia sáng định hướng mạnh mẽ và giao nhau xuyên qua các khe cửa, tạo nên những lớp bóng đổ và điểm sáng như đang điêu khắc lên khuôn mặt và cơ thể. Phong cách ánh sáng chiaroscuro tương phản cao, táo bạo và trừu tượng, được thực hiện với ống kính 85mm trong studio. ” | ![][image4] |

### **Sử dụng prompt phủ định để có kết quả tinh chỉnh**

Prompt phủ định giúp nâng cao chất lượng đầu ra của AI bằng cách loại bỏ những thành phần hình ảnh không mong muốn. Việc xác định rõ ràng những gì không nên xuất hiện trong prompt sẽ định hướng đầu ra của mô hình, từ đó thường tạo ra những kết quả chuyên nghiệp hơn. Prompt phủ định hoạt động giống như một danh sách kiểm tra của người chỉnh sửa ảnh, dùng để xử lý các khía cạnh của hình ảnh nhằm nâng cao chất lượng và sức hấp dẫn. Ví dụ: "Không có bàn tay dị dạng. Không có góc mờ. Không có hiệu ứng hoạt hình. Chắc chắn không có watermark." Prompt phủ định giúp tạo ra những bố cục sạch sẽ, rõ ràng và không chứa các yếu tố gây mất tập trung hay biến dạng.

Bảng dưới đây cung cấp các ví dụ về những từ khóa bổ sung có thể sử dụng trong prompt phủ định.

| Artifact Type | Tokens to Use |
| :---- | :---- |
| Chất lượng thấp hoặc nhiễu | mờ, độ phân giải thấp, lỗi nén JPEG, nhiễu |
| Vấn đề về giải phẫu hoặc mô hình | biến dạng, thừa chi, tay xấu, thiếu ngón tay |
| Xung đột phong cách | hoạt hình, minh họa, anime, tranh vẽ |
| Lỗi kỹ thuật | watermark, văn bản, chữ ký, phơi sáng quá mức |
| Dọn dẹp chung | xấu xí, vẽ kém, biến dạng, chất lượng tệ nhất |

Ví dụ sau minh họa cách một prompt phủ định được cấu trúc tốt có thể nâng cao tính chân thực của ảnh.

| Loại | Mô tả | Hình ảnh |
| :--- | :--- | :--- |
| Không Prompt phủ định | Prompt “một buồng làm việc bằng kính đầy lôi cuốn, với nhiều màu sắc và đường viền vàng ánh kim đã ngả màu thời gian. Phong cách hiện đại, tiết kiệm không gian, ghế ngồi được bọc nệm, được đặt trong một khu vườn đương đại. Nội thất tinh tế, trang trí phong cách, ánh sáng rực rỡ, chỗ ngồi thoải mái. Kiệt tác, chất lượng tốt nhất, ảnh thô, chân thực, rất thẩm mỹ, tông màu tối“ | ![][image5] |
| Với Prompt phủ định | Prompt “một buồng làm việc bằng kính đầy lôi cuốn, với nhiều màu sắc và đường viền vàng ánh kim đã ngả màu thời gian. Phong cách hiện đại, tiết kiệm không gian, ghế ngồi được bọc nệm, được đặt trong một khu vườn đương đại. Nội thất tinh tế, trang trí phong cách, ánh sáng rực rỡ, chỗ ngồi thoải mái. Kiệt tác, chất lượng tốt nhất, ảnh thô, chân thực, rất thẩm mỹ, tông màu tối” Negative Prompt “hoạt hình, render 3D, đồ họa máy tính, màu sắc bão hòa cao, bề mặt nhựa mịn, ánh sáng không thực, nhân tạo, hậu cảnh ít chi tiết” | ![][image6] |

### **Nhấn mạnh hoặc giảm thiểu các thành phần bằng prompt weighting**

Trọng số Prompt (Prompt Weighting) là kỹ thuật kiểm soát mức độ ảnh hưởng của từng thành phần riêng lẻ trong quá trình tạo ảnh AI. Các trọng số bằng số này giúp ưu tiên những phần cụ thể trong prompt hơn những phần khác. Ví dụ, để nhấn mạnh nhân vật so với phông nền, bạn có thể áp dụng trọng số 1.8 cho "nhân vật" (character:1.8) và 1.1 cho "phông nền" (background:1.1). Điều này đảm bảo mô hình ưu tiên chi tiết cho nhân vật trong khi vẫn duy trì ngữ cảnh môi trường. Sự nhấn mạnh có mục tiêu này tạo ra đầu ra chính xác hơn bằng cách giảm thiểu sự cạnh tranh giữa các thành phần trong prompt và làm rõ các ưu tiên của mô hình.

Cú pháp cho trọng số prompt là (\<từ khóa\>:\<trọng số\>). Bạn cũng có thể sử dụng cách viết tắt như ((\<từ khóa\>)), trong đó số lượng cặp dấu ngoặc đơn thể hiện mức trọng số. Các giá trị từ 0.0 đến 1.0 làm giảm tầm quan trọng của từ khóa. Các giá trị từ 1.1 đến 2.0 làm tăng tầm quan trọng của từ khóa.

Ví dụ:

* (từ khóa:1.2): Nhấn mạnh  
* (từ khóa:0.8): Giảm nhấn mạnh  
* ((từ khóa)): Cách viết tắt cho (từ khóa:1.2)  
* (((((((((từ khóa))))))))): Cách viết tắt cho (từ khóa:1.8)

Ví dụ dưới đây minh họa cách trọng số prompt đóng góp vào kết quả đầu ra được tạo ra.
| :---- | :---- |
| Prompt với trọng số “Ảnh sản phẩm biên tập của (một lọ kem dưỡng ẩm dạng gel trong mờ:1.4) đặt trên (bệ kính mờ:1.2), được bao quanh bởi (những cánh hoa hồng đẫm sương:1.1), với (ánh sáng khuếch tán:1.3) dịu nhẹ, những giọt nước li ti, độ sâu trường ảnh nông.” | ![][image7] |
| Prompt without weights “Ảnh sản phẩm biên tập của một lọ kem dưỡng ẩm dạng gel trong mờ đặt trên bệ kính mờ, được bao quanh bởi những cánh hoa hồng đẫm sương, với ánh sáng dịu nhẹ, những giọt nước li ti, độ sâu trường ảnh nông.” | ![][image8] |

Bạn cũng có thể sử dụng trọng số trong prompt phủ định để giảm mức độ "né tránh" của mô hình với một yếu tố cụ thể. Ví dụ: “(chữ:0.5), (mờ:0.2), (độ phân giải thấp:0.1).” Điều này ra lệnh cho mô hình phải đặc biệt chắc chắn trong việc tránh tạo ra nội dung chữ viết bị mờ hoặc có độ phân giải thấp

## **Đưa ra định hướng phong cách cụ thể**

 Để viết prompt hiệu quả khi sử dụng các Dịch vụ Hình ảnh Stability AI như Chuyển đổi Phong cách ([Style Transfer](https://platform.stability.ai/docs/api-reference#tag/Control/paths/~1v2beta~1stable-image~1control~1style-transfer/post)) và Hướng dẫn Phong cách ([Style Guide](https://platform.stability.ai/docs/api-reference#tag/Control/paths/~1v2beta~1stable-image~1control~1style/post)) đòi hỏi hiểu biết tốt về việc kết hợp phong cách và kỹ thuật prompt dựa trên tham chiếu. Những kỹ thuật này giúp cung cấp chỉ dẫn phong cách rõ ràng cho cả hai quy trình tạo ảnh từ văn bản và từ ảnh mẫu.

 Chuyển đổi phong cách từ ảnh (Image-to-image style transfer) trích xuất các yếu tố phong cách từ một ảnh đầu vào (ảnh kiểm soát) và sử dụng chúng để định hướng việc tạo ra ảnh đầu ra dựa trên prompt. Hãy tiếp cận việc viết prompt như thể bạn đang chỉ đạo một nhiếp ảnh gia hoặc nhà tạo mẫu chuyên nghiệp. Tập trung vào chất liệu, chất lượng ánh sáng và ý đồ nghệ thuật—không chỉ đơn thuần là các đối tượng. Ví dụ, một prompt được xây dựng tốt có thể là: "Ảnh biên tập cận cảnh một tuýp son bóng màu xanh lục trong suốt đặt trên nền nhựa dập vỡ óng ánh, ánh sáng màu khuếch tán, DOF nông, tạo dáng sản phẩm thời trang cao cấp.

### **Phân lớp thẻ phong cách: Các nhãn thẩm mỹ quen thuộc phù hợp với bản sắc thương hiệu**

Nghệ thuật tạo ra các prompt hiệu quả thường dựa vào việc kết hợp các thẻ phong cách(style tags) đã được thiết lập, phù hợp với ngôn ngữ hình ảnh và bộ dữ liệu quen thuộc. Bằng cách kết hợp một cách chiến lược các thuật ngữ từ các danh mục thẩm mỹ được công nhận (từ nhiếp ảnh biên tập và phim analog đến anime, cảnh quan thành phố cyberpunk và cấu trúc brutalist), người sáng tạo có thể hướng AI đến các kết quả hình ảnh cụ thể phù hợp với bản sắc thương hiệu của họ. Các mô tả phong cách này đóng vai trò là các neo mạnh mẽ trong quá trình kỹ thuật prompt. Tính linh hoạt của các thẻ này còn mở rộng hơn nữa thông qua khả năng kết hợp và trọng số, cho phép kiểm soát sắc thái đối với tính thẩm mỹ cuối cùng. Ví dụ, một thương hiệu chăm sóc da có thể kết hợp các đường nét sạch sẽ của nhiếp ảnh sản phẩm với các yếu tố siêu thực, mơ màng, trong khi một công ty công nghệ có thể kết hợp cấu trúc brutalist với các yếu tố cyberpunk để có một bản sắc hình ảnh đặc biệt. Cách tiếp cận pha trộn phong cách này giúp người sáng tạo cải thiện đầu ra của họ trong khi vẫn duy trì mối liên hệ rõ ràng với các thể loại hình ảnh dễ nhận biết, phù hợp với đối tượng mục tiêu của họ. Chìa khóa là hiểu cách các thẻ phong cách này tương tác và sử dụng sự kết hợp của chúng để tạo ra các biểu hiện hình ảnh độc đáo, nhưng có liên quan về mặt văn hóa, phục vụ các mục tiêu sáng tạo hoặc thương mại cụ thể. Bảng sau cung cấp các ví dụ về prompt cho một phong cách thẩm mỹ mong muốn:

| Phong cách thẩm mỹ mong muốn | Cụm từ gợi ý (Prompt phrases) | Trường hợp sử dụng ví dụ |
| ----- | ----- | ----- |
| Retro / Y2K | hoài niệm những năm 2000, chụp ảnh bằng đèn flash, tông màu kẹo ngọt, ánh sáng gắt | Kết cấu kim loại, phông chữ mảnh, mang cảm giác kỹ thuật số thời kỳ đầu. |
| Clean modern (Hiện đại tinh gọn) | tông màu trung tính, chuyển sắc nhẹ, phong cách tối giản, bố cục như tạp chí | Rất phù hợp cho các sản phẩm chăm sóc sức khỏe hoặc chăm sóc da. |
| Bold streetwear (Thời trang đường phố mạnh mẽ) | bối cảnh đô thị, trang phục rộng rãi, tư thế mạnh mẽ, bóng đổ giữa trưa | Phù hợp cho nhiếp ảnh thời trang hoặc quảng cáo phong cách sống. Nên nhấn mạnh vào cấu trúc trang phục và vị trí tạo dáng. |
| Hyperreal surrealism (Siêu thực – hiện thực hóa) | ánh sáng phong cách dreamcore, chất liệu bóng, độ sâu trường ảnh điện ảnh, bóng đổ siêu thực | Rất phù hợp cho các chiến dịch âm nhạc, thời trang hoặc văn hóa nghệ thuật thay thế. |

### **Gọi một phong cách đã đặt tên làm tham chiếu**

Một số cấu trúc prompt sẽ hiệu quả hơn nếu bạn “gọi” (invoke) một chữ ký thị giác (visual signature) của một nghệ sĩ cụ thể, đặc biệt khi kết hợp với cách diễn đạt hoặc quy trình phong cách riêng của bạn, như trong ví dụ sau.

| Prompt “chân dung studio biên tập của một người phụ nữ với làn da sáng mịn trong lớp trang điểm glam tối giản, ánh sáng tương phản cao, nền sạch, (mô tả phong cách Van Gogh:1.3)” | ![][image9] |
| :---- | :---- |

Sau đây là một ví dụ mang tính khái niệm hơn.

| Prompt “ảnh sản phẩm một chai dầu dưỡng tóc màu bạc với những phản chiếu mềm mại trên chrome cong, (mô tả phong cách Wes Anderson:1.2), dưới ánh sáng studio lạnh” | ![][image10] |
| :---- | :---- |

Những cụm từ này hoạt động như việc gọi một thể loại; chúng ngụ ý các lựa chọn về vật liệu, ánh sáng, bố cục và tông màu

### **Sử dụng hình ảnh tham chiếu để định hướng phong cách**

Một kỹ thuật hữu ích khác là sử dụng hình ảnh tham chiếu (reference image) để định hướng tư thế, màu sắc hoặc bố cục của đầu ra. Ví dụ, bạn có thể dùng hình ảnh từ lookbook để bắt chước tư thế, lấy bảng màu từ một khung hình trong chiến dịch quảng cáo, hoặc sao chép cách ánh sáng và bóng đổ từ một buổi chụp ảnh — tất cả đều dựa trên việc trích xuất và áp dụng cấu trúc hoặc phong cách từ hình ảnh tham chiếu.

Stability AI Image Services hỗ trợ nhiều quy trình image-to-image (chuyển đổi từ hình ảnh sang hình ảnh), trong đó bạn có thể dùng hình ảnh tham chiếu (còn gọi là control image) để định hướng kết quả đầu ra — chẳng hạn như  [Structure](https://platform.stability.ai/docs/api-reference#tag/Control/paths/~1v2beta~1stable-image~1control~1structure/post)(cấu trúc), [Sketch](https://platform.stability.ai/docs/api-reference#tag/Control/paths/~1v2beta~1stable-image~1control~1sketch/post)(phác thảo), và [Style](https://platform.stability.ai/docs/api-reference#tag/Control/paths/~1v2beta~1stable-image~1control~1style/post)(phong cách). Các công cụ như [ControlNet](https://stability.ai/news/sd3-5-large-controlnets)(một kiến trúc mạng nơ-ron do Stability AI phát triển giúp tăng khả năng kiểm soát), [IP-Adapter](https://github.com/tencent-ailab/IP-Adapter) (bộ chuyển đổi dùng hình ảnh làm prompt), hoặc clip-based captioning (chú thích dựa trên mô hình CLIP) cũng cho phép điều khiển linh hoạt hơn khi được kết hợp với các mô hình của Stability AI.

Chúng tôi sẽ thảo luận chi tiết hơn về ControlNet, IP-Adapter, và clip-based captioning trong bài viết tiếp theo.

Dưới đây là một ví dụ về quy trình image-to-image:

1. Tìm một hình ảnh tham chiếu chất lượng cao — chẳng hạn như ảnh editorial (ảnh thời trang, quảng cáo, hoặc nghệ thuật).  
2. Sử dụng hình ảnh đó cùng với ControlNet (dạng depth, canny, hoặc seg) để cố định tư thế của chủ thể.  
3. Tạo phong cách bằng prompt — thêm mô tả hoặc hướng dẫn để định hình phong cách, màu sắc, hay không khí của hình ảnh đầu ra

| Prompt “Ảnh thời trang (fashion editorial) của một người mẫu mặc đồ len nhiều lớp (layered knitwear), với ánh sáng màu sắc kịch tính, bóng đổ mạnh, và hiệu ứng hạt ảnh (texture) ISO cao” | ![][image11] |
| :---- | :---- |

### **Tạo bầu không khí phù hợp bằng cách kiểm soát ánh sáng**

Trong một prompt, ánh sáng giúp thiết lập tông cảm xúc, tăng chiều sâu không gian, và mô phỏng ngôn ngữ nhiếp ảnh. Ánh sáng không nên chỉ được hiểu đơn giản là “sáng hay tối”. Ánh sáng thường chính là phong cách của hình ảnh — đặc biệt với các khán giả trẻ như Gen Z, những người ưa chuộng các phong cách như ánh flash kiểu đầu những năm 2000, ánh sáng nền gắt (harsh backlight), hay bộ lọc màu (color gels) phổ biến trên TikTok. Bảng sau đây cung cấp một số từ khóa gợi ý về phong cách ánh sáng hữu ích cho prompt.

| Phong cách ánh sáng | Từ khóa gợi ý  | Ví dụ về use case |
| ----- | ----- | ----- |
| Studio tương phản cao | ánh sáng hướng mạnh, bóng đổ sâu, vùng sáng được kiểm soát | Chụp ảnh làm đẹp, công nghệ, thời trang với hình ảnh mạnh mẽ, ấn tượng |
| Biên tập mềm mại | ánh sáng tán xạ, bóng mềm, ánh sáng dịu, bầu trời âm u nhẹ | Chăm sóc da, thời trang, phong cách sống lành mạnh |
| Ánh sáng lọc màu | ánh sáng lọc màu xanh/hồng, bóng đổ màu kịch tính, ánh sáng viền quanh chủ thể | Thời trang gắn với âm nhạc, phong cách trẻ trung, ảnh đêm |
| Ánh sáng tự nhiên phản chiếu | ánh sáng hoàng hôn, ánh sáng tự nhiên dịu, lóe sáng mặt trời, tông màu ấm | Ngoài trời, phong cách sống, thương hiệu tối giản thân thiện |

### **Xây dựng chủ đích với các từ khóa về tư thế và khung hình**

Tư thế tốt giúp sản phẩm trông có sức hút và nhân vật kỹ thuật số trở nên sinh động hơn. Với AI, bạn phải có mục đích rõ ràng — vì gợi ý về khung hình và tư thế sẽ giúp tránh sự cứng nhắc, lỗi giải phẫu, và tính ngẫu nhiên. Bảng dưới đây cung cấp một số từ khóa hữu ích về tư thế và khung hình khi viết prompt:

| Gợi ý  | Mô tả  | Mẹo  |
| ----- | ----- | ----- |
| Nhìn ra ngoài khung hình | Tạo cảm giác tự nhiên, giống phong cách ảnh biên tập (editorial) | Phù hợp với lookbook hoặc trang quảng cáo |
| Đôi tay đang chuyển động | Thêm tính chân thực và sự linh hoạt | Giúp tránh tư thế cứng nhắc, tĩnh và thiếu tự nhiên |
| Ngồi với phần thân xoay nhẹ | Tạo chiều sâu và độ xoắn nhẹ cho phần thân | Giảm đối xứng, tạo cảm giác tự nhiên hơn |
| Chụp từ góc thấp | Gợi cảm giác quyền lực hoặc địa vị cao | Phù hợp với phong cách streetwear hoặc ảnh sản phẩm nổi bật (hero shot) |

### **Example: Putting it all together**

Ví dụ sau đây kết hợp tất cả những gì chúng ta đã thảo luận trong bài viết này

| Prompt “Ảnh chân dung trong studio của một người mẫu tóc bạch kim, mặc quần cargo ánh kim và áo hoodie lưới ngắn (cropped mesh hoodie), ngồi dạng chân trên bậc thang acrylic (acrylic stairs:1.6). Ánh sáng màu hồng tím (magenta) và xanh ngọc (teal) chiếu từ bên trái và phía sau, tạo độ tương phản mạnh và kịch tính. Chụp bằng ống kính 50mm, mang phong cách biên tập thời trang streetwear hướng đến chiến dịch dành cho Gen Z.” Negative prompt “Mờ, thừa chi, có watermark, kiểu hoạt hình, méo mặt, thiếu ngón tay, giải phẫu sai” | ![][image12] |
| :---- | :---- |

Phân tích prompt ở trên. Chúng ta xác định rõ ngoại hình của chủ thể (tóc bạch kim, trang phục ánh kim), tư thế (ngồi dạng chân, tự tin, tự nhiên không gượng), môi trường (bậc thang acrylic trong studio, có kiểm soát, hiện đại), ánh sáng (nguồn sáng màu gel phối hợp, phong cách táo bạo), ống kính (50mm, chân thực kiểu ảnh chân dung), và cuối cùng là mục đích (chiến dịch dành cho Gen Z, thể hiện tông văn hóa và thị giác). Tất cả những yếu tố này kết hợp lại giúp prompt tạo ra kết quả mong muốn

## **Thực hành tốt nhất và khắc phục sự cố**

Việc viết prompt hiếm khi chỉ cần làm một lần là hoàn hảo — đặc biệt trong các trường hợp sáng tạo. Phần lớn những hình ảnh tốt nhất đến từ quá trình tinh chỉnh ý tưởng qua nhiều lần thử nghiệm. Hãy tham khảo phương pháp dưới đây để cải thiện và lặp lại prompt của bạn:

* Ghi nhật ký prompt  
* Thay đổi từng biến một   
* Lưu seed và ảnh gốc   
* Dùng lưới so sánh

Đôi khi mọi thứ có thể sai lệch — ví dụ như mô hình không hiểu prompt, hoặc ảnh bị rối. Những lỗi này khá phổ biến và thường dễ khắc phục. Với mỗi lần điều chỉnh, bạn có thể tạo ra hình ảnh sắc nét, sạch sẽ và có chủ đích hơn. Bảng dưới đây gợi ý khắc phục lỗi khi viết prompt

| Vấn đề | Nguyên nhân | Cách khắc phục |
| ----- | ----- | ----- |
| Phong cách ngẫu nhiên | Mô hình bị rối hoặc mô tả quá mơ hồ | Làm rõ phong cách, tăng trọng số, loại bỏ xung đột |
| Méo mặt | Quá nhiều phong cách hoặc thiếu gợi ý khuôn mặt | Thêm “portrait of”, “headshot”, hoặc điều chỉnh tư thế, ánh sáng |
| Ảnh quá tối | Không mô tả rõ ánh sáng | Thêm “softbox from left”, “natural light”, hoặc “time of day” |
| Tư thế lặp lại | Dùng cùng seed hoặc cấu trúc cố định | Thay đổi seed, góc máy, hoặc hành động của chủ thể |
| Thiếu chân thực hoặc trông “giả AI” | Tông màu hoặc chi tiết sai lệch | Thêm các từ phủ định như “cartoon”, “digital texture”, “distorted” |

## **Kết luận**

Nắm vững kỹ thuật viết prompt nâng cao có thể biến việc tạo hình ảnh cơ bản thành sản phẩm sáng tạo chuyên nghiệp. Stability AI Image Services trong Amazon Bedrock mang lại khả năng kiểm soát chính xác trong việc tạo và chỉnh sửa hình ảnh, giúp doanh nghiệp chuyển ý tưởng thành sản phẩm sẵn sàng cho sản xuất. Sự kết hợp giữa kỹ năng kỹ thuật và ý đồ sáng tạo giúp người sáng tạo đạt được độ chính xác và nhất quán cần thiết trong môi trường chuyên nghiệp. Khả năng này đặc biệt hữu ích cho các ứng dụng như chiến dịch marketing, duy trì hình ảnh thương hiệu, và hình ảnh sản phẩm. Bài viết này đã minh họa cách tối ưu Stability AI Image Services trong Amazon Bedrock để tạo ra hình ảnh chất lượng cao phù hợp với mục tiêu sáng tạo của bạn.

Để áp dụng các kỹ thuật này, bạn có thể Truy cập Stability AI Image Services thông qua Amazon Bedrock, Hoặc khám phá các [mô hình nền tảng của Stability AI](https://aws.amazon.com/blogs/machine-learning/stability-ai-builds-foundation-models-on-amazon-sagemaker/) có sẵn trong [Amazon SageMaker JumpStart](https://aws.amazon.com/sagemaker/ai/jumpstart/). Ngoài ra, bạn có thể xem các ví dụ mã thực tế trong  [GitHub repository](https://github.com/aws-samples/stabilityai-sample-notebooks/tree/main) của chúng tôi.

---

### **About the authors**

![ Yourprofile picture](/images/image13.png) Phó chủ tịch phụ trách Cộng đồng và Phát triển Kinh doanh tại Stability AI. Ông là người tiên phong trong lĩnh vực AI tạo sinh (generative AI), từng tham gia xây dựng các nền tảng hướng đến người sáng tạo như Civitai và Dream Studio. Ông thường xuyên chia sẻ hướng dẫn và tài liệu giúp các kỹ thuật AI nâng cao trở nên dễ tiếp cận hơn.

![ Yourprofile picture](/images/image14.jpg) Suleman Patel là Kiến trúc sư Giải pháp cấp cao tại Amazon Web Services (AWS), tập trung đặc biệt vào máy học và hiện đại hóa. Tận dụng chuyên môn của mình trong cả kinh doanh và công nghệ, Suleman giúp khách hàng thiết kế và xây dựng các giải pháp giải quyết các vấn đề kinh doanh thực tế. Khi không làm việc, Suleman thích khám phá ngoài trời, đi du lịch đường dài và nấu những món ăn ngon trong bếp.

![ Yourprofile picture](/images/image15.png) Isha Dua là Kiến trúc sư Giải pháp cấp cao tại Khu vực Vịnh San Francisco, làm việc với các nhà cung cấp mô hình AI tạo sinh và giúp khách hàng tối ưu hóa khối lượng công việc AI tạo sinh của họ trên AWS. Cô ấy giúp các doanh nghiệp phát triển bằng cách hiểu các mục tiêu và thách thức của họ, đồng thời hướng dẫn họ cách kiến trúc các ứng dụng của mình theo cách dựa trên đám mây trong khi vẫn hỗ trợ khả năng phục hồi và khả năng mở rộng. Cô ấy đam mê công nghệ máy học và tính bền vững môi trường.

![ Yourprofile picture](/images/image16.png)  Branco là Giám đốc Giải pháp Khách hàng cấp cao tại Amazon Web Services (AWS) và là cố vấn chiến lược, giúp khách hàng đạt được chuyển đổi kinh doanh, thúc đẩy đổi mới thông qua AI tạo sinh và các giải pháp dữ liệu, đồng thời điều hướng thành công hành trình lên đám mây của họ. Trước khi gia nhập AWS, ông đã đảm nhiệm các vai trò Quản lý Sản phẩm, Kỹ thuật, Tư vấn và Triển khai Công nghệ tại nhiều công ty thuộc Fortune 500 trong các ngành, bao gồm bán lẻ và hàng tiêu dùng, dầu khí, dịch vụ tài chính, bảo hiểm, và hàng không vũ trụ và quốc phòng.































