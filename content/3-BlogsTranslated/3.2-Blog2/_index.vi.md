---
title: "Blog 2"
date: "2025-09-09"
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

## [AWS for Industries](https://aws.amazon.com/blogs/industries/)

# Kết nối với màn hình hiển thị trong lĩnh vực ô tô hoặc tại nhà máy sản xuất bằng VNC và AWS IoT Secure Tunneling


Bởi Andrew Timpone, Jonathan Johnson, and Joseph O'Such vào ngày 18 tháng 9 năm 2025 trong mục [Automotive](https://aws.amazon.com/blogs/industries/category/industries/automotive/), [Industries](https://aws.amazon.com/blogs/industries/category/industries/), [Internet of Things](https://aws.amazon.com/blogs/industries/category/internet-of-things/) [Permalink](https://aws.amazon.com/blogs/industries/connect-to-automotive-or-manufacturing-plant-displays-using-vnc-and-aws-iot-secure-tunneling/)  [Comments](https://aws.amazon.com/blogs/industries/connect-to-automotive-or-manufacturing-plant-displays-using-vnc-and-aws-iot-secure-tunneling/#Comments)  [Share](https://aws.amazon.com/blogs/industries/connect-to-automotive-or-manufacturing-plant-displays-using-vnc-and-aws-iot-secure-tunneling/#)

Trong ngành công nghiệp ô tô đang phát triển nhanh chóng, việc truy cập từ xa vào màn hình xe đã trở thành yếu tố thay đổi cuộc chơi cho việc bảo trì, phát triển và hỗ trợ. Bằng cách sử dụng công nghệ VNC và AWS IoT Secure Tunneling, các chuyên gia ô tô giờ đây có thể kết nối với màn hình xe từ xa cho nhiều ứng dụng khác nhau. Khả năng này cho phép chẩn đoán và khắc phục sự cố từ xa, cho phép kỹ thuật viên truy cập an toàn vào hệ thống thông tin giải trí hoặc màn hình cụm đồng hồ của xe đang hoạt động, trong khi các kỹ sư có thể xem và tương tác từ xa với màn hình hệ thống hỗ trợ người lái tiên tiến (ADAS) để giúp xác định nhu cầu gỡ lỗi và điều chỉnh. Trong quá trình cập nhật qua mạng (OTA), các kỹ sư có thể giám sát tiến độ và màn hình trạng thái từ xa để xác nhận triển khai thành công. Các nhà quản lý đội xe có thể sử dụng công nghệ này để xem màn hình thông tin người lái hoặc màn hình đo từ xa của nhiều xe để giúp giám sát hiệu suất, hiệu quả nhiên liệu và cải thiện hành vi người lái. Trong sản xuất ô tô, nhân viên kiểm soát chất lượng có thể hưởng lợi từ khả năng truy cập và kiểm tra từ xa các màn hình kỹ thuật số trên xe đang di chuyển qua dây chuyền lắp ráp. Đối với thử nghiệm và phát triển xe, các kỹ sư thử nghiệm có thể truy cập từ xa màn hình xe nguyên mẫu trong quá trình thử nghiệm đường bộ hoặc mô phỏng để giúp tăng cường an toàn và hiệu quả trong các quy trình thử nghiệm. Ngoài các ứng dụng này, công nghệ còn mở ra khả năng cho nhiều trường hợp sử dụng bảo trì và hỗ trợ khác, bao gồm khắc phục sự cố trên xe và trải nghiệm đồng duyệt được hướng dẫn, cách mạng hóa cách các chuyên gia ô tô tương tác và hỗ trợ xe đang hoạt động.

### **Tổng quan về giải pháp**

