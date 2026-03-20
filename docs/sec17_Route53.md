<h1 align="center">
    <strong>TÊN BÀI</strong>
</h1>

## MỤC LỤC

- [Lí thuyết](#lí-thuyết)
- [Lab](#lab)
- [Tài liệu bổ sung](#tài-liệu-bổ-sung)


## <div align="center"><strong>LÍ THUYẾT</strong></div>
<!-- Thêm lí thuyết vào sau dòng này -->

- **Route 53 - Định nghĩa**
    - DNS – Domain Name System: là một hệ thống quản lý các tên miền và ánh xạ chúng thành địa chỉ IP của máy chủ, **truy cập** các dịch vụ **internet** bằng **tên miền thay vì sử dụng địa chỉ IP**.
    - AWS Route 53 là một dịch vụ quản lý tên miền và DNS của AWS.
        - DNS AWS Route 53 cho phép bạn đăng ký và quản lý tên miền, tạo và cấu hình các bản ghi DNS, và điều hướng các yêu cầu từ tên miền đến các nguồn tài nguyên khác nhau trên AWS và bên ngoài.
        - Route 53 cung cấp một loạt tính năng như đám mây DNS, chống chịu tải, đám mây phân phối nội dung, bảo mật và theo dõi tình trạng tài nguyên. Nó cũng **tích hợp tốt với các dịch vụ khác của AWS**, cho phép bạn tự động cập nhật bản ghi DNS khi tạo hoặc xoá các nguồn tài nguyên trên AWS.

- **3 tính năng chính**
    - **Register Domain name**: cho phép bạn **mua và quản lý tên miền**, tự đông gia hạn (optional). Bạn cũng có thể đem một domain đã mua của bên thứ 3 và quản lý trên Route 53.
    - **DNS Routing**: **điều hướng internet traffic tới một resource** như: EC2 IP, Application Load Balancer, Cloud Front, API Gateway, RDS,...
    - **Health checking**: **tự động gửi request đến các resource để check tình trạng hoạt động**. Có thể kết hợp với Cloud Watch alarm để notify khi có resource unhealthy.

- **DNS Resolver**: Hình dưới mô tả một flow từ khi người dùng gõ địa chỉ của một trang web **`www.example.com`** lên trình duyệt cho tới khi nội dung thực sự được trả về từ máy chủ.

    <p align="center">
        <img src = "docs_imgs\route53_dns_resolver.png" width="400">
    </p>

    *Lưu ý*:
    -  *(3): Name server = máy chủ có nhiệm vụ dịch domain thành IP*
    - *(3): TLD (Top-Level Domain) là phần đuôi cuối cùng của domain. Phổ biến: .com, .net,...*
    - *(7): Khi người dùng lần đầu tiên nhấn Enter truy cập website, IP từ DNS Resolver sẽ cache vào bên trong máy.*

- **Route 53 Hosted Zone**
    - **Route 53 Hosted Zone** là một khái niệm trong AWS Route 53. **Một hosted zone đại diện cho một tên miền hoặc một tên miền con trong hệ thống DNS**. Nó là nơi bạn quản lý các bản ghi DNS(DNS Record) và cấu hình liên quan cho tên miền cụ thể.
        - **Hosted zone public**: **map với một tên miền** cụ thể (có thể mua bởi Route 53 hoặc bên thứ 3). 
        - **Private hosted zone**: **map với tên miền** tuy nhiên **chỉ có tác dụng trong scope một VPC**.
    - Sau khi tạo hosted zone, bạn có thể **thêm các bản ghi DNS vào** nó như A Record, CNAME Record, MX Record và nhiều loại bản ghi khác. Bạn có thể **cấu hình các bản ghi này để điều hướng yêu cầu tới các tài nguyên khác nhau**, chẳng hạn như máy chủ web, máy chủ email hoặc dịch vụ khác trên internet.

        |DNS Record|Tác dụng|
        |----------|--------|
        |**A Record** (Address Record)| Xác định một địa chỉ **IPv4** cho tên miền. Nó ánh xạ một tên miền vào một địa chỉ IP v4. <br> *Ví dụ*: **`website.com`** **→** **A** **→** **`10.0.1.123`**.|
        |AAAA Record (IPv6 Address Record)|Tương tự như A Record, nhưng sử dụng để xác định một địa chỉ **IPv6** cho tên miền. <br> *Ví dụ*: **`website.com`** **→** **AAAA** **→** **`2001:0db8::1`**.|
        |**CNAME Record** (Canonical Name Record)|Nó được sử dụng để tạo đường dẫn từ một tên **miền thứ cấp (subdomain)** đến một tên miền ở bất cứ đâu trên internet. <br> *Ví dụ*: **`api.website.com`** **→** **CNAME** **→** **`backend.com`**.|
        |**NS Record** (Name Server Record)| Xác định máy chủ tên miền (name server) chịu trách nhiệm quản lý các bản ghi DNS cho tên miền cụ thể. Nó cho phép bạn chỉ định máy chủ DNS mà bạn muốn sử dụng cho tên miền của mình. <br> *Ví dụ*: **`myweb.com`** **→ NS →** **`ns-123.awsdns-45.com`**.|
        |MX Record(Mail Exchanger Record)| Xác định các máy chủ chịu trách nhiệm nhận và xử lý thư điện tử cho một tên miền. Nó được sử dụng để định vị máy chủ email. <br> *Ví dụ*: **`myweb.com`** **→** **MX** **→** **`mail.myweb.com (priority 10)` HOẶC `mail2.myweb.com (priority 20)`**.|
        |**PTR Record** (Pointer Record)| Sử dụng để thực hiện ánh xạ địa chỉ IP thành tên miền. Nó được sử dụng chủ yếu trong việc xác định tên miền từ một địa chỉ IP cụ thể. <br> *Ví dụ*: **`1.2.3.4`** **→ PTR →** **`mail.myweb.com`**.|
        |**TXT Record** (Text Record)| Cho phép bạn lưu trữ các dữ liệu văn bản tùy ý cho tên miền. Nó thường được sử dụng để xác thực tên miền và cung cấp thông tin khác nhau cho các dịch vụ khác. <br> *Ví dụ*: **myweb.com** **→ TXT →** **`"v=spf1 include:spf.myweb.com ~all"`**.|
        |**SRV Record** (Service Record)| Xác định vị trí và cấu hình dịch vụ cụ thể trên mạng. Nó được sử dụng chủ yếu trong việc xác định các máy chủ chịu trách nhiệm cho các dịch vụ như VoIP (Voice over IP) và IM (Instant Messaging). <br> *Ví dụ*: **`_sip._tcp.myweb.com`** **→ SRV →** **`10 60 5060 sipserver.myweb.com`**.|

        <p align="center">
            <img src = "docs_imgs\route53_dns_record_subdomain.png" width="700">
        </p>



- **Routing policy**

    | Routing Policy            | Mục đích                                                                           | Khi dùng                                                | Notes                                                            |
    | ------------------------- | ---------------------------------------------------------------------------------- | ------------------------------------------------------- | ---------------------------------------------------------------- |
    | **Simple**                | Trỏ domain → 1 IP hoặc 1 resource.                                                 | Website bình thường.                                    | Không có load balancing hay failover.                            |
    | **Weighted**              | Phân traffic % tới nhiều endpoint.                                                 | A/B testing, canary release.                            | Có thể phân traffic theo weight.                                 |
    | **Latency**               | Chọn endpoint có **latency thấp nhất so với user**.                                | Khi muốn user nối server gần nhất → giảm lag.           | Không quan tâm vị trí địa lý.                                    |
    | **Geolocation**           | Chọn endpoint dựa trên **vị trí địa lý của user**.                                 | Khi muốn serve theo region cụ thể (VD: EU → EU server). | Không đo latency thực tế.                                        |
    | **Geoproximity**          | Điều chỉnh traffic dựa trên **vị trí của resource và user**, có thể shift traffic. | Khi muốn cân bằng load giữa các region phức tạp.        | Cần bật **Route 53 Traffic Flow**.                               |
    | **Failover**              | Chỉ chọn primary → secondary khi fail.                                             | Backup server.                                          | Kết hợp health check để detect failure.                          |
    | **IP-based (IP routing)** | Chọn endpoint dựa trên **IP address của client**.                                  | Khi muốn phân loại user theo IP hoặc subnet.            | Thường dùng nội bộ hoặc network enterprise.                      |
    | **Multivalue answer**     | Trả nhiều IP cho một domain, Route 53 tự lọc các IP “healthy”.                     | Load balancing đơn giản + health check.                 | Giống weighted nhưng không có weight, mỗi response trả nhiều IP. |










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

- Nội dung phục vụ lab: [Code](/labs/sec_17)
- AWS Console liên quan:
    - [Route 53](/aws_console/README.md#route-53)
