<h1 align="center">
    <strong>Identity and Access Managment (IAM)</strong>
</h1>

## MỤC LỤC

- [Lí thuyết](#lí-thuyết)
- [Lab](#lab)

## <div align="center"><strong>LÍ THUYẾT</strong></div>
<!-- Thêm lí thuyết vào sau dòng này -->

- **Policy**
    - Quy định việc ai/cái gì có thể hoặc không thể làm gì.
    - Một policy thường bao gồm nhiều Statement quy định Allow/Deny hành động trên resource dựa trên condition.
        Mỗi statement cần định nghĩa các thông tin:
        - Effect: có 2 loại là Allow & Deny; Deny được ưu tiên hơn.
        - Action: tập hợp các action cho phép thực thi.
        - Resource: tập hợp các resource cho phép tương tác.
        - [Condition]: Điều kiện kèm theo để apply statement này.
    - Policy có thể gắn vào Role/Group/User.
    - Policy có 2 loại là: Inline Policy và Managed Policy
        - Inline policy: được đính trực tiếp lên Role/User/Group và 
        không thể tái sử dụng ở Role/User/Group khác.
        - Managed Policy: Được tạo riêng và có thể gắn vào nhiều 
        User/Group/Role.
            - Managed Policy lại được chia thành 2 loại là AWS Managed và User Managed.
            - Việc lựa chọn giữa Inline vs Managed phải được tính toán 
            dựa trên các yếu tố như: tính tái sử dụng, quản lý thay đổi 
            tập trung, versioning & rollback.

    - Ví dụ:

        ```json
        {   
            "Version": "2012-10-17",
            "Statement": [
                {
                    "Effect": "Allow",
                    "Action": [
                        "ec2:StartInstance",
                        "ec2:StopInstance"
                    ],
                    "Resource": "arn:aws:ec2:*:*:instance/*",
                    "Condition": {
                        "StringEquals": {
                            "aws:ResourceTag/Environment": "Dev"
                        }
                    }
                }
            ]
        }
        ```
        Policy này quy định đối tượng được gán policy này 
        được phép thực hiện 2 hành động là StartInstance và 
        StopInstance trên toàn bộ các EC2 instance với điều 
        kiện instance đó có 1 thẻ tag tên Environment và giá 
        trị = Dev.

- **User**
    - Đại diện cho 1 profile của 1 người dùng trên AWS account.
    - User có thể login vào AWS Console sử dụng username/password.
    - User mặc định khi tạo ra sẽ không có quyền gì. Cần cấp quyền cho user thông qua Policy hoặc Group.
    - User có thể phát hành access-key/secret-key để sử dụng cho CLI hoặc test SDK trong quá trình test code. Cặp access/secret key này cũng sẽ đại diện cho user (thay vì dùng username/password).

- **Role**
    Sử dụng khi muốn cấp quyền cho 1 thực thể có thể tương tác với các resources khác trên AWS. Thường dùng để gắn vào EC2, Lambda, Container,...

    **Lưu ý**: một resource trên AWS không thể tương tác với resource khác nếu không được gán Role với các quyền thích hợp. Đây cũng chính là lý do khiến cho việc Role & Permission khiến cho mọi người tốn thời gian trouble shooting nếu không nắm rõ dịch vụ mà mình đang sử dụng.

- Group
    - Đại diện cho 1 nhóm user trên hệ thống.
    - Sử dụng khi  muốn phân chia quyền dựa theo vai trò trong dự án, phòng ban,...

        Nên thiết kế các nhóm user và phân quyền hợp lý, sau đó khi có người mới chúng ta chỉ cần add user đó vào các nhóm cần thiết giúp tiết kiệm thời gian và tránh sai sót (cấp dư hoặc thiếu quyền).




<!-- Thêm lí thuyết vào trước dòng này -->

<br><br>


## <div align="center"><strong>LAB</strong></div>

<details>
<summary></summary>
<!-- Thêm lab vào sau dòng này -->




<!-- Thêm lab vào trước dòng này -->
</details> 
