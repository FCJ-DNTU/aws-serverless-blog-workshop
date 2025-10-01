+++
title = "Tạo bảng DynamoDB"
date = 2025
weight = 3
chapter = false
pre = "<b>3.1 </b>"
+++

#### DynamoDB là gì?
**Amazon DynamoDB** là một cơ sở dữ liệu NoSQL serverless, cung cấp khả năng lưu trữ và truy xuất dữ liệu với hiệu suất cao, tự động mở rộng, và tích hợp tốt với các dịch vụ AWS như Lambda.

#### Vì sao cần tạo bảng DynamoDB?
- **Lưu trữ dữ liệu bài viết**: Tạo bảng `BlogPosts` để lưu thông tin như ID, tiêu đề, nội dung, và ngày tạo của bài viết.
- **Hỗ trợ serverless**: DynamoDB không cần quản lý máy chủ, phù hợp với kiến trúc Lambda và API Gateway.
- **Hiệu suất cao**: Đảm bảo truy vấn nhanh và tự động điều chỉnh theo lưu lượng truy cập.

#### Các bước thực hiện:
1. Truy cập **AWS Management Console**.
2. Chọn dịch vụ **Amazon DynamoDB**.
3. Trong menu điều hướng, chọn [Tables](https://ap-southeast-1.console.aws.amazon.com/dynamodbv2/home?region=ap-southeast-1#tables/)

![dynamodb-interface.png](/images/3-Create-DynamoDB-Table/3.1-create-dynamodb-table/3.1.png)

4. Chọn **Create table**.
5. Trong giao diện **Create table**:
   - **Table name**: Nhập `BlogPosts`.
   - **Partition key**: Nhập `postId` (loại **String**) làm khóa chính để xác định duy nhất mỗi bài viết.
   - **Settings**: Chọn **Default settings** (chế độ **On-demand**, phù hợp với Free Tier).
   - Nhấn **Create table**.

![create-table.png](/images/3-Create-DynamoDB-Table/3.1-create-dynamodb-table/3.0.png)

6. Kiểm tra bảng:
   - Sau khi tạo, bảng `BlogPosts` sẽ xuất hiện trong danh sách **Tables** với trạng thái **Active**.

![table-created.png](/images/3-Create-DynamoDB-Table/3.1-create-dynamodb-table/3.2.png)

{{% notice note %}}
- Đảm bảo tên bảng là `BlogPosts`, vì Lambda functions (`getPosts`, `createPost`) sẽ sử dụng tên này trong environment variable `TABLE_NAME`.
- Tạo bảng trong cùng region với Lambda functions (ví dụ: `us-east-1`).
- IAM Role của Lambda cần quyền `dynamodb:Scan` và `dynamodb:PutItem` (đã cấu hình ở bước trước).
{{% /notice %}}

#### Hoàn thành
- Bạn đã tạo bảng `BlogPosts` để lưu trữ dữ liệu bài viết.
