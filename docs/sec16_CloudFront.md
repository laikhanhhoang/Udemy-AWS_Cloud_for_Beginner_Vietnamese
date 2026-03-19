<h1 align="center">
    <strong>CLOUD FRONT</strong>
</h1>

## MỤC LỤC

- [Lí thuyết](#lí-thuyết)
- [Lab](#lab)
- [Tài liệu bổ sung](#tài-liệu-bổ-sung)


## <div align="center"><strong>LÍ THUYẾT</strong></div>
<!-- Thêm lí thuyết vào sau dòng này -->


- **CDN - Định nghĩa**
    - Viết tắt của Content Delivery Network, **là** một mạng lưới giúp delivery nội dung tới người dùng cuối một cách nhanh chóng nhờ vào việc điều hướng request của họ tới các máy chủ chứa tài nguyên gần nhất.
        - Khi không có CDN, tài nguyên của server sẽ được deliver tới client một cách trực tiếp. Do đó tuỳ vào khoảng cách địa lý, tốc độ truy cập sẽ nhanh hay chậm.
        - Khi có CDN, **tài nguyên của server sẽ được cache trên các máy chủ Edge**, **request của user** tới một tài nguyên trên CloudFront sẽ được **redirect tới máy chủ Edge gần nhất**.

- **Cloud Front - Khái niệm cơ bản**
    - **Là** một CDN service của AWS.
    - Các khái niệm cần biết khi dùng dịch vụ:
        - Cache: AWS CloudFront lưu trữ các tài nguyên tại edge location để giảm thời gian phản hồi và tăng tốc độ tải trang web. Các tài nguyên này bao gồm hình ảnh, tập tin CSS và JavaScript.
        - Logging and Reporting: AWS CloudFront cung cấp các báo cáo về hoạt động của distribution, bao gồm lưu lượng và số lần truy cập.
        - Security: AWS CloudFront hỗ trợ nhiều tính năng bảo mật, bao gồm kết nối HTTPS, chữ ký số (Certificate) và xác thực người dùng.
        - Customize at the Edge: thông qua cơ chế Lambda@Edge, cho phép bạn thực thi các function trên các sự kiện CloudFront. Lợi thế về tốc độ và hiệu suất so với thực thi ở Origin. Một số use-case có thể kể đến như: Authen/Author, xử lý tính toán đơn giản, SEO, Intelligent routing, Anti Bot, Realtime image transformation, A/B Testing, User prioritilization.

- **Cách hoạt động của Cloud Front**
    - User access website, request một tài nguyên vd HTML file, Image, CSS..
    - DNS điều hướng request của user tới CloudFront edge location gần nhất (dựa theo độ trễ).
    - Tại CloudFront edge:
        - Nếu là **lần đầu request tài nguyên** đó:
            - CloudFront forward request tới Origin server (một HTTP server hoặc s3 bucket).
            - Origin server trả kết quả cho edge location.
            - Ngay sau khi nhận được firstbyte response, edge location forward object tới end-user đồng thời cache lại nội dung cho request lần sau.
        - **Từ lần request thứ 2 trở đi**, mặc định edge location sẽ trả về object được yêu cầu mà không gọi tới origin server giúp tăng tốc độ truy cập.

- **Usecase** 
    - Tăng tốc website (Image, CSS, Document, Video,...).
    - On demand video & video streaming.
    - Field level encrypt: CloudFront tự động mã hoá data được gửi lên từ người dùng, chỉ có backend application có key có thể giải mã.
    - Customize at the edge: Trả về mã lỗi khi server maintain hoặc authorize user trước khi forward request tới backend. Cần sử dụng Lambda@Edge.

- **Pricing**

    CloudFront tính phí dựa trên các mục sau:
    - No Up-front fee, người dùng chỉ trả tiền cho những gì sử dụng.
    - Lượng data thực tế tranasfer out to internet.
    - CloudFront function, Lambda@Edge.
    - Invalidation Request (clear cache).
    - Real-time log.
    - Field level encrypt.

- **CloudFront behavior**
    - Bằng việc cấu hình các behavior khác nhau, CloudFront giúp điều hướng request của người dùng tới đúng origin mong muốn.
    
        <p align="center">
            <img src = "docs_imgs\cloudfront_behaviour_example.png" width="450">
        </p>

    - *Lưu ý:* Khi setting behavior cần **quan tâm tới thứ tự trước sau của các behavior**, vì khi match 1 path rồi sẽ không đánh giá path tiếp theo.
        - Ví dụ với thiết kế trong hình phái trên:
            - Dễ thấy nếu để **`/*`** lên trên cùng sẽ làm các behavior bên dưới bị sai lệch. 
            - Vì **`/*`** đại diện cho mọi path nên mọi behaviour khác như **`/api/*`**, **`/images/*`** hay **`/videos/*`** đều bị hiểu là **`/*`**, và trỏ đến Website Bucket.

- **CloudFront Cache Policy**
    - CloudFront cache policy là **một cấu hình định nghĩa CloudFront sẽ cache và serves content như thế nào đối với User**.
    - Cache policy có thể được **định nghĩa riêng cho từng distribution hoặc behavior**. 

        Ví dụ có thể tùy chỉnh các một số rule như sau:
        - Thời gian content được cache.
        - Compress/non-compress.
        - Cho phép forward cookies & query strings hay không.
    - Việc apply caching policy khác nhau cho từng distribution hoặc behavior, cho 
    phép tinh chỉnh việc control caching cho từng URL hoặc request pattern. Việc này 
    giúp tối ưu hoá caching behavior cho từng loại content khác nhau như là: static 
    assets (image, css, js, video...), dynamic API response,...

- **CloudFront Origin request policy**
    - CloudFront origin request policy định nghĩa cách CloudFront **xử lý request tới origin**.
    - Khi user request content trên CloudFront, request được forward tới các **origin như S3, EC2, ALB**. 
    - Origin Request Policy cho phép modify request trước khi forward tới origin. Bạn có thể add/modify/remove header hoặc query string để optimize caching, tăng performance. Ngoài ra bạn có thể cấu hình CloudFront sign hoặc encrypt request để bảo vệ backend khỏi những access unauthorized.
    - Origin request policy hữu dụng khi có kiến trúc backend phức tạp vd như có nhiều loại origin server.









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

- Nội dung phục vụ lab: [Code](/labs/sec_16)
- AWS Console liên quan:
    - [Cloud Front](/aws_console/README.md#cloud-front)
