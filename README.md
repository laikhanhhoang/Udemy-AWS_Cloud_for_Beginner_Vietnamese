# AWS Cloud for beginner (Vietnamese)

Đây là repo cho khóa học về AWS cơ bản trên Udemy.

Nội dung file **`README.md`**:
- [**Kiến thức bổ sung**](#kiến-thức-bổ-sung)
- [**Mục lục**](#mục-lục)
- [**Tóm tắt nội dung từng Section**](#tóm-tắt-nội-dung-từng-section)


## Kiến thức bổ sung

<details>
<summary><strong>HTTP & HTTPS</strong></summary>

- Nếu không chỉ định port trong URL, trình duyệt sẽ sử dụng port mặc định của giao thức: 80 cho HTTP và 443 cho HTTPS.

</details>


<details>
<summary><strong>OSI Model</strong></summary>

<p align="center">
    <img src="md_imgs\osi_model.jpg" width="400" />
</p>

- Tóm tắt
    - **Physical**: Truyền tín hiệu vật lý dưới dạng bit (0 và 1) qua dây điện, cáp quang hoặc sóng WiFi.

    - **Data Link**: Là frame gồm **Dst MAC - Src MAC - EtherType - Payload (IP Packet) - FCS**.  
    Truyền dữ liệu trong LAN và phát hiện lỗi frame.

    - **Network**: Là IP packet gồm **Src IP - Dst IP - TTL - Protocol - Payload (TCP/UDP Segment)**.  
    Định tuyến dữ liệu qua nhiều mạng.

    - **Transport**: Là segment/datagram gồm **Src Port - Dst Port - Sequence Number, ACK, Flags (HTTP) hoặc Length, Checksum (UDP) - Payload (Application Data)**.  
    Quản lý giao tiếp giữa các ứng dụng.

    - **Session**: Quản lý phiên giao tiếp gồm **Session Establishment - Session Maintenance - Session Termination**.  
    Thực tế thường được xử lý ở tầng Application hoặc trong TLS.

    - **Presentation**: Xử lý **Encryption/Decryption - Compression - Data Encoding (UTF-8, JSON, …)**.

    - **Application**: Chứa dữ liệu ứng dụng như HTTP message gồm **Method - Path - Headers - Body**.  
    Đây là nơi Django, browser, API hoạt động.

- Tầng Transport (Layer 4)

    Tầng Transport chịu trách nhiệm truyền dữ liệu giữa các process (ứng dụng) trên hai máy khác nhau thông qua port. Nó đảm bảo phân biệt ứng dụng nào đang gửi/nhận dữ liệu và quyết định cách truyền có đáng tin cậy hay không (reliable hay không reliable).

- TCP/UDP là gì, đóng gói cái gì?

    Transmission Control Protocol (TCP) và User Datagram Protocol (UDP) là hai giao thức của tầng Transport. Chúng đóng gói dữ liệu từ tầng Application (ví dụ HTTP) thành segment (TCP) hoặc datagram (UDP), thêm header chứa source port và destination port, rồi chuyển xuống tầng IP để gửi đi. TCP đảm bảo thứ tự và độ tin cậy; UDP thì không.

- Ví dụ với Django request

    Khi client gửi một HTTP request đến server chạy Django, request đó được đóng gói thành HTTP message \
    → được TCP đóng thành segment (thêm port) \
    → được IP đóng thành packet (thêm địa chỉ IP) \
    → truyền qua mạng. 
    
    Ở phía server, dữ liệu được tháo ngược lại và web server (gunicorn/uwsgi) chuyển HTTP request đã hoàn chỉnh cho Django xử lý ở tầng Application.

</details>

## Mục lục
- [Section 07 - EC2](docs/sec07_EC2.md)
- [Section 08 - IAM](docs/sec08_IAM.md)
- [Section 09 - S3](docs/sec09_S3.md)
- [Section 10 - ELB & ASG](docs/sec10_ELB&ASG.md)
- [Section 11 - RDS](docs/sec11_RDS.md)
- [Section 12 - DynamoDB](docs/sec12_DynamoDB.md)
- [Section 13 - Lambda](docs/sec13_Lambda.md)
- [Section 14 - VPC](docs/sec14_VPC.md)
- [Section 15 - API Gateway & Cognito](docs/sec15_APIGW&Cognito.md)
- [Section 16 - CloudFront](docs/sec16_CloudFront.md)
- [Section 17 - Route 53](docs/sec17_Route53.md)
- [Section 18 - CloudWatch & CloudTrail](docs/sec18_CloudWatch&CloudTrail.md)

## Tóm tắt nội dung từng Section

- [Section 07 - EC2](docs/sec07_EC2.md)




- [Section 08 - IAM](docs/sec08_IAM.md)
    - [Policy](docs/sec08_IAM.md#policy): quy định việc ai/cái gì có thể hoặc không thể làm gì.
    - [User](docs/sec08_IAM.md#user): dại diện cho 1 profile của 1 người dùng trên AWS account.
    - [Role](docs/sec08_IAM.md#role): cấp quyền cho 1 thực thể có thể tương tác với các resources khác trên AWS.
    - [Group](docs/sec08_IAM.md#group): đại diện cho 1 nhóm user trên hệ thống.



- [Section 09 - S3](docs/sec09_S3.md)
    - [Định nghĩa](docs/sec09_S3.md#định-nghĩa)
    - [Các tính năng cơ bản](docs/sec09_S3.md#các-tính-năng-cơ-bản)
    - [Kết hợp dịch vụ khác với S3](docs/sec09_S3.md#kết-hợp-dịch-vụ-khác-với-s3)
    - [S3 Storage Class](docs/sec09_S3.md#s3-storage-class): S3 Standard, S3 Standard-IA, S3 Glacier,...
    - [S3 Lyfe Cycle](docs/sec09_S3.md#s3-lyfe-cycle): Tính năng cho phép tự động move object xuống các class lưu trữ thấp hơn hoặc xoá luôn sau một khoảng thời gian nhằm tiết kiệm chi phí.
    - [S3 event trigger](docs/sec09_S3.md#s3-event-trigger): cung cấp cơ chế trigger 1 event sang dịch vụ khác khi có thay đổi đối với object (upload, delete).
    - [Best Practices](docs/sec09_S3.md#best-practices)



- [Section 10 - ELB & ASG](docs/sec10_ELB&ASG.md)
    - [Elastic Load Balancer](docs/sec10_ELB&ASG.md#elastic-load-balancer)
        - "Single point of failure" - Sự ra đời của Load Balancer
        - [Elastic Load Balancing - Định nghĩa](docs/sec10_ELB&ASG.md#elastic-load-balancing---định-nghĩa)
        - [Các thành phần cơ bản của Load Balancer](docs/sec10_ELB&ASG.md#các-thành-phần-cơ-bản-của-load-balancer)
        - [Các loại Load Balancer](docs/sec10_ELB&ASG.md#các-loại-load-balancer): Application Load Balancer, Network Load Balancer, Gateway Load Balancer.
        - [Cách Load Balancer hoạt động](docs/sec10_ELB&ASG.md#cách-load-balancer-hoạt-động)

            <p align="center">
                <img src="docs\docs_imgs\elb_theory.png" width="500" />
            </p>

        - [Load Balancer tính phí như thế nào?](docs/sec10_ELB&ASG.md#load-balancer-pricing)
        - [Cross zone load balancer](docs/sec10_ELB&ASG.md#cross-zone-load-balancer)
        - [Một số lưu ý về Load Balancer](docs/sec10_ELB&ASG.md#một-số-lưu-ý-về-load-balancer)
    - [Auto Scaling Groups](docs/sec10_ELB&ASG.md#auto-scaling-groups)
        - Scaling
        - Auto Scaling Groups - Định nghĩa
        - Launch Configuration and Launch Template
        - Các phương pháp scale hệ thống
    - AWS Console liên quan:
        - [ELB](/aws_console/README.md#elastic-load-balancer-elb)
        - [ASG](/aws_console/README.md#auto-scaling-groups-asg)



- [Section 11 - RDS](docs/sec11_RDS.md)
    - [RDS - Định nghĩa](docs/sec11_RDS.md#rds---định-nghĩa)
    - [Các tính năng cơ bản của RDS](docs/sec11_RDS.md#các-tính-năng-cơ-bản-của-rds)
    - [RDS Usecase](docs/sec11_RDS.md#rds-usecase)
    - [Các mô hình triển khai RDS](docs/sec11_RDS.md#các-mô-hình-triển-khai-rds): Single Instance, Single Instance with Multi-AZ, Master – Read Only cluster, Master – Read Only cluster with Multi-AZ, Master – Multi Read cluster.
    - [Aurora](docs/sec11_RDS.md#aurora)
    - [Parameter Groups](docs/sec11_RDS.md#parameter-groups): thông qua 1 cơ chế gián tiếp để can thiệp vào setting ở cấp độ DB (không phải setting OS).

        <p align="center">
            <img src="docs\docs_imgs\rds_para_groups.png" width="500" />
        </p>

    - AWS Console liên quan: [Relational Database Service](/aws_console/README.md#relational-database-service-rds).




- [Section 12 - DynamoDB](docs/sec12_DynamoDB.md)
- [Section 13 - Lambda](docs/sec13_Lambda.md)
    - [Lambda - Định nghĩa](docs/sec12_DynamoDB.md#lambda---định-nghĩa)
    - [Đặc trưng của Lambd](docs/sec12_DynamoDB.md#đặc-trưng-của-lambda)
    - [Hệ sinh thái Lambda](docs/sec12_DynamoDB.md#hệ-sinh-thái-lambda)
    - [Ưu/Nhược điểm](docs/sec12_DynamoDB.md#ưunhược-điểm)
    - [Khi nào dùng Lambda ?](docs/sec12_DynamoDB.md#khi-nào-dùng-lambda-)
    - [Lưu ý](docs/sec12_DynamoDB.md#lưu-ý)
    - AWS Console liên quan: [Lambda](/aws_console/README.md#lambda).



- [Section 14 - VPC](docs/sec14_VPC.md)

    <p align="center">
        <img src = "docs\docs_imgs\vpc_minhhoa.png" width="600">
    </p>

    - [VPC - Định nghĩa](docs/sec14_VPC.md#vpc---định-nghĩa): **tạo một mạng ảo** (virtual network) và **control toàn bộ network in/out của mạng đó**.
    - [Các thành phần cơ bản của VPC](docs/sec14_VPC.md#các-thành-phần-cơ-bản-của-vpc): **Security Group**, **Security Group**, **Internet Gateway**, **NAT Gateway**, **VPC Endpoint**,...
    - AWS Console liên quan: [VPC](/aws_console/README.md#virtual-private-cloud-vpc).



- [Section 15 - API Gateway & Cognito](docs/sec15_APIGW&Cognito.md)
    - [API Gateway](docs/sec15_APIGW&Cognito.md#api-gateway)

        - [API Gateway - Định nghĩa](docs/sec15_APIGW&Cognito.md#api-gateway---định-nghĩa): **xây dựng, quản lý và bảo mật** các **RESTful API hoặc WebSocket**.
        - [Đặc trưng của API Gateway](docs/sec15_APIGW&Cognito.md#đặc-trưng-của-api-gateway)
        - [Hệ sinh thái API Gateway](docs/sec15_APIGW&Cognito.md#hệ-sinh-thái-api-gateway)
        - [Khi nào nên sử dụng API Gateway?](docs/sec15_APIGW&Cognito.md#khi-nào-nên-sử-dụng-api-gateway)
        - [Pricing](docs/sec15_APIGW&Cognito.md#api-gateway-pricing)
        - [Authentication cho API Gateway](docs/sec15_APIGW&Cognito.md#authentication-cho-api-gateway): gồm 2 phương thức tích hợp trực tiếp: **Cognito Authorizer** và **Lambda Authorizer**.
    - AWS Console liên quan:
        - [API Gateway](/aws_console/README.md#api-gateway)
        - [Cognito](/aws_console/README.md#cognito)



- [Section 16 - CloudFront](docs/sec16_CloudFront.md)

    - [CDN - Định nghĩa](docs/sec16_CloudFront.md#cdn---định-nghĩa): **giúp delivery nội dung nhanh chóng nhờ cache tài nguyên** trên máy chủ gần.
    - [Cloud Front - Khái niệm cơ bản](docs/sec16_CloudFront.md#cloud-front---khái-niệm-cơ-bản): cache, loggin, security.
    - [Cách hoạt động của Cloud Front](docs/sec16_CloudFront.md#cách-hoạt-động-của-cloud-front)
    - [Usecase](docs/sec16_CloudFront.md#usecase)
    - [Pricing](docs/sec16_CloudFront.md#pricing)
    - [CloudFront behavior](docs/sec16_CloudFront.md#cloudfront-behavior): **điều hướng request tới đúng origin mong muốn** như **`/*`, `api/*`**,...

        <p align="center">
            <img src = "docs\docs_imgs\cloudfront_behaviour_example.png" width="500">
        </p>

    - [CloudFront Cache Policy](docs/sec16_CloudFront.md#cloudfront-cache-policy): **tùy chỉnh cách cache và serves content**: có thể cache hoặc không đối với API,...
    - [CloudFront Origin request policy](docs/sec16_CloudFront.md#cloudfront-cache-policy): định nghĩa cách CloudFront **xử lý request tới origin** như S3, EC2, ALB,...
    - AWS Console liên quan: [Cloud Front](/aws_console/README.md#cloud-front).



- [Section 17 - Route 53](docs/sec17_Route53.md)
    - [Route 53 - Định nghĩa](docs/sec17_Route53.md#route-53---định-nghĩa): **quản lý các tên miền và ánh xạ chúng**.
    - [3 tính năng chính](docs/sec17_Route53.md#3-tính-năng-chính)
    - [DNS Resolver](docs/sec17_Route53.md#dns-resolver)
    - [Route 53 Hosted Zone](docs/sec17_Route53.md#route-53---định-nghĩa)
        - Các loại DNS Record: **`A (domain → IP), CNAME (domain → domain)`**,...

        - **Ví dụ**:

            <p align="center">
                <img src = "docs\docs_imgs\route53_dns_record_subdomain.png" width="500">
            </p>

            - **api.linhnguyen.click** thuộc **A record**.
            - **web.linhnguyen.click** thuộc **CName record**.

    - [Routing policy](docs/sec17_Route53.md#routing-policy): Simple, Weighted, Latency,...
    - [SSL Certificate](docs/sec17_Route53.md#ssl-certificate): Do ACM cấp cho một **domain hoặc subdomain duy nhất** để **xác minh quyền sở hữu domain**.
    - [Pricing](docs/sec17_Route53.md#pricing)
    - [Lưu ý](docs/sec17_Route53.md#lưu-ý)
    - AWS Console liên quan: [Route 53](/aws_console/README.md#route-53).

- [Section 18 - CloudWatch & CloudTrail](docs/sec18_CloudWatch&CloudTrail.md)
    - [Cloudwatch](#cloudwatch)
        - [Cloudwatch - Định nghĩa](#cloudwatch---định-nghĩa): **giám sát và quản lý hệ thống và ứng dụng trên nền tảng AWS**.
        - [Các thành phần của Cloudwatch](#các-thành-phần-của-cloudwatch)
            - [Cloudwatch Metrics](#cloudwatch-metrics): **các thông số đo lường được thu thập và lưu trữ bởi CloudWatch**.
            - [Cloudwatch Alarm](#cloudwatch-alarm): **tạo ra cảnh báo tự động dựa trên các giá trị và threshold trong Metrics**.
            - [Cloudwatch Log](#cloudwatch-log): **lưu trữ, xem và phân tích các logs từ các ứng dụng và hệ thống trong môi trường AWS cũng như on-premise**. 

                &nbsp;&nbsp;&nbsp;&nbsp; *Cách **phân biệt Log và Metric** xem ở **Lưu ý**.*

            - [Cloudwatch Log Insight](#cloudwatch-log-insight): **một công cụ cho phép bạn truy vấn log thông qua một cú pháp do AWS định nghĩa**.
            - [Cloudwatch Insight](#cloudwatch-insight)
            - [Xray](#xray): cung cấp cái nhìn toàn cảnh và chi tiết đường đi của request trong application.
            - [Cloudwatch Dashboard](#cloudwatch-dashboard)
            - [Setup Cloudwatch Agent cho Custom Metricks và intergrate vào Cloudwatch Dashboard](#setup-cloudwatch-agent-cho-custom-metric)
                - Cách **phân biệt Metric và Log của từng Resource** trên Cloudwatch: dùng **namespace và dimension** với **metric**, **log group và log stream** cho **log**.
                - Cách **cài và chạy Cloudwatch Agent**: **`Tạo Iam Policy cho EC2` → `Config metric&log agent setting` → `Fetch setting json và launch`**.
            - [Lưu ý](#lưu-ý)
                - Khi thiết kế monitor cần quan tâm những gì?
                - Chú ý cho CW Alarm trong quá trình thiết kế và setting
            - [Pricing](#pricing)
        - [Cloudtrail](#cloudtrail)
            - [Cloudtrail - Định nghĩa](#cloudtrail---định-nghĩa): **Khác với CloudWatch** có mục đích **giám sát tình trạng của resource**, **CloudTrail ghi lại những hành động đã được thực thi** trong môi trường AWS **(who did what?)**.
            - [Các chức năng cơ bản của CloudTrail](#các-chức-năng-cơ-bản-của-cloudtrail)
    - AWS Console liên quan:
        - [CloudWatch](/aws_console/README.md#cloudwatch)
        - [CloudTrail](/aws_console/README.md#cloudtrail)




















