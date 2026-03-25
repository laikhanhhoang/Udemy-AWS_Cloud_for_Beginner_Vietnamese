<h1 align="center">
    <strong>EventBridge</strong>
</h1>

## MỤC LỤC

- [Lí thuyết](#lí-thuyết)
    - [Định nghĩa](#định-nghĩa): **là một dịch vụ dùng để quản lý và định tuyến sự kiện trong hệ thống hoạt động dựa trên kiến trúc publish-subscribe**.
    - [Các thành phần của EventBridge](#các-thành-phần-của-eventbridge): Event, Rule, Event Bus, Schema, Schema registry, Pipes, Scheduler.
    - [Usecase](#usecase): EventBus, Pipes, Scheduler.
- [Lab](#lab)
- [Tài liệu bổ sung](#tài-liệu-bổ-sung)


## <div align="center"><strong>LÍ THUYẾT</strong></div>
<!-- Thêm lí thuyết Event Bridge vào sau dòng này -->

### **Định nghĩa**

- AWS EventBridge **là một dịch vụ dùng để quản lý và định tuyến sự kiện trong hệ thống**. Nó cho phép bạn theo dõi, xử lý và phản ứng tự động với các sự kiện từ các nguồn khác nhau trong môi trường AWSCloud.
- EventBridge **hoạt động dựa trên kiến trúc publish-subscribe**, trong đó event source gửi sự kiện của mình tới EventBridge. EventBridge sau đó định tuyến sự kiện tới các event targets đã được đăng ký, ví dụ các Lambda function, SQS, WorkFlow, các dịch vụ AWS khác hoặc các ứng dụng tự xây dựng.
- EventBridge cung cấp **khả năng linh hoạt trong việc quản lý các event rules** (quy tắc định tuyến sự kiện) - **cho phép bạn lọc và xử lý sự kiện theo các tiêu chí** như kiểu sự kiện, nguồn sự kiện, hoặc nội dung sự kiện. Bạn có thể sử dụng các quy tắc này để triển khai tự động các tác vụ, như kích hoạt các hàm Lambda để xử lý sự kiện, gửi thông báo qua email hoặc SMS, hoặc đưa sự kiện vào các workflow phức tạp hơn.
- Với EventBridge, bạn có thể xây dựng các hệ thống ứng dụng phản ứng thời gian thực, tự động và linh hoạt hơn trong môi trường đám mây AWS, giúp bạn giảm thời gian triển khai và tăng khả năng mở rộng và module hóa của hệ thống.

<p align="center">
    <img src = "docs_imgs\eventbridge_kethop.png" width="600">
</p>

### **Các thành phần của EventBridge**
- **Event**: Một sự kiện nào đó xảy ra trong hệ thống AWS hoặc được chủ động tạo ra.
- **Rule**: Quy định các event và message sẽ được xử lý như thế nào nếu match rule.
- **Event Bus**: kênh giao tiếp để nhận và gửi event.
- **Schema**: Định nghĩa cấu trúc của event được gửi tới EventBridge.
- **Schema registry**: nơi lưu trữ những schema được detect tự động hoặc tạo bởi user.
- **Pipes**: một phương thức nhanh chóng để kết nối source và target. Có thể apply filter hoặc enrichment (transform) cho data.
- **Scheduler**: đặt lịch cho các tác vụ.

### Usecase
- EventBus: đóng vai trò thành phần trung gian **nhận các event và định tuyến chúng đến các target dựa trên các rule** (pattern matching), mà không yêu cầu producer biết trước consumer.
    - Lưu ý: Mỗi rule đều cần IAM Role để gửi message đến resource thuộc AWS.
<p align="center">
    <img src = "docs_imgs\eventbridge_eventbus.png" width="800">
</p>

- Pipes
<p align="center">
    <img src = "docs_imgs\eventbridge_pipes.png" width="800">
</p>

- Scheduler
<p align="center">
    <img src = "docs_imgs\eventbridge_schduler.png" width="800">
</p>














<!-- Thêm lí thuyết Event Bridge vào trước dòng này -->






<br><br>


## <div align="center"><strong>LAB</strong></div>

<details>
<summary>&nbsp;&nbsp;<strong>Lab 01</strong></summary>
<!-- Thêm lab vào sau dòng này -->




<!-- Thêm lab vào trước dòng này -->
</details> 

<br><br><br><br><br>

## <div align="center"><strong>TÀI LIỆU BỔ SUNG</strong></div>

- Nội dung phục vụ lab: [Code](/labs/sec_20)
- AWS Console liên quan:
    - [Event Bridge](/aws_console/README.md#eventbridge)
