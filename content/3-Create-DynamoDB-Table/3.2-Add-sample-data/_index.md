+++
title = "Add Sample Data"
date = 2025
weight = 4
chapter = false
pre = "<b>3.2 </b>"
+++

#### Why add sample data?

- **Test table**: Adding sample data to the `BlogPosts` table helps verify that the table works properly before integrating with Lambda functions.

- **Test query**: Sample data allows testing that the Lambda function `getPosts` returns the desired results.

- **Understand data structure**: Helps familiarize yourself with how data is stored in DynamoDB.

#### Steps:

1. Go to **AWS Management Console** and go to the **Amazon DynamoDB** service.

2. In the **Tables** list, select the `BlogPosts` table.

3. Select the **Explore table items** tab.

4. Select **Create item**.

5. Add the following properties:

- `postId` (String): `1`
- `title` (String): `Sample Post`
- `content` (String): `This is a sample blog post.`
- `createdAt` (String): `2025-08-20T12:00:00Z`
6. Click **Create item**.

![add-sample-item.png](/images/3-Create-DynamoDB-Table/3.2-Add-sample-data/3.3.png)

{{% notice note %}}
- Sample data to test that the Lambda function `getPosts` returns the correct result.
- Make sure the properties (`postId`, `title`, `content`, `createdAt`) match the expected structure in the Lambda functions.
- If adding multiple items, make sure each `postId` is unique.
{{% /notice %}}

#### Done
- You've added sample data to the `BlogPosts` table, ready to test with Lambda.
