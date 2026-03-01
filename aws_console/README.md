<h1 align="center">
    <strong>AWS CONSOLE</strong>
</h1>

## Mục lục

- [Elastic Load Balancer (ELB)](#elastic-load-balancer-elb)
    - [Tạo **Target Group**](#tạo-target-group)
    - [**Console** khi truy cập **ELB**](#console-khi-truy-cập-elb)
    - [Tạo **ELB**](#tạo-elb)

- [Auto Scaling Groups](#auto-scaling-groups)
    - [Launch Template](#asg---launch-template)
    - [Tạo group mới](#asg--tạo-group-mới)
    - [Tinh chỉnh phương pháp scale ASG](#asg---tinh-chỉnh-phương-pháp-scale)


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
    - Ngoài ra, muốn tinh chỉnh các thông tin khác vào tùy chọn **`Edit`** trên Console.

</details>




