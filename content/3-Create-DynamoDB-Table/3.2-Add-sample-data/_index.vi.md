+++
title = "Thêm dữ liệu mẫu"
date = 2025
weight = 4
chapter = false
pre = "<b>3.2 </b>"
+++

#### Vì sao cần thêm dữ liệu mẫu?
- **Kiểm tra bảng**: Thêm dữ liệu mẫu vào bảng `BlogPosts` giúp xác minh bảng hoạt động đúng trước khi tích hợp với Lambda functions.
- **Kiểm tra truy vấn**: Dữ liệu mẫu cho phép kiểm tra Lambda function `getPosts` trả về kết quả mong muốn.
- **Hiểu cấu trúc dữ liệu**: Giúp làm quen với cách dữ liệu được lưu trong DynamoDB.

#### Các bước thực hiện:
1. Truy cập **AWS Management Console** và vào dịch vụ **Amazon DynamoDB**.
2. Trong danh sách **Tables**, chọn bảng `BlogPosts`.
3. Chọn tab **Explore table items**.
4. Chọn **Create item**.
5. Thêm các thuộc tính:
   - `postId` (String): `1`
   - `title` (String): `Sample Post`
   - `content` (String): `This is a sample blog post.`
   - `createdAt` (String): `2025-08-20T12:00:00Z`
6. Nhấn **Create item**.

![add-sample-item.png](/images/3-Create-DynamoDB-Table/3.2-Add-sample-data/3.3.png)

{{% notice note %}}
- Dữ liệu mẫu giúp kiểm tra Lambda function `getPosts` trả về kết quả đúng.
- Đảm bảo các thuộc tính (`postId`, `title`, `content`, `createdAt`) khớp với cấu trúc dự kiến trong Lambda functions.
- Nếu thêm nhiều mục, đảm bảo mỗi `postId` là duy nhất.
{{% /notice %}}

#### Hoàn thành
- Bạn đã thêm dữ liệu mẫu vào bảng `BlogPosts`, sẵn sàng để kiểm tra với Lambda.
