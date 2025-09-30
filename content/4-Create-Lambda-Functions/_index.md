+++
title = "Create Lambda Functions"
date = 2025
weight = 4
chapter = false
pre = "<b>4. </b>"
+++

#### AWS Lambda

**AWS Lambda** is a serverless service that allows you to run code without managing servers. It automatically scales with traffic and only charges when your code is executed, making it ideal for applications like backend APIs.

#### What is a Lambda Function?
**Lambda Function** is a piece of code (written in Node.js, Python, etc.) deployed on AWS Lambda to handle specific tasks, like retrieving data from DynamoDB or handling API requests.

#### Lambda Deployment Model in Workshop
In this workshop, we will create two Lambda functions:
- **`getPosts`**: Retrieves a list of posts from the `BlogPosts` table in DynamoDB.
- **`createPost`**: Add new posts to the `BlogPosts` table.
- These functions will:
- Integrate with API Gateway to handle HTTP requests.
- Use IAM Role (created in the previous step) to access DynamoDB.
- Written in Node.js with AWS SDK v3.

#### Reasons to choose Lambda:
- **Serverless**: No need to manage servers, reduce operating costs.
- **Low cost**: Free Tier provides 1 million free requests per month, suitable for testing.
- **Easy integration**: Works well with API Gateway and DynamoDB.

#### Reasons to choose Node.js:
- **Popular**: Good support for web and serverless applications.
- **AWS SDK v3**: Provides lightweight modules (like `@aws-sdk/client-dynamodb`) to optimize performance.
- **Large Community**: Easy to find documentation and support libraries.

#### Create Lambda Functions

#### 1. Access AWS Console
- Open a browser and access [Lambda Console](https://ap-southeast-1.console.aws.amazon.com/lambda/home?region=ap-southeast-1#/begin/)

#### 2. Create the function `getPosts`
- In **Lambda Dashboard**, select **Create function**.

![create-function.png](/images/4-Create-Lambda-Functions/4.1.png)

#### 3. Configure the function
- In the **Create function** section:

- **Function name**: Enter `getPosts`.

- **Runtime**: Select **Node.js 20.x**.

- **Role**: Select **Use an existing role** and select `lambda-blog-role` (created in the IAM step).

- Click **Create function**.

![configure-getposts.png](/images/4-Create-Lambda-Functions/4.2.png)

#### 4. Add code for `getPosts`
- In the function interface, select the **Code** tab.
- Replace the default code in `index.mjs` with the following code:

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
Add environment variables:
In the Configuration → Environment variables tab, select Edit.
Add: TABLE_NAME = BlogPosts.
Click Save.
{{% /notice %}}

#### 5. Create function createPost

{{% notice note %}}
Repeat steps 2-3 to create a new function with:
Function name: createPost.
Runtime: Node.js 20.x.
Role: lambda-blog-role.
{{% /notice %}}

Replace the code in index.mjs with:

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
createAt: new Date().toISOString() 
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
Add environment variable TABLE_NAME = BlogPosts as in step 4.
{{% /notice %}}

#### 6. Configure package.json
To add dependencies @aws-sdk/client-dynamodb and @aws-sdk/util-dynamodb, create a package.json file in the source code directory:

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

## Package the code:
- Create a local directory for each function (e.g. getPosts/, createPost/).
- Run npm install in each directory.
- Compress the directory into a .zip file (including node_modules, index.mjs, package.json).
- Upload the .zip file to Lambda in the Code → Upload from → .zip file tab.

{{% notice warning %}}
Make sure to use ES Module (index.mjs) and handler is index.handler.
The .zip file must include node_modules to avoid missing dependency errors.
The environment variable TABLE_NAME must match the DynamoDB table name (BlogPosts).
{{% /notice %}}

#### 7. Test Lambda Functions

In the interface of each function, select the Test tab:
```
For getPosts, create an event with empty JSON {}.
For createPost, create a sample event:{
"body": "{\"postId\": \"1\", \"title\": \"Test Post\", \"content\": \"This is a test post.\"}"
}
```

## Click Test and check the result:
- getPosts: Returns a list of posts (or an empty array if there is no data yet).
- createPost: Returns the message { "message": "Post created successfully" }.
