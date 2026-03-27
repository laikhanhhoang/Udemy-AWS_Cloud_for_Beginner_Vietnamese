<h1 align="center">
    <strong>SECURITY SERVICES</strong>
</h1>

## MỤC LỤC

- [Lí thuyết](#lí-thuyết)
    - [Identity and Access Management](#identity-and-access-management)
- [Lab](#lab)
- [Tài liệu bổ sung](#tài-liệu-bổ-sung)


## <div align="center"><strong>LÍ THUYẾT</strong></div>
<!-- Thêm lí thuyết vào sau dòng này -->

- Có một số nhóm dịch vụ Security chính:

    - Identity and Access management
    - Data encryption and protection
    - Threat detection
    - Application and infrastructure protection
    - Management


### **Identity and Access Management**
- **AWS Identity Center**: Công cụ thay thế single-sign-on (SSO) của AWS, có nhiệm vụ **quản lý tập trung quyền access tới nhiều account & application** trên AWS, **kết hợp AWS Organization**.

    <p align="center">    
        <img src = "docs_imgs\sec_iamcenter.png" width="200">
    </p>

- **AWS Resource Access Manager**: Đơn giản hoá việc chia sẻ một cách bảo mật các resource giữa các AWS Account. **Quản lý access đến *TỪNG* resource thông qua sharing detail**, cho phép chia sẻ tới IAM Role, user, AWS Account khác trong cùng Organization.

    <p align="center">    
        <img src = "docs_imgs\sec_resrcacessmng.png" width="200">
    </p>

### **Data Encryption And Protection**
- **AWS Key Management Service**: dịch vụ quản lý khóa của AWS, giúp tạo và kiểm soát key được sử dụng để mã hóa hoặc ký số dữ liệu của bạn.

    <p align="center">    
        <img src = "docs_imgs\sec_kms.png" width="200">
    </p>

- **AWS Secrets Manager**: **Giúp quản lý các thông tin mật** như username, password của DB - cung cấp cơ chế để quản lý access tới các secret và hỗ trợ rotation.

    <p align="center">    
        <img src = "docs_imgs\sec_secretmng.png" width="200">
    </p>

- **AWS Certificate Manager**: **Giúp tạo và quản lý các SSL certificate** một cách dễ dàng *- Certificate tạo ra bởi ACM dễ dàng tích hợp với các dịch vụ như CloudFront, Application Load Balancer*.

    <p align="center">    
        <img src = "docs_imgs\sec_acm.png" width="200">
    </p>

- **Amazon Macie**: Dịch vụ giúp **phát hiện và bảo vệ dữ liệu nhạy cảm** nhờ vào việc **apply cơ chế Machine Learning**.

    <p align="center">    
        <img src = "docs_imgs\sec_macie.png" width="200">
    </p>


<br><br><br><br>

### **Thread Detection**

- **Amazon GuardDuty**: liên tục **theo dõi khối lượng công việc và tài khoản AWS của bạn để phát hiện hoạt động có hại**, đồng thời cung cấp các nội dung phát hiện bảo mật chi tiết nhằm nhận biết và khắc phục.

    <p align="center">    
        <img src = "docs_imgs\sec_guarduty.png" width="200">
    </p>

- **Amazon Inspector**: Quản lý **lỗ hổng bảo mật tự động và liên tục trên quy mô lớn** - *Hỗ trợ các dịch vụ như EC2, Lambda, ECR*.

    <p align="center">    
        <img src = "docs_imgs\sec_inspector.png" width="200">
    </p>

- **Amazon Detective**: **Phân tích và trực quan hóa dữ liệu về bảo mật** để điều tra các sự cố bảo mật tiềm ẩn qua **phân tích các log như VPC Log, CloudTrail Log, EKS Audit log,..,** Detective có thể phân tích và điều tra những nguy cơ tiềm ẩn.

    <p align="center">    
        <img src = "docs_imgs\sec_detective.png" width="200">
    </p>

- **Amazon Config**:
    - Xem xét, kiểm tra và đánh giá cấu hình các tài nguyên của bạn.
    - AWS Config liên tục xem xét, kiểm tra và đánh giá các cấu hình và mối quan hệ của các tài nguyên.
    - Bạn có thể đưa ra các rule để apply cho các resource trên AWS. AWS Config có nhiệm vụ phát hiện ra resource nào vi phạm rule và có action kịp thời (notify, alarm, log..)
    - *Chức năng Proactive Compliance giúp ngăn chặn hoặc thực thi hành động khi có violate rule.*

    <p align="center">    
        <img src = "docs_imgs\sec_config.png" width="200">
    </p>

- **AWS Security Hub**: **Tự động hóa hoạt động kiểm tra bảo mật AWS tổng hợp** qua các dịch vụ thread detection con đã giới thiệu và tập trung cảnh báo bảo mật.

    <p align="center">    
        <img src = "docs_imgs\sec_sechub.png" width="200">
    </p>

    <p align="center">    
        <img src = "docs_imgs\sec_sechub_architect.png" width="800">
    </p>  


<br><br><br><br>

### **Application and infra protection**

Nhóm này có nhiệm vụ chống lại các cuộc tấn công đến backend.

- **AWS WAF**: giúp bạn bảo vệ **chống lại các bot và tình huống khai thác web phổ biến** có thể ảnh hưởng đến mức độ sẵn sàng, xâm phạm bảo mật hoặc tiêu tốn quá nhiều tài nguyên.

    <p align="center">    
        <img src = "docs_imgs\sec_waf.png" width="200">
    </p>

    <p align="center">    
        <img src = "docs_imgs\sec_waf_arch.png" width="800">
    </p>  

- **AWS Shield**: 
    - **Tối đa hóa mức độ sẵn sàng và khả năng đáp ứng của ứng dụng** với tính năng bảo vệ được quản lý **chống lại DDoS**.
    - *Hạ tầng của AWS được bảo vệ bởi Shield  basic (by default).*
    - *Nếu Enable Advanced Shield sẽ tốn phí (**$3000/month** và **charge ngay lập tức**) -> Lưu ý **không bật shield advanced**.*


<br><br><br><br>

### **Management**

- **AWS Trusted Advisor**: cung cấp các đề xuất giúp bạn tuân theo những biện pháp thực hành tốt nhất về AWS. Trusted Advisor đánh giá tài khoản của bạn bằng các nội dung kiểm tra. Những nội dung kiểm tra này xác định các cách để tối ưu hóa cơ sở hạ tầng AWS của bạn, tăng cường bảo mật và hiệu năng, giảm chi phí cũng như giám sát service limit. Sau đó, bạn có thể làm theo các đề xuất để tối ưu hóa dịch vụ 
và tài nguyên của mình.
    - *Để tận dụng tối đa các tính năng của Trusted Advisor, bạn cần mua gói Business suport plan trở lên.*.



- **AWS Organization**:  cho phép bạn tạo tài khoản AWS mới mà không phải trả thêm phí. Lợi ích cụ thể: 
    - Tập trung việc quản lý chi phí cũng như theo dõi chi phí của từng account.
    - Phân chia đơn vị trong tổ chức dễ dàng.
    - Apply policy đặc thù cho từng đơn vị tổ chức.
    - Tập trung data cho bài toán monitor and audit.
    - Cơ sở để triển khai Landing zone.

    <p align="center">    
        <img src = "docs_imgs\sec_org_arch.png" width="400">
    </p>  

<!-- Thêm lí thuyết vào trước dòng này -->






<br><br>


## <div align="center"><strong>LAB</strong></div>

<details>
<summary>&nbsp;&nbsp;<strong>Lab 01</strong></summary>
<!-- Thêm lab vào sau dòng này -->






<!-- Thêm lab vào trước dòng này -->
</details> 

<br><br><br><br><br>

## <div align="center"><strong>TÀI LIỆU BỔ SUNG</strong></div>

- Nội dung phục vụ lab: [Code](/labs/sec_)
- AWS Console liên quan:
    - [Dịch vụ 1](/aws_console/README.md#tên-dịch-vụ-1)
    - [Dịch vụ 2](/aws_console/README.md#tên-dịch-vụ-2)