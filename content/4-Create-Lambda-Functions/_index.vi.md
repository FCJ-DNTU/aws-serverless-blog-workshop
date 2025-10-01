+++
title = "Tạo Lambda Functions"
date = 2025
weight = 4
chapter = false
pre = "<b>4. </b>"
+++

#### AWS Lambda

**AWS Lambda** là một dịch vụ serverless cho phép chạy mã mà không cần quản lý máy chủ. Nó tự động mở rộng theo lưu lượng truy cập và chỉ tính phí khi mã được thực thi, rất phù hợp cho các ứng dụng như API backend.

#### Lambda Function là gì?
**Lambda Function** là một đoạn mã (viết bằng Node.js, Python, v.v.) được triển khai trên AWS Lambda để xử lý các tác vụ cụ thể, như truy xuất dữ liệu từ DynamoDB hoặc xử lý yêu cầu API.

#### Mô hình triển khai Lambda trong Workshop
Trong workshop này, chúng ta sẽ tạo hai Lambda functions:
- **`getPosts`**: Truy xuất danh sách bài viết từ bảng `BlogPosts` trong DynamoDB.
- **`createPost`**: Thêm bài viết mới vào bảng `BlogPosts`.
- Các functions này sẽ:
  - Tích hợp với API Gateway để xử lý yêu cầu HTTP.
  - Sử dụng IAM Role (đã tạo ở bước trước) để truy cập DynamoDB.
  - Được viết bằng Node.js với AWS SDK v3.

#### Lý do chọn Lambda:
- **Serverless**: Không cần quản lý máy chủ, giảm chi phí vận hành.
- **Chi phí thấp**: Free Tier cung cấp 1 triệu yêu cầu miễn phí mỗi tháng, phù hợp cho thử nghiệm.
- **Tích hợp dễ dàng**: Hoạt động tốt với API Gateway và DynamoDB.

#### Lý do chọn Node.js:
- **Phổ biến**: Hỗ trợ tốt cho các ứng dụng web và serverless.
- **AWS SDK v3**: Cung cấp các module nhẹ (như `@aws-sdk/client-dynamodb`) để tối ưu hiệu suất.
- **Cộng đồng lớn**: Dễ tìm tài liệu và thư viện hỗ trợ.

#### Tạo Lambda Functions

#### 1. Truy cập AWS Console
- Mở trình duyệt và truy cập vào [Lambda Console](https://ap-southeast-1.console.aws.amazon.com/lambda/home?region=ap-southeast-1#/begin/)

#### 2. Tạo function `getPosts`
- Trong **Lambda Dashboard**, chọn **Create function**.

![create-function.png](/images/4-Create-Lambda-Functions/4.1.png)

#### 3. Cấu hình function
- Trong phần **Create function**:
  - **Function name**: Nhập `getPosts`.
  - **Runtime**: Chọn **Node.js 20.x**.
  - **Role**: Chọn **Use an existing role** và chọn `lambda-blog-role` (đã tạo ở bước IAM).
  - Nhấn **Create function**.

![configure-getposts.png](/images/4-Create-Lambda-Functions/4.2.png)

#### 4. Thêm mã cho `getPosts`
- Trong giao diện function, chọn tab **Code**.
- Thay mã mặc định trong `index.mjs` bằng mã sau:
```javascript
import { DynamoDBClient, ScanCommand } from '@aws-sdk/client-dynamodb';

const dynamodb = new DynamoDBClient({});

export const handler = async () => {
  const params = {
    TableName: process.env.TABLE_NAME
  };
  try {
    const data = await dynamodb.send(new ScanCommand(params));
    return {
      statusCode: 200,
      body: JSON.stringify(data.Items || []),
      headers: { 'Content-Type': 'application/json' }
    };
  } catch (err) {
    console.error('Error:', err);
    return {
      statusCode: 500,
      body: JSON.stringify({ error: 'Could not retrieve posts' })
    };
  }
};
```
{{% notice note %}}
Thêm environment variable:
Trong tab Configuration → Environment variables, chọn Edit.
Thêm: TABLE_NAME = BlogPosts.
Nhấn Save.
{{% /notice %}}

#### 5. Tạo function createPost

{{% notice note %}}
Lặp lại bước 2-3 để tạo function mới với:
Function name: createPost.
Runtime: Node.js 20.x.
Role: lambda-blog-role.
{{% /notice %}}

Thay mã trong index.mjs bằng:

```
import { DynamoDBClient, PutItemCommand } from '@aws-sdk/client-dynamodb';
import { marshall } from '@aws-sdk/util-dynamodb';

const dynamodb = new DynamoDBClient({});

export const handler = async (event) => {
  const body = JSON.parse(event.body || '{}');
  const params = {
    TableName: process.env.TABLE_NAME,
    Item: marshall({
      postId: body.postId || Date.now().toString(),
      title: body.title || 'Untitled',
      content: body.content || '',
      createdAt: new Date().toISOString()
    })
  };
  try {
    await dynamodb.send(new PutItemCommand(params));
    return {
      statusCode: 200,
      body: JSON.stringify({ message: 'Post created successfully' }),
      headers: { 'Content-Type': 'application/json' }
    };
  } catch (err) {
    console.error('Error:', err);
    return {
      statusCode: 500,
      body: JSON.stringify({ error: 'Could not create post' })
    };
  }
};
```
{{% notice note %}}
Thêm environment variable TABLE_NAME = BlogPosts như bước 4.
{{% /notice %}}

#### 6. Cấu hình package.json

Để thêm dependency @aws-sdk/client-dynamodb và @aws-sdk/util-dynamodb, tạo file package.json trong thư mục mã nguồn:

```
{
  "name": "lambda-blog",
  "version": "1.0.0",
  "type": "module",
  "dependencies": {
    "@aws-sdk/client-dynamodb": "^3.0.0",
    "@aws-sdk/util-dynamodb": "^3.0.0"
  }
}
```

## Đóng gói mã:
- Tạo thư mục cục bộ cho mỗi function (ví dụ: getPosts/, createPost/).
- Chạy npm install trong mỗi thư mục.
- Nén thư mục thành file .zip (bao gồm node_modules, index.mjs, package.json).
- Upload file .zip vào Lambda trong tab Code → Upload from → .zip file.


{{% notice warning %}}
Đảm bảo sử dụng ES Module (index.mjs) và handler là index.handler.
File .zip phải bao gồm node_modules để tránh lỗi thiếu dependency.
Environment variable TABLE_NAME phải khớp với tên bảng DynamoDB (BlogPosts).
{{% /notice %}}

#### 7. Kiểm tra Lambda Functions

Trong giao diện mỗi function, chọn tab Test:
```
Đối với getPosts, tạo event với JSON rỗng {}.
Đối với createPost, tạo event mẫu:{
  "body": "{\"postId\": \"1\", \"title\": \"Test Post\", \"content\": \"This is a test post.\"}"
}
```

## Nhấn Test và kiểm tra kết quả:
- getPosts: Trả về danh sách bài viết (hoặc mảng rỗng nếu chưa có dữ liệu).
- createPost: Trả về thông báo { "message": "Post created successfully" }.
