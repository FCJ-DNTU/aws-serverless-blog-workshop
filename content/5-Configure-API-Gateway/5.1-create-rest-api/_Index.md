+++
title = "Create REST API"
date = 2025
weight = 1
chapter = false
pre = "<b>5.1 </b>"
+++

#### Amazon API Gateway

**Amazon API Gateway** is a serverless service that allows you to create and manage RESTful or WebSocket APIs. It acts as a gateway between the frontend (React/Vite) and backend (Lambda functions), handling HTTP requests and forwarding them to AWS services.

#### API Gateway in the Workshop
In this section, we will:

- Create a REST API to handle HTTP requests.

- Create a `/posts` resource to support endpoints for posts.

- Ensure the API integrates with the Lambda functions (`getPosts`, `createPost`) in the next step.

#### Why choose API Gateway:

- **Serverless**: No need to manage servers, automatically scales with traffic.

- **Easy integration**: Connects directly to Lambda and supports HTTP requests.

- **Low cost**: Free Tier offers 1 million free requests per month.

#### Create REST API

1. Go to **AWS Management Console**.

- Find and select **API Gateway**.

- Select **APIs** in the navigation menu.

![api-gateway-interface.png](/images/5-Configure-API-Gateway/5.1-create-rest-api/5.1.png)

2. In the **APIs** interface:

- Select **Create API**.

- Select **REST API** and click **Build**.

![create-api.png](/images/5-Configure-API-Gateway/5.1-create-rest-api/5.2.png)

3. In the **Create API** interface: 
- **API name**: Enter `BlogAPI`. 
- **Endpoint Type**: Select **Regional**. 
- Click **Create API**. 

![configure-api.png](/images/5-Configure-API-Gateway/5.1-create-rest-api/5.3.png)

4. Create resource `/posts`: 
- In **Resources**, select **Actions** → **Create Resource**. 
- **Resource Name**: Enter `posts`. 
- **Resource Path**: `/posts`. 
- Turn on **Enable API Gateway CORS** (so the frontend can access it). 
- Click **Create Resource**.

![create-resource.png](/images/5-Configure-API-Gateway/5.1-create-rest-api/5.4.png)

{{% notice warning %}}
Make sure CORS is enabled so that the React/Vite frontend can make requests to the API.
Save the API name (`BlogAPI`) and resource (`/posts`) for use in the next step.
{{% /notice %}}

#### Done
- You have created a REST API and a `/posts` resource, ready to configure the GET/POST method.
