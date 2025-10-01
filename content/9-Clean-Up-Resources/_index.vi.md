+++
title = "Dọn dẹp tài nguyên"
date = 2025
weight = 9
chapter = false
pre = "<b>9. </b>"
+++

#### Dọn dẹp tài nguyên
Sau khi hoàn thành hội thảo, việc dọn dẹp các tài nguyên AWS là điều cần thiết để tránh phát sinh chi phí ngoài phạm vi Free Tier. Phần này hướng dẫn bạn cách xóa các tài nguyên đã tạo: **CloudFront**, **S3**, **API Gateway**, **Lambda**, **DynamoDB**, và **IAM**.

#### Lý do để dọn dẹp:
- **Tiết kiệm chi phí**: Ngăn chặn các chi phí không cần thiết từ các tài nguyên không còn sử dụng.
- **Bảo mật**: Loại bỏ quyền IAM và các tài nguyên để tránh rủi ro truy cập trái phép.
- **Quản Lý Sạch Sẽ**: Giữ cho tài khoản AWS của bạn sạch sẽ cho các dự án trong tương lai.

#### Dọn dẹp tài nguyên

1. **Xóa Phân phối CloudFront**
- Truy cập **CloudFront** trong Bảng điều khiển Quản lý AWS. 
- Chọn phân phối mà bạn đã tạo (ví dụ: `d1234567890abcdef.cloudfront.net`).
- Nhấp vào **Vô hiệu hóa**, chờ cho trạng thái chuyển sang **Đã vô hiệu hóa**.
- Nhấp vào **Xóa** để xóa phân phối.

![delete-distribution.png](/images/9-Clean-Up-Resources/9.1.png)

2. **Xóa Bucket S3**
- Truy cập **S3** trong AWS Management Console.
- Chọn bucket mà bạn đã tạo (ví dụ: `blog-workshop-<mã-tài-khoản-của-bạn>`).
- Nhấp vào **Empty**, nhập `permanently delete` để xác nhận, sau đó chọn **Empty**.
- Sau khi bucket trống, chọn **Delete**, nhập tên bucket vào hộp xác nhận, sau đó nhấp **Delete bucket**.

![delete-bucket.png](/images/9-Clean-Up-Resources/9.2.png)

3. **Xóa API Gateway**

- Truy cập **API Gateway** trong Bảng điều khiển Quản lý AWS. 
- Chọn API `BlogAPI`. 
- Chọn **Actions** → **Delete**, nhập tên API để xác nhận, sau đó nhấn **Delete**.

![delete-api.png](/images/9-Clean-Up-Resources/9.3.png)

4. **Xóa các Hàm Lambda**

- Vào **Lambda** trong AWS Management Console. 
- Chọn hàm `getPosts`, nhấn **Actions** → **Delete**, xác nhận việc xóa. 
- Lặp lại cho hàm `createPost`.

![delete-lambda.png](/images/9-Clean-Up-Resources/9.4.png)

5. **Xóa Bảng DynamoDB**

- Truy cập **DynamoDB** trong Bảng điều khiển quản lý AWS. 
- Chọn bảng `Posts`. 
- Nhấp **Hành động** → **Xóa bảng**, nhập tên bảng để xác nhận, sau đó nhấp **Xóa**.

![delete-table.png](/images/9-Clean-Up-Resources/9.5.png)

6. **Xóa các tài nguyên IAM**

- Vào **IAM** trong Bảng điều khiển quản lý AWS.
- Trong **Policies**, chọn chính sách `lambda-blog-policy` (hoặc tương tự), nhấp vào **Delete**, xác nhận việc xóa.
- Trong **Roles**, chọn vai trò `lambda-blog-role`, nhấp vào **Delete**, xác nhận việc xóa.

![delete-iam.png](/images/9-Clean-Up-Resources/9.6.png)

{{% notice warning %}}
- Đảm bảo rằng bucket S3 trống trước khi xóa. 
- Kiểm tra kỹ các tài nguyên để đảm bảo chúng không bị xóa nhầm từ dự án khác. 
- Nếu gặp lỗi khi xóa (ví dụ: một tài nguyên đang được sử dụng), kiểm tra các phụ thuộc (chẳng hạn như quyền Lambda hoặc tích hợp API Gateway).
{{% /notice %}}

#### Hoàn Thành
- Bạn đã xóa tất cả các tài nguyên liên quan đến hội thảo, đảm bảo không phát sinh chi phí.
