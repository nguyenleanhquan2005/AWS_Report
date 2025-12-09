---
title: "Blog 3"
date: "2025-09-09"
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

## [AWS Public Sector Blog](https://aws.amazon.com/blogs/publicsector/)

# **Tối ưu độ tin cậy của EC2 Spot Instance cho Nextflow trên AWS Batch với Memory Machine Batch**

Bởi Gabriela Karina Paulus, PhD và Christian Kniep vào ngày 18 tháng 9 năm 2025 trong các mục [Amazon EC2](https://aws.amazon.com/blogs/publicsector/category/compute/amazon-ec2/), [AWS Batch](https://aws.amazon.com/blogs/publicsector/category/compute/aws-batch/), [AWS Partner Network](https://aws.amazon.com/blogs/publicsector/category/aws-partner-network/), [Life Sciences](https://aws.amazon.com/blogs/publicsector/category/industries/life-sciences/), [Nonprofit](https://aws.amazon.com/blogs/publicsector/category/public-sector/nonprofit/), [Partner solutions](https://aws.amazon.com/blogs/publicsector/category/post-types/partner-solutions/), [Public Sector](https://aws.amazon.com/blogs/publicsector/category/public-sector/), [Public Sector Partners](https://aws.amazon.com/blogs/publicsector/category/public-sector/public-sector-partners/), [Technical How-to](https://aws.amazon.com/blogs/publicsector/category/post-types/technical-how-to/) [Permalink](https://aws.amazon.com/blogs/publicsector/maximizing-ec2-spot-instance-reliability-for-nextflow-on-aws-batch-with-memory-machine-batch/)  [Share](https://aws.amazon.com/blogs/publicsector/maximizing-ec2-spot-instance-reliability-for-nextflow-on-aws-batch-with-memory-machine-batch/#)

![AWS branded background with text "Maximizing EC2 Spot Instance reliability for Nextflow on AWS Batch with Memory Machine Batch"][image1]

[Amazon Web Services (AWS)](https://aws.amazon.com/) cung cấp một bộ dịch vụ toàn diện cho phép các tổ chức khoa học sự sống vận hành các quy trình genomics phức tạp ở quy mô lớn. Qua nhiều năm hỗ trợ các viện nghiên cứu tiên tiến, AWS đã phát triển chuyên môn sâu về điện toán genomics, cung cấp các giải pháp kết hợp hiệu năng cao với hiệu quả chi phí tối ưu. Trọng tâm của nhiều quy trình xử lí(pipeline) genomics là [AWS Batch](https://aws.amazon.com/batch/), một dịch vụ điện toán được quản lý hoàn toàn, khi kết hợp với [Amazon Elastic Compute Cloud (Amazon EC2)](https://aws.amazon.com/ec2/) Spot Instances, sẽ mang lại cơ sở hạ tầng mạnh mẽ và tiết kiệm chi phí để vận hành các quy trình Nextflow. Bằng cách dựa vào nền tảng này, các nhà nghiên cứu có thể tập trung vào khám phá khoa học thay vì quản lý cơ sở hạ tầng. Để nâng cao hơn nữa nền tảng mạnh mẽ này, AWS hợp tác với các nhà cung cấp giải pháp sáng tạo như [MemVerge](https://aws.amazon.com/marketplace/seller-profile?id=3b7c724c-fae7-4187-ae45-de1625e51395), một Đối tác Công nghệ Nâng cao của Mạng lưới Đối tác AWS (APN), có công nghệ Memory Machine Batch (MMBatch) bổ sung cho AWS Batch bằng cách cung cấp các khả năng checkpointing (lưu trạng thái) nâng cao cho [EC2 Spot Instances](https://aws.amazon.com/ec2/spot/). Sự hợp tác này là một minh chứng cho cam kết của AWS trong việc cung cấp cho khách hàng sự lựa chọn và linh hoạt trong việc tối ưu hóa các quy trình nghiên cứu của họ.

Các viện nghiên cứu luôn phải tìm kiếm những giải pháp để cắt giảm chi phí tính toán. Amazon EC2 Spot Instances là một lựa chọn tuyệt vời cho những quy trình khoa học có khối lượng xử lý lớn và không yêu cầu tương tác trực tiếp. Với khả năng tiết kiệm chi phí lên đến 90% so với giá theo yêu cầu (on-demand), Spot Instances phù hợp cho việc xử lý thông lượng cao với ngân sách hạn hẹp, nhưng mặc định chúng chủ yếu phù hợp với các tải công việc có khả năng chịu lỗi vì chúng có thể bị thu hồi bất cứ lúc nào nếu giá yêu cầu vượt quá giá đặt.

Tuy nhiên, điều quan trọng cần phải hiểu là việc tiết kiệm chi phí của Spot Instance áp dụng cho thời gian tính toán chứ không phải tiến độ công việc. Khi Spot Instance bị thu hồi, các tác vụ đang chạy sẽ bị gián đoạn, có thể dẫn đến mất thời gian tính toán—tiềm ẩn hàng giờ xử lý trong các quy trình genomics kéo dài. Mặc dù AWS Batch cung cấp cơ chế tự động thử lại cho các tác vụ bị gián đoạn, việc thu hồi Spot Instance thường xuyên có thể ảnh hưởng đến thời gian hoàn thành công việc tổng thể và làm cho chi phí theo từng công việc trở nên khó dự đoán hơn. Các tổ chức cần đánh giá cẩn thận yêu cầu về thời gian hoàn thành kết quả và triển khai các chiến lược phù hợp để quản lý hiệu quả các gián đoạn này.

Tính khó lường này khiến nhiều trung tâm phải bám vào On-Demand Instances vì họ không thể mạo hiểm với các phân tích dở dang hoặc trễ hạn. Đây không phải trải nghiệm cá biệt. Khi mà nhiều người dùng Nextflow đã gặp những thách thức khi làm việc với Spot Instances trong nỗ lực cân bằng giữa hiệu quả chi phí và độ tin cậy cho các quy trình genomics trên đám mây.

Trong bài viết này, chúng tôi xem xét cách công nghệ của MemVerge có thể vượt qua các thách thức do quy trình xử lý (pipeline) bị gián đoạn, bằng cách cho phép các pipeline dừng giữa chừng có thể khởi động lại từ đúng điểm đã dừng, bất kể việc Spot Instance bị thu hồi. Chúng tôi cũng sẽ cho thấy cách Batch Viewer của MemVerge và các thực tiễn tốt nhất đã được kiểm chứng giúp các nhà nghiên cứu trực quan hóa khối lượng công việc, xác định nút thắt cổ chai(bottlenecks), và áp dụng các chiến lược thông minh để khiến quy trình của họ hiệu quả hơn, bền bỉ hơn và tối ưu hơn cho môi trường đám mây.

# **Sự đánh đổi giữa chi phí và độ tin cậy khi dùng EC2 Spot Instances**

Đối với các nhóm nghiên cứu chịu ràng buộc ngân sách chặt chẽ, Spot Instances mang đến một đề nghị hấp dẫn: truy cập cùng năng lực tính toán như EC2 On-Demand Instances nhưng chỉ với một phần nhỏ chi phí. Đây là cách mạnh mẽ để kéo dài nguồn kinh phí tài trợ, rút ngắn thời gian khám phá, và xử lý nhiều mẫu hơn mà không phải hy sinh mục tiêu khoa học.

Nhưng dùng Spot Instances có một hạn chế.

Spot Instances có thể bị AWS thu hồi với thời gian cảnh báo 2 phút, tùy thuộc vào nhu cầu năng lực tổng thể. Trên thực tế, điều này đồng nghĩa tài nguyên tính toán có thể biến mất giữa chừng, khiến các tiến trình chạy lâu thất bại mà không báo trước. Dù Nextflow được thiết kế để thử chạy lại các tác vụ thất bại, việc gián đoạn liên tục khiến toàn bộ tiến trình phải chạy lại, làm tăng tổng thời gian thực thi và lãng phí tài nguyên tính toán đã tiêu thụ.

Rốt cuộc, các trung tâm thường phải chuyển những giai đoạn quan trọng trong quy trình công việc về lại EC2 On-Demand Instances chỉ để có được độ tin cậy cần thiết nhằm kịp thời hạn. Dù cách làm này hiệu quả, nó đi kèm chi phí cao-xóa nhòa lợi thế ngân sách vốn là lý do ban đầu để chuyển lên đám mây.

Tình huống này bộc lộ một mâu thuẫn cốt lõi: dùng Spot Instances có thể mang lại mức giảm giá sâu cho tài nguyên tính toán, nhưng không nhất thiết phù hợp với yêu cầu của quy trình tính toán đang điều phối sức mạnh xử lý. Với các khối lượng công việc tin sinh học chạy trong nhiều giờ hoặc nhiều ngày, sự lệch pha đó có thể rất tốn kém.

Vậy các nhóm nghiên cứu có thể tiếp tục dùng EC2 Spot Instances mà không phải đánh đổi độ tin cậy bằng cách nào?

Trong hệ sinh thái này, MMBatch của MemVerge đóng vai trò công cụ bổ trợ, bổ sung cơ chế checkpoint minh bạch ở cấp độ container để tăng cường độ tin cậy hơn nữa.

# **Vai trò của Nextflow — cùng những giới hạn**

Nextflow được thiết kế để tích hợp liền mạch với các nền tảng thực thi thuần đám mây, và khi chạy trên AWS, nó giao việc thực thi tác vụ cho AWS Batch. Điều này tách rời logic của quy trình công việc khỏi lớp tính toán bên dưới, cho phép AWS Batch chịu trách nhiệm hoàn toàn về việc quản lý thử lại, mở rộng tài nguyên và phản ứng với việc thu hồi Spot Instance.

Bên trong AWS Batch, các chiến lược thử lại được dùng để kiểm soát điều gì xảy ra với một job trong những tình huống nhất định. Một chiến lược phổ biến là thử chạy lại job khi máy chủ gặp sự cố-ví dụ, Spot instance bị AWS thu hồi do nhu cầu tăng cao từ khách hàng on‑demand.

Khi một Spot Instance bị thu hồi, AWS Batch sẽ đưa job vào hàng đợi lại cho đến khi đạt đến số lần thử tối đa (tối đa 10 lần). Ở góc nhìn của Nextflow, tác vụ đơn giản là được thử lại-không cần can thiệp thủ công. Cách tiếp cận này tinh gọn vì cho phép AWS Batch áp dụng các chiến lược và tối ưu hóa đặc thù đám mây mà không làm gánh nặng cho công cụ điều phối quy trình. Đây là một sự phân công thông minh: Nextflow định nghĩa quy trình, còn AWS Batch xử lý việc thực thi.

Tuy nhiên, dù mô hình này cải thiện độ tin cậy, nó có một điểm yếu nghiêm trọng: Mặc định, mỗi lần thử lại sẽ khởi động job từ đầu. Tiến độ đạt được ở lần chạy trước thường bị mất.

Với các tác vụ ngắn, điều đó không quá quan trọng. Nhưng đối với các bước chạy lâu-như canh chỉnh toàn bộ hệ gen (whole‑genome alignment) hoặc gọi biến thể hợp nhất trên các tập mẫu lớn-tổn thất tính toán có thể rất lớn. Một tác vụ chạy 6 giờ và bị gián đoạn ở giờ thứ 5 vẫn sẽ phải bắt đầu lại từ đầu. Nếu việc thu hồi Spot Instance diễn ra thường xuyên, chi phí và độ trễ sẽ tăng nhanh, làm suy giảm lợi thế ngân sách khi dùng Spot Instances ngay từ đầu.

Đó là lúc MMBatch của MemVerge xuất hiện để thay đổi cuộc chơi.

MMBatch không thay thế hay ghi đè AWS Batch-nó bổ trợ cách khởi chạy container trong các môi trường tính toán của AWS Batch. Khi đã tích hợp, MMBatch cho phép checkpoint trực tiếp các job đang chạy ở cấp độ container.

Khi AWS Batch sắp xếp chạy lại một job-chẳng hạn sau khi Spot Instance bị thu hồi-MMBatch sẽ can thiệp trong giai đoạn khởi động container. Trước khi job chạy lại, MMBatch kiểm tra xem có checkpoint hay không. Nếu có, nó tự động khôi phục container từ snapshot đó, tiếp tục từ trạng thái đã lưu gần nhất thay vì chạy lại từ đầu. Ở góc nhìn của AWS Batch, không có gì thay đổi; nó vẫn thấy một job đang được thử lại. Nhưng giờ đây, lần thử lại không còn là “làm lại từ đầu”-mà là phục hồi nhanh.

Kết quả rất ấn tượng: Thời gian tính toán đã đầu tư không bị mất, ngay cả khi bị thu hồi Spot Instance. Các pipeline trở nên bền bỉ hơn đáng kể, các tác vụ chạy dài ít dễ tổn thương hơn, và hiệu quả kinh tế của Spot Instances lại trở nên khả thi-ngay cả với những quy trình trước đây đòi hỏi độ tin cậy kiểu on‑demand.

## **Trực quan hóa và tối ưu hóa với MMBatch WebUI**

Trong khi checkpointing mang lại bước nhảy vọt về khả năng chịu lỗi, việc thấu hiểu và tối ưu cách các pipeline hoạt động trên đám mây cũng quan trọng không kém. Đó là lúc MMBatch Viewer xuất hiện-một giao diện mạnh mẽ cung cấp cho các nhà nghiên cứu và kỹ sư pipeline khả năng quan sát theo thời gian thực đối với khối lượng công việc của họ.

Với nhiều nhóm, việc thực thi trên đám mây có thể giống như một “hộp đen”. Các tác vụ chạy từ xa, trên một số instance, với chi phí biến động và thỉnh thoảng gặp lỗi. Khi sự cố xảy ra-hoặc chỉ đơn giản là chạy chậm hơn kỳ vọng-việc gỡ lỗi trở nên tốn thời gian và thường dựa nhiều vào phỏng đoán.

MMBatch WebUI thay đổi điều đó bằng cách làm cho lớp thực thi trở nên minh bạch.

Với MMBatch WebUI, người dùng có thể:

* Trực quan hóa dòng thời gian của công việc để xem tác vụ thực sự mất bao lâu, các lần retry diễn ra ở đâu, và việc thu hồi Spot Instance ảnh hưởng đến tiến độ như thế nào.  
* Theo dõi hoạt động checkpoint, xác định job nào hưởng lợi từ snapshotting và lượng tài nguyên tính toán được tiết kiệm. Ví dụ, khoảng thời gian checkpoint 15 phút sẽ không tạo checkpoint cho các job chạy ngắn, qua đó giảm áp lực checkpoint.  
* Phân tích xu hướng chi phí và hiệu suất, giúp các nhóm xác định các giai đoạn tốn kém hoặc lãng phí và tinh chỉnh yêu cầu tài nguyên.  
* Chẩn đoán nhanh các nút thắt cổ chai, kiểm tra tệp log hoặc tải xuống gói log để mở phiếu hỗ trợ (support ticket).

Đối với trung tâm nghiên cứu genomics, mức độ quan sát này là bước ngoặt. Thay vì chỉ phản ứng trước các lần chạy thất bại hoặc chi phí leo thang, họ có thể chủ động tinh chỉnh cấu hình Nextflow và chiến lược AWS Batch. Họ phát hiện những bước nào cần được ưu tiên checkpointing, điều chỉnh phân bổ bộ nhớ và vCPU dựa trên dữ liệu thực tế, và thậm chí thử nghiệm các chiến lược đa dạng hóa loại instance để giảm tần suất bị thu hồi Spot Instance.

Kết hợp với khả năng checkpointing của MemVerge, MMBatch WebUI đã giúp họ biến Spot Instances từ một canh bạc rủi ro thành một nền tảng hiệu quả về chi phí và thông lượng cao cho công việc giải trình tự của mình.

## **Khai thác EC2 Spot Instances hiệu quả hơn**

Genomics ở quy mô đám mây không nhất thiết phải lựa chọn giữa chi phí và độ tin cậy. Nextflow và AWS Batch cung cấp một nền tảng vững chắc-nhưng khi việc thu hồi EC2 Spot Instance đe dọa các thời hạn quan trọng, bạn cần những công cụ vượt ra ngoài cơ chế retry.

Với MMBatch của MemVerge, checkpointing giúp ngay cả các công việc chạy dài cũng chống chịu gián đoạn. Và với MMBatch WebUI, bạn có được khả năng quan sát và hiểu biết cần thiết để tinh chỉnh quy trình làm việc nhằm đạt hiệu quả và tiết kiệm chi phí.

Đối với các nhóm nghiên cứu chạy khối lượng công việc quy mô lớn, nhạy thời gian, những công cụ này khai mở toàn bộ tiềm năng của EC2 Spot Instances-mà không phải đánh đổi.

Lợi thế của AWS trong tính toán cho nghiên cứu bắt nguồn từ nhiều năm kinh nghiệm hỗ trợ các khối lượng công việc khoa học đòi hỏi khắt khe nhất thế giới. Với hạ tầng toàn cầu cung cấp mức khả dụng 99,99%, đổi mới liên tục trong điện toán hiệu năng cao (HPC), và chuyên môn sâu về quy trình công việc hệ gen, AWS cung cấp nền tảng cho những đột phá nghiên cứu. Các giải pháp đối tác như MMBatch của MemVerge có thể tăng cường nền tảng này, kết hợp với danh mục dịch vụ toàn diện của AWS, năng lực bảo mật, và tối ưu chi phí ở quy mô lớn. Khi các tổ chức nghiên cứu mở rộng ranh giới khám phá khoa học, các dịch vụ của AWS cũng tiến hóa để đáp ứng nhu cầu ngày càng tăng, bảo đảm công nghệ không bao giờ trở thành nút thắt cổ chai đối với tiến bộ khoa học.

Tìm hiểu cách đội ngũ của bạn có thể chạy nhanh hơn, chi tiêu thông minh hơn, và hoàn thành mọi thời hạn-dù khoa học có phức tạp đến đâu.

Nếu bạn sẵn sàng tìm hiểu sâu hơn:

* [Xem bản demo hướng dẫn để thấy MemVerge hoạt động thực tế](https://www.youtube.com/watch?v=kDBJyl5qF9g)  
* Thử [MMEngine Workshop](https://catalog.us-east-1.prod.workshops.aws/workshops/86acff36-22b7-4ac8-987e-2b21d493e60e/en-US) của chúng tôi trên AWS Workshop Studio để có trải nghiệm thực hành.  
* Liên hệ đội ngũ phụ trách tài khoản MemVerge của bạn hoặc ghé thăm [MemVerge](https://www.memverge.com/) để đặt lịch workshop cá nhân hóa.

TAGS: [Amazon EC2](https://aws.amazon.com/blogs/publicsector/tag/amazon-ec2/), [AWS Batch](https://aws.amazon.com/blogs/publicsector/tag/aws-batch/), [AWS Partner Network](https://aws.amazon.com/blogs/publicsector/tag/aws-partner-network/), [AWS Public Sector](https://aws.amazon.com/blogs/publicsector/tag/aws-public-sector/), [AWS Public Sector Partners](https://aws.amazon.com/blogs/publicsector/tag/aws-public-sector-partners/), [life sciences](https://aws.amazon.com/blogs/publicsector/tag/life-sciences/), [nonprofit](https://aws.amazon.com/blogs/publicsector/tag/nonprofit/), [technical how-to](https://aws.amazon.com/blogs/publicsector/tag/technical-how-to/)

![Gabriela Karina Paulus, PhD][image2]

### Gabriela Karina Paulus, PhD

Gabriela là solution architect tại AWS, làm việc trong lĩnh vực tổ chức phi lợi nhuận (NPO) và tập trung vào khách hàng nghiên cứu. Cô có bằng Tiến sĩ Sinh học Phân tử và Tin sinh học, Thạc sĩ Dược lý, và Cử nhân Công nghệ Sinh học. Sống tại Khu vực Đại đô thị NYC, Gabriela không chỉ là solution architect mà còn được công nhận là chuyên gia genomics trong AWS. Cô kết hợp nền tảng khoa học với chuyên môn điện toán đám mây để thúc đẩy đổi mới và giúp các nhà khoa học, viện nghiên cứu đạt hiệu quả cao nhất. Chuyên môn về công nghệ đám mây của Gabriela còn được bổ trợ bởi kinh nghiệm nghiên cứu phong phú, với các công bố đã bình duyệt trước đây đóng góp những hiểu biết giá trị

![Christian Kniep][image3]

### Christian Kniep

Niềm đam mê của Christian là cải thiện tính dễ dùng của những cỗ máy “bướng bỉnh” mà chúng ta gọi là thiết bị IT. Anh bắt đầu sự nghiệp là Kỹ sư Hệ thống HPC trong ngành ô tô và kết thúc công việc quản trị hệ thống với vai trò phụ trách Infiniband cho một cụm thử nghiệm va chạm gồm hàng nghìn nút vào đầu những năm 2010\. Sau đó, anh chuyển đến Paris để làm về kết nối liên kết HPC. Khi Docker xuất hiện vào năm 2014, Christian quay về Berlin và bắt đầu làm việc trong môi trường container (đang nổi) tại một startup ở Berlin, Playstation Now, và Docker Inc. Năm 2019, Christian trở thành “SA chuyên gia EC2 Spot” tại AWS và là nhà truyền bá kỹ thuật cấp cao đầu tiên trong đội sản phẩm HPC. Năm 2022, Christian rời AWS để một lần nữa theo đuổi dự án đam mê thúc đẩy việc sử dụng container trong HPC. Anh làm việc cho Viện Max Planck về Human Development và gia nhập MemVerge với vai trò kiến trúc sư HPC chính để thúc đẩy tích hợp checkpoint ứng dụng và các stack HPC trên đám mây
