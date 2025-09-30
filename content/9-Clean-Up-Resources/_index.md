+++
title = "Clean Up Resources"
date = 2025
weight = 9
chapter = false
pre = "<b>9. </b>"
+++

#### Clean Up Resources

After completing the workshop, it’s essential to clean up AWS resources to avoid unexpected costs beyond the Free Tier. This section guides you through deleting the resources created: **CloudFront**, **S3**, **API Gateway**, **Lambda**, **DynamoDB**, and **IAM**.

#### Why Clean Up:
- **Cost Savings**: Prevent unnecessary charges from unused resources.
- **Security**: Remove IAM permissions and resources to avoid unauthorized access.
- **Organization**: Keep your AWS account tidy for future projects.

#### Clean Up Resources

1. **Delete CloudFront Distribution**
   - Go to [CloudFront](https://us-east-1.console.aws.amazon.com/cloudfront/v4/home#/distributions).
   - Select the distribution you created (e.g., `d1234567890abcdef.cloudfront.net`).
   - Click **Disable**, wait for the status to change to **Disabled**.
   - Click **Delete** to remove the distribution.

   ![delete-distribution.png](/images/9-Clean-Up-Resources/9.1.png)

2. **Delete S3 Bucket**
   - Go to [S3](https://console.aws.amazon.com/s3/home).
   - Select the bucket you created (e.g., `blog-workshop-<your-account-id>`).
   - Click **Empty**, type `permanently delete` to confirm, then select **Empty**.
   - After the bucket is emptied, click **Delete**, enter the bucket name in the confirmation field, and click **Delete bucket**.

   ![delete-bucket.png](/images/9-Clean-Up-Resources/9.2.png)

3. **Delete API Gateway**
   - Go to [API Gateway](https://console.aws.amazon.com/apigateway).
   - Select the `BlogAPI` API.
   - Click **Actions** → **Delete**, enter the API name to confirm, then click **Delete**.

   ![delete-api.png](/images/9-Clean-Up-Resources/9.3.png)

4. **Delete Lambda Functions**
   - Go to [Lambda](https://console.aws.amazon.com/lambda).
   - Select the `getPosts` function, click **Actions** → **Delete**, and confirm.
   - Repeat for the `createPost` function.

   ![delete-lambda.png](/images/9-Clean-Up-Resources/9.4.png)

5. **Delete DynamoDB Table**
   - Go to [DynamoDB](https://console.aws.amazon.com/dynamodb).
   - Select the `Posts` table.
   - Click **Actions** → **Delete table**, enter the table name to confirm, then click **Delete**.

   ![delete-table.png](/images/9-Clean-Up-Resources/9.5.png)

6. **Clean Up IAM Resources**
   - Go to [IAM](https://console.aws.amazon.com/iam).
   - Under **Policies**, select the `lambda-blog-policy` (or similar), click **Delete**, and confirm.
   - Under **Roles**, select the `lambda-blog-role`, click **Delete**, and confirm.

   ![delete-iam.png](/images/9-Clean-Up-Resources/9.6.png)

{{% notice warning %}}
Ensure the S3 bucket is emptied before deleting it.
Double-check resources to avoid deleting those used by other projects.
If deletion fails (e.g., resources in use), check dependencies (like Lambda permissions or API Gateway integrations).
{{% /notice %}}

#### Done
- You have successfully cleaned up all resources related to the workshop, preventing further costs.