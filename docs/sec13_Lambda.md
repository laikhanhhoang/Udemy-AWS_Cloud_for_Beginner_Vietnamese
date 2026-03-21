<h1 align="center">
    <strong>LAMBDA</strong>
</h1>

## MỤC LỤC

- [Lí thuyết](#lí-thuyết)
    - [Lambda - Định nghĩa](#lambda---định-nghĩa)
    - [Đặc trưng của Lambd](#đặc-trưng-của-lambda)
    - [Hệ sinh thái Lambda](#hệ-sinh-thái-lambda)
    - [Ưu/Nhược điểm](#ưunhược-điểm)
    - [Khi nào dùng Lambda ?](#khi-nào-dùng-lambda-)
    - [Lưu ý](#lưu-ý)
- [Lab](#lab)
- [Tài liệu bổ sung](#tài-liệu-bổ-sung)
    - Nội dung phục vụ lab: [Code](/labs/sec_13)
    - AWS Console liên quan:
        - [Lambda](/aws_console/README.md#lambda)


## <div align="center"><strong>LÍ THUYẾT</strong></div>
<!-- Thêm lí thuyết vào sau dòng này -->

#### **Lambda - Định nghĩa**
- **Là** một service serverless của AWS cho phép người dùng thực thi code mà không cần quan tâm tới hạ tầng phía sau.
- Lamda hỗ trợ các ngôn ngữ (runtime) sau:
    - Java
    - Python
    - .NET
    - GO
    - Ruby
    - Custom Runtime
    - Container

- Lambda function bản thân nó không có một link public trực tiếp như một website. Để truy cập Lambda từ bên ngoài, bạn cần dùng một trong các cách sau:
    - API Gateway:
        - Tạo REST API hoặc HTTP API trên API Gateway và **liên kết với Lambda bằng cách tạo IAM Role cho API GW ở Resource-based Policy trong Lambda**.

            <p align="center">
                <img src = "docs_imgs\lambda_resourcebased_policy.png" width="700">
            </p>

        - API Gateway sẽ cung cấp một URL public, ví dụ: **`https://abcd1234.execute-api.us-east-1.amazonaws.com/prod/my-function`**.
    - Application Load Balancer (ALB).
    - AWS Lambda Function URL (mới).

#### **Đặc trưng của Lambda**
- Khi tạo 1 lambda function, bạn quyết định cấu hình thông qua thông số Memory. Min = 128MB, Max = 10GB. Memory càng cao, CPU được allocate càng lớn.
- Lambda khi khởi chạy được cấp phát 1 vùng nhớ tạm min = 512MB max =10GB, sẽ bị xoá khi lambda thực thi xong.
- Timeout tối đa 15 phút (quá thời gian này nếu execute chưa xong vẫn tính là failed và bị thu hồi resource).
- Lambda có thể được trigger từ nhiều nguồn: Trigger trực tiếp (Console or CLI), API Gateway, event từ các service khác (S3, SQS, DynamoDB, Kinesis, IoT...), hoặc chạy theo lịch (trigger từ EventBridge).
- Lambda có 2 mode chạy là chạy ngoài VPC và chạy trong VPC. Thông thường nếu lambda cần kết nối với RDS Database, ElastiCache thì cần để trong VPC. 
- Lambda sau khi chạy xong sẽ không lưu lại bất cứ gì, cần có các dịch vụ khác để lưu trữ kết quả đã tính toán. Do vậy, nên lưu Log/File output/Data output > CloudWatch log/S3/RDS hoặc DynamoDB.
- Lambda cũng cần được cấp IAM Role để có thể tương tác với các resource khác. Mặc định Lambda khi tạo ra sẽ được gán Role có các quyền cơ bản (vd write log to CloudWatch).
- Lambda không chỉ chứa 1 file code mà có thể chứa các file library, file common,... Để tiện dụng ta có thể gom nhóm chúng lại thành các layer và tái sử dụng ở nhiều function, tránh duplicate code.
- **Khi có nhiều request từ client**, Lambda scale horizontal bằng cách gia tăng số lượng concurent execute **(sacle ngang)**. Giới hạn này **mặc định** khi tạo account AWS là **10 concurent executions**. Cần request tăng số này lên trước khi release production.
- Lambda có thể được cài đặt một số reserve concurent để tránh bị ảnh hưởng bởi các lambda khác.


#### **Hệ sinh thái Lambda**

Lambda là một service có thể liên kết với gần như tất cả các service khác của AWS, miễn là nó được cấp IAM Role phù hợp.


#### **Ưu/Nhược điểm**
- Ưu điểm
    - Không tốn effort cho quản trị hạ tầng, High Availablity.
    - Zero idle cost. Do lambda chỉ phát sinh chi phí khi chạy, nếu hệ thống không phát sinh nhu cầu sử dụng nên cost gần như zero.
    - Kết hợp được với nhiều service của AWS.
    - Khả năng scale mạnh mẽ (bằng cách nhân bản số lượng concurrent).
    - Support nhiều ngôn ngữ.
    - Dễ dàng triển khai bằng nhiều tool do AWS phát hành hoặc 3rd party.

- Nhược điểm
    - Cold start: Code cần thời gian để nạp lên memory trước khi thực sự bắt đầu chạy. Gần đây AWS đã cải thiện được vấn đề này rất nhiều.
    - Giới hạn về bộ nhớ: 10GB. Không phù hợp cho các tác vụ nặng.
    - Khó tích hợp. Hệ thống để deploy lên lambda cần chia nhỏ do đó làm tăng tính phức tạp và khó debug.
    - Giới hạn về thời gian chạy, max 15min. Không phù hợp với các tác vụ tính toán tốn thời gian.
    - Không lưu lại trạng thái sau khi chạy. Cần có external storage, database, logging.

#### **Khi nào dùng Lambda ?**
- Khi nào nên sử dụng Lambda?
    Do những ưu điểm kể trên, Lambda phù hợp cho những usecase
    - Tác vụ automation trên AWS, nhận trigger từ các AWS service như S3, DynamoDB, SNS, SQS,...
    - Backend cho API hoặc IoT (kết hợp với API Gateway).
    - Xử lý data trong bài toán data ETL.
    - Hệ thống có kiến trúc microservice nói chung.
    - Công ty start up muốn tối ưu cost cho giai đoạn đầu.

- Khi nào KHÔNG nên sử dụng Lambda?
    Do những hạn chế kể trên, Lambda Không phù hợp cho những usecase
    - Hệ thống Monolithic (do souce code quá nặng) hoặc team không có kinh nghiệm phát triển hệ thống microservice.
    - Xử lý dữ liệu lớn, phân tích, tổng hợp data (hoặc chạy nhiều hơn 15p).
    - Machine Learning.

#### **Lưu ý**

- Mọi IAM user / IAM role trong cùng account đều dùng chung concurrency quota của account đó.

    ```
    AWS Account (quota concurrency)
    │
    ├── IAM user A
    │     └── Lambda function A
    │
    ├── IAM user B
    │     └── Lambda function B
    │
    └── IAM role C
        └── Lambda function C
    ```
    Tất cả các Lambda này **dùng chung concurrency quota của account**.
    
    ```
    Total concurrency = X

    Unreserved ≥ 10

    Σ Reserved ≤ X - 10

    Provisioned_Ri ≤ Reserved_Ri
    ```


<!-- Thêm lí thuyết vào trước dòng này -->






<br><br>


## <div align="center"><strong>LAB</strong></div>

<details>
<summary>&nbsp;&nbsp;<strong>Lab 01</strong></summary>
<!-- Thêm lab vào sau dòng này -->

<p align="center">
    <img src = "docs_imgs\lambda_lab01.png" width="500">
</p>

<!-- Thêm lab vào trước dòng này -->
</details> 


<details>
<summary>&nbsp;&nbsp;<strong>Lab 02</strong></summary>
<!-- Thêm lab vào sau dòng này -->

<p align="center">
    <img src = "docs_imgs\lambda_lab02.png" width="500">
</p>

<!-- Thêm lab vào trước dòng này -->
</details> 


<details>
<summary>&nbsp;&nbsp;<strong>Lab 03</strong></summary>
<!-- Thêm lab vào sau dòng này -->

<p align="center">
    <img src = "docs_imgs\lambda_lab03.png" width="500">
</p>

<!-- Thêm lab vào trước dòng này -->
</details> 

<br><br><br><br><br>

## <div align="center"><strong>TÀI LIỆU BỔ SUNG</strong></div>

- Nội dung phục vụ lab: [Code](/labs/sec_13)
- AWS Console liên quan:
    - [Lambda](/aws_console/README.md#lambda)
