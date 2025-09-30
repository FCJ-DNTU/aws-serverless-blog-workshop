+++
title = "Giới thiệu"
date = 2020-05-14T00:38:32+07:00
weight = 1
chapter = false
pre = "<b>1. </b>"
+++

# Workshop: Triển Khai Website Blog Serverless Trên AWS Với Lambda, API Gateway, DynamoDB, S3 & CloudFront

Trong workshop này, bạn sẽ học cách triển khai một **Website Blog Serverless** trên AWS, sử dụng **AWS Lambda** để xử lý logic backend, **API Gateway** để tạo API RESTful, **DynamoDB** làm cơ sở dữ liệu NoSQL, **S3** để host frontend tĩnh (React/Vite), và **CloudFront** để tối ưu hiệu suất phân phối nội dung.

![Workshop Architecture](/public/images/workshop_architecture.png)

### Mục tiêu:

- Hiểu cách thiết kế và triển khai ứng dụng serverless trên AWS.
- Tạo và cấu hình **Lambda functions**, **API Gateway**, **DynamoDB**, **S3**, và **CloudFront**.
- Triển khai ứng dụng frontend React/Vite trên S3.
- Kết nối frontend với backend qua API Gateway.
- Quản lý quyền truy cập giữa các dịch vụ bằng **AWS IAM**.
- Tối ưu chi phí với Free Tier và dọn dẹp tài nguyên.

### Yêu cầu:

