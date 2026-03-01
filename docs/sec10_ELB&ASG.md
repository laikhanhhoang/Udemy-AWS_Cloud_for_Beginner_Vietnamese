<h1 align="center">
    <strong>LOAD BALANCER AND AUTO SCALING</strong>
</h1>

## MỤC LỤC

- [Lí thuyết](#lí-thuyết)
    - [Elastic Load Balancer](#elastic-load-balancer)
        - "Single point of failure" - Sự ra đời của Load Balancer
        - Elastic Load Balancing - Định nghĩa
        - Các thành phần cơ bản của Load Balancer
        - Các loại Load Balancer
        - Cách Load Balancer hoạt động
        - Load Balancer tính phí như thế nào?
        - Cross zone load balancer
        - Một số lưu ý về Load Balancer

    - [Auto Scaling Groups](#auto-scaling-groups)
        - Scaling
        - Auto Scaling Groups - Định nghĩa
        - Launch Configuration and Launch Template
        - Các phương pháp scale hệ thống

- [Lab](#lab)
- [Tài liệu bổ sung](#tài-liệu-bổ-sung)

## <div align="center"><strong>LÍ THUYẾT</strong></div>

### Elastic Load Balancer
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

- **Các thành phần cơ bản của Load Balancer**
    - Load Balancer cho phép setting các listener (trên 1 port nào đó vd HTTP:80, HTTPS:443)
    - **Mỗi Listener** cho phép cấu hình **nhiều rule**.
    - Request sau khi đi vào listener, được đánh giá bởi các rule sẽ được forward tới target group phù hợp.
    - **Target group có nhiệm vụ health check** để phát hiện và loại bỏ target un-healthy.

    <br>

    <p align="center">
        <img src="docs_imgs\elb_theory.png" width="500" />
    </p>

- **Các loại Load Balancer**
    - Các loại ELB được phân chia phụ thuộc vào việc nó hoạt động trên layer nào của mô hình OSI 7 layers.

        <p align="center">
            <img src="docs_imgs\elb_osi_model.jpg" width="400" />
        </p>


        <details>
        <summary><strong>OSI Model - Note</strong></summary>

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
        
        - Xem kĩ hơn trong mục Kiến thức bổ sung tại [Readme.md](/README.md).

        </details>

        <br>

        <p align="center">
            <img src="docs_imgs\elb_all_kinds.png" width="500" />
        </p>
    
    <br>

    - Application Load Balancer
        - Là loại Load Balancer thường dùng, phù hợp cho đa số các nhu cầu.
        - Hoạt động trên layer 7 – Application.
        - Do hoạt động trên layer 7 nên có một số ưu thể vượt trội so với các loại LB khác:
            - Hỗ trợ Path routing condition
            - Hỗ trợ host condition, cho phép dùng nhiều domain cùng trỏ vào 1 ALB
            - Hỗ trợ routing dựa trên thuộc tính của request (header, ip..)
            - Tích hợp được với Lambda, Container service
            - Hỗ trợ trả về custom HTTP response
        - 
            <details>
            <summary>Nếu traffic là TCP thuần (không phải HTTP), ALB có hiểu nội dung để route theo path được không?</summary>

            - AWS Application Load Balancer là Layer 7 Load Balancer. Điều này có nghĩa là nó hoạt động ở tầng Application và được thiết kế để hiểu các giao thức:
                - HTTP/1.1
                - HTTP/2
                - HTTPS (HTTP over TLS)
                - WebSocket (HTTP Upgrade)
                - gRPC (chạy trên HTTP/2)

            - Khi cấu hình ALB, bạn phải khai báo listener protocol (HTTP hoặc HTTPS). ALB chỉ chấp nhận traffic phù hợp với protocol đó. Nếu một TCP stream không tuân theo chuẩn HTTP được gửi vào listener HTTP, ALB sẽ từ chối vì không parse được request line và header theo RFC của HTTP.
            </details>
    
    <br>

    - Network Load Balancer
        - Hoạt động trên layer 4 – Transport
        - Hỗ trợ 2 giao thức TCP và UDP
        - Không hỗ trợ nhiều hình thức rule routing.
        - Thường dùng cho những hệ thống cóworkoad rất cao, lên tới hàng triệu request/s.

    - Gateway Load Balancer
        - Giúp triển khai, scale và quản lý các Virtual Applicance (3rd party)
        - Mục đích: Firewall, Phát hiện ngăn chặn xâm nhập (intrusion detection and prevention systems), kiểm tra gói tin chuyên sâu.
        - Hoạtđộngtrên layer 3 (Network) & Layer 4 (Transport).
        - GLB listen trên tất cả các port và forward traffic đến các target group dựa trên các rule.
        - GLB sửdụngGLB Endpoint để trao đổi traffic giữa VPC của service provider & VPC của consumer.
        - Danhsách các provider có tại: https://aws.amazon.com/elasticloadbalancing/partners/
        - Trong dự án thực tế thì đây là loại LB **ít được sử dụng nhất**.
        - Ví dụ

            <p align="center">
                <img src="docs_imgs\elb_gateway.png" width="520" />
            </p>

            - Gateway LB được đặt bên trong VPC của Security Provider
            - Traffic đi vào và đi ra hệ thống bên VPC của consumer được cấu hình routing để đi qua Gateway LB trước khi đến được target cần đến.
            - Mũi tên màu xanh: traffic đi từ internet vào. 
            - MàuCam: traffic từ bên trong đi ra.

- **Cách Load Balancer hoạt động**
    - Load Balancer có thể điều hướng tới nhiều hơn 1 target. Trong trường hợp multi-target, việc điều hướng tới target nào sẽ được quyết định bởi 1 số rule sau:
        - Listener port
        - Path pattern (Application LB only)
        - Fixed ratio. VD Target Group 01 nhận 20%, Target Group 02 nhận 80% traffic.

    - Mặc định Load Balancer sẽ phân phối request từ client đến các target trong 1 Target Group theo tỷ lệ cân bằng (round robin), kể cả khi target đó nằm trong nhiều hơn 1 target group.

        Có nghĩa là nếu **Load Balancer** quyết định cho **1 loại request** với pattern xyz/abc **đến một target group** EC2, thì nó sẽ được **phân chia đều** đến mọi EC2 trong group.

        *Ví dụ:* một target group gồm **6 EC2**, thì mỗi EC2 có **16,66% (1/6)** được nhận xử lí request đó.

- **Load Balancer tính phí như thế nào?**
    - Mặc định Elastic Load Balancer tính tiền theo giờ ($/hour), giá phụ thuộc vào region.
    - Ngoài ra còn tính phí dựa trên số lượng request, lượng data transfer qua Load Balancer quy đổi ra Load Balancer Capacity Units (LCUs)
    - Khi estimate cho Load Balancer, user sẽ input các thông số như số lượng request per second (RPS), lưu lượng data (GB/TB per hour), số lượng connection new, thời gian trung bình cho một connection, số lượng rule. AWS sẽ quy đổi thành LCUs để ước tính chi phí.

- **Cross zone load balancer**
    - ELB là 1 dịch vụ hoạt động cross zone (AWS suggest chọn tất cả các zone có thể khi khởi tạo ELB).
        - Nếu **Cross zone load balance** được **enable**, ELB sẽ điều hướng request từ client một cách **cân bằng tới các target**.
        - Nếu **Cross zone load balance** được **disable**, ELB sẽ điều hướng request từ client một cách **cân bằng tới mỗi zone**, trong 1 zone sẽ chia đều tiếp cho các instances.

    - Lưu ý
        - Mặc định, Application Load Balancer sẽ Enable cross zone, không thể tắt.
        - Mặc định, Network Load Balancer sẽ Disable cross zone. Cần enable sau khi tạo.

- **Một số lưu ý về Load Balancer**
    - Load Balancer là 1 dịch vụ Cross Zone, lưu ý khi **tạo ELB** nhớ **chọn tối đa số zone có thể chọn**. 
    - NếuLoad Balancer được tạo không chọn zone có chứa ec2 instance, khi access sẽ bị lỗi không kết nối được (502 Bad Gateway).

<br><br>

### Auto Scaling Groups 

- **Scaling**
    - Là việc điều chỉnh cấu hình của các tài nguyên để đáp ứng với nhu cầu workload (số request từ người dung, số lượng công việc phải xử lý,…).

        Có 2 hình thức scale:
        - Scale Up/Down: Tăng/Giảm cấu hình của resources (vd tăng CPU/Ram cho Server, database, tăng dung lượng ổ cứng,…).

            <p align="center">
                <img src="docs_imgs\asg_scale_updown.png" width="550" />
            </p>

        - Scale Out/In: Tăng/giảm số lượng thành phần trong 1 cụm chức năng. (Vd add thêm server vào cụm application, add thêm node vào k8s cluster,…).
            
            <p align="center">
                <img src="docs_imgs\asg_scale_outin.png" width="550" />
            </p>

- **Auto Scaling Groups - Định nghĩa**
    - Có nhiệm vụ **điều chỉnh số lượng của instance** cho **phù hợp với workload**, nhằm:
        - Tiết kiệm chi phí.
        - Tự động hóa việc mở rộng & phục hồi sự cố.
    - ASG sử dụng **Launch Template** để biết được cần phải launch EC2 như thế nào.

- **Launch Configuration and Launch Template**
    - Mục đích: **Chỉ dẫn Auto Scaling Group** biết được cần phải **launch instance như thế nào**.
    - Các thông tin có thể định nghĩa trong launch template:
        - AMI
        - Instance Type
        - Keypair (trong trường hợp bạn cần login vào instance sau khi tạo)
        - Subnet (Thường không chọn mà để Auto Scaling Group quyết định)
        - Security Group(s)
        - Volume(s)
        - Tag(s)
        - Userdata (script tự động chạy khi instance start)
        - ...
    - *Một số thông tin như Instance Type, Subnet, Security Group có thể được overwrite bởi Auto Scaling Group.*
    - <i> <strong>Launch Template thường được sử dụng hơn</strong> bởi nó có thể quản lý được version.</i>

- **Các phương pháp scale hệ thống**

    |Phương pháp|Đặc điểm|
    |-----------|--------|
    |**No** Scale|**Duy trì cố định 1 số lượng instances** (nếu instance die thì tạo con mới để bổ sung, ngoài ra không làm gì cả).|
    |**Manually** Scaling|**Điều chỉnh 3 thông số: min/max/desire** để quyết định số lượng instance trong ASG.|
    |**Dynamic** Scaling|**Scale tự động** dựa trên việc **monitor các thông số**. <br> <ul><li> **Target tracking scaling**: Monitor **thông số ngay trên chính cluster**, vd CPU, Memory, Network in-out. </li> <li>**Step scaling**: Điều chỉnh số lượng instance (tăng/giảm) **dựa trên 1 tập hợp các alarm** (có thể đến từ các resource khác không phải bản thân cluster). </li><li> **Simple scaling**: **Tương tự Step scaling** tuy nhiên **có apply “cool down period”**.</li></ul>|
    |**Schedule** Scaling|**Đặt lịch tự động tăng giảm số instance theo thời gian**, phù hợp với các **hệ thống có workload tăng vào 1 thời điểm cố định trong ngày**.|
    |**Predict Scaling**| **AWS đưa ra dự đoán** dựa vào việc học **từ thông số hằng ngày, hằng tuần để điều chỉnh** số lượng instance một cách tự động. <br> Độ **chính xác** phụ thuộc vào **thời gian application đã vận hành** và **tính ổn định của traffic** đi vào hệ thống.|

- **Lưu ý**
    - Nếu deploy AMI version mới, hãy tăng lên gấp đôi số instance, sau đó lại hạ xuống để AWS tự terminate code cũ mà không ảnh hưởng người dùng cuối (tối ưu Elastic Load Balancer).
    - Về bản chất ASG nhìn vào thông số Desire capacity để biết được cần thêm hay bớt instance trong cluster.
        - Ví dụ:
            - Cluster đang có 2 instances, set desire = 4 => ASG sẽ add thêm 2 instance.
            - Cluster đang có 4 instances, set desire = 3 => ASG sẽ terminate bớt 1 instance.


            




<!-- Thêm lí thuyết vào trước dòng này -->






<br><br>


## <div align="center"><strong>LAB</strong></div>

<details>
<summary>&nbsp;&nbsp;<strong>Lab 01</strong></summary>
<!-- Thêm lab vào sau dòng này -->

<br>

<p align="center">
    <img src = "docs_imgs\elb_lab01.png" width="500">
</p>


<p align="center">
    <img src = "docs_imgs\elb_lab01_step02.png" width="500">
</p>



<!-- Thêm lab vào trước dòng này -->
</details> 


<details>
<summary>&nbsp;&nbsp;<strong>Lab 02</strong></summary>
<!-- Thêm lab vào sau dòng này -->

<p align="center">
    <img src = "docs_imgs\asg_lab02.png" width="500">
</p>




<!-- Thêm lab vào trước dòng này -->
</details> 

<br><br><br><br><br>

## <div align="center"><strong>TÀI LIỆU BỔ SUNG</strong></div>

- Nội dung phục vụ lab: [Code](/labs/sec_10)
- AWS Console liên quan:
    - [ELB](/aws_console/README.md#elastic-load-balancer-elb)
    - [ASG](/aws_console/README.md#auto-scaling-groups-asg)


