<h1 align="center">
    <strong>RELATIONAL DATABASE SERVICE (RDS)</strong>
</h1>

## MỤC LỤC

- [Lí thuyết](#lí-thuyết)
    - RDS - Định nghĩa
    - Các tính năng cơ bản của RDS
    - RDS Usecase
    - Các mô hình triển khai RDS
    - Aurora
    - Parameter Groups
- [Lab](#lab)
- [Tài liệu bổ sung](#tài-liệu-bổ-sung)


## <div align="center"><strong>LÍ THUYẾT</strong></div>
<!-- Thêm lí thuyết vào sau dòng này -->


- **RDS - Định nghĩa**
    - **Là** một service (Database as a Service) giúp người dùng tạo và quản lý các Relational Database.
        - User không cần quan tâm tới hạ tầng ở bên dưới.
        - Cho phép người dùng tạo ra các database instance độc lập hoặc cụm instance hoạt động dưới mode Cluster.
        - Không thể login vào instance level (khác với việc tự cài DB lên 1 EC2 instance).
        - Có thể scale theo 2 hình thức
            - Scale virtical: tăng hoặc giảm cấu hình instance.
            - Scale horizontal: thêm hoặc bớt node tuy nhiên node này chỉ có thể read (read-replica).
        - Có giới hạn về dung lượng ổ cứng tối đa (64TB đối với MySQL, Maria,... 16TB đối với Microsoft SQL).

- **Các tính năng cơ bản của RDS**
    - Cho phép **tạo các Instance hoặc cụm cluster DB** một cách nhanh chóng.
    - Tự động fail-over giữa master-slave instance khi có sự cố.
    - **High-Availability**: tự động cấu hình instance stand by, người dùng chỉ cần chọn.
    - **Tự động scale dung lượng lưu trữ** (optional).
    - **Liên kết với CloudWatch** để giám sát dễ dàng.
    - Automate backup & manage retention.
    - Dễ dàng chỉnh sửa setting ở cấp độ Database sử dụng parameter goup.

- **RDS Usecase**
    - RDS được sử dụng trong hầu hết các trường hợp cần database dạng quan hệ. 

        &nbsp;&nbsp;&nbsp;&nbsp;*Ví dụ*: lưu trữ thông tin của user, website e-commerce, education,...

    - RDS thích hợp cho các bài toán OLAP (Online Analatical Processing) nhờ khả năng truy vấn mạnh mẽ, cấu hình có thể scale theo yêu cầu.


- **Các mô hình triển khai RDS**

    - <strong>Single Instance</strong>

        - **Chỉ có 1 database instance duy nhất** được tạo ra trên 1 Availability Zone (AZ)
        - Nếu **sự cố xảy ra** ở cấp độ AZ, database **không thể truy cập**.
        - Phù hợp cho **môi trường Dev-Test** để tiết kiệm chi phí.

        <p align="center">
            <img src="docs_imgs\rds_deploy_type01.png" width="400" />
        </p>
        


    - <strong>Single Instance with Multi-AZ option enabled</strong>

        - Một bản sao của instance sẽ được tạo ra ở AZ khác và hoạt động dưới mode standby.
        - Nhiệm vụ của instance standby này là sync data từ master, không thể truy cập instance này. Khi có sự cố, instance standby sẽ - được chuyển lên thành master (việc này được AWS thực hiện tự động, endpoint url được giữ nguyên).
        - Số tiền bỏ ra sẽ x2, phù hợp cho Database Production.

        <p align="center">
            <img src="docs_imgs\rds_deploy_type02.png" width="400" />
        </p>


    - <strong>Master – Read Only cluster</strong>
        
        - Một instance với **mode ReadOnly** sẽ được tạo ra và **liên tục replica data từ master** instance.
        - Phù hợp cho **hệ thống có workload read > write**, muốn tối ưu performance của Database.
        - **Sau khi thiết lập quan hệ, instance được tạo ra sẽ kết hợp thành 1 cluster**. Trong trạng thái này, **nếu Master instance gặp sự cố, failover sẽ được tự động thực hiện**, ReadOnly instance được promote lên làm Master.

        <p align="center">
            <img src="docs_imgs\rds_deploy_type03.png" width="400" />
        </p>


    - <strong>Master – Read Only cluster with Multi-AZ option enabled</strong>

        - Tương tự mô hình Master – Read Only tuy nhiên các node đều được bật multi-AZ enabled.
        - Chi phí sẽ gấp 4 lần mô hình Single Instance.
        
        <p align="center">
            <img src="docs_imgs\rds_deploy_type04.png" width="400" />
        </p>


    - <strong>Master – Multi Read cluster</strong>

        - AWS cung cấp cơ chế cho phép tạo ra 1 cluster RDS giúp quản lý node và failover dễ dàng hơn, cụ thể:
            - Quản lý endpoint ở cấp độ cluster, không bị thay đổi khi instance trong cluster gặp sự cố.
            - Failover tự động.
            - Scale read instance dễ dàng.

        - Với mô hình này, nhiều hơn 1 reader instance sẽ được tạo ra.
        
        <p align="center">
            <img src="docs_imgs\rds_deploy_type05.png" width="400" />
        </p>


- **Aurora**

    - Aurora là công nghệ database độc quyền của AWS, hỗ trợ 2 loại engine là MySQL và PostgreSQL. Cluster Aurora bao gồm:
        - 1 Master node (Primary instance)
        - 1 hoặc nhiều Replica node (tối đa 15)
    - Aurora có 2 hình thức triển khai:
        - <details>
            <summary><strong>Cluster (Master + Multi Read Replica)</strong></summary>

            Là một cơ chế cho phép tạo ra cụm cluster cross trên nhiều regions.
            - Tăng tốc độ read tại mỗi region tương đương với local read.
            - Mở rộng khả năng scale số lượng node read (limit 15 read nodes cho 1 cluster).
            - Failover, Disaster recovery: rút ngắn RTO và giảm thiểu RPO khi xảy ra sự cố ở cấp độ region.
            
            Mặc định cluster ở region thứ 2 trở đi chỉ có thể read, tuy nhiên có thể enable write forwarding để điều hướng request tới primary cluster.

        </details>

        - <details>
            <summary><strong>Serverless</strong></summary>

            - Aurora Serverless là 1 công nghệ cho phép tạo Database dưới dạng serverless. 
            - Thay vì điều chỉnh cấu hình của DB instance, người dùng sẽ điều chỉnh ACU (Aurora Capacity Unit), ACU càng cao hiệu suất DB càng mạnh.
            - Phù hợp cho các hệ thống chưa biết rõ workload, hoặc workload có đặc trưng thay đổi lên xuống thường xuyên.

        </details>

        Cluster Volume tự động tăng size dựa vào nhu cầu của người dùng (không thể fix cứng size).
    - Những tính năng vượt trội của Aurora so với RDS thông thường
        - Hiệu năng tốt hơn (so với RDS instance cùng cấu hình. *Cái này là AWS quảng cáo ☺)
        - Hỗ trợ backtracking (1 tính năng cho phép revert database về trạng thái trong quá khứ tối đa 72h). Khác với restore từ snapshot đòi - hỏi tạo instance mới, backtracking restore ngay trên chính cluster đang chạy.
        - Tự động quản lý Write endpoint , Read endpoint ở cấp độ cluster.


- **Parameter Groups**
    - RDS là một managed service do đó không thể login vào instance. Nếu muốn can thiệp vào setting ở cấp độ DB (không phải setting OS) ta cần thông qua 1 cơ chế gián tiếp là Parameter Groups.
    - Khi tạo RDS nếu không chỉ định gì AWS sẽ sử dụng Parameter Group default của hệ database đang chọn. Default Parameter Group không thể chỉnh sửa.
    - Custom Parameter Group được tạo ra bằng cách copy default Parameter Group sau đó chỉnh sửa những tham số phù hợp với nhu cầu.
    - Parameter Groups có 2 loại là Cluster Parameter Groups và Instance Parameter Group. Hai loại này khác nhau về scope có thể apply.
    - Một số Parameter thường được custom riêng theo nhu cầu hệ thống:

        <br>
        
        <p align="center">
            <img src="docs_imgs\rds_para_groups.png" width="550" />
        </p>


- <details>
    <summary><strong>Để monitor query log cho RDS (ví dụ PostgreSQL) có mấy cách ?</strong></summary>

    - Slow Query Log (log_min_duration_statement)
        - Cấu hình log_min_duration_statement để ghi lại các query vượt quá một ngưỡng thời gian (ví dụ 1000ms). Log có thể xem trực tiếp trên server hoặc đẩy sang CloudWatch để theo dõi tập trung.
        - Phù hợp để phát hiện query chậm trong production mà không log toàn bộ truy vấn. Tuy nhiên nếu đặt ngưỡng quá thấp sẽ làm tăng I/O và dung lượng log.

    - pg_stat_statements extension
        - Bật extension pg_stat_statements để thu thập thống kê như tổng thời gian chạy, thời gian trung bình và số lần thực thi cho từng query pattern. Dữ liệu được lưu trong hệ thống catalog và truy vấn bằng SQL.
        - Phù hợp cho performance tuning và tối ưu index vì có thể xác định query nào tiêu tốn tài nguyên nhiều nhất. **Không lưu từng câu query riêng lẻ mà lưu theo dạng đã được normalize**.

    - Performance Insights (RDS/Aurora)
        - Performance Insights cung cấp dashboard trực quan về DB load, wait events và top SQL theo thời gian thực. Có thể drill-down để xem query nào đang chiếm nhiều tài nguyên nhất.

        - Phù hợp cho production monitoring vì có giao diện trực quan và không cần tự phân tích log. Dữ liệu mặc định lưu 7 ngày, muốn lưu lâu hơn cần trả phí.

    - CloudWatch Logs Integration
        - PostgreSQL trên RDS có thể export log sang CloudWatch để tập trung hóa monitoring và thiết lập alarm. Có thể kết hợp với metric filter để cảnh báo khi query vượt ngưỡng.

        - Phù hợp khi bạn muốn tích hợp với hệ thống giám sát tổng thể của hệ thống backend. Tuy nhiên cần cấu hình thêm và có chi phí lưu trữ log.

    - Application-level logging (Django debug logging)
        - Bật django.db.backends logger để log query và thời gian thực thi ở tầng ứng dụng. Phù hợp cho môi trường development và debugging nhanh.
        - **Không nên bật ở production** vì sẽ log toàn bộ query và ảnh hưởng hiệu năng. Chỉ dùng để kiểm tra logic hoặc tối ưu cục bộ.

</details>










<!-- Thêm lí thuyết vào trước dòng này -->






<br><br>


## <div align="center"><strong>LAB</strong></div>

<details>
<summary>&nbsp;&nbsp;<strong>Lab 01</strong></summary>
<!-- Thêm lab vào sau dòng này -->

<p align="center">
    <img src = "docs_imgs\rds_lab01.png" width="500">
</p>





<!-- Thêm lab vào trước dòng này -->
</details> 

<br><br><br><br><br>

## <div align="center"><strong>TÀI LIỆU BỔ SUNG</strong></div>

- Nội dung phục vụ lab: [Code](/labs/sec_11)
- AWS Console liên quan:
    - [Relational Database Service](/aws_console/README.md#relational-database-service-rds)
