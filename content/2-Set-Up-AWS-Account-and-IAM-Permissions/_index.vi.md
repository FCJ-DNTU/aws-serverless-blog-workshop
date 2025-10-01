+++
title = "Thiết lập tài khoản AWS và quyền IAM"
date = 2025
weight = 2
chapter = false
pre = "<b>2. </b>"
+++

> **Best practice**: Hạn chế sử dụng **Root User**. Thay vào đó, hãy tạo **IAM Role** với quyền tối thiểu cho Lambda để truy cập DynamoDB, đảm bảo bảo mật và dễ quản lý.

Trong **section này**, chúng ta sẽ tạo **IAM Role** và **Policy** để cho phép Lambda functions (`getPosts` và `createPost`) tương tác với bảng `BlogPosts` trong DynamoDB, giới hạn trong region **us-east-1** (hoặc region bạn chọn).

---

#### 1. Truy cập AWS IAM Management Console

- Đăng nhập vào AWS Management Console.
- Truy cập [IAM Console](https://us-east-1.console.aws.amazon.com/iam/home?region=ap-southeast-1#/home)

![iam-management.png](/images/2-Set-Up-AWS-Account-and-IAM-Permissions/2.1.png)

---

#### 2. Tạo Custom Policy
2.1. Tại thanh điều hướng bên trái, chọn [Policies](https://us-east-1.console.aws.amazon.com/iam/home?region=ap-southeast-1#/policies)

![policies.png](/images/2-Set-Up-AWS-Account-and-IAM-Permissions/2.2.png)

2.2. Tại giao diện **Policies**, chọn **Create policy**.

![create-policy.png](/images/2-Set-Up-AWS-Account-and-IAM-Permissions/2.3.png)

2.3. **Bước 1 - Specify permissions**  
Chọn **JSON tab** và dán JSON bên dưới vào **Policy Editor**, sau đó chọn **Next**:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "dynamodb:Scan",
        "dynamodb:PutItem"
      ],
      "Resource": "arn:aws:dynamodb:us-east-1:*:table/BlogPosts",
      "Condition": {
        "StringEquals": {
          "aws:RequestedRegion": "us-east-1"
        }
      }
    },
    {
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:PutLogEvents"
      ],
      "Resource": "*"
    }
  ]
}
```
![create-policy.png](/images/2-Set-Up-AWS-Account-and-IAM-Permissions/2.4.png)
![finish-policy.png](/images/2-Set-Up-AWS-Account-and-IAM-Permissions/2.5.png)

{{% notice note %}}
Policy này cho phép Lambda thực hiện dynamodb:Scan (lấy bài viết) và dynamodb:PutItem (tạo bài viết) trên bảng BlogPosts trong region us-east-1.
Quyền CloudWatch Logs (logs:*) cho phép Lambda ghi log.
{{% /notice %}}

#### 3. Tạo IAM Role cho Lambda
{{% notice info %}}
IAM Role được gán cho Lambda functions để cấp quyền truy cập. Policy lambda-dynamodb-access sẽ được gắn vào role này.
{{% /notice %}}

- 3.1. Truy cập Roles trong IAM Console
- 3.2. Chọn Create role
- 3.3. Trong giao diện Create role:
![Create-IAM-Role-for-Lambda.png](/images/2-Set-Up-AWS-Account-and-IAM-Permissions/3.1.png)
![Create-IAM-Role-for-Lambda.png](/images/2-Set-Up-AWS-Account-and-IAM-Permissions/3.2.png)
![Create-IAM-Role-for-Lambda.png](/images/2-Set-Up-AWS-Account-and-IAM-Permissions/3.3.png)

{{% notice note %}}
Chọn AWS service làm Trusted entity type
Chọn Lambda trong Use case
Chọn Next
{{% /notice %}}

- 3.4. Trong Add permissions:

{{% notice note %}}
Tìm và chọn policy lambda-dynamodb-access
(Tuỳ chọn) Thêm AWSLambdaBasicExecutionRole để bổ sung quyền CloudWatch Logs cơ bản
Chọn Next
{{% /notice %}}
![Add-permissions.png](/images/2-Set-Up-AWS-Account-and-IAM-Permissions/3.4.png)
![Add-permissions.png](/images/2-Set-Up-AWS-Account-and-IAM-Permissions/3.4.1.png)
- 3.5. Trong Name, review, and create:

{{% notice note %}}
Role name: lambda-blog-role
Description: Role for Lambda to access DynamoDB BlogPosts table
Chọn Create role
{{% /notice %}}
![Finish-IAM-Role-for-Lambda.png](/images/2-Set-Up-AWS-Account-and-IAM-Permissions/3.5.png)
#### 4. Gán IAM Role cho Lambda Functions
- 4.1. Truy cập Lambda Console
- 4.2. Mở function getPosts:

{{% notice note %}}
Đi tới Configuration → Permissions
Trong Execution role, chọn Edit
Chọn Existing role → lambda-blog-role
Chọn Save
{{% /notice %}}
![Assign-IAM-Role-to-Lambda-Functions.png](/images/2-Set-Up-AWS-Account-and-IAM-Permissions/4.1.png)
![Assign-IAM-Role-to-Lambda-Functions.png](/images/2-Set-Up-AWS-Account-and-IAM-Permissions/4.2.png)
- 4.3. Lặp lại bước 4.2 cho function createPost
![Assign-IAM-Role-to-Lambda-Functions.png](/images/2-Set-Up-AWS-Account-and-IAM-Permissions/4.3.png)
![Assign-IAM-Role-to-Lambda-Functions.png](/images/2-Set-Up-AWS-Account-and-IAM-Permissions/4.3.1.png)
- 4.4. Kiểm tra quyền:

{{% notice note %}}
Trong IAM Console, mở role lambda-blog-role
Đảm bảo policy lambda-dynamodb-access đã được gắn
Xác minh ARN của bảng BlogPosts trong policy khớp với bảng DynamoDB của bạn (vd: arn:aws:dynamodb:us-east-1:<account-id>:table/BlogPosts)
{{% /notice %}}
![Verify-in-the-IAM-Console.png](/images/2-Set-Up-AWS-Account-and-IAM-Permissions/4.4.png)
![Verify-in-the-IAM-Console.png](/images/2-Set-Up-AWS-Account-and-IAM-Permissions/4.4.1.png)
#### 5. Kiểm tra IAM Role với Lambda
- 5.1. Đăng nhập bằng IAM User (không dùng Root User)
- 5.2. Test function getPosts trong Lambda Console:
{{% notice note %}}
Đi tới tab Test, tạo event với {}
Nhấn Test
Kết quả mong đợi:
{{% /notice %}}
![Test-the-IAM-Role-with-Lambda.png](/images/2-Set-Up-AWS-Account-and-IAM-Permissions/5.1.png)
```json
{
  "statusCode": 200,
  "body": "[]",
  "headers": { "Content-Type": "application/json" }
}
```
{{% notice note %}}
(hoặc danh sách bài viết nếu bảng BlogPosts có dữ liệu).
{{% /notice %}}

- 5.3. Nếu gặp lỗi (vd: AccessDenied):

{{% notice note %}}
Kiểm tra CloudWatch Logs
Đảm bảo ARN trong policy khớp với bảng DynamoDB
Xác minh region = us-east-1 và biến môi trường TABLE_NAME=BlogPosts
{{% /notice %}}


Hoàn thành! Bạn đã tạo IAM Role và Policy cho Lambda functions để truy cập DynamoDB.