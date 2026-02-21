## 01

<p align="center">
    <img src="sec07_lab01.png" width="600" />
</p>

- **httpd** sẽ phục vụ file `/var/www/html/index.html` khi truy cập vào root URL (`/`). Nếu muốn chỉnh sửa nội dung website, cần chỉnh file này bằng quyền root.
    - Mở file bằng lệnh `sudo vi /var/www/html/index.html` hoặc `sudo nano /var/www/html/index.html`.  
    - Sau khi chỉnh sửa xong, lưu file và reload httpd bằng `sudo systemctl restart httpd`.
    - Để phục vụ thư mục khác, chỉnh `DocumentRoot` trong `/etc/httpd/conf/httpd.conf` và restart httpd.  
    - Để phục vụ file mặc định khác, chỉnh `DirectoryIndex` trong cùng file config.

## 02

<p align="center">
    <img src="sec07_lab02.png" width="600" />
</p>

- Truy cập vào máy ảo bằng **Remote Desktop Connection**, do đó phải thêm connect **RDP** từ **My IP** vào phần Network.

## 03

<p align="center">
    <img src="sec07_lab03.png" width="600" />
</p>

- Vào **Advanced details**, copy nội dung file **`lab_03/lab3-user-data-meta-data.sh`** vào **`User data`**.

    <p align="center">
        <img src="sec07_lab03_userdata_dashboard.png" width="400" />
    </p>



## 04

<p align="center">
    <img src="sec07_lab04.png" width="600" />
</p>