<h1 align="center">
    <strong>API GATEWAY & COGNITO</strong>
</h1>

## MỤC LỤC

- [Lí thuyết](#lí-thuyết)
    - [API Gateway](#api-gateway)
        - [API Gateway - Định nghĩa](#api-gateway---định-nghĩa): **xây dựng, quản lý và bảo mật** các **RESTful API hoặc WebSocket**.
        - [Đặc trưng của API Gateway](#đặc-trưng-của-api-gateway)
        - [Hệ sinh thái API Gateway](#hệ-sinh-thái-api-gateway)
        - [Khi nào nên sử dụng API Gateway?](#khi-nào-nên-sử-dụng-api-gateway)
        - [Pricing](#api-gateway-pricing)
        - [Authentication cho API Gateway](#authentication-cho-api-gateway): gồm 2 phương thức tích hợp trực tiếp: **Cognito Authorizer** và **Lambda Authorizer**.
    - [Cognito](#cognito)
        - [AWS Cognito - Định nghĩa](#aws-cognito---định-nghĩa)
        - [Tính năng cơ bản của Cognito](#tính-năng-cơ-bản-của-cognito)
        - [Pricing](#pricing)
        - [Hạn chế của Cognito](#hạn-chế-của-cognito)
- [Lab](#lab)
- [Tài liệu bổ sung](#tài-liệu-bổ-sung)
    - Nội dung phục vụ lab: [Code](/labs/sec_15)
    - AWS Console liên quan:
        - [API Gateway](/aws_console/README.md#api-gateway)
        - [Cognito](/aws_console/README.md#cognito)


## <div align="center"><strong>LÍ THUYẾT</strong></div>


### <div><strong>API GATEWAY</strong></div>
<!-- Thêm lí thuyết API Gateway vào sau dòng này -->

#### **API Gateway - Định nghĩa**
- Là một dịch vụ **của AWS** giúp **xây dựng, quản lý và bảo mật** các **RESTful API hoặc WebSocket**. 
- Là một dịch vụ quan trọng trong kiến trúc dựa trên các dịch vụ của AWS (**AWS-based microservices architecture**) và **thường được sử dụng cùng với các dịch vụ AWS khác như AWS Lambda, EC2, S3, Amazon DynamoDB**.
- AWS API Gateway cung cấp các tính năng:
    - Cho phép **thiết kế và phát triển API RESTful hoặc WebSocket thông qua web GUI**.
    - **Điều phối API** đến các hệ thống hoặc dịch vụ khác nhau.
    - **Authen/Author request** tới các API hoặc mã hóa secure communication giữa các hệ thống khác nhau.
    - **Quản lý và giám sát các yêu cầu API**, ví dụ số lượng request, response time...

#### **Đặc trưng của API Gateway**
- Là một fully managed service của AWS với khả năng **Scale và High Availablity không giới hạn**.
- **Zero idle cost**, easy to setup.
- **Dễ dàng kết hợp với các dịch vụ khác** như CloudWatch, WAF cho mục đích monitor & security.

#### **Hệ sinh thái API Gateway**

API Gateway là một service chủ yếu có nhiệm vụ nhận request của client sau đó forward tới các service phía sau.

<p align="center">
    <img src = "docs_imgs\apigw_ecosystem.png" width="380">
</p>


#### **Khi nào nên sử dụng API Gateway?**

API Gateway phù hợp cho những bài toán sau:

- Kiến trúc **Micro-service sử dụng lambda làm backend**.
- **Backend API cho hầu hết các use case** (web API, IoT).
- Gateway nhận data trực tiếp từ client sau đó lưu vào DynamoDB (DB First).
- Web Socket cho những hệ thống realtime communication.


#### **API Gateway Pricing**

API Gateway là một dịch vụ có idle cost = 0. Người dùng chỉ trả tiền cho chi phí chạy thực tế, cụ thể:
- Với REST API:
    - Số lượng request (Vd Singapore region:$ 4.25/1M requests).
    - Data transfer out ($/GB).
    - Caching size tính theo GB/hour.
- Với Web socket:
    - Message number (đối với Web socket). Vd $1.15/1M message với block 32KB.
    - Connection minutes: $0.288/1M connection minutes.

####  **Authentication cho API Gateway**

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




### <div><strong>COGNITO</strong></div>
<!-- Thêm lí thuyết Cognito vào sau dòng này -->

#### **AWS Cognito - Định nghĩa**
- Là một dịch vụ **quản lý danh tính và xác thực người dùng** của Amazon Web Services (AWS). 
- Dịch vụ này cho phép bạn tạo ra các ứng dụng web và di động an toàn với khả năng xác thực người dùng, phân quyền, và đăng nhập với nhiều tùy chọn như user account, Social login hoặc đăng nhập qua Identity Provider.

#### **Tính năng cơ bản của Cognito**
- **Đăng ký & Xác thực người dùng** sử dụng **username/pw/email hoặc tài khoản mạng xã hội**.
- **Phân quyền** người dùng vào các ứng dụng hoặc tài nguyên
- **Xác thực** email/số điện thoại.
- Tích hợp với các dịch vụ khác (API Gateway, Lambda) để xây dựng ứng dụng.
- Hỗ trợ cho ứng dụng di động (iOS,Android) thông qua SDK
- Cognito sync: sync data giữa các mobile device với nhau
- Advanced Security: giám sát & phân tích truy cập của user để phát hiện và ngăn chặn truy cập bất thường (optional).

#### **Pricing**

Pricing của Cognito dựa trên:
- Số lượng Monthly Active User. VD ở Singapore là $0.0055/MAU (càng lên cao càng rẻ)
- User sign in thông qua SAML hoặc OIDC: $0.015/MAU
- Tính năng Advance Security: $0.05/MAU nếu enable
- SMS trong trường hợp gửi message MFA: Tuỳ theo khu vực.

#### **Hạn chế của Cognito**
- Hạn chế về **User Pool**:
    - Số lượng User trên 1 user pool: 40M (contact AWS nếu muốn tăng)
    - Số lượng user pool tối đa: default 1,000, max 10,000
    - Custom attribute: 50
- Hạn chế về **tần suất Admin API call**:
    - UserCreation: 50 RPS. Tăng thêm 10RPS cho mỗi 1 triệu MAU
    - AdminUserRead: 120 RPS. Tăng thêm 40 RPS cho mỗi 1 triệu MAU
    - RevokeToken: 120 RPS. Tăng thêm 40 RPS cho mỗi 1 triệu MAU
    - UserUpdate: 25 RPS không thể tăng thêm.
    - ...

- *Lưu ý về **cơ chế verify token của Cognito**:*
    - *JWT token do Cognito phát hành thông thường sẽ dùng client side verify (sử dụng Public Key do Cognito cung cấp. Lưu ý AWS không cung cấp private key của Cognito).Việc này đồng nghĩa với việc nếu user logout thì access-token vẫn có hiệu lực cho tới khi expired (vd 30 min).*
    - *Nếu hệ thống có nhu cầu revoke access-token đã phát hành khi user có các hành động như change password, log-out thì không thể thực hiện với Cognito. Tất nhiên có thể workaround sử dụng các kỹ thuật Caching/DB.*






<!-- Thêm lí thuyết Cognito vào trước dòng này -->


<br><br>


## <div align="center"><strong>LAB</strong></div>

<details>
<summary>&nbsp;&nbsp;<strong>Lab 01</strong></summary>
<!-- Thêm lab vào sau dòng này -->

<p align="center">
    <img src = "docs_imgs\apigw_lab01.png" width="500">
</p>




<!-- Thêm lab vào trước dòng này -->
</details> 



<details>
<summary>&nbsp;&nbsp;<strong>Lab 02</strong></summary>
<!-- Thêm lab vào sau dòng này -->

<p align="center">
    <img src = "docs_imgs\cognito_lab02.png" width="500">
</p>




<!-- Thêm lab vào trước dòng này -->
</details> 


<details>
<summary>&nbsp;&nbsp;<strong>Lab 03</strong></summary>
<!-- Thêm lab vào sau dòng này -->

<p align="center">
    <img src = "docs_imgs\cognito_lab03.png" width="500">
</p>




<!-- Thêm lab vào trước dòng này -->
</details> 


<details>
<summary>&nbsp;&nbsp;<strong>Lab 04</strong></summary>
<!-- Thêm lab vào sau dòng này -->

<p align="center">
    <img src = "docs_imgs\cognito_lab04.png" width="500">
</p>




<!-- Thêm lab vào trước dòng này -->
</details> 



<br><br><br><br><br>

## <div align="center"><strong>TÀI LIỆU BỔ SUNG</strong></div>

- Nội dung phục vụ lab: [Code](/labs/sec_15)
- AWS Console liên quan:
    - [API Gateway](/aws_console/README.md#api-gateway)
    - [Cognito](/aws_console/README.md#cognito)