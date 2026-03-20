<h1 align="center">
    <strong>NoSQL - DynamoDB</strong>
</h1>

## MỤC LỤC

- [Lí thuyết](#lí-thuyết)
- [Lab](#lab)
- [Tài liệu bổ sung](#tài-liệu-bổ-sung)
    - Nội dung phục vụ lab: [Code](/labs/sec_12)
    - AWS Console liên quan:
        - [DynamoDB](/aws_console/README.md#DynamoDB)


## <div align="center"><strong>LÍ THUYẾT</strong></div>
<!-- Thêm lí thuyết vào sau dòng này -->

#### **NoSQL là gì?**

- Non-Relational Database, còn được gọi là NoSQL (Not Only SQL) là một hệ 
thống cơ sở dữ liệu mà không sử dụng mô hình quan hệ truyền thống dưới 
dạng các bảng và các quan hệ khóa ngoại. Thay vào đó, nó sử dụng một cấu 
trúc dữ liệu khác, phù hợp hơn với các ứng dụng có khối lượng dữ liệu lớn, 
tốc độ truy vấn nhanh và tính mở rộng cao hơn.

- Một số mô hình NoSQL
    - Key-value: Lưu trữ dữ liệu dưới dạng các cặp key-value (khóa-giá trị). Các khóa được sử dụng để truy cập và lấy dữ liệu, trong khi giá trị có thể là bất kỳ kiểu dữ liệu nào.
    - Document: Lưu trữ dữ liệu dưới dạng tài liệu, thường là định dạng JSON hoặc XML. Các tài liệu được lưu trữ theo dạng phi cấu trúc, cho phép dữ liệu được lưu trữ một cách linh hoạt và thêm vào dễ dàng.
    - Column oriented: Lưu trữ dữ liệu dưới dạng các bảng với hàng và cột, nhưng khác với cơ sở dữ liệu quan hệ, các cột có thể được thêm và loại bỏ một cách độc lập.
    - Graph: Lưu trữ dữ liệu dưới dạng các nút và mối quan hệ giữa chúng, cung cấp khả năng xử lý dữ liệu phức tạp.

- So sánh SQL và NoSQL
    <p align="center">
        <img src="docs_imgs\dynamodb_sosanh_sqlvsnosql.png" width="500" />
    </p>

#### **DynamoDB**
- **Định nghĩa**: “Amazon DynamoDB is a fully managed, serverless, key-value NoSQL database designed to run high-performance applications at any scale. DynamoDB offers built-in security, continuous backups, automated multi Region replication, in-memory caching, and data import and export tools.”
- **Đặc trưng**:
    - **Serverless**: hạ tầng được quản lý bởi AWS. User tương tác với DynamoDB thông qua Console, CLI, Các tool client hoặc Software SDK.
    - Data được tổ chức thành các đơn vị **table**.
    - Độ trễ thấp (single digit milisecond).
    - SLA: 99.999% availability.
    - Automatic Scale Up/Down tuỳ theo workload (WCU, RCU).
    - Kết hợp được với nhiều service khác của AWS.
- **Ưu/Nhược điểm**

    <table>
    <thead>
        <tr>
        <th>Ưu điểm</th>
        <th>Nhược điểm</th>
        </tr>
    </thead>
    <tbody>
        <tr>
        <td>
            <ul>
            <li>Managed serverless, không cần quản lý hạ tầng.</li>
            <li>Auto scaling gần như không giới hạn (theo partition).</li>
            <li>Độ trễ rất thấp (single-digit millisecond).</li>
            <li>Hỗ trợ Strongly Consistent Read (tuỳ chọn).</li>
            <li>Tích hợp tốt với hệ sinh thái AWS (Lambda, IAM, API Gateway).</li>
            <li>Mã hoá dữ liệu at-rest và in-transit mặc định.</li>
            </ul>
        </td>
        <td>
            <ul>
            <li>Không hỗ trợ JOIN và relational constraint.</li>
            <li>Không phù hợp cho query phức tạp và OLAP workload.</li>
            <li>Thiết kế phụ thuộc chặt vào access pattern.</li>
            <li>Có thể gặp vấn đề hot partition nếu thiết kế partition key kém.</li>
            <li>Modeling và debugging khó hơn relational database.</li>
            </ul>
        </td>
        </tr>
    </tbody>
    </table>

#### **Usecases**
- Software application: hầu hết các software có nhu cầu về high concurrent cho hàng triệu user đều có thể cân nhắc sử dụng DynamoDB. Vd E-commerce.
- Media metadata store: Lưu trữ metadata cho các media.
- Gaming platform.
- Social media: mạng xã hội, bài đăng, bình luận.
- Logistic system.
- Ứng dụng IoT.

#### **Cấu trúc**
- Table: đơn vị quản lý cao nhất của DynamoDB. Table không thể trùng tên trên một region.
- Primary key: thông tin bắt buộc khi tạo table, Primary key chia làm 2 loại
    - Simple Primary key: chỉ bao gồm Partition key
    - Composite primary  key: bao gồm Partition key và Sort key.
- Global Secondary index (optional): bao gồm một cặp partition key & sort key tuỳ ý.
- Local Secondary index (Optional): bao gồm một cặp partition key giống với partition key của table và sort key tuỳ ý.
- Query: thực hiện truy vấn data trên một index.
- Scan: tìm toàn bộ giá trị thoả mãn điều kiện (tất nhiên sẽ chậm hơn query).
- PartiQL: SQL-compatible query language cho phép access data quan hệ, bán cấu trúc hoặc cấu trúc lồng nhau (nested).

<p align="center">
    <img src="docs_imgs\dynamodb_datatypes.png" width="600" />
</p>


#### **DynamoDB - Indexing**
- DynamoDB hỗ trợ 2 loại index là Global Secondary Index(GLI) và Local Secondary Index(LSI)
- Local Secondary Index(LSI): loại index phụ thuộc vào table và sử dụng partition key của table để 
xác định vị trí index. LSI giới hạn số lượng index của mỗi table là 5 và chỉ có thể tạo lúc tạo table. 
Các Attribute của LSI phải nằm trong bảng gốc.
- Global Secondary Index(GLI): một loại index độc lập với table và có thể xác định các attribute 
khác ngoài partition key để tìm kiếm nhanh dữ liệu. Với GSI, bạn có thể tạo ra các index mới mà 
không cần phải thay đổi cấu trúc của table gốc. DynamoDB hỗ trợ tối đa 20 GSI trên mỗi table.

- Khi tạo index, ta có thể setting attribute projection như sau:
- Project all: Secondary index sẽ bao gồm luôn các attribute của bảng chính
- Project key only: Secondary index chỉ bao gồm thông tin primary key.
- Project selected attribute: chỉ những attributes được chỉ định sẽ bao gồm trong index.












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

- Nội dung phục vụ lab: [Code](/labs/sec_12)
- AWS Console liên quan:
    - [DynamoDB](/aws_console/README.md#DynamoDB)
