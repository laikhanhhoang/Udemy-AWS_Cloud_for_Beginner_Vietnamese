<h1 align="center">
    <strong>AWS CONSOLE</strong>
</h1>

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


## Elastic Load Balancer (ELB)

<p align="center">
    <img src = "elb\elb_new.png" width="800">

</p>

<details>
<summary>&nbsp;&nbsp;</summary>

### Tạo **Target Group**


<p align="center">
    <img src = "elb\elb_create_target_group_step01.png" width="1000">

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


</details>


## Auto Scaling Groups

<p align="center">
    <img src = "asg\asg_create_scale_policy.png" width="800">
</p>


<details>
<summary></summary>


### ASG - Launch Template

<p align="center">
    <img src = "asg\asg_launch_template.png" width="1000">

</p>

<br>


### ASG -Tạo Group mới

Gồm 7 bước:

<p align="center">
    <img src = "asg\asg_create_step01.png" width="1000">

</p>

<p align="center">
    <img src = "asg\asg_create_step02.png" width="1000">

</p>

<p align="center">
    <img src = "asg\asg_create_step03.png" width="1000">

</p>

<br>

### ASG - Tinh chỉnh phương pháp scale

<p align="center">
    <img src = "asg\asg_create_scale_policy.png" width="1000">

</p>

<p align="center">
    <img src = "asg\asg_modify_policy.png" width="1000">

</p>

<br>

- Ghi chú:
    - Ngoài ra, muốn tinh chỉnh các thông tin khác vào tùy chọn **`Action/Edit`** trên Console.

</details>



## Relational Database Service (RDS)

<p align="center">
    <img src = "rds/rds_create.png" width="800">

</p>


<details>
<summary></summary>

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




</details>




## DynamoDB


<details>
<summary></summary>

Không có thao tác đặc biệt




</details>




## Lambda


<details>
<summary></summary>

### Thao tác



<p align="center">
    <img src = "link_tới_ảnh" width="1000">

</p>

<br>


- Ghi chú: <!--Ghi chú ở đây-->
    - z

</details>



