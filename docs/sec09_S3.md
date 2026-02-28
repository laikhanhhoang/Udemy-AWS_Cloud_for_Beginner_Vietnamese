<h1 align="center">
    <strong>Simple Storage Service (S3)</strong>
</h1>

## MỤC LỤC

- [Lí thuyết](#lí-thuyết)
- [Lab](#lab)

## <div align="center"><strong>LÍ THUYẾT</strong></div>
<!-- Thêm lí thuyết vào sau dòng này -->


- Là một dịch vụ lưu trữ dạng Object cung cấp khả năng mở rộng availability, performance.

    - Là một Managed Service cho phép lưu file dưới dạng object với size từ 0 - 5TB.
    - High Durability (11 9s), Scalability, High Availability (99.99%), High performance.
    - Cung cấp nhiều class lưu trữ để tiết kiệm chi phí cho từng loại data, khả năng phân quyền và giới hạn truy cập một cách chi tiết.
    - Usecase đa dạng (mọi bài toán về lưu trữ từ lớn tới nhỏ đều có thể sd S3).

- **Các tính năng cơ bản**

    - **Storage classes**:      cung cấp nhiều hình thức lưu trữ phù hợp cho nhiều loại data khác nhau về nhu cầu access, durability,...
    - **Storage management**:   Cung cấp nhiều tính năng liên quan quản lý như: Life Cycle, Object Lock, Replication, Batch Operation.
    - **Access Management**:    quản lý truy cập đến bucket và các thư mục thông qua cơ chế resource permission & access list. 
    - **Data processing**:      kết hợp với lambda, SNS, SQS để hỗ trợ xử lý data 1 cách nhanh chóng.
    - **Strong consistency**:   Provice strong read-after-write consistency for PUT and DELETE object.
    - **Monitoring**
        - Auto Logging and Monitoring: cung cấp công cụ monitor S3 bucket và truy vết sử dụng CloudTrail.
        - Manual Monitoring Tool: Log lại từng record thực hiện trên bucket.
        - Analytic and insight: phân tích storage để optimize.

- **Kết hợp dịch vụ khác với S3**

    <p align="center">
        <img src="docs_imgs/s3_connect.png" width="400" />
    </p>

    <details>
    <summary></summary>

    - Dùng làm nơi **lưu trữ file** cho các ứng dụng chạy trên EC2, Container, Lambda. Các file có thể đa dạng về loại & kích thước (Image,Video, Document,...).
    - Dùng làm nơi **chứa/archive log** cho hầu hết các dịch vụ khác của AWS (VPC, ALB, APIGateway,...).
    - Dùng làm **data source** cho các bài toán big data & data warehouse.
    - Nơi lưu dữ liệu gửi lên từ các thiết bị IoT.
    - Vùng **lưu trữ tạm thời cho bài toán ETL** (Extract – Transform - Load) khi kết hợp với Lambda.
    - **Host 1 website tĩnh** (html,css,js) khi kết hợp với CloudFront.
    </details>

- **S3 Storage Class**

    - S3 cung cấp nhiều storage class khác nhau nhằm giúp người dùng linh động trong việc lựa chọn class  phù hợp với nhu cầu, tiết kiệm chi phí.
        
        Việc lựa chọn class phụ thuộc vào các yếu tố như:
        - Durability, High Availability
        - Thời gian lưu trữ (1 tháng, 1 năm, 5 năm...)
        - Tần suất truy cập, thời gian cần có file khi có yêu cầu
        - Mục đích sử dụng: document, image, log file, backup file, archive

    - Phân biệt cc S3 Storage Class
        | Lớp lưu trữ | Mục đích sử dụng | Giá | Thời gian truy xuất | Minimum storage duration | Phù hợp với |
        |-----------|----------------|-----|---------------------|---------------------------|-----------|
        | S3 Standard | Dữ liệu truy cập thường xuyên | Cao | Ngay lập tức | Không có | Web assets, user uploads, model ML |
        | S3 Intelligent-Tiering | Tự động chuyển tier dựa trên mức độ truy cập | Trung bình | Ngay lập tức (sau khi chuyển tier) | Không có | Dữ liệu có pattern truy cập không ổn định |
        | S3 Standard-IA | Ít truy cập nhưng cần truy xuất nhanh | Thấp | Ngay lập tức | 30 ngày | Backup, logs, dữ liệu ít đọc |
        | S3 One Zone-IA | Giống Standard-IA nhưng chỉ 1 Availability Zone (rẻ hơn, ít bền hơn) | Rất thấp | Ngay lập tức | 30 ngày | Dữ liệu không quan trọng, có thể tái tạo |
        | S3 Glacier | Archive, dữ liệu rất lạnh | Rất thấp | Vài giờ (phải restore) | 90 ngày | Compliance, backup dài hạn |
        | S3 on Outposts | S3 chạy trên hạ tầng AWS Outposts (on-prem) | Tùy cấu hình Outposts | Latency nội bộ | Không có | Hybrid workload cần lưu trữ tại data center riêng |

        Lưu ý:
        - Minimum storage duration trong Amazon S3 nghĩa là: AWS yêu cầu bạn trả tiền tối thiểu cho một khoảng thời gian, dù bạn xóa object sớm hơn.

- **S3 Lyfe Cycle**
    - Tính năng cho phép tự động move object xuống các class lưu trữ thấp hơn hoặc xoá luôn sau một khoảng thời gian nhằm tiết kiệm chi phí.
    - Khác với Intelligent Tiering, người dùng sẽ tự quyết định life cycle cho objects (hoặc 1 thư mục), vd sau 90 ngày thì cho xuống Glacier, sau 270 ngày thì xoá hoàn toàn.
    - Phù hợp cho các bài toán lưu trữ Log đã biết trước thời gian thường xuyên access và thời gian có thể xoá.

- **S3 event trigger**
    - S3 cung cấp cơ chế trigger 1 event sang dịch vụ khác khi có thay đổi đối với object (upload, delete).
    - Target của trigger có thể là Lambda Function, SNS, SQS.
    - Sample usecase
        - Resize image khi có người upload image lên s3 bucket, lưu vào các thư mục size khác nhau
        - Giải nén 1 file zip khi có người upload.
        - Extract csv file, xử lý data rồi lưu vào database
        - Notification tới Operator khi có ai xoá 1 file

- **Best practices**
    - Chọn region của S3 cùng region với application (EC2, ECS) để tối ưu performance.
    - Sử dụng bucket policy cho những data quan trọng. Cấp quyền vừa đủ cho user/role, hạn chế cấp S3FullAccess.
    - Bật versioning để bảo vệ data tránh bị mất, xoá nhầm.
    - Mã hoá data nhạy cảm (client side or server side).
    - Enforce TLS để yêu cầu sd HTTPS khi truyền nhận file (chống hack).
    - Sử dụng VPC endpoint để tăng tốc truy cập từ application (sẽ học ở bài VPC).
    - Khi host static web, nên kết hợp với CloudFront để tối ưu chi phí và tăng trải nghiệm người dùng.




<br><br>
## <div align="center"><strong>LAB</strong></div>

<details>
<summary></summary>

</details>