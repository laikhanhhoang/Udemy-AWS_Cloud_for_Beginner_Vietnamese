<h1 align="center">
    <strong>MESSAGING SERVICES - SQS, SNS & SES</strong>
</h1>

## MỤC LỤC

- [Lí thuyết](#lí-thuyết)
    - [SQS](#sqs)
        - [Định nghĩa](#sqs---định-nghĩa): là một **dịch vụ hàng đợi thông điệp** mạnh mẽ và dễ sử dụng từ AWS, có **2 loại queue**: **Standard queue** và **FIFO queue**.
        - [Đặc trưng](#đặc-trưng-của-sqs): là một **managed service nên bạn không quản lý hạ tầng phía sau**. *Lưu ý với StandQ, phải tự xóa message sau khi xử lí.*
        - [Thông số](#thông-số-trong-sqs) (Xem **ví dụ trực quan** về các thông số **`Visibility Timeout`/ `Receive count`/ `Long polling wait time`**)
            - **Visibility Timeout**: **thời gian message tạm bị ẩn đi với các consumer khác trong khi được receive bởi một consumer nào đó**. Quá thời gian này nếu **message chưa bị xoá** sẽ quay trở lại queue. 
            - **Receive count**: **được cộng thêm 1 mỗi khi message được receive bởi một consumer**, dùng để setting **Dead-letter Queue**.
            - **Long polling wait time**: thời gian mà Amazon SQS **giữ request ReceiveMessage mở để chờ message** trước khi trả về empty nếu queue không có message.
    - [SNS](#sns)
    - [SES](#ses)
- [Lab](#lab)
- [Tài liệu bổ sung](#tài-liệu-bổ-sung)


## <div align="center"><strong>LÍ THUYẾT</strong></div>


### **SQS**
<!-- Thêm lí thuyết SNS vào sau dòng này -->

#### **SQS - Định nghĩa**

Simple Queue Service (SQS) là một **dịch vụ hàng đợi thông điệp** mạnh mẽ và dễ sử dụng từ AWS. SQS cho phép bạn truyền tin nhắn giữa các thành phần của hệ thống phân tán một cách đáng tin cậy và có khả năng mở rộng.

- Với SQS, bạn có thể **tạo ra các message queue và gửi/nhận message trên queue đó**. Hàng đợi được quản lý bởi SQS, đảm bảo tính đáng tin cậy và khả năng mở rộng cao. Việc các ứng dụng khác nhau có gửi nhận message trên queue một cách độc lập giúp tăng tính chịu lỗi và sự phân tán trong hệ thống của bạn (**de-coupling**).
- SQS cung cấp **hai loại queue**: 
    - **Standard queue** cung cấp khả năng mở rộng cao và đáng tin cậy.
    - **FIFO queue** đảm bảo tin nhắn được **xử lý theo tuần tự**, nhược điểm là bị giới hạn về tần suất gửi nhận.
- SQS cũng cung cấp các tính năng như **chế độ retry tự động**, **message filtering** và **khả năng xác nhận (acknowledge) tin nhắn**. Ngoài ra, SQS tích hợp với các dịch vụ AWS khác, cho phép bạn xây dựng các hệ thống phức tạp và đáng tin cậy.


#### **Đặc trưng của SQS**
- SQS là một **managed service do đó bạn không quản lý hạ tầng phía sau**. 
- SQS được tạo và **quản lý dưới các đơn vị message queue**.
- Tương tác gửi nhận message với queue thông qua Console, SDK, API
- Khả năng mở rộng của SQS là không giới hạn (về lý thuyết).
- Về cơ bản message nào **được gửi vào queue trước sẽ được xử lý trước** (khi có hành động get message). **Tuy nhiên** với Queue **type standard**, AWS **không đảm bảo 100%**.
- Với FIFO Queue, SQS đảm bảo deliver đúng thứ tự tuy nhiên tần suất gửi nhận message bị giảm xuống 300/s và 3000/s đối với batch process.
- SQS có thể được cấu hình notify message sang Lambda mỗi khi có mesage mới, sử dụng cho bài toán xử lý tự động hoặc ETL. 
- Về cơ bản SQS là nơi có **nhiệm vụ trung chuyển giữa Message Producer (Sender) và  Message Consumer(Receiver)**.
    - Với **Standard Queue**, message khi được gửi vào **queue sẽ tồn tại ở đó cho tới khi bị xoá hoặc hết thời gian retention**. Do vậy **Consumer phải chủ động xoá message đã xử lý xong**.
    - Với **FIFO Queue**, message sẽ được **delivery chính xác 1 lần tới consumer** (tự động bị xoá sau khi có event receive message).

    <p align="center">
        <img src = "docs_imgs\sqs_message_process.png" width="500">
    </p>

#### **Thông số trong SQS**



- **Visibility Timeout**: **thời gian message tạm bị ẩn đi với các consumer khác trong khi được receive bởi một consumer nào đó**. Quá thời gian này nếu **message chưa bị xoá** sẽ quay trở lại queue. 
    - Việc **apply** Visibility Timeout như thế nào cho **phù hợp** **hoàn toàn phụ thuộc vào nghiệp vụ của bạn**. 
    - *Ví dụ: một tác vụ xử lý **decode một video mất 10 mins** thì bạn nên để **`Visibility Timeout > 10 phút`** để **tránh tình trạng xung đột xử lý giữa các consumers**.*

        <p align="center">
            <img src = "docs_imgs\sqs_consumer_conflict_no_timeout.png" width="500">
        </p>

        <p align="center">
            <img src = "docs_imgs\sqs_consumer_conflict.png" width="500">
        </p>

- **Receive count**: **được cộng thêm 1 mỗi khi message được receive bởi một consumer**. 
    - Ta thường dựa vào đó để setting **Dead-letter queue (tự động move message sang queue khác để xử lí riêng)**.
    - *Ví dụ: hình dưới ta có 2 queue, apply dead-letter queue với receive count = 5, khi tới ngưỡng giới hạn mà message vẫn chưa được xử lý thành công và xoá khỏi queue, ta sẽ move sang một queue khác (dead-letter queue) để xử lý sau.*

        <p align="center">
            <img src = "docs_imgs\sqs_dead_letter_queue.png" width="500">
        </p>


- **Long polling wait time**: **thời gian SQS đợi trước khi return empty cho consumer** trong trường hợp **không có message nào trên queue**.


- **Workflow đầy đủ của SQS** có **Visibility Timeout**, **Receive count** và **Long polling wait time** đối với một message:

    ```
    Consumer gọi ReceiveMessage → SQS KHÔNG TRẢ NGAY → đợi tối đa X giây (long polling wait time)
        - Nếu CÓ message, trả message về NGAY cho consumer:
            1. Receive → count = 1 → visibility timeout
            → không delete → hết timeout → quay lại queue

            2. Receive → count = 2 → visibility timeout
            → không delete → hết timeout → quay lại queue

            ...

            n. Receive → count = n → visibility timeout
            → không delete → hết timeout → quay lại queue

            💥 n+1. Receive attempt → count = n+1
            → vượt maxReceiveCount
            👉 chuyển sang DLQ NGAY (không deliver cho consumer nữa)
    
        - Nếu KHÔNG, chờ hết Long polling wait time rồi trả empty message cho consumer.
    ```

<!-- Thêm lí thuyết SNS vào trước dòng này -->




### **SNS**
<!-- Thêm lí thuyết SQS vào sau dòng này -->









<!-- Thêm lí thuyết SQS vào trước dòng này -->



### **SES**
<!-- Thêm lí thuyết SES vào sau dòng này -->









<!-- Thêm lí thuyết SES vào trước dòng này -->













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

- Nội dung phục vụ lab: [Code](/labs/sec_19)
- AWS Console liên quan:
    - [SNS](/aws_console/README.md#sns)
    - [SQS](/aws_console/README.md#sqs)
    - [SES](/aws_console/README.md#ses)