<h1 align="center">
    <strong>ELASTIC BEANSTALK</strong>
</h1>

## MỤC LỤC

- [Lí thuyết](#lí-thuyết)
    - [Định nghĩa](#định-nghĩa): là một dịch vụ được sử dụng để **triển khai, quản lý và mở rộng web app, web service một cách dễ dàng**, đóng vai trò như **một PaaS nơi bạn tập trung vào việc phát triển ứng dụng**.
    - [Các tính năng của Elastic Beanstalk](#các-tính-năng-của-elastic-beanstalk)
    - [Ưu/Nhược](#ưunhược): Chú yếu nằm ở việc **khó customise sâu**.
    - [Usecase](#usecase): Backend đơn giản/Monopolith/Team chưa có nhiều kinh nghiệm với AWS.
- [Lab](#lab)
- [Tài liệu bổ sung](#tài-liệu-bổ-sung)
    - Nội dung phục vụ lab: [Code](/labs/sec_21)
    - AWS Console liên quan:
        - [Elastic Beanstalk](/aws_console/README.md#elastic-beanstalk)

## <div align="center"><strong>LÍ THUYẾT</strong></div>
<!-- Thêm lí thuyết vào sau dòng này -->

### **Định nghĩa**
- Elastic Beanstalk là một dịch vụ được sử dụng để **triển khai, quản lý và mở rộng web app, web service một cách dễ dàng** trên môi trường AWS. Elastic Beanstalk cung cấp một nền tảng quản lý tự động cho việc triển khai ứng dụng và điều chỉnh các tài nguyên cần thiết như Server, Network, Database và các dịch vụ liên quan khác.
- Elastic Beanstalk **đóng vai trò như một PaaS nơi bạn tập trung vào việc phát triển ứng dụng** còn **AWS phụ trách phần hạ tầng và deployment**. Nó hỗ trợ nhiều ngôn ngữ lập trình và framework phổ biến như Java, .NET, PHP, Node.js, Python và Ruby.
- Để triển khai ứng dụng, bạn **chỉ cần đóng gói mã nguồn của mình và tải lên Elastic Beanstalk**. D**ịch vụ tự động xây dựng và cấu hình môi trường chạy ứng dụng của bạn dựa trên các yêu cầu và tùy chọn**. Nó cũng tự động quản lý việc scaling tài nguyên dựa trên tải lượng và yêu cầu của ứng dụng.
- Elastic Beanstalk cung cấp một giao diện quản lý đơn giản và tùy chọn linh hoạt cho phép bạn tùy chỉnh cấu hình và kiểm soát ứng dụng của mình. 

### **Các tính năng của Elastic Beanstalk**
- **Tự động tạo môi trường và các tài nguyên liên quan**.
- **Cung cấp khả năng auto scaling**.
- Monitoring thông qua giao diện thân thiện giúp bạn theo dõi được tình trạng của app cũng như môi trường.
- Hỗ trợ nhiều platform: Java, .NET, Node.js, PHP, Ruby, Python, Go, và Docker.
- Đa dạng hình thức deploy vd Console, CLI, VS Code, Eclipse... 
- Hỗ trợ deployment policies vd rolling, blue-green...
- **Tự động update platform version khi cần thiết**.

### **Ưu/Nhược**
- Ưu điểm:
    - **Dễ sử dụng**: Elastic Beanstalk cung cấp một giao diện quản lý đơn giản và tương tác trực quan, giúp bạn triển khai và quản lý ứng dụng một cách dễ dàng mà không cần hiểu rõ về cấu hình và quản lý hạ tầng.
    - **Tự động hóa quy trình triển khai**, giúp giảm thời gian và công sức cần thiết cho việc triển khai ứng dụng.
    - **Quản lý tự động tài nguyên**: Elastic Beanstalk tự động quản lý việc mở rộng và thu hẹp tài nguyên dựa trên tải lượng và yêu cầu của ứng dụng. 
    - **Hỗ trợ nhiều ngôn ngữ và framework**: Elastic Beanstalk hỗ trợ nhiều ngôn ngữ lập trình và framework phổ biến như Java, .NET, PHP, Node.js, Python và Ruby.
- Nhược điểm:
    - **Giới hạn tùy chỉnh**: Mặc dù Elastic Beanstalk cung cấp các tùy 
    chọn cấu hình và kiểm soát, nhưng nó có thể **hạn chế đối với 
    những ứng dụng phức tạp đòi hỏi sự tùy chỉnh cao hơn**. Nếu bạn 
    có nhu cầu tùy chỉnh chi tiết hơn, có thể cần sử dụng các dịch vụ 
    khác của AWS như EC2, ECS, EKS.
    - **Giới hạn của mô hình phân phối**: Elastic Beanstalk dựa trên mô 
    hình phân phối dựa trên máy ảo EC2, điều này có thể gây ra một 
    số hạn chế trong việc mở rộng và quản lý tài nguyên trong một số 
    tình huống đặc biệt. Đối với các ứng dụng đòi hỏi mô hình phân 
    phối linh hoạt hơn, có thể cần sử dụng service khác. 

### **Usecase**
- Elastic Beanstalk phù hợp với:
    - Hệ thống có backend đơn giản.
    - Team muốn đơn giản hoá quy trình deploy hoặc chưa có nhiều kinh nghiệm với AWS.
    - Hệ thống monolithic.
- Elastic Beanstalk không phù hợp với:
    - Hệ thống có backend phức tạp như microservices.
    - Có nhu cầu customize nhiều.


### **Pricing**
Về cơ bản Elastic Beanstalk không tính phí tuy nhiên bạn sẽ trả 
phí cho các resource được Elastic Beanstalk tạo ra vd: EC2, ALB, 
Database,...









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

- Nội dung phục vụ lab: [Code](/labs/sec_21)
- AWS Console liên quan:
    - [Elastic Beanstalk](/aws_console/README.md#elastic-beanstalk)
