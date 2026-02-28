# AWS Cloud for beginner (Vietnamese)

Đây là repo cho khóa học về AWS cơ bản trên Udemy.

## Mục lục

- [Section 07 - EC2](docs/sec07_EC2.md)
- [Section 08 - IAM](docs/sec08_IAM.md)
- [Section 09 - S3](docs/sec09_S3.md)
- [Section 10 - ELB & ASG](docs/sec10_ELB&ASG.md)

## Kiến thức bổ sung

<details>
<summary><strong>HTTP & HTTPS</strong></summary>

- Nếu không chỉ định port trong URL, trình duyệt sẽ sử dụng port mặc định của giao thức: 80 cho HTTP và 443 cho HTTPS.

</details>


<details>
<summary><strong>OSI Model</strong></summary>

<p align="center">
    <img src="md_imgs\osi_model.jpg" width="400" />
</p>

- Tóm tắt
    - **Physical**: Truyền tín hiệu vật lý dưới dạng bit (0 và 1) qua dây điện, cáp quang hoặc sóng WiFi.

    - **Data Link**: Là frame gồm **Dst MAC - Src MAC - EtherType - Payload (IP Packet) - FCS**.  
    Truyền dữ liệu trong LAN và phát hiện lỗi frame.

    - **Network**: Là IP packet gồm **Src IP - Dst IP - TTL - Protocol - Payload (TCP/UDP Segment)**.  
    Định tuyến dữ liệu qua nhiều mạng.

    - **Transport**: Là segment/datagram gồm **Src Port - Dst Port - Sequence Number, ACK, Flags (HTTP) hoặc Length, Checksum (UDP) - Payload (Application Data)**.  
    Quản lý giao tiếp giữa các ứng dụng.

    - **Session**: Quản lý phiên giao tiếp gồm **Session Establishment - Session Maintenance - Session Termination**.  
    Thực tế thường được xử lý ở tầng Application hoặc trong TLS.

    - **Presentation**: Xử lý **Encryption/Decryption - Compression - Data Encoding (UTF-8, JSON, …)**.

    - **Application**: Chứa dữ liệu ứng dụng như HTTP message gồm **Method - Path - Headers - Body**.  
    Đây là nơi Django, browser, API hoạt động.

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

</details>
















