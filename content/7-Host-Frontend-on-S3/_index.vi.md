+++
title = "Host Frontend trên S3"
date = 2025
weight = 7
chapter = false
pre = "<b>7. </b>"
+++

![s3-overview.png](/static/images/7-Host-Frontend-on-S3/7.1.png)

#### Amazon S3

**Amazon S3 (Simple Storage Service)** là một dịch vụ lưu trữ đối tượng (object storage) của AWS, cho phép lưu trữ và phục vụ các file tĩnh như HTML, CSS, JavaScript, và hình ảnh. Trong dự án này, S3 được sử dụng để host ứng dụng frontend React/Vite dưới dạng static website.

#### S3 trong Workshop
Trong phần này, chúng ta sẽ:
- Tạo một S3 bucket để lưu trữ các file tĩnh của ứng dụng frontend (thư mục `dist/` từ bước 6).
- Bật tính năng **Static website hosting** trên S3.
- Upload các file tĩnh từ dự án React/Vite.
- Cấu hình quyền truy cập công khai để người dùng có thể truy cập website.

#### Lợi ích khi sử dụng S3:
- **Chi phí thấp**: Free Tier cung cấp 5GB lưu trữ, 20.000 yêu cầu GET, và 2.000 yêu cầu PUT miễn phí mỗi tháng.
- **Dễ triển khai**: Hỗ trợ static website hosting, phù hợp với ứng dụng React/Vite.
- **Tích hợp với CloudFront**: Tăng tốc phân phối nội dung và hỗ trợ HTTPS (bước 8).
- **Độ bền cao**: Đảm bảo dữ liệu an toàn với độ bền 99.999999999%.

#### Tạo và cấu hình S3 Bucket

1. **Truy cập AWS Management Console**
   - Tìm và chọn **S3** trong AWS Console.

   ![search-s3.png](/static/images/7-Host-Frontend-on-S3/7.2.png)

2. **Tạo S3 Bucket**
   - Trong giao diện **S3**, chọn **Buckets** → **Create bucket**.
   - **Bucket name**: Nhập tên duy nhất, ví dụ: `blog-workshop-<your-account-id>`.
   - **Region**: Chọn cùng region với API Gateway (ví dụ: `us-east-1`).
   - **Object Ownership**: Chọn **ACLs disabled**.
   - **Block Public Access settings**: Tạm giữ nguyên mặc định (sẽ cập nhật sau).
   - Nhấn **Create bucket**.

   ![create-bucket.png](/static/images/7-Host-Frontend-on-S3/7.4.png)

3. **Bật Static Website Hosting**
   - Mở bucket vừa tạo, chọn tab **Properties**.
   - Cuộn xuống **Static website hosting**, chọn **Edit**.
   - Chọn **Enable**.
   - **Index document**: Nhập `index.html`.
   - Nhấn **Save changes**.
   - Lưu **Bucket website endpoint** (ví dụ: `http://blog-workshop-<your-account-id>.s3-website-<region>.amazonaws.com`).

   ![enable-static-hosting.png](/static/images/7-Host-Frontend-on-S3/7.5.png)

4. **Cấu hình quyền truy cập**
   - Trong tab **Permissions**, chọn **Edit** trong **Block public access**.
   - Bỏ chọn **Block all public access** và các tùy chọn con, nhấn **Save changes**.
   - Trong **Bucket policy**, chọn **Edit** và thêm policy sau:
     ```json
     {
       "Version": "2012-10-17",
       "Statement": [
         {
           "Effect": "Allow",
           "Principal": "*",
           "Action": "s3:GetObject",
           "Resource": "arn:aws:s3:::blog-workshop-<your-account-id>/*"
         }
       ]
     }
     ```
   - Nhấn **Save changes**.

   ![bucket-policy.png](/static/images/7-Host-Frontend-on-S3/7.6.png)

5. **Upload file frontend**
   - Trong thư mục `code/frontend/` (từ bước 6), chạy lệnh build nếu chưa thực hiện:
     ```bash
     cd code/frontend
     npm run build
     ```
   - Trong bucket, chọn **Upload** và upload toàn bộ thư mục `dist/`.
   - Đảm bảo tất cả file (HTML, CSS, JS) được upload với **Content-Type** mặc định.

   ![upload-files.png](/static/images/7-Host-Frontend-on-S3/7.7.png)

6. **Kiểm tra website**
   - Truy cập **Bucket website endpoint** trong trình duyệt (ví dụ: `http://blog-workshop-<your-account-id>.s3-website-<region>.amazonaws.com`).
   - Kiểm tra giao diện hiển thị danh sách bài viết và form tạo bài viết.
   - Nếu gặp lỗi (ví dụ: 403 Forbidden):
     - Kiểm tra bucket policy và quyền public access.
     - Xác minh **Invoke URL** trong `src/App.jsx` khớp với API Gateway.
     - Kiểm tra console log trình duyệt hoặc CloudWatch Logs của Lambda.

{{% notice warning %}}
- Tên bucket phải duy nhất trên toàn cầu (thêm `<your-account-id>` để tránh trùng lặp).
- Đảm bảo bucket policy cho phép `s3:GetObject` công khai.
- Lưu **Bucket website endpoint** để sử dụng trong bước cấu hình CloudFront.
{{% /notice %}}

#### Hoàn thành
- Bạn đã tạo S3 bucket, bật static website hosting, và upload ứng dụng frontend.
