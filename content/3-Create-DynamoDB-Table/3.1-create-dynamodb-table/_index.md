+++
title = "Create DynamoDB Table"
date = 2025
weight = 3
chapter = false
pre = "<b>3.1 </b>"
+++

#### What is DynamoDB?

**Amazon DynamoDB** is a serverless NoSQL database that provides high-performance, scalable data storage and retrieval, and integrates well with AWS services like Lambda.

#### Why create a DynamoDB table?

- **Post data storage**: Create a `BlogPosts` table to store information such as ID, title, content, and creation date of a post.

- **Serverless support**: DynamoDB does not require server management, suitable for Lambda and API Gateway architectures.

- **High performance**: Ensures fast queries and automatically scales according to traffic.

#### Steps:
1. Go to **AWS Management Console**.
2. Select the **Amazon DynamoDB** service.
3. In the navigation menu, select [Tables](https://ap-southeast-1.console.aws.amazon.com/dynamodbv2/home?region=ap-southeast-1#tables/)

![dynamodb-interface.png](/images/3-Create-DynamoDB-Table/3.1-create-dynamodb-table/3.1.png)

4. Select **Create table**.
5. In the **Create table** interface:

- **Table name**: Enter `BlogPosts`.

- **Partition key**: Enter `postId` (type **String**) as the primary key to uniquely identify each post.
- **Settings**: Select **Default settings** (**On-demand** mode, suitable for Free Tier).

- Click **Create table**.

![create-table.png](/images/3-Create-DynamoDB-Table/3.1-create-dynamodb-table/3.0.png)
6. Check the table:
- After creation, the `BlogPosts` table will appear in the **Tables** list with the status **Active**.

![table-created.png](/images/3-Create-DynamoDB-Table/3.1-create-dynamodb-table/3.2.png)

{{% notice note %}}
- Make sure the table name is `BlogPosts`, because Lambda functions (`getPosts`, `createPost`) will use this name in the environment variable `TABLE_NAME`.
- Create a table in the same region as the Lambda functions (e.g. `us-east-1`).
- The Lambda IAM Role needs the `dynamodb:Scan` and `dynamodb:PutItem` permissions (configured in the previous step).
{{% /notice %}}

#### Done
- You have created a `BlogPosts` table to store your post data.
