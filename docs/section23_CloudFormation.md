<h1 align="center">
    <strong>INFRA AS CODE - CLOUDFORMATION</strong>
</h1>

## MỤC LỤC

- [Lí thuyết](#lí-thuyết)
    - [Định nghĩa](#định-nghĩa)
    - [Benefit](#benefit)
    - [CloudFormation Template](#cloudformation-template)
- [Lab](#lab)
- [Tài liệu bổ sung](#tài-liệu-bổ-sung)


## <div align="center"><strong>LÍ THUYẾT</strong></div>
<!-- Thêm lí thuyết vào sau dòng này -->

### **Định nghĩa**

- Infrastructure as Code (IaC) là một **phương pháp quản lý hạ tầng IT bằng cách sử dụng template hoặc code để triển khai** hạ tầng. 
- IaC nhằm tự động hóa quy trình triển khai, cấu hình và quản lý hạ tầng, thay vì thực hiện các tác vụ này bằng tay.
- AWS CloudFormation là một **dịch vụ AWS cung cấp cách tự động hóa việc triển khai và quản lý các nguồn tài nguyên AWS**. Nó cho phép bạn xác định các nguồn tài nguyên AWS của mình dưới dạng mã (infrastructure as code), sử dụng cú pháp JSON hoặc YAML, và sau đó triển khai và quản lý các tài nguyên này trong một cấu trúc tổ chức gọi là stack.

### **Benefit**

- Tăng tốc độ triển khai.
- Đảm bảo tính nhất quán về kiến trúc cũng như setting của resources giữa các môi trường.
- Giảm thiểu sai sót do triển khai & cấu hình bằng tay (console, cli).
- Tăng tính tái sử dụng của resource (cho các dự án khác nhau).

### **CloudFormation Template**
Gồm các thành phần:
- Version
- Description
- Metadata
- Parameters
- Rules
- Mapping
- Condition
- Transform
- **Resources** *(là thành phần bắt buộc)*
- Outputs

<p align="center">    
    <img src = "docs_imgs\clf_example.png" width="350">
    <br>
    <i>Ví dụ một CloudFormation Template</i>
</p>







<!-- Thêm lí thuyết vào trước dòng này -->






<br><br>


## <div align="center"><strong>LAB</strong></div>

### **Lab 01**
<!-- Thêm lab vào sau dòng này -->

<p align="center">    
    <img src = "docs_imgs\clf_lab01.png" width="700">
</p>




<!-- Thêm lab vào trước dòng này -->


<br><br><br><br><br>

## <div align="center"><strong>TÀI LIỆU BỔ SUNG</strong></div>

- Nội dung phục vụ lab: [Code](/labs/sec_23)
- AWS Console liên quan:
    - [CloudFormation](/aws_console/README.md#cloudformation)