Giải pháp kết nối an toàn này sử dụng IoT Secure Tunneling để thiết lập kết nối hai chiều với các thiết bị từ xa thông qua đường truyền bảo mật được quản lý bởi [AWS IoT](https://aws.amazon.com/iot/). Giải pháp bao gồm một máy khách có cài đặt VNC Viewer, một máy chủ trung gian đóng vai trò là proxy cục bộ an toàn, và một màn hình ô tô mà bạn muốn kết nối tới.

![Figure 1 Architecture IoT Secure Tunneling components and messages](/images/image1_02.png)


*Figure 1: Architecture IoT Secure Tunneling components and messages*

### **Điều kiện tiên quyết**

Để thực hiện hướng dẫn này, bạn cần chuẩn bị các điều kiện sau:

* [Tài khoản AWS](https://signin.aws.amazon.com/signin?redirect_uri=https%3A%2F%2Fportal.aws.amazon.com%2Fbilling%2Fsignup%2Fresume&client_id=signup)
* Một VPC có subnet riêng (private subnet)
* Kiến thức cơ bản về các lệnh Linux và dịch vụ AWS IoT
* Một thiết bị đóng vai trò là IoT thing (đích đến / destination)
  * Khuyến nghị sử dụng Raspberry Pi 4
  * Đã cài đặt và bật máy chủ VNC (VNC server)
  * Đã cài đặt và cấu hình AWS CLI
  * Có tệp nhị phân (binary) của AWS IoT Secure Tunneling local proxy
* Một máy chủ đóng vai trò là proxy relay (nguồn / source)
  * Có thể là EC2 instance hoặc container trên ECS/EKS
  * Đã cài đặt và cấu hình AWS CLI
  * Có tệp nhị phân hoặc container của AWS IoT Secure Tunneling local proxy
* Một máy khách (client machine)
  * Khuyến nghị sử dụng Windows Bastion host trong AWS hoặc máy tính cá nhân
  * Đã cài đặt VNC viewer
  * Có kết nối mạng tới proxy relay (source)

### **Walkthrough**

#### Cài đặt Raspberry Pi

Bước đầu tiên là cài đặt và kích hoạt máy chủ VNC, cấu hình Raspberry Pi của bạn như một IoT thing, và cài đặt hoặc build proxy cục bộ cho IoT Secure Tunneling.

Nếu Raspberry Pi không có màn hình để chia sẻ, hãy kết nối Pi với một màn hình.

* Tải VNC server:

```bash
sudo apt update
sudo apt install realvnc-vnc-server
```

* Kích hoạt VNC server thông qua raspi-config:

```bash
sudo raspi-config
Navigate to "Interface Options" > "VNC" > Enable
```


* Đặt VNC password:

```bash
vncpasswd
```

#### Định cấu hình Raspberry Pi của bạn dưới dạng một "Thiết bị IoT" (IoT thing)

* Đăng ký Raspberry Pi của bạn dưới dạng một AWS IoT thing:

```bash
aws iot create-thing --thing-name 
raspberry-pi-vnc
```

* Tạo các chính sách và chứng chỉ IoT được yêu cầu (xem các bước thiết lập chi tiết trong hướng dẫn):

```bash
  aws iot create-policy \
  --policy-name SecureTunnelingPolicy \
  --policy-document '{
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": "iotsecuretunneling:RotateTunnelAccessToken",
        "Resource": "*"
      },
      {
        "Effect": "Allow",
        "Action": "iotsecuretunneling:SubscribeToTunnel",
        "Resource": "*"
      },
      {
        "Effect": "Allow",
        "Action": "iot:Connect",
        "Resource": "*"
      },
      {
        "Effect": "Allow",
        "Action": "iot:Subscribe",
        "Resource": "*"
      }
    ]
  }'
```

* Theo tài liệu AWS IoT để tạo và tải chứng chỉ ([here](https://docs.aws.amazon.com/iot/latest/developerguide/device-certs-create.html)):

```bash
  aws iot create-keys-and-certificate \
  --set-as-active \
  --certificate-pem-outfile certificate_filename.pem \
  --public-key-outfile public_filename.key \
  --private-key-outfile private_filename.key
```

* Gắn chính sách vào chứng chỉ:

```bash
aws iot attach-policy --policy-name SecureTunnelingPolicy --target <certificate-arn>
```

* Cài đặt và cấu hình local proxy (hướng dẫn có thể được tìm thấy trong tài liệu AWS IoT Secure Tunneling Local Proxy hoặc trong các phần Thiết lập Local Proxy bên dưới)

#### Tạo một IoT secure tunnel

* Từ dòng lệnh, hãy thực thi lệnh AWS CLI sau cho tài khoản và khu vực (region) mà bạn đang triển khai tài nguyên:
aws iotsecuretunneling open-tunnel \

```bash
--destination-config thingName=raspberry-pi-vnc,services=VNC \
--region <your-aws-region>
```

* Lệnh này sẽ trả về một payload JSON chứa `sourceAccessToken` và `destinationAccessToken` (see documentation here). Hãy ghi lại chúng để sử dụng sau. Cách làm tốt nhất là đặt các token nguồn và token đích vào các tệp .txt riêng biệt để dễ sử dụng.

#### Thiết lập proxy cục bộ bằng tệp nhị phân

Trong mạng private subnet, hãy tạo một instance EC2 Ubuntu và làm theo các bước từ tài liệu [AWS IoT Secure Tunneling Local Proxy](https://github.com/aws-samples/aws-iot-securetunneling-localproxy) để xây dựng tệp nhị phân proxy cục bộ. Đảm bảo rằng bạn có thể giao tiếp từ máy chủ bastion Windows (được mô tả bên dưới) hoặc thiết bị cá nhân tới instance EC2 Ubuntu qua cổng TCP 5000, và instance có IAM Role cho phép truy cập Systems Manager.

* Kết nối vào instance EC2 Ubuntu thông qua AWS Session Manager hoặc SSH
* Chạy lệnh sau từ dòng lệnh trong thư mục chứa tệp nhị phân localproxy:

```bash
localproxy binary is located
./localproxy -s 5000 -b 0.0.0.0 -r <your-aws-region> -t <sourceAccessToken>
```

* Trên Raspberry Pi của bạn, hãy chạy lệnh sau từ dòng lệnh để khởi động tệp nhị phân proxy cục bộ:

```bash
./localproxy -d 5000 -b 0.0.0.0 -r <your-aws-region> -t <destinationAccessToken>
```

#### Thiết lập proxy cục bộ bằng container

Tham khảo [Docker documentation](https://docs.docker.com/engine/install/debian/) để cài đặt docker trước khi tiếp tục. Container proxy cục bộ có thể chạy trên Amazon Elastic Kubernetes Service (EKS) và Amazon Elastic Container Service (ECS) nếu muốn.

Để tải image container và chạy trên instance EC2 của bạn, hãy chạy lệnh sau từ thư mục chứa token nguồn `sourceToken.txt` của bạn. Thay thế `<distro>` bằng tùy chọn phân phối phù hợp cho máy của bạn từ danh sách tại đây:

```bash
  docker run -d -it --network=host \
  public.ecr.aws/aws-iot-securetunneling-localproxy/<distro>-bin:amd64-latest \
  --region us-east-1 \
  -s 5000 \
  -c /etc/ssl/certs \
  -t $(cat sourceToken.txt) \
  -b 0.0.0.0
```

Đối với Raspberry Pi, hãy chạy lệnh sau tại thư mục chứa destinationToken của bạn `destinationToken.txt`. Thay thế `<distro>` bằng tùy chọn phân phối phù hợp cho máy của bạn từ danh sách tại đây:

```bash
  docker run -d -it --network=host \
  public.ecr.aws/aws-iot-securetunneling-localproxy/<distro>-bin:amd64-latest \
  --region us-east-1 \
  -d 5000 \
  -c /etc/ssl/certs \
  -t $(cat destinationToken.txt)
```

#### Thiết lập máy chủ bastion Windows với VNC Viewer

![Figure 2 VPC with bastion and Ubuntu local proxy](/images/image2_02.png)


*Figure 2: VPC với bastion và proxy cục bộ Ubuntu*

Để thử nghiệm với IoT Secure Tunnel và IoT LocalProxy, hãy tạo một VPC với public subnet chứa máy chủ bastion Windows, Internet Gateway và NAT Gateway.

Nếu bạn sử dụng thiết bị cá nhân làm máy khách, hãy đặt instance EC2 Ubuntu với LocalProxy của bạn trong public subnet này thay thế.

#### Cài đặt máy khách

Trên máy khách của bạn (có thể là máy chủ bastion Windows hoặc máy tính cá nhân), bạn cần cài đặt ứng dụng VNC viewer. Làm theo hướng dẫn tại đây. Nếu bạn sử dụng thiết bị cá nhân, hãy đảm bảo proxy EC2 của bạn nằm trong public subnet.

* Lấy địa chỉ IP của instance EC2 Ubuntu. Nếu sử dụng thiết bị cá nhân, hãy đảm bảo đây là địa chỉ IP public.

![VNC Viewer application](/images/image3_02.png)


* Mở ứng dụng VNC Viewer. Tạo kết nối mới. Đối với địa chỉ kết nối, sử dụng Địa chỉ IP của máy chủ Ubuntu và chỉ định cổng là 5000 (ví dụ: `172.31.59.116:5000`)

*Nếu bạn đang sử dụng máy tính cá nhân, bạn cần đảm bảo máy chủ Ubuntu của bạn có IP public và nằm trong public subnet.*

* Khi prompted, hãy nhập thông tin xác thực (ví dụ: tên người dùng và mật khẩu bạn đã thiết lập trên Raspberry Pi cho VNC)

### **Dọn Dẹp**

Để tránh phát sinh chi phí không cần thiết trong tương lai, hãy ngừng sử dụng các tài nguyên không còn cần thiết:

* Xóa AWS IoT thing, chứng chỉ và chính sách bạn đã tạo
* Đóng và xóa mọi tunnels đang mở bằng AWS IoT console hoặc CLI
* Dừng máy chủ VNC nếu không còn cần thiết
* Chấm dứt và xóa các instance EC2 hoặc container ECS đã tạo
* Xóa VPC và các tài nguyên VPC bạn đã tạo

### **Kết luận**

Trong bài viết này, chúng tôi đã trình bày cách thiết lập quyền truy cập giao diện người dùng bảo mật vào một IoT thing bằng AWS IoT Secure Tunneling và VNC. Giải pháp này có thể được điều chỉnh cho nhiều kịch bản IoT trong ô tô, sản xuất và công nghiệp nơi quyền truy cập chia sẻ màn hình trực tiếp bảo mật là rất quan trọng. Các ví dụ bao gồm chẩn đoán từ xa hệ thống giải trí ô tô và giám sát thời gian thực màn hình thiết bị sản xuất. Chúng tôi cũng chia sẻ chi tiết về việc tận dụng nhắn tin IoT tích hợp để tự động hóa việc thiết lập và thu dọn đường hầm bảo mật IoT và proxy cục bộ. Hơn nữa, blog này đã trình bày một kiến trúc minh họa cách mở rộng tự động hóa đường hầm bảo mật IoT với kiến trúc serverless của AWS. Cách tiếp cận này giúp quản lý hiệu quả việc tạo và chấm dứt các đường hầm bảo mật IoT trên một loạt các IoT thing, cho dù chúng được kết nối với màn hình xe hơi hay các màn hình khác. Bằng cách triển khai các giải pháp này, các nhà sản xuất ô tô và nhà điều hành công nghiệp có thể nâng cao đáng kể khả năng giám sát từ xa, khắc phục sự cố và bảo trì, tạo ra tiềm năng cải thiện hiệu quả và giảm thời gian ngừng hoạt động.

Để tìm hiểu thêm hãy xem qua [AWS IoT Secure Tunneling documentation](https://docs.aws.amazon.com/iot/latest/developerguide/secure-tunneling.html) hoặc khám phá thêm các [AWS IoT solutions](https://aws.amazon.com/iot/).

---

### **About the authors**

![Andrew Timpone](/images/image4_02.png)


**Andrew Timpone**

Với sáu năm kinh nghiệm trong kiến trúc đám mây, Andrew hỗ trợ các khách hàng doanh nghiệp lớn giải quyết các vấn đề kinh doanh bằng cách sử dụng AWS. Andrew có hơn 25 năm kinh nghiệm trong lĩnh vực CNTT với chuyên môn về các mẫu tích hợp doanh nghiệp. Andrew đã kết hôn và có ba con, hiện đang sinh sống tại phía nam Cleveland, Ohio, nơi ông thích đạp xe, bắn cung và làm vườn trồng rau.

![Jonathan Johnson](/images/image5_02.jpg)


**Jonathan Johnson**

Jonathan Johnson ("JJ") là một Kiến trúc sư Giải pháp Cấp cao tại AWS. Anh đã làm việc trong lĩnh vực kiến trúc đám mây được 10 năm và có hơn 25 năm kinh nghiệm trong lĩnh vực Phát triển ứng dụng & CNTT. Anh thích làm việc với khách hàng để thiết kế các giải pháp cho các yêu cầu kinh doanh phức tạp. Gần đây, JJ đã và đang tích cực thúc đẩy việc sử dụng Amazon Q Developer để xây dựng các giải pháp. Khi không thiết kế các kiến trúc AWS, JJ có thể được tìm thấy trên chiếc thuyền buồm Hunter cũ 'Seas the Day' của mình.

![Joseph O'Such](/images/image6_02.png)


**Joseph O'Such**

Joseph O'Such là một Kiến trúc sư Giải pháp Đối tác tại AWS, chủ yếu hỗ trợ các công ty trong Mạng lưới Đối tác AWS. Joseph thường xuyên tham gia vào các dự án và đối tác liên quan đến IoT, bắt nguồn từ nền tảng Kỹ sư Cơ khí của anh. Joseph sống và làm việc tại khu vực Bắc Virginia, bên ngoài Washington DC, nơi anh thích các hoạt động thể chính như leo núi, bóng chuyền và chạy bộ.

[image1]: /images/image1.png
[image2]: /images/image2.png
[image3]: /images/image3.png
[image4]: /images/image4.png
[image5]: /images/image5.png
[image6]: /images/image6.png
