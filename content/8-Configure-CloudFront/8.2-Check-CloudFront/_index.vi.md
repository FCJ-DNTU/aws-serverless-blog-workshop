+++
title = "Cập nhật và kiểm tra CloudFront"
date = 2025
weight = 9
chapter = false
pre = "<b>8.2 </b>"
+++

![cloudfront-test.png](/static/images/8-Configure-CloudFront/8.2-cloudfront-origin-website/8.4.png)

#### Cập nhật và kiểm tra CloudFront
Trong phần này, chúng ta sẽ:
- Cập nhật bucket policy để CloudFront truy cập S3 thông qua Origin Access Control (OAC).
- Kiểm tra ứng dụng frontend qua CloudFront domain.
- Xác nhận website hoạt động với HTTPS.

#### Lý do cần cập nhật và kiểm tra:
- **Bảo mật**: OAC giới hạn truy cập S3 chỉ qua CloudFront, tăng cường bảo mật.
- **Xác nhận hoạt động**: Đảm bảo website hiển thị đúng và API hoạt động qua HTTPS.
- **Chuẩn bị hoàn thiện**: Kết nối toàn bộ hệ thống (frontend, backend, CDN).

#### Cập nhật và kiểm tra CloudFront

1. **Cập nhật Bucket Policy**
   - Truy cập S3 bucket (`blog-workshop-<your-account-id>`), chọn tab **Permissions**.
   - Trong **Bucket policy**, chọn **Edit** và thay policy hiện tại bằng policy sau (thay `<your-bucket-name>` và `<oac-id>` bằng tên bucket và OAC ID từ bước 8.1):
     ```json
     {
       "Version": "2008-10-17",
       "Id": "PolicyForCloudFrontPrivateContent",
       "Statement": [
         {
           "Effect": "Allow",
           "Principal": {
             "AWS": "arn:aws:iam::cloudfront:user/CloudFront Origin Access Identity <oac-id>"
           },
           "Action": "s3:GetObject",
           "Resource": "arn:aws:s3:::<your-bucket-name>/*"
         }
       ]
     }
     ```
   - Nhấn **Save changes**.

   ![update-bucket-policy.png](/static/images/8-Configure-CloudFront/8.2-cloudfront-origin-website/8.5.png)

2. **Tắt Public Access**
   - Trong **Permissions**, chọn **Edit** trong **Block public access**.
   - Bật **Block all public access** và tất cả tùy chọn con, nhấn **Save changes**.
   - Điều này đảm bảo chỉ CloudFront có thể truy cập bucket.

   ![block-public-access.png](/static/images/8-Configure-CloudFront/8.2-cloudfront-origin-website/8.6.png)

3. **Kiểm tra Website qua CloudFront**
   - Truy cập **Distribution domain name** từ bước 8.1 (ví dụ: `https://d1234567890abcdef.cloudfront.net`) trong trình duyệt.
   - Kiểm tra giao diện hiển thị danh sách bài viết và form tạo bài viết.
   - Thử tạo bài viết mới để xác nhận API Gateway hoạt động qua HTTPS.


4. **Debug lỗi (nếu có)**
   - Nếu website không hiển thị:
     - Kiểm tra bucket policy có đúng OAC ID và tên bucket.
     - Xác minh file `index.html` tồn tại trong bucket.
   - Nếu API không hoạt động:
     - Kiểm tra **Invoke URL** trong `src/App.jsx` (bước 6).
     - Xác minh CORS trong API Gateway (bước 5.2).
     - Xem console log trình duyệt hoặc CloudWatch Logs của Lambda.


{{% notice warning %}}
- Đảm bảo bucket policy sử dụng đúng OAC ID và tên bucket.
- Chỉ truy cập website qua **Distribution domain name** (HTTPS), không dùng **Bucket website endpoint** (HTTP).
- Nếu gặp lỗi 403, kiểm tra OAC và bucket policy.
{{% /notice %}}

#### Hoàn thành
- Bạn đã cập nhật bucket policy và kiểm tra ứng dụng qua CloudFront với HTTPS.
