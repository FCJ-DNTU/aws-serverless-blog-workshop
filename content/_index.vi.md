+++
title = "Workshop: Triển Khai Website Blog Serverless Trên AWS Với Lambda, API Gateway, DynamoDB, S3 & CloudFront"
date = 2025
weight = 1
chapter = false
+++

# Workshop: Triển Khai Website Blog Serverless Trên AWS Với Lambda, API Gateway, DynamoDB, S3 & CloudFront

### Tổng quan

Trong workshop này, bạn sẽ học cách triển khai một **Website Blog Serverless** trên AWS, sử dụng **AWS Lambda** để xử lý logic backend, **API Gateway** để tạo API RESTful, **DynamoDB** làm cơ sở dữ liệu NoSQL, **S3** để host frontend tĩnh (React/Vite), và **CloudFront** để tối ưu hiệu suất phân phối nội dung.

![Workshop Architecture](/static/images/workshop_architecture.png)

### Mục tiêu:

- Hiểu cách thiết kế và triển khai ứng dụng serverless trên AWS.
- Tạo và cấu hình Lambda functions, API Gateway, DynamoDB, S3, và CloudFront.
- Biết cách triển khai ứng dụng frontend React/Vite trên S3.
- Kết nối frontend với backend thông qua API Gateway.
- Quản lý quyền truy cập giữa các dịch vụ bằng AWS IAM.
- Dọn dẹp tài nguyên để tối ưu chi phí.

### Yêu cầu:

- Tài khoản AWS với quyền truy cập IAM (Free Tier: https://aws.amazon.com/free).
- Kỹ năng JavaScript (Node.js) cơ bản.
- Công cụ: Node.js, npm, AWS CLI, Git, và trình duyệt web.
- (Tùy chọn) Postman để test API.

### Nội dung

1. [Giới thiệu](1-Introduction/)
2. [Thiết lập tài khoản AWS và quyền IAM](2-Set-Up-AWS-Account-and-IAM-Permissions/)
3. [Tạo bảng DynamoDB](3-Create-DynamoDB-Table/)
4. [Tạo Lambda Functions](4-Create-Lambda-Functions/)
5. [Cấu hình API Gateway](5-Configure-API-Gateway/)
6. [Chuẩn bị ứng dụng Frontend](6-Prepare-Frontend-Application/)
7. [Host Frontend trên S3](7-Host-Frontend-on-S3/)
8. [Cấu hình CloudFront](8-Configure-CloudFront/)
9. [Dọn dẹp tài nguyên](9-Clean-Up-Resources/)
