<h1 align="center">
    <strong>API GATEWAY & COGNITO</strong>
</h1>

## MỤC LỤC

- [Lí thuyết](#lí-thuyết)
    - [API Gateway](#api-gateway)
- [Lab](#lab)
- [Tài liệu bổ sung](#tài-liệu-bổ-sung)


## <div align="center"><strong>LÍ THUYẾT</strong></div>


### <div><strong>API GATEWAY</strong></div>
<!-- Thêm lí thuyết API Gateway vào sau dòng này -->

- **API Gateway - Định nghĩa**
    - Là một dịch vụ **của AWS** giúp **xây dựng, quản lý và bảo mật** các **RESTful API hoặc WebSocket**. 
    - Là một dịch vụ quan trọng trong kiến trúc dựa trên các dịch vụ của AWS (**AWS-based microservices architecture**) và **thường được sử dụng cùng với các dịch vụ AWS khác như AWS Lambda, EC2, S3, Amazon DynamoDB**.
    - AWS API Gateway cung cấp các tính năng:
        - Cho phép **thiết kế và phát triển API RESTful hoặc WebSocket thông qua web GUI**.
        - **Điều phối API** đến các hệ thống hoặc dịch vụ khác nhau.
        - **Authen/Author request** tới các API hoặc mã hóa secure communication giữa các hệ thống khác nhau.
        - **Quản lý và giám sát các yêu cầu API**, ví dụ số lượng request, response time...

- **Đặc trưng của API Gateway**
    - Là một fully managed service của AWS với khả năng **Scale và High Availablity không giới hạn**.
    - **Zero idle cost**, easy to setup.
    - **Dễ dàng kết hợp với các dịch vụ khác** như CloudWatch, WAF cho mục đích monitor & security.

- **Hệ sinh thái API Gateway**

    API Gateway là một service chủ yếu có nhiệm vụ nhận request của client sau đó forward tới các service phía sau.

    <p align="center">
        <img src = "docs_imgs\apigw_ecosystem.png" width="380">
    </p>


- **Khi nào nên sử dụng API Gateway?**

    API Gateway phù hợp cho những bài toán sau:

    - Kiến trúc **Micro-service sử dụng lambda làm backend**.
    - **Backend API cho hầu hết các use case** (web API, IoT).
    - Gateway nhận data trực tiếp từ client sau đó lưu vào DynamoDB (DB First).
    - Web Socket cho những hệ thống realtime communication.


- **Pricing**

    API Gateway là một dịch vụ có idle cost = 0. Người dùng chỉ trả tiền cho chi phí chạy thực tế, cụ thể:
    - Với REST API:
        - Số lượng request (Vd Singapore region:$ 4.25/1M requests).
        - Data transfer out ($/GB).
        - Caching size tính theo GB/hour.
    - Với Web socket:
        - Message number (đối với Web socket). Vd $1.15/1M message với block 32KB.
        - Connection minutes: $0.288/1M connection minutes.

- **Authentication cho API Gateway**

    API Gateway cung cấp 2 phương thức authen tích hợp trực tiếp (authorizer) thường được sử dụng đó là:
    - **Cognito Authorizer**: **liên kết trực tiếp** với một **Cognito User Pool sử dụng làm authorizer**.

        **Khi access API, client passing trực tiếp token lấy được thông qua login với Cognito**, **API Gateway sẽ check token và allow access nếu token hợp lệ**.

        *Ví dụ:*

        <p align="center">
            <img src = "docs_imgs\apigw_cognito_author.png" width="450">
        </p>


    - **Lambda Authorizer** (custom authorizer): **tự implement logic authen trên Lambda**.

        **Khi sử dụng loại authorizer này**, **có 2 hình thức authen** là dựa vào **TOKEN (JWT) hoặc request parameter based** (VD username/password).

        *Ví dụ:*

        <p align="center">
            <img src = "docs_imgs\apigw_lambda_author.png" width="450">
        </p>







<!-- Thêm lí thuyết API Gateway vào trước dòng này -->






<br><br>


## <div align="center"><strong>LAB</strong></div>

<details>
<summary>&nbsp;&nbsp;<strong>Lab 01</strong></summary>
<!-- Thêm lab vào sau dòng này -->






<!-- Thêm lab vào trước dòng này -->
</details> 

<br><br><br><br><br>

## <div align="center"><strong>TÀI LIỆU BỔ SUNG</strong></div>

- Nội dung phục vụ lab: [Code](/labs/sec_15)
- AWS Console liên quan:
    - [API Gateway](/aws_console/README.md#tên-dịch-vụ-1)
    - [Cognito](/aws_console/README.md#tên-dịch-vụ-2)