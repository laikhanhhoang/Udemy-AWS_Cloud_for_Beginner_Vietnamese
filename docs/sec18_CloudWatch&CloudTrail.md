<h1 align="center">
    <strong>MONITORING AND AUDITING - CLOUDWATCH, CLOUDTRAIL</strong>
</h1>

## MỤC LỤC

- [Lí thuyết](#lí-thuyết)
    - [Cloudwatch](#cloudwatch)
        - [Cloudwatch - Định nghĩa](#cloudwatch---định-nghĩa): **giám sát và quản lý hệ thống và ứng dụng trên nền tảng AWS**.
        - [Các thành phần của Cloudwatch](#các-thành-phần-của-cloudwatch)
            - [Cloudwatch Metrics](#cloudwatch-metrics): **các thông số đo lường được thu thập và lưu trữ bởi CloudWatch**.
            - [Cloudwatch Alarm](#cloudwatch-alarm): **tạo ra cảnh báo tự động dựa trên các giá trị và threshold trong Metrics**.
            - [Cloudwatch Log](#cloudwatch-log): **lưu trữ, xem và phân tích các logs từ các ứng dụng và hệ thống trong môi trường AWS cũng như on-premise**. 

                &nbsp;&nbsp;&nbsp;&nbsp; *Cách **phân biệt Log và Metric** xem ở **Lưu ý**.*

            - [Cloudwatch Log Insight](#cloudwatch-log-insight): **một công cụ cho phép bạn truy vấn log thông qua một cú pháp do AWS định nghĩa**.
            - [Cloudwatch Insight](#cloudwatch-insight)
            - [Xray](#xray): cung cấp cái nhìn toàn cảnh và chi tiết đường đi của request trong application.
            - [Cloudwatch Dashboard](#cloudwatch-dashboard)
            - [Setup Cloudwatch Agent cho Custom Metricks và intergrate vào Cloudwatch Dashboard](#setup-cloudwatch-agent-cho-custom-metric)
                - Cách **phân biệt Metric và Log của từng Resource** trên Cloudwatch: dùng **namespace và dimension** với **metric**, **log group và log stream** cho **log**.
                - Cách **cài và chạy Cloudwatch Agent**: **`Tạo Iam Policy cho EC2` → `Config metric&log agent setting` → `Fetch setting json và launch`**.
            - [Lưu ý](#lưu-ý)
                - Khi thiết kế monitor cần quan tâm những gì?
                - Chú ý cho CW Alarm trong quá trình thiết kế và setting
            - [Pricing](#pricing)
        - [Cloudtrail](#cloudtrail)
            - [Cloudtrail - Định nghĩa](#cloudtrail---định-nghĩa): **Khác với CloudWatch** có mục đích **giám sát tình trạng của resource**, **CloudTrail ghi lại những hành động đã được thực thi** trong môi trường AWS **(who did what?)**.
            - [Các chức năng cơ bản của CloudTrail](#các-chức-năng-cơ-bản-của-cloudtrail)
    
- [Lab](#lab)
- [Tài liệu bổ sung](#tài-liệu-bổ-sung)


## <div align="center"><strong>LÍ THUYẾT</strong></div>


### <div><strong>CLOUDWATCH</strong></div>
<!-- Thêm lí thuyết Cloudwatch vào sau dòng này -->


#### **Cloudwatch - Định nghĩa**
- AWS CloudWatch là một dịch vụ được thiết kế để **giám sát và quản lý hệ thống và ứng dụng trên nền tảng AWS**. Nó cung cấp khả năng thu thập, xử lý và hiển thị các thông tin liên quan đến hiệu suất, khả năng mở rộng và lỗi của các tài nguyên trong môi trường AWS.
- CloudWatch cho phép:
    - **Theo dõi các thông số quan trọng** như CPU usage, network traffic, storage, database,... 
    - **Ghi log** để lưu trữ và phân tích các sự kiện quan trọng từ các ứng dụng và hệ thống. 
    - **Tạo ra các đồ thị và báo cáo** để theo dõi hiệu suất và tình trạng của các ứng dụng và hệ thống trên AWS.
    - **Hỗ trợ các cảnh báo tự động (Alarm) dựa trên ngưỡng** mà bạn đặt để thông báo khi các tài nguyên vượt quá giới hạn hoặc xảy ra lỗi. 

    <p align="center">
        <img src = "docs_imgs\cloudwatch_dinhnghia.png" width="700">
    </p>

<br>

#### **Các thành phần của Cloudwatch**

<p align="center">
    <img src = "docs_imgs\cloudwatch_thanhphan.png" width="600">
</p>

##### **CloudWatch Metrics**
- **Metrics** là **các thông số đo lường được thu thập và lưu trữ bởi CloudWatch**, như một tập hợp time-series data và được dùng để **làm input cho Alarm hoặc hiển thị trên graph của dashboard** phục vụ mục đích giám sát. *Chúng đại diện cho các giá trị số hoặc các điểm dữ liệu liên quan đến hoạt động của các tài nguyên trong môi trường AWS, VD EC2, RDS, Elastic Load Balancer, hoặc các dịch vụ tạo ra bởi người dùng.*

- Có **2 loại metrics** là 
    - **Default metrics** (do AWS thu thập) 
    - **Custom metrics** (do người dùng tự định nghĩa). 
        - Một số thông số trên EC2 instance mặc định không thể được thu thập bởi AWS, cần phải cài đặt agent lên để thu thập và gửi metrics lên CloudWatch. 
            *Ví dụ về agent*: CloudWatch Agent, Prometheus, Data dog agent, Telegraf, StatsD,...
        - Các metrics có thể được tính toán để tạo ra một metrics khác phục vụ nhu cầu autoscaling, alarm. 
        - *Ví dụ*: (số lượng instance) / (số lượng message trên SQS).

- CloudWatch Metrics có quy định về thời gian lưu trữ cho metrics, cụ thể như sau:
    - 1 second metrics are available for 3 hours.
    - 60 second metrics are available for 15 days.
    - 5 minute metrics are available for 63 days.
    - 1 hour metrics are available for 455 days (15 months).

    Điều này đồng nghĩa với việc những khoảng thời gian càng lâu sẽ càng ít detail hơn (độ dày của data point ít hơn).

##### **CloudWatch Alarm**
- CloudWatch Alarm cho phép bạn **tạo ra cảnh báo tự động dựa trên các giá trị Metrics**. Khi một CloudWatch Alarm được định cấu hình, nó **kiểm tra liên tục các giá trị Metrics và kích hoạt cảnh báo nếu các giá trị vượt quá hoặc thỏa mãn các ngưỡng xác định**.
- Khi một CloudWatch Alarm được kích hoạt, nó có thể thực hiện các hành động xác định trước, bao gồm:
    - Gửi thông báo qua email (kết hợp với SNS, SES).
    - Gửi thông báo qua tin nhắn văn bản (SMS) khi kết hợp với SNS.
    - Kích hoạt các hành động tự động: CloudWatch Alarm có thể kích hoạt các hành động tự động, chẳng hạn như restart EC2, adjust storage, hoặc call API đến các dịch vụ khác trong AWS. 
    - Các CloudWatch Alarm có thể được tạo ra dựa trên nhiều điều kiện khác nhau, bao gồm giá trị Metrics vượt ngưỡng, giá trị Metrics thấp hơn ngưỡng, trung bình hay tổng hợp các giá trị Metrics trong một khoảng thời gian nhất định, và nhiều điều kiện khác nữa.

##### **CloudWatch Log**
- CloudWatch Logs là một dịch vụ cho phép bạn **lưu trữ, xem và phân tích các logs từ các ứng dụng và hệ thống trong môi trường AWS cũng như on-premise**. 
- Nhiều service của AWS có optinion cho export thẳng log ra CloudWatch, chỉ cần enable lên là có thể xem được.
- CloudWatch Log hỗ trợ các thao tác:
    - **Xem các logs theo thời gian thực** trong giao diện CloudWatch Logs hoặc sử dụng API để truy xuất logs.
    - **Lọc và Tìm kiếm Logs**: CloudWatch Logs cung cấp công cụ để lọc và tìm kiếm logs theo các điều kiện xác định. Bạn có thể tìm kiếm các mẫu, từ khóa hoặc các thuộc tính đặc biệt trong logs để tìm kiếm và phân tích thông tin cần thiết.
    - **Lưu trữ Logs**: CloudWatch Logs cho phép bạn lưu trữ logs trong một kho lưu trữ lâu dài để duy trì lịch sử và thực hiện các phân tích sau này.
    - **Phân tích Logs**: Bạn có thể sử dụng các dịch vụ và công cụ khác của AWS như Amazon Athena, Amazon Elasticsearch hoặc các công cụ khác để phân tích logs từ CloudWatch Logs và trích xuất thông tin hữu ích.
- Concepts:
    - **Log Group**: Level cao nhất của CloudWatch Log. Thông thường **mỗi nhóm service hoặc resource sẽ push log ra một log group cụ thể**.
    - **Log Stream**: Đơn vị nhỏ hơn trong log group.
    - **Log Metrics Filter**: Định nghĩa các pattern của log để thống kê. Khi log message được set filter, đồng thời bạn cũng tạo ra một metrics trên log group đó.
    - **Log retention**: Thời gian log tồn tại trên CloudWatch, được set riêng cho từng log group.
    - **Log streaming and archive**: Bạn có thể export log ra các service như S3 nhằm mục đích lưu trữ lâu dài với giá rẻ hoặc stream sang Kinesis phục vụ mục đích realtime analytic.


##### **CloudWatch Log Insight**

- Là **một công cụ cho phép bạn truy vấn log thông qua một cú pháp do AWS định nghĩa**.


##### **CloudWatch Insight**
- Cung cấp công cụ hỗ trợ đơn giản hoá việc collect metrics và log một cách chi tiết. 
- Áp dụng cho **ứng dụng chạy trên Container và Lambda**.



##### **Xray**

Cung cấp cái nhìn toàn cảnh và chi tiết đường đi của request trong application, giúp điều tra, visualize dựa theo function, api, service.

<p align="center">
    <img src = "docs_imgs\cloudwatch_xray.png" width="700">
</p>

*Ví dụ về tracing đối với API Gateway + lambda:*
<p align="center">
    <img src = "docs_imgs\cloudwatch_xray_examplewapigw&lambda.png" width="700">
</p>


##### **CloudWatch Dashboard**
- CloudWatch Dashboard cho phép bạn theo dõi nhiều resource (cross regions) trên một view duy nhất.
- Bạn có thể add nhiều biểu đồ (widget) với nhiều hình dạng, customize size, màu sắc, title, đơn vị, vị trí...
- Widget có thể là biểu đồ, con số biểu diễn một metrics của một resource hoặc danh sách log từ một log group.


##### **Setup Cloudwatch Agent cho Custom Metric**
    
- Cách **phân biệt Metric và Log của từng Resource** trên Cloudwatch
    - **Metric** trong CloudWatch được phân biệt bởi **3 yếu tố chính**:
        - **Namespace**:  **nhóm metric**, ví dụ **`"ProjectA"`**.
        - **MetricName**: **tên metric**, ví dụ **`"cpu_usage_active"`**.
        - **Dimension(s)**: **một hoặc nhiều cặp key-value**, ví dụ **`"InstanceId=i-abc123, ClusterName=MyCluster"`**.
    - **Log** trong CloudWatch được lưu trong CloudWatch Logs, mỗi log được định nghĩa bởi:
        - **Log Group**: nhóm logs, ví dụ **`/prod/django-app`**.
        - **Log Stream**: luồng log cụ thể, ví dụ **`{instance_id}`** hoặc **`{container_id}`**.

    ```python
    # /opt/aws/amazon-cloudwatch-agent/config.json
    
    {
        "agent": {
            "metrics_collection_interval": 60,
            "run_as_user": "root"
        },
        "metrics": {
            "namespace": "ProjectA",                        # THỨ DÙNG ĐỂ PHÂN BIỆT METRIC!!!!
            "metrics_collected": {
                "cpu": {"measurement": ["cpu_usage_active"]},
                "mem": {"measurement": ["mem_used_percent"]}
            },
            "append_dimensions": {                          # THỨ DÙNG ĐỂ PHÂN BIỆT METRIC!!!!
                "InstanceId": "${aws:InstanceId}", 
                "Environment": "Production"
            }
        },
        "logs": {
            "logs_collected": {
            "files": {
                "collect_list": [
                {
                    "file_path": "/var/log/myapp/*.log",
                    "log_group_name": "/prod/myapp",        # THỨ DÙNG ĐỂ PHÂN BIỆT LOG!!!!
                    "log_stream_name": "{instance_id}"      # THỨ DÙNG ĐỂ PHÂN BIỆT LOG!!!!
                }
                ]
            }
            }
        }
    }
    ```

- Cách cài và chạy Cloudwatch Agent:
    - Tạo **IAM Role** với quyền **`CloudWatchAgentServerPolicy`** cho EC2 cho phép integrate vào Cloudwatch hoặc modify kĩ policy:
        ```json
        {
        "Version": "2012-10-17",
        "Statement": [
            {
            "Sid": "AllowCloudWatchMetrics",
            "Effect": "Allow",
            "Action": [
                "cloudwatch:PutMetricData"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                "cloudwatch:namespace": "ProjectA"
                }
            }
            },
            {
            "Sid": "AllowEC2ReadOnlyForAgent",
            "Effect": "Allow",
            "Action": [
                "ec2:DescribeInstances",
                "ec2:DescribeTags",
                "ec2:DescribeVolumes"
            ],
            "Resource": "*"
            },
            {
            "Sid": "AllowCloudWatchLogsLimited",
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:PutLogEvents",
                "logs:DescribeLogStreams"
            ],
            "Resource": [
                "arn:aws:logs:ap-southeast-1:123456789012:log-group:/prod/django-app*"
            ]
            }
        ]
        }
        ```
    - Cài Cloudwatch Agent bằng cách SSH hoặc trong User Data:
        ```bash
        sudo yum install amazon-cloudwatch-agent
        ```
    - Chạy Cloudwatch Agent:
        - **Cách 1**: OS sẽ cùng người dùng setup qua các câu hỏi:

            ```bash
            sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard
            ```
            *Lưu ý: Khi setting bằng cách này, **trong một số phiên bản Linux AMI**, Wizard **có thể không hỏi setting namespace** mà **để mặc định CWAGENT**.*
        - **Cách 2**: Chuẩn bị sẵn file config **`/opt/aws/amazon-cloudwatch-agent/config.json`** như ở trên, sau đó:

            ```bash
            # Cách 1: Lấy config từ S3
                aws s3 cp s3://my-bucket/config.json /opt/aws/amazon-cloudwatch-agent/config.json

            # Cách 2: Lấy từ trong máy cá nhân copy config.json lên EC2
                scp -i my-key.pem config.json ec2-user@54.123.45.67:/home/ec2-user/

                # Sau đó SSH lại vào EC2
                ssh -i my-key.pem ec2-user@54.123.45.67

                # Trên EC2, copy config.json vào thư mục agent
                sudo cp /home/ec2-user/config.json /opt/aws/amazon-cloudwatch-agent/config.json

            # Start CloudWatch Agent với config
            sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl \
                -a fetch-config -m ec2 -c file:/opt/aws/amazon-cloudwatch-agent/config.json -s
            ```


#### **Pricing**

- CloudWatch tính tiền dựa trên các yếu tố:
    - Số lượng metrics: $0.3/metrics/month
    - Số lượng Alarm: $0.1/alarm/month
        *Metrics và Alarm sẽ bị tính phí cao hơn nếu sd high resolution.*
    - Số lượng Dashboard: $3/dashboard/month
    - Số lượng event push lên CloudWatch
    - Dung lượng log lưu trữ trên CloudWatch: $0.7/GB
    - Dung lượng log scan khi sd CloudWatch Log Insight
    - and more...

- Một số phương án tiết kiệm cost cho CloudWatch
    - Chỉ monitor những thông số cần thiết thay vì monitor toàn bộ thông số.
    - Sử dụng metrics resolution phù hợp (vd 60s thay vì 5s), bởi không phải metrics nào cũng cần phải monitor với tần suất cao.
    - Đặt retention phù hợp cho các log group (vd 30d, 90d,...) thay vì để unlimited.
    - **Archive những log cũ không có nhu cầu tra cứu xuống S3, S3-Glacier** để giảm cost nhưng vẫn đảm bảo compliance về lưu trữ.
    - Tắt log DEBUG, INFO không cần thiết trên môi trường production để giảm lượng log gửi lên CloudWatch.

#### **Lưu ý**

- **Khi thiết kế monitor cần quan tâm những gì?**

    Đặt câu hỏi và trả lời các mục sau:
    - Hệ thống có những resource nào cần monitor?
    - Với mỗi resource cần monitor những thông số nào?
    - Những thông số nào cần set alarm? thông số nào cần visualize (dashboard)?
    - Những resource nào cần collect log?
    - Với mỗi resource có log, collect những loại log nào?
    - Có set alarm cho log không?
    - Metrics và Log lưu trữ ở đâu? (Native service or tự dựng?)
    - Khi có alarm cần thông báo tới ai?
    - Những yêu cầu khác liên quan quy trình vận hành...

- **Chú ý cho CW Alarm trong quá trình thiết kế và setting**
    - **Naming rule: dễ đọc, dễ hiểu**, nhìn vô biết ngay là hệ thống nào, môi trường nào, resource nào, vấn đề gì.
        - Tham khảo naming rule: <system-name>-<env>-<resource>-<alarm>
        - *Ví dụ*: ABCBank-dev-master_database-CPU-is-higher-80%-in-10-mins
    - **Threshold phải hợp lý**. Điều này phải được **kiểm chứng thông qua quá trình performance test và tuning**, **rất khó để setting kiểu “một phát ăn ngay”**.
    - Phân chia notification target cho những nhóm resource và người phụ trách phù hợp. Có thể tách mỗi nhóm resource thành 1 topic SNS.
    - Xác nhận với khách hàng/người thiết kế về những thông số cần collect/set alarm từ giai đoạn sớm của dự án.




<!-- Thêm lí thuyết Cloudwatch vào trước dòng này -->





<br><br><br><br>

### <div><strong>CLOUDTRAIL</strong></div>
<!-- Thêm lí thuyết Cloudtrail vào sau dòng này -->

#### **CloudTrail - Định nghĩa**
- CloudTrail là một dịch vụ **quản lý và giám sát log (log) hoạt động của resource trong môi trường AWS**. CloudTrail ghi lại và theo dõi các hoạt động của người dùng, tài khoản, dịch vụ và tài nguyên trong tài khoản AWS của bạn. Dịch vụ này giúp bạn hiểu rõ hơn về những gì đã xảy ra trong tài khoản AWS của bạn, bảo mật hơn và giúp tuân thủ security compliance.
- **Khác với CloudWatch** có mục đích **giám sát tình trạng của resource**, **CloudTrail ghi lại những hành động đã được thực thi** trong môi trường AWS **(who did what?)**.

#### **Các chức năng cơ bản của CloudTrail**
- Ghi lại các sự kiện quan trọng: CloudTrail ghi lại các sự kiện như việc create, update, delete 
resource, truy cập vào resource, tạo, sửa đổi hoặc xóa IAM roles, và các hoạt động khác liên 
quan đến tài nguyên AWS.
- Giám sát và kiểm tra tuân thủ: CloudTrail cung cấp thông tin chi tiết về các hoạt động trong tài 
khoản AWS, giúp bạn kiểm tra và đảm bảo tuân thủ các quy định, chính sách và quy trình an 
ninh nội bộ.
- Phân tích và bảo mật: Dữ liệu log của CloudTrail có thể được sử dụng để phân tích hoạt động, 
phát hiện sự cố bảo mật, theo dõi và phản ứng kịp thời đối với các sự kiện không mong muốn 
hoặc đe dọa bảo mật.
- Tương thích với các dịch vụ khác: CloudTrail tích hợp với các dịch vụ AWS khác như IAM, AWS 
Config và Amazon CloudWatch, tạo ra khả năng theo dõi và quản lý toàn diện.
- CloudTrail cung cấp các log record được lưu trữ trong S3 của AWS, nơi bạn có thể truy cập và 
phân tích dữ liệu log theo nhu cầu.








<!-- Thêm lí thuyết Cloudtrail vào trước dòng này -->






<br><br>


## <div align="center"><strong>LAB</strong></div>

<details>
<summary>&nbsp;&nbsp;<strong>Lab 01</strong></summary>
<!-- Thêm lab vào sau dòng này -->






<!-- Thêm lab vào trước dòng này -->
</details> 

<br><br><br><br><br>

## <div align="center"><strong>TÀI LIỆU BỔ SUNG</strong></div>

- Nội dung phục vụ lab: [Code](/labs/sec_)
- AWS Console liên quan:
    - [Dịch vụ 1](/aws_console/README.md#tên-dịch-vụ-1)
    - [Dịch vụ 2](/aws_console/README.md#tên-dịch-vụ-2)