- Tài khoản AWS với quyền truy cập IAM (Free Tier: https://aws.amazon.com/free).
- Kỹ năng JavaScript (Node.js) cơ bản.
- Công cụ: Node.js, npm, AWS CLI, Git.
- (Tùy chọn) Postman để test API.

### Kiến thức được cung cấp trong Workshop

**Các dịch vụ AWS và chi phí:**

**1. AWS Lambda**
**Công dụng:**
- AWS Lambda được sử dụng để chạy các hàm backend (`getPosts`, `createPost`) xử lý logic lấy và tạo bài viết.
- Lambda hoạt động theo mô hình serverless, tự động mở rộng và chỉ tính phí khi có yêu cầu.

**Chi phí:**
| Trạng thái | Loại | Chi phí/yêu cầu | Chi phí/tháng |
|------------|------|----------------|--------------|
| **Free Tier** | 1M yêu cầu miễn phí, 400,000 GB-giây | **$0.00** | **$0.00** |
| **Hết Free Tier** | $0.20/1M yêu cầu, $0.0000167/GB-giây | ~$0.01 (100K yêu cầu) | ~$0.30 (100K yêu cầu) |

---

**2. Amazon API Gateway**
**Công dụng:**
- API Gateway tạo các endpoint HTTP (GET `/posts`, POST `/posts`) để kết nối frontend với Lambda.
- Hỗ trợ RESTful API, quản lý yêu cầu và tích hợp với Lambda.

**Chi phí:**
| Trạng thái | Loại | Chi phí/yêu cầu | Chi phí/tháng |
|------------|------|----------------|--------------|
| **Free Tier** | 1M yêu cầu miễn phí | **$0.00** | **$0.00** |
| **Hết Free Tier** | $3.50/1M yêu cầu | ~$0.35 (100K yêu cầu) | ~$10.50 (3M yêu cầu) |

---

**3. Amazon DynamoDB**
**Công dụng:**
- DynamoDB là cơ sở dữ liệu NoSQL để lưu trữ và quản lý dữ liệu bài viết (bảng `BlogPosts`).
- Hỗ trợ đọc/ghi nhanh và tự động mở rộng.

**Chi phí:**
| Trạng thái | Loại | Chi phí/ngày | Chi phí/tháng |
|------------|------|--------------|--------------|
| **Free Tier** | 25 đơn vị đọc/ghi, 25GB lưu trữ | **$0.00** | **$0.00** |
| **Hết Free Tier** | $1.25/1M đơn vị đọc, $0.25/1M đơn vị ghi, $0.09/GB | ~$0.02 (10K đọc/ghi, 5GB) | ~$0.60 (10K đọc/ghi, 5GB) |

---

**4. Amazon S3**
**Công dụng:**
- S3 lưu trữ tệp tĩnh của frontend React/Vite (HTML, CSS, JS, hình ảnh).
- Hỗ trợ hosting website tĩnh.

**Chi phí:**
| Trạng thái | Dung lượng | Chi phí/ngày | Chi phí/tháng |
|------------|-----------|--------------|--------------|
| **Free Tier** | 5GB lưu trữ, 2,000 GET, 20,000 PUT | **$0.00** | **$0.00** |
| **Hết Free Tier** | $0.023/GB, $0.005/1,000 GET | ~$0.03 (5GB, 10K GET) | ~$1.00 (5GB, 10K GET) |

---

**5. Amazon CloudFront**
**Công dụng:**
- CloudFront là CDN giúp phân phối nội dung từ S3 với tốc độ cao và HTTPS.
- Giảm tải trực tiếp từ S3 và cải thiện trải nghiệm người dùng.

**Chi phí:**
| Trạng thái | Dung lượng | Chi phí/ngày | Chi phí/tháng |
|------------|-----------|--------------|--------------|
| **Free Tier** | 1TB băng thông, 10M yêu cầu | **$0.00** | **$0.00** |
| **Hết Free Tier** | $0.085/GB, $0.01/10,000 yêu cầu | ~$0.10 (50GB, 100K yêu cầu) | ~$3.00 (50GB, 100K yêu cầu) |

---

**6. IAM**
**Công dụng:**
- IAM quản lý quyền truy cập cho Lambda, API Gateway, và DynamoDB.
- Đảm bảo bảo mật bằng cách cấp quyền cụ thể (ví dụ: `dynamodb:Scan`, `dynamodb:PutItem`).

**Chi phí:**
- **Miễn phí** theo AWS.

---

**7. Tổng hợp chi phí**

**7.1. Chi phí khi còn Free Tier**  
| Dịch vụ | Loại | Chi phí/ngày | Chi phí/tháng |
|---------|------|--------------|--------------|
| **AWS Lambda** | 1M yêu cầu | **$0.00** | **$0.00** |
| **Amazon API Gateway** | 1M yêu cầu | **$0.00** | **$0.00** |
| **Amazon DynamoDB** | 25 đơn vị đọc/ghi | **$0.00** | **$0.00** |
| **Amazon S3** | 5GB lưu trữ | **$0.00** | **$0.00** |
| **Amazon CloudFront** | 1TB băng thông | **$0.00** | **$0.00** |
| **IAM** | Miễn phí | **$0.00** | **$0.00** |
| **Tổng cộng** |  | **$0.00** | **$0.00** |

---

**7.2. Chi phí khi hết Free Tier**  
| Dịch vụ | Loại | Chi phí/ngày | Chi phí/tháng |
|---------|------|--------------|--------------|
| **AWS Lambda** | 100K yêu cầu | ~$0.01 | ~$0.30 |
| **Amazon API Gateway** | 3M yêu cầu | ~$0.35 | ~$10.50 |
| **Amazon DynamoDB** | 10K đọc/ghi, 5GB | ~$0.02 | ~$0.60 |
| **Amazon S3** | 5GB, 10K GET | ~$0.03 | ~$1.00 |
| **Amazon CloudFront** | 50GB, 100K yêu cầu | ~$0.10 | ~$3.00 |
| **IAM** | Miễn phí | **$0.00** | **$0.00** |
| **Tổng cộng** |  | ~$0.51 | ~$15.40 |

---

**8. Kết luận**
- Khi còn trong Free Tier, hệ thống hoạt động với **chi phí $0**.
- Khi Free Tier hết hạn, chi phí duy trì hệ thống sẽ **dao động khoảng $15 - $16/tháng** (với giả định 100K yêu cầu Lambda, 3M yêu cầu API Gateway, 10K đọc/ghi DynamoDB, 5GB S3, 50GB CloudFront).
- Nếu hệ thống mở rộng, chi phí có thể tăng tùy theo lưu lượng truy cập và dữ liệu.

### Luồng hoạt động:

**1. Client truy cập trang web**
- Người dùng truy cập blog qua trình duyệt.
- Nội dung tĩnh (React/Vite) được lưu trên S3 và phân phối qua CloudFront.

**2. CloudFront tiếp nhận request**
- CloudFront phục vụ nội dung tĩnh từ S3 (HTML, CSS, JS).
- Chuyển tiếp các yêu cầu API (GET `/posts`, POST `/posts`) đến API Gateway.

**3. API Gateway xử lý request**
- API Gateway nhận yêu cầu từ CloudFront và chuyển đến Lambda functions (`getPosts` hoặc `createPost`).
- Lambda xử lý logic backend.

**4. Lambda truy vấn DynamoDB**
- Lambda function `getPosts` gọi DynamoDB để lấy danh sách bài viết.
- Lambda function `createPost` lưu bài viết mới vào DynamoDB.

**5. Trả kết quả về client**
- Lambda trả kết quả qua API Gateway.
- API Gateway gửi response về CloudFront.
- CloudFront có thể cache response để tối ưu tốc độ.
- Response được gửi đến client qua trình duyệt.

**6. Quản lý quyền truy cập bằng IAM**
- IAM kiểm soát quyền truy cập cho Lambda (ví dụ: `dynamodb:Scan`, `dynamodb:PutItem`).
- Đảm bảo bảo mật giữa các dịch vụ AWS.

