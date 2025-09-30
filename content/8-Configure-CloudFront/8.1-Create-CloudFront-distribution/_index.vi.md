+++
title = "Tạo CloudFront Distribution"
date = 2025
weight = 8
chapter = false
pre = "<b>8.1 </b>"
+++

#### Amazon CloudFront

**Amazon CloudFront** là một dịch vụ CDN (Content Delivery Network) của AWS, giúp phân phối nội dung tĩnh (HTML, CSS, JavaScript) và động (API) với tốc độ cao và bảo mật HTTPS. Trong dự án này, CloudFront được sử dụng để phân phối ứng dụng frontend từ S3, cải thiện hiệu suất và thêm HTTPS.

#### CloudFront trong Workshop
Trong phần này, chúng ta sẽ:
- Tạo một CloudFront distribution để phân phối nội dung từ S3 bucket.
- Cấu hình Origin Access Control (OAC) để bảo mật truy cập vào bucket.
- Đặt các thiết lập cơ bản như HTTPS và HTTP methods.

#### Lợi ích khi sử dụng CloudFront:
- **Tốc độ cao**: Phân phối nội dung qua các edge locations toàn cầu.
- **Bảo mật**: Hỗ trợ HTTPS, tích hợp với AWS WAF (tùy chọn).
- **Chi phí thấp**: Free Tier cung cấp 1TB dữ liệu truyền đi mỗi tháng.
- **Tích hợp với S3**: Dễ dàng kết nối với bucket chứa file tĩnh.

#### Tạo CloudFront Distribution

1. **Truy cập AWS Management Console**
   - Tìm và chọn **CloudFront** trong AWS Console.

   ![search-cloudfront.png](/static/images/8-Configure-CloudFront/8.1-create-cloudfront/8.1.png)

2. **Tạo Distribution**
   - Trong giao diện **CloudFront**, chọn **Distributions** → **Create distribution**.

   ![create-distribution.png](/static/images/8-Configure-CloudFront/8.1-create-cloudfront/8.2.png)

3. **Cấu hình Distribution**
   - **Origin domain**: Chọn S3 bucket đã tạo (ví dụ: `blog-workshop-<your-account-id>`).
   - **Origin access**: Chọn **Origin access control settings (recommended)**.
   - **Origin access control**: Chọn **Create new OAC**.
     - Trong **Create new OAC**, giữ mặc định và nhấn **Create**.
     - Quay lại và chọn OAC vừa tạo.

   - **Viewer protocol policy**: Chọn **Redirect HTTP to HTTPS**.
   - **Allowed HTTP methods**: Chọn **GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE**.
   - **Web Application Firewall (WAF)**: Chọn **Do not enable security protections**.
   - Các phần còn lại giữ mặc định, cuộn xuống và nhấn **Create distribution**.

   ![configure-settings.png]()

4. **Chờ Distribution khởi tạo**
   - Quá trình khởi tạo mất khoảng 4-5 phút.
   - Sau khi hoàn thành, ghi lại **Distribution domain name** (ví dụ: `d1234567890abcdef.cloudfront.net`).

   ![distribution-created.png](/static/images/8-Configure-CloudFront/8.1-create-cloudfront/8.3.png)

{{% notice warning %}}
- Đảm bảo chọn đúng S3 bucket làm origin.
- Lưu **Distribution domain name** để kiểm tra và sử dụng trong bước tiếp theo.
- OAC yêu cầu cập nhật bucket policy (sẽ thực hiện ở bước 8.2).
{{% /notice %}}

#### Hoàn thành
- Bạn đã tạo CloudFront distribution để phân phối ứng dụng frontend.
