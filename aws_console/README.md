<h1 align="center">
    <strong>AWS CONSOLE</strong>
</h1>

Truy cập nhanh đến AWS Console của service bạn muốn tìm hiểu:

<p align="center">
  <a href="#virtual-private-cloud-vpc">VPC</a>
  <a href="#elastic-load-balancer-elb">ELB</a>
  <a href="#auto-scaling-groups">ASG</a>
  <a href="#lambda">Lambda</a>
  <a href="#api-gateway">API Gateway</a>
  <a href="#cloud-front">CloudFront</a>
  <a href="#cloudwatch">CloudWatch</a>
  <a href="#cloudtrail">CloudTrail</a>
  <a href="#sqs">SQS</a>
</p>

## Mục lục

- [Elastic Load Balancer (ELB)](#elastic-load-balancer-elb)
    - [Tạo **Target Group**](#tạo-target-group)
    - [**Console** khi truy cập **ELB**](#console-khi-truy-cập-elb)
    - [Tạo **ELB**](#tạo-elb)

- [Auto Scaling Groups (ASG)](#auto-scaling-groups)
    - [**Launch Template**](#asg---launch-template)
    - [**Tạo** ASG](#asg--tạo-group-mới)
    - [**Tinh chỉnh phương pháp scale** ASG](#asg---tinh-chỉnh-phương-pháp-scale)

- [Relational Database Service (RDS)](#relational-database-service-rds)

- [DynamoDB](#dynamodb)

- [Lambda](#lambda)
    - [Đóng gói Layer bằng file .zip](#lambda---tạo-layer)
    - [Tạo **Lambda function**](#lambda---tạo-lambda-function)

- [Virtual Private Cloud (VPC)](#virtual-private-cloud-vpc)
    - [Tạo VPC mới](#vpc---tạo-vpc-mới)
    - [Tùy chỉnh](#vpc---tùy-chỉnh)

- [API Gateway](#api-gateway)
    - [Tạo API Gateway](#api-gateway---tạo-api-gateway)


- [Cognito](#cognito)

- [Cloud Front](#cloud-front)
    - [Tạo CloudFront Distribution mới](#cloud-front---tạo-một-cloudfront-distribution)
    - [Add Origin mới vào CloudFront Distribution](#cloud-front----add-thêm-origin-vào-cloudfront-distribution)

- [Route 53](#route-53) **KHÔNG HỖ TRỢ VỚI ACC FREE TIER**

- [CloudWatch](#cloudwatch)

- [CloudTrail](#cloudtrail)

- [SQS](#sqs)
    - [Tạo SQS Queue mới](#sqs---tạo-sqs-queue-mới)

## Elastic Load Balancer (ELB)

<p align="center">
    <img src = "elb/elb_new.png" width="800">

</p>



### Tạo **Target Group**


<p align="center">
    <img src = "elb/elb_create_target_group_step01.png" width="1000">

</p>

<br>

- Ghi chú:
    - Port ở đây là port đang mở <strong>của instance</strong></i>


### **Console** khi truy cập **ELB**



<p align="center">
    <img src="elb/elb_thaotac.png" width="1000">
    <br>
</p>





### Tạo **ELB**



<p align="center">
    <img src="elb/elb_lb_listener&port.png" width="1000">
</p>

<br>

- Ghi chú:
    - Port ở đây là port <strong>của ELB</strong> khi truy cập &lt;ELB_DNS&gt;:&lt;DES_PORT&gt;.





## Auto Scaling Groups

<p align="center">
    <img src = "asg/asg_create_scale_policy.png" width="800">
</p>





### ASG - Launch Template

<p align="center">
    <img src = "asg/asg_launch_template.png" width="1000">

</p>

<br>


### ASG -Tạo Group mới

Gồm 7 bước:

<p align="center">
    <img src = "asg/asg_create_step01.png" width="1000">

</p>

<p align="center">
    <img src = "asg/asg_create_step02.png" width="1000">

</p>

<p align="center">
    <img src = "asg/asg_create_step03.png" width="1000">

</p>

<br>

### ASG - Tinh chỉnh phương pháp scale

<p align="center">
    <img src = "asg/asg_create_scale_policy.png" width="1000">

</p>

<p align="center">
    <img src = "asg/asg_modify_policy.png" width="1000">

</p>

<br>

- Ghi chú:
    - Ngoài ra, muốn tinh chỉnh các thông tin khác vào tùy chọn **`Action/Edit`** trên Console.





## Relational Database Service (RDS)

<p align="center">
    <img src = "rds/rds_create.png" width="800">

</p>




### Tạo Database



<p align="center">
    <img src = "rds/rds_create.png" width="1000">

</p>

<p align="center">
    <img src = "rds/rds_create_step02.png" width="1000">

</p>

<p align="center">
    <img src = "rds/rds_create_step03.png" width="1000">

</p>

<p align="center">
    <img src = "rds/rds_create_step04.png" width="1000">

</p>

<br>









## DynamoDB




Không có thao tác đặc biệt









## Lambda


<p align="center">
    <img src = "lambda/lambda_base.png" width="800">
</p>



### Lambda - Tạo Layer
<p align="center">
    <img src = "lambda/lambda_tao_layer.png" width="600">
</p>

### Lambda - Tạo Lambda Function


<p align="center">
    <img src = "lambda/lambda_layer&srccode.png" width="1000">
</p>


<p align="center">
    <img src = "lambda/lambda_configure.png" width="1000">
</p>

<p align="center">
    <img src = "lambda/lambda_alias.png" width="1000">
</p>



<br>






## Virtual Private Cloud (VPC)

<p align="center">
    <img src = "vpc/vpc_main_dashboard.png" width="800">
</p>



### VPC - Tạo VPC mới



<p align="center">
    <img src = "vpc/vpc_create_subnet.png" width="1000">
    <br>
    <i>Tạo subnet</i>
</p>

<p align="center">
    <img src = "vpc/vpc_create_rtb.png" width="1000">
    <br>
    <i>Tạo Route Table</i>
</p>

<p align="center">
    <img src = "vpc/vpc_igw_dashboard.png" width="1000">
    <br>
    <i>Tạo Internet Gateway</i>
</p>

<p align="center">
    <img src = "vpc/vpc_create_natgw.png" width="1000">
    <br>
    <i>Tạo NAT Gateway</i>
</p>

<p align="center">
    <img src = "vpc/vpc_create_s3_endpoint.png" width="1000">
    <br>
    <i>Tạo S3 Endpoint</i>
</p>

<br>


### VPC - Tùy chỉnh



<p align="center">
    <img src = "vpc/vpc_subnet_associate.png" width="1000">
    <br>
    <i>Tùy chỉnh Subnet</i>
</p>


<p align="center">
    <img src = "vpc/vpc_edit_pub_rtb.png" width="1000">
    <br>
    <i>Tùy chỉnh Route Table</i>
</p>


<br>








## API Gateway

<p align="center">
    <img src = "api_gw/api_gw_main_dashboard.png" width="800">
</p>




### API Gateway - Tạo API Gateway



<p align="center">
    <img src = "api_gw/api_gw_createapi_01_choose_api_type.png" width="1000">
    <br>
    <i>Bước 1: Chọn loại API.</i>
</p>

<p align="center">
    <img src = "api_gw/api_gw_createapi_02_conf.png" width="1000">
    <br>
    <i>Bước 2: Configure API.</i>
</p>

<p align="center">
    <img src = "api_gw/api_gw_createapi_03_create_resource.png" width="1000">
    <br>
    <i>Bước 3: Tạo đường dẫn.</i>
</p>

<p align="center">
    <img src = "api_gw/api_gw_createapi_04_create_method.png" width="1000">
    <br>
    <i>Bước 3: Chọn method (POST/GET/...) và tùy chỉnh tham số (ví dụ query params,...).</i>
</p>

<p align="center">
    <img src = "api_gw/api_gw_createapi_05_trigger_lambda_successful.png" width="1000">
    <br>
    <i>Trigger Lambda thành công.</i>
</p>

<p align="center">
    <img src = "api_gw/api_gw_createapi_06_test_postman_successful.png" width="1000">
    <br>
    <i>Test API thành công trên Postman.</i>
</p>

<br>









## Cognito

<p align="center">
    <img src = "cognito/cognito_configure_setup_app_userpool.png" width="800">
</p>







## Cloud Front

<p align="center">
    <img src = "cloudfront/cloudfront_createdist_success.png" width="800">
</p>




### Cloud Front - Tạo một CloudFront Distribution



<p align="center">
    <img src = "cloudfront/cloudfront_createdist_step01.png" width="1000">
</p>

<p align="center">
    <img src = "cloudfront/cloudfront_createdist_step02.png" width="1000">
</p>

- Lưu ý: 
    - **Origin Path** là một phần path được CloudFront tự động thêm vào mỗi request gửi về origin.
        - Ví dụ:
            - Bạn có CloudFront distribution với:
                - **Origin domain**: **`https://abcd1234.execute-api.us-east-1.amazonaws.com`** (API Gateway)
                - **Origin path**: **`/dev`**
            - Khi client gọi CloudFront URL: **`https://d123.cloudfront.net/calculate?a=5&b=6`**.
            - CloudFront sẽ gửi request về origin là: **`https://abcd1234.execute-api.us-east-1.amazonaws.com/dev/calculate?a=5&b=6`**
            - Nếu không điền Origin Path, CloudFront gửi nguyên path client tới origin, tức là **`/calculate?a=5&b=6`**.



<p align="center">
    <img src = "cloudfront/cloudfront_createdist_step03_confirm.png" width="1000">
</p>

<p align="center">
    <img src = "cloudfront/cloudfront_createdist_success.png" width="1000">
</p>



### Cloud Front -  Add thêm Origin vào CloudFront Distribution

<p align="center">
    <img src = "cloudfront/cloudfront_addorigin_step01.png" width="1000">
</p>

<p align="center">
    <img src = "cloudfront/cloudfront_addorigin_behaviour.png" width="1000">
</p>

<p align="center">
    <img src = "cloudfront/cloudfront_addorigin_connectapigw_success.png" width="1000">
</p>


<br>









## **Route 53**

**KHÔNG HỖ TRỢ ACC FREE TIER**








## **CloudWatch**






## **CloudTrail**



## **SQS**

<p align="center">
    <img src = "sqs/sqs_createqueue_success.png" width="800">
</p>

### SQS - Tạo SQS queue mới

<p align="center">
    <img src = "sqs/sqs_createqueue_step01_pic01.png" width="1000">
    <p style="text-align: center;">
        <span style="display: inline-block; text-align: left;">
            <i><strong>Visibility Timeout</strong>: thời gian message tạm bị ẩn đi với các consumer khác trong khi được receive bởi một consumer nào đó</i> 
            <br>
            <i><strong>Delivery Delay</strong>: thời gian message phải chờ trong queue trước được gửi cho consumer</i>
            <br>
            <i><strong>Message Retention Speed</strong>: thời gian 1 message được lưu trong queue trước khi bị xóa tự động</i>
            <br>
            <i><strong>Receive message waiting time</strong>: Long polling wait time</i>
        </span>
    </p>
</p>

<p align="center">
    <img src = "sqs/sqs_createqueue_step01_pic02.png" width="1000">
    <br>
    <i>Setting Policy và Dead-letter queue</i>
</p>


<p align="center">
    <img src = "sqs/sqs_createqueue_success.png" width="1000">
    <br>
    <i>Tạo queue thành công!!! Dùng <strong>Purge</strong> để xóa tất cả message trong queue (Dev Stage), <strong>Delete</strong> để xóa hẳn queue</i>
</p>


<p align="center">
    <img src = "sqs/sqs_testing_create_send_polling_message.png" width="1000">
    <br>
    <i>Testing message</i>
</p>

<p align="center">
    <img src = "sqs/sqs_monitor.png" width="1000">
    <br>
    <i>Monitoring message</i>
</p>
