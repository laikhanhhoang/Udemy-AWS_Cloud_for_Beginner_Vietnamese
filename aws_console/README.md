<h1 align="center">
    <strong>AWS CONSOLE</strong>
</h1>

## Mục lục

- [Elastic Load Balancer (ELB)](#elastic-load-balancer-elb)
    - [Tạo **Target Group**](#tạo-target-group)
    - [**Console** khi truy cập **ELB**](#console-khi-truy-cập-elb)
    - [Tạo **ELB**](#tạo-elb)


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









