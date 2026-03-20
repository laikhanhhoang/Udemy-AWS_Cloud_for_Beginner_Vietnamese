<h1 align="center">
    <strong>VIRTUAL PRIVATE CLOUD (VPC)</strong>
</h1>

## MỤC LỤC

- [Lí thuyết](#lí-thuyết)
    - [VPC - Định nghĩa](#vpc---định-nghĩa): *tạo một mạng ảo** (virtual network) và **control toàn bộ network in/out của mạng đó**.
    - [Các thành phần cơ bản của VPC](#các-thành-phần-cơ-bản-của-vpc): **Security Group**, **Security Group**, **Internet Gateway**, **NAT Gateway**, **VPC Endpoint**,...
- [Lab](#lab)
- [Tài liệu bổ sung](#tài-liệu-bổ-sung)
    - Nội dung phục vụ lab: [Code](/labs/sec_14)
    - AWS Console liên quan:
        - [VPC](/aws_console/README.md#virtual-private-cloud-vpc)


## <div align="center"><strong>LÍ THUYẾT</strong></div>
<!-- Thêm lí thuyết vào sau dòng này -->


#### **VPC - Định nghĩa**
- **Là** một service, viết tắt của Virtual Private Cloud, cho phép người dùng **tạo một mạng ảo** (virtual network) và **control toàn bộ network in/out của mạng đó**.
- VPC tương đối giống với network ở datacenter truyền thống tuy nhiên các khái niệm đã được AWS đơn giản hoá giúp người dùng dễ tiếp cận.

#### **Các thành phần cơ bản của VPC**
- VPC: Một mạng ảo được tạo ra ở cấp độ region.
- Subnet: Một dải IP được định nghĩa nằm trong VPC. Mỗi subnet phải được quyết định Availability Zone tại thời điểm tạo ra.
- Routing: xác định traffic sẽ được điều hướng đi đâu trong mạng.
- **Route table**: **bảng định tuyến cho traffic rời khỏi subnet**.
    - Route Table sẽ quyết định một subnet sẽ là Private hay Public.
    - Subnet được gọi là Public khi có route đi tới 
    - Internet Gateway và ngược lại.
    - Một Subnet chỉ có thể associate 1 route table.
    - Default VPC do AWS tạo sẵn sẽ có 1 main route table associate với toàn bộ subnet.
    - Ví dụ:
        |Route Table|Thành phần|
        |-----------|----------|
        |Public RT|**`10.0.0.0/16 → local`** <br> **`0.0.0.0/0 → IGW`**|
        |Private RT|**`10.0.0.0/16 → local`** <br> **`0.0.0.0/0 → NAT GW`**|

- IP Address: IP V4 hoặc V6 được cấp phát. Có 2 loại là Public IP và Private IP.
- Elastic IP: IP được cấp phát riêng, có thể access từ internet (public), không bị thu hồi khi instance start -> stop.
- **Security Group**: Đóng vai trò như một firewall ở **cấp độ instance, định nghĩa traffic được đi vào /đi ra**. 
    - Thường được **dùng để gom nhóm các resource có chung network setting (in/out, protocol, port)**.
    - Khi thiết kế cần quan tâm tới tính tái sử dụng, dễ quản lý.
    - Source của một Security Group có thể là CIDR hoặc id của một Security Group khác.
    - Rule của Security Group là stateful và không có deny rule.
    - *Statefull có nghĩa là nếu Inbound cho phép traffic đi vào thì khi request tới sẽ nhận được response mà không cần explicit allow Outbound. Khác với Network ACL.*
- **Network Access Controll List (ACL)**: được apply ở **cấp độ subnet, tương tự như security group** nhưng có rule Deny và các rule được đánh độ ưu tiên. Khi tạo VPC sẽ có **một ACL mặc định** được apply cho toàn bộ subnet trong VPC **là mở all traffic không chặn gì cả**.
    - Control network in/out đối với subnet được associate.
    - Mỗi rule sẽ có các thông số:
        - Priority
        - Allow/Deny
        - Protocol
        - Port range
        - Source IP / Destination IP
    - Default ACL sẽ allow all.
    - Sử dụng quá nhiều rule của ACL sẽ làm giảm performance.
    - Rule của ACL là **stateless** (đặc điểm phân **khác với Security Group**).
    - **Trong production, người ta thường dùng Security Group thay vì ACL**.
- VPC Flow Log: capture các thông tin di chuyển của traffic trong network.
- VPN Connection: kết nối VPC trên AWS với hệ thống dưới On-premise.
- Elastic Network Interface: đóng vai trò như 1 card mạng ảo.
- **Internet Gateway**: Kết nối VPC với Internet, là **cổng vào từ internet tới các thành phần trong VPC**.
    - Là cửa ngõ để truy cập các thành phần trong VPC.
    - **Nếu VPC không được gắn Internet Gateway thì không thể kết nối SSH tới instance kể cả instance đó có được gắn public IP**.
    - Mặc định default-vpc do AWS tạo sẵn đã có gắn Internet Gateway.
- **NAT Gateway**: dịch vụ NAT của AWS cho phép các thành phần bên trong kết nối tới internet nhưng không cho bên ngoài kết nối tới.
    - Giúp cho các instance trong **Private Subnet có thể đi ra internet mà không cần tới public IP**, **ví dụ khi EC2 cần tải một package nào đó**.
    - Giúp tăng cường bảo mật cho các resource cần private (App, DB).
- **VPC Endpoint**: **kênh kết nối private giúp kết nối tới các services khác của AWS mà không đi qua internet**.
    - Giúp các resource trong VPC có thể kết nối tới các dịch vụ khác của AWS thông qua private connection.
    - Công dụng: secure, tăng tốc độ.
    - Có 2 loại endpoint là Gateway Endpoint (S3, Dynamodb) và Interface Endpoint (SQS, CloudWatch,...)
    - Endpoint có thể được cấu hình Security Group để hạn chế truy cập.
- Peering connection: kênh kết nối giữa 2 VPC.
- Transit gateways: đóng vai trò như 1 hub đứng giữa các VPCs, VPN Connection, Direct Connect.

<p align="center">
    <img src = "docs_imgs\vpc_minhhoa.png" width="700">
</p>











<!-- Thêm lí thuyết vào trước dòng này -->






<br><br>


## <div align="center"><strong>LAB</strong></div>


<summary>&nbsp;&nbsp;<strong>Lab 01</strong></summary>
<!-- Thêm lab vào sau dòng này -->

<p align="center">
    <img src = "docs_imgs\vpc_lab01.png" width="500">
</p>

**Bài làm:**


<p align="center">
    <img src = "docs_imgs\vpc_lab01_drawio.png" width="500">
</p>






<!-- Thêm lab vào trước dòng này -->


<details>
<summary>&nbsp;&nbsp;<strong>Lab 02</strong></summary>
<!-- Thêm lab vào sau dòng này -->

<p align="center">
    <img src = "docs_imgs\vpc_lab02.png" width="500">
</p>













<!-- Thêm lab vào trước dòng này -->
</details> 

<br><br><br><br><br>

## <div align="center"><strong>TÀI LIỆU BỔ SUNG</strong></div>

- Nội dung phục vụ lab: [Code](/labs/sec_14)
- AWS Console liên quan:
    - [VPC](/aws_console/README.md#virtual-private-cloud-vpc)
