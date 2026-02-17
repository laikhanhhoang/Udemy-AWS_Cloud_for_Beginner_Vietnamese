# AWS Cloud for beginner (Vietnamese)

Đây là repo cho khóa học về AWS cơ bản trên Udemy.

## Mục lục

- [Section 07 - EC2](#section-07)
- [Section 08 - IAM](#section-08)
- [Section 09 - S3](#section-09)
- [Section 10 - ELB & ASG](#section-10)

## Section 07 

<p align="center">
    EC2 - Computing
</p>

<details>
<summary></summary>



- EC2 là một service cung cấp tài nguyên server ảo theo nhu cầu.
    - Đa dạng cấu hình, dễ dàng triển khai.
    - Khả năng mở rộng gần như không giới hạn.
    - là một máy chủ ảo chạy trên Hypervisor (Trình ảo hóa) của AWS, bên dưới là các phần cứng.

- Một số khái niệm cơ bản
    - AMI (Amazon Machine Image): Giống như 1 file ISO/Ghost chứa toàn bộ thông tin của hệ điều hành. EC2 được khởi động lên từ 1 AMI tương tự như việc cài Win lần đầu cho PC/Laptop. Muốn khởi động một EC2 bắt buộc phải có AMI.
    - EBS Volume: Ổ cứng ảo được cấp phát bởi Amazon. Chỉ có thể đọc được dữ liệu khi được gắn vào 1 Instance.
    - Snapshot: Ảnh chụp của 1 EBS Volume tại 1 thời điểm. Có thể sử dụng để phục hồi dữ liệu khi có sự cố.

    <p align="center">
        <img src="md_imgs/ec2_term.png" width="250" />
    </p>

- Vòng đời của EC2 Instance:
    - EC2 Default
        - Lưu ý: EC2 ở trạng thái Terminated không thể phục hồi lại được. Ổ cứng EBS gắn với instance tùy setting mà sẽ bị xóa hay không.

        <p align="center">
            <img src="md_imgs/lifecycle_df.png" width="450" />
        </p>

    - EC2 using instance-store volume
        - Loại EC2 này gắn trực tiếp trn máy host, sẽ mất dữ liệu khi instance stop nhưng IOPS rất cao. Phù hợp lưu trữ data không quan trọng hoặc data caching đòi hỏi tốc độ đọc ghi cao. 
        - Instance loại này sẽ **không thể stop tạm thời** mà chỉ có thể launch, reboot, terminate.

        <p align="center">
            <img src="md_imgs/ec2_lifecycle_str.png" width="250" />
        </p>



<!-- Thêm nội dung vào trước dòng này -->
</details>









## Section 08

<p align="center">
    Identity and Access Managment (IAM)
</p>

<details>
<summary></summary>
<!-- Thêm nội dung vào sau dòng này -->




<!-- Thêm nội dung vào trước dòng này -->
</details>













## Section 09

<p align="center">
    Simple Storage Service (S3)
</p>

<details>
<summary></summary>
<!-- Thêm nội dung vào sau dòng này -->




<!-- Thêm nội dung vào trước dòng này -->
</details>













## Section 10

<p align="center">
    Load Balancing và Autoscaling
</p>

<details>
<summary></summary>
<!-- Thêm nội dung vào sau dòng này -->




<!-- Thêm nội dung vào trước dòng này -->
</details>