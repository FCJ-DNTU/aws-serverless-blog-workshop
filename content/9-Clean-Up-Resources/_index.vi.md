+++
title = "Clean Up Resources"
date = 2025
weight = 9
chapter = false
pre = "<b>9. </b>"
+++

#### Clean Up Resources

After completing the workshop, it is necessary to clean up AWS resources to avoid incurring costs outside of the Free Tier. This section guides you through deleting the created resources: **CloudFront**, **S3**, **API Gateway**, **Lambda**, **DynamoDB**, and **IAM**.

#### Reasons for cleaning up:

- **Cost Savings**: Prevent unnecessary costs from resources that are no longer in use.

- **Security**: Remove IAM permissions and resources to avoid the risk of unauthorized access.

- **Clean Management**: Keep your AWS account clean for future projects.

#### Clean Up Resources

1. **Delete CloudFront Distribution**
- Go to **CloudFront** in the AWS Management Console.
- Select the distribution you created (e.g. `d1234567890abcdef.cloudfront.net`).
- Click **Disable**, wait for the status to change to **Disabled**.
- Click **Delete** to delete the distribution.

![delete-distribution.png](/static/images/9-Clean-Up-Resources/9.1.png)

2. **Delete S3 Bucket**
- Go to **S3** in the AWS Management Console.
- Select the bucket you created (e.g. `blog-workshop-<your-account-id>`).
- Click **Empty**, enter `permanently delete` to confirm, then select **Empty**.
- After the bucket is empty, select **Delete**, enter the bucket name in the confirmation box, then click **Delete bucket**.

![delete-bucket.png](/static/images/9-Clean-Up-Resources/9.2.png)

3. **Delete API Gateway**

- Go to **API Gateway** in AWS Management Console.

- Select API `BlogAPI`.

- Select **Actions** → **Delete**, enter the API name to confirm, then click **Delete**.

![delete-api.png](/static/images/9-Clean-Up-Resources/9.3.png)

4. **Delete Lambda Functions**

- Go to **Lambda** in AWS Management Console.

- Select function `getPosts`, click **Actions** → **Delete**, confirm deletion.
- Repeat for the `createPost` function.

![delete-lambda.png](/static/images/9-Clean-Up-Resources/9.4.png)

5. **Delete DynamoDB Table**

- Go to **DynamoDB** in the AWS Management Console.

- Select the `Posts` table.

- Click **Actions** → **Delete table**, enter the table name to confirm, then click **Delete**.

![delete-table.png](/static/images/9-Clean-Up-Resources/9.5.png)

6. **Delete IAM Resources**

- Go to **IAM** in the AWS Management Console.

- In **Policies**, select the `lambda-blog-policy` policy (or similar), click **Delete**, confirm the deletion.
- In **Roles**, select the role `lambda-blog-role`, click **Delete**, confirm deletion.

![delete-iam.png](/static/images/9-Clean-Up-Resources/9.6.png)

{{% notice warning %}}
- Make sure the S3 bucket is empty before deleting.

- Double check the resources to make sure they are not accidentally deleted from another project.

- If you encounter an error when deleting (e.g. a resource is in use), check the dependencies (such as Lambda permissions or API Gateway integrations).

{{% /notice %}}

#### Done
- You have deleted all resources related to the workshop, ensuring no costs are incurred.
