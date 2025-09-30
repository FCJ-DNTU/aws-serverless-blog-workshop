+++
title = "Cấu hình và kiểm tra API"
date = 2025
weight = 5
chapter = false
pre = "<b>5.2 </b>"
+++

#### Cấu hình API Gateway
Trong phần này, chúng ta sẽ:
- Thêm phương thức GET và POST cho resource `/posts` để truy xuất và thêm bài viết.
- Tích hợp với Lambda functions (`getPosts`, `createPost`).
- Bật CORS và triển khai API để sử dụng trong frontend.
- Kiểm tra API để đảm bảo hoạt động đúng.

#### Lý do cần cấu hình và kiểm tra:
- **Xử lý yêu cầu HTTP**: Phương thức GET/POST cho phép frontend tương tác với backend.
- **Tích hợp Lambda**: Kết nối API với Lambda để xử lý logic nghiệp vụ.
- **Kiểm tra hoạt động**: Đảm bảo API trả về kết quả đúng trước khi tích hợp với frontend.

#### Cấu hình và kiểm tra API

1. Truy cập vào **AWS Management Console** và chọn **API Gateway**.
2. Mở API `BlogAPI` đã tạo ở bước trước.

3. Tạo phương thức GET cho `/posts`:
   - Trong **Resources**, chọn resource `/posts`.
   - Chọn **Actions** → **Create Method**.
   - Chọn **GET** từ dropdown.
   - **Integration type**: Chọn **Lambda Function**.
   - **Lambda Function**: Chọn `getPosts`.
   - Nhấn **Save** và xác nhận quyền IAM nếu được yêu cầu.

   ![configure-get.png](/static/images/5-Configure-API-Gateway/5.2-API-Test-Configuration/5.5.png)

4. Tạo phương thức POST cho `/posts`:
   - Lặp lại bước 3, nhưng chọn **POST** và tích hợp với function `createPost`.

   ![configure-post.png](/static/images/5-Configure-API-Gateway/5.2-API-Test-Configuration/5.6.png)

5. Cấu hình CORS:
   - Trong **Resources**, chọn **Actions** → **Enable CORS**.
   - Chấp nhận các giá trị mặc định và nhấn **Enable CORS and replace existing CORS headers**.

   ![enable-cors.png](/static/images/5-Configure-API-Gateway/5.2-API-Test-Configuration/5.7.png)

6. Triển khai API:
   - Chọn **Actions** → **Deploy API**.
   - **Deployment stage**: Chọn **[New Stage]** và đặt tên (ví dụ: `prod`).
   - Nhấn **Deploy**.
   - Lưu **Invoke URL** (ví dụ: `https://gcnpmry3bd.execute-api.ap-southeast-1.amazonaws.com/prod`).

   ![deploy-api.png](/static/images/5-Configure-API-Gateway/5.2-API-Test-Configuration/5.8.png)

{{% notice warning %}}
- Đảm bảo bật CORS để frontend React/Vite có thể gửi yêu cầu đến API.
- Lưu **Invoke URL** để sử dụng trong frontend (bước 6).
- Kiểm tra quyền IAM của Lambda (`lambda-blog-role`) để đảm bảo API Gateway có thể gọi functions.
{{% /notice %}}

7. Kiểm tra API:
   - Sử dụng Postman hoặc trình duyệt để test:
     - **GET** `<Invoke URL>/posts`: Trả về danh sách bài viết từ DynamoDB.
     - **POST** `<Invoke URL>/posts` với body:
       ```json
       {
         "postId": "2",
         "title": "Test Post",
         "content": "This is a test post."
       }
       ```
   - Kiểm tra kết quả trong DynamoDB (tab **Explore table items**).

   ![test-api.png](/static/images/5-Configure-API-Gateway/5.2-API-Test-Configuration/5.9.png)

8. Xác nhận và tiếp tục:
   - API đã hoạt động và sẵn sàng kết nối với frontend.
   - Ghi lại **Invoke URL** để sử dụng trong bước cấu hình frontend.

   ![success.png](/static/images/5-Configure-API-Gateway/5.2-API-Test-Configuration/5.10.png)

#### Hoàn thành
- Bạn đã cấu hình và kiểm tra API để xử lý yêu cầu GET/POST.
