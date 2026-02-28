<h1 align="center">
    <strong>LOAD BALANCER AND AUTO SCALING</strong>
</h1>

## MỤC LỤC

- [Lí thuyết](#lí-thuyết)
- [Lab](#lab)

## <div align="center"><strong>LÍ THUYẾT</strong></div>
<!-- Thêm lí thuyết vào sau dòng này -->

- <details>
    <summary>&nbsp;&nbsp;<strong>"Single point of failure" - Sự ra đời của Load Balancer</strong></summary>

    - Khi một sự cố xảy ra ở **một thành phần nào đó** có thể dẫn đến hệ thống bị **dừng hoạt động**, **không thể phục vụ người dùng**, ta gọi đó là **single point of failure**.
        <details>
        <summary>&nbsp;&nbsp;Ví dụ</summary>

        - Một chương trình bị lỗi và crash.
        - Database bị sập không response.
        - Hệđiều hành (OS) bị treo.
        - Một trong các phần cứng vật lý (RAM, CPU, Disk, Power,…) bị hỏng.

        Lên cấp độ cao hơn chúng ta có các sự cố như:
        - Bộ cung cấp nguồn cho cả 1 Server Rack bị sự cố.
        - Xảy ra sự cố cúp điện cả data center (trong trường hợp không có nguồn dự phòng: bị lũ lụt, bão, đánh bom, thiên thạch rớt trúng,...)
        </details> 

    - Do đó, cần nhiều hơn 1 thành phần cho mỗi layer của hệ thống để tránh “single point of failure”.
        <p align="center">
            <img src="docs_imgs\elb_single_failure.png" width="500" />
        </p>

        Trong kiến trúc single point 
        of failure như hình bên, bất
        kỳ sự cố hư hỏng ở 1 trong
        các cấp độ từ App, OS, 
        Hypervisor(ảo hoá) cho tới
        hardware đều có thể ảnh
        hưởng tới Availability (tính 
        khả dụng) của hệ thống.

    - Nếu hệ thống có nhiều hơn 1 thành phần, cần có 1 **cơ chế để phân phối request từ client đến các thành phần ở backend** => Sự ra đời của Load Balancer.
    - AWS cho phép dễ dàng setup load balance tới nhiều target nằm ở các Availability Zone khác nhau. 

        Lưu ý: Mỗi Availability Zone (AZ) tương ứng với 1 data center cách nhau từ vài chục
        tới vài trăm km. Bằng việc phân bổ các instance nằm trên nhiều hơn 1 AZ, hệ thống
        có thể chịu được các sự cố ở cấp độ data center của AWS mà vẫn hoạt động bình
        thường.

</details>

- **Elastic Load Balancing - Định nghĩa**
    - Là một dịch vụ của AWS có nhiệm vụ **điều hướng request từ client đến các target backend**, **đảm bảo request được cân bằng giữa các target**:
        - **High Availability**.
        - **Scalability**: về lý thuyết là không giới hạn.
        - **High Security**: nếu kết hợp với các dịch vụ khác như WAF, Security Group.
    - ELB có thể dễ dàng **kết hợp** với đa dạng backend sử dụng EC2, Container, Lambda.

- **Các thành phần cơ bản của Loab Balancer**
    - Load Balancer cho phép setting các listener (trên 1 port nào đó vd HTTP:80, HTTPS:443)
    - **Mỗi Listener** cho phép cấu hình **nhiều rule**.
    - Request sau khi đi vào listener, được đánh giá bởi các rule sẽ được forward tới target group phù hợp.
    - **Target group có nhiệm vụ health check** để phát hiện và loại bỏ target un-healthy.












<!-- Thêm lí thuyết vào trước dòng này -->






<br><br>


## <div align="center"><strong>LAB</strong></div>

<details>
<summary></summary>
<!-- Thêm lab vào sau dòng này -->






<!-- Thêm lab vào trước dòng này -->
</details> 