+++
title = "Tạo REST API"
date = 2025
weight = 5
chapter = false
pre = "<b>5.1 </b>"
+++

#### Amazon API Gateway

**Amazon API Gateway** là một dịch vụ serverless cho phép tạo và quản lý các API RESTful hoặc WebSocket. Nó hoạt động như một cổng giao tiếp giữa frontend (React/Vite) và backend (Lambda functions), xử lý các yêu cầu HTTP và chuyển tiếp đến các dịch vụ AWS.

#### API Gateway trong Workshop
Trong phần này, chúng ta sẽ:
- Tạo một REST API để xử lý các yêu cầu HTTP.
- Tạo resource `/posts` để hỗ trợ các endpoint cho bài viết.
- Đảm bảo API tích hợp với Lambda functions (`getPosts`, `createPost`) ở bước tiếp theo.

#### Lý do chọn API Gateway:
- **Serverless**: Không cần quản lý máy chủ, tự động mở rộng theo lưu lượng truy cập.
- **Tích hợp dễ dàng**: Kết nối trực tiếp với Lambda và hỗ trợ các yêu cầu HTTP.
- **Chi phí thấp**: Free Tier cung cấp 1 triệu yêu cầu miễn phí mỗi tháng.

#### Tạo REST API

1. Truy cập vào **AWS Management Console**.
   - Tìm và chọn **API Gateway**.
   - Chọn **APIs** trong menu điều hướng.

   ![api-gateway-interface.png](/images/5-Configure-API-Gateway/5.1-create-rest-api/5.1.png)

2. Trong giao diện **APIs**:
   - Chọn **Create API**.
   - Chọn **REST API** và nhấn **Build**.

   ![create-api.png](/images/5-Configure-API-Gateway/5.1-create-rest-api/5.2.png)

3. Trong giao diện **Create API**:
   - **API name**: Nhập `BlogAPI`.
   - **Endpoint Type**: Chọn **Regional**.
   - Nhấn **Create API**.

   ![configure-api.png](/images/5-Configure-API-Gateway/5.1-create-rest-api/5.3.png)

4. Tạo resource `/posts`:
   - Trong **Resources**, chọn **Actions** → **Create Resource**.
   - **Resource Name**: Nhập `posts`.
   - **Resource Path**: `/posts`.
   - Bật **Enable API Gateway CORS** (để frontend truy cập được).
   - Nhấn **Create Resource**.

   ![create-resource.png](/images/5-Configure-API-Gateway/5.1-create-rest-api/5.4.png)

{{% notice warning %}}
- Đảm bảo bật CORS để frontend React/Vite có thể gửi yêu cầu đến API.
- Lưu tên API (`BlogAPI`) và resource (`/posts`) để sử dụng ở bước tiếp theo.
{{% /notice %}}

#### Hoàn thành
- Bạn đã tạo REST API và resource `/posts`, sẵn sàng để cấu hình phương thức GET/POST.
