+++
title = "Configure and Test API"
date = 2025
weight = 2
chapter = false
pre = "<b>5.2 </b>"
+++

#### Configure API Gateway
In this section, we will:

- Add GET and POST methods to the `/posts` resource to retrieve and add posts.

- Integrate with Lambda functions (`getPosts`, `createPost`).

- Enable CORS and deploy the API for use in the frontend.

- Test the API to ensure proper operation.

#### Reasons for configuring and testing:

- **HTTP request handling**: GET/POST methods allow the frontend to interact with the backend.

- **Lambda integration**: Connect the API to Lambda to handle business logic.

- **Performance testing**: Ensure the API returns correct results before integrating with the frontend.

#### Configure and Test API

1. Go to **AWS Management Console** and select **API Gateway**.

2. Open the `BlogAPI` API created in the previous step.

3. Create a GET method for `/posts`:

- In **Resources**, select the `/posts` resource.

- Select **Actions** → **Create Method**.

- Select **GET** from the dropdown.

- **Integration type**: Select **Lambda Function**.

- **Lambda Function**: Select `getPosts`.

- Click **Save** and confirm IAM permissions if prompted.

![configure-get.png](/images/5-Configure-API-Gateway/5.2-API-Test-Configuration/5.5.png)

4. Create a POST method for `/posts`:

- Repeat step 3, but select **POST** and integrate with the `createPost` function.

![configure-post.png](/images/5-Configure-API-Gateway/5.2-API-Test-Configuration/5.6.png)

5. Configure CORS:

- In **Resources**, select **Actions** → **Enable CORS**.

- Accept the default values ​​and click **Enable CORS and replace existing CORS headers**.

![enable-cors.png](/images/5-Configure-API-Gateway/5.2-API-Test-Configuration/5.7.png)

6. Deploy API:

- Select **Actions** → **Deploy API**.

- **Deployment stage**: Select **[New Stage]** and name it (e.g. `prod`).

- Click **Deploy**.

- Save **Invoke URL** (e.g. `https://gcnpmry3bd.execute-api.ap-southeast-1.amazonaws.com/prod`).

![deploy-api.png](/images/5-Configure-API-Gateway/5.2-API-Test-Configuration/5.8.png)

{{% notice warning %}}
Make sure CORS is enabled so that the React/Vite frontend can make requests to the API.
Save the **Invoke URL** for use in the frontend (step 6).
Check the IAM permissions of Lambda (`lambda-blog-role`) to ensure that API Gateway can call functions.
{{% /notice %}}

7. Test the API:

- Use Postman or a browser to test:

- **GET** `<Invoke URL>/posts`: Returns a list of posts from DynamoDB.
- **POST** `<Invoke URL>/posts` with body:
```json
{
"postId": "2",
"title": "Test Post",
"content": "This is a test post."
}
```
- Check the result in DynamoDB (**Explore table items** tab).

![test-api.png](/images/5-Configure-API-Gateway/5.2-API-Test-Configuration/5.9.png)

8. Confirm and continue:

- The API is up and running and ready to connect to the frontend.

- Record the **Invoke URL** for use in the frontend configuration step.

![success.png](/images/5-Configure-API-Gateway/5.2-API-Test-Configuration/5.10.png)

#### Done
- You have configured and tested the API to handle GET/POST requests.
