+++
title = "Create CloudFront Distribution"
date = 2025
weight = 8
chapter = false
pre = "<b>8.1 </b>"
+++

#### Amazon CloudFront

**Amazon CloudFront** is a CDN (Content Delivery Network) service from AWS, which helps deliver static (HTML, CSS, JavaScript) and dynamic (API) content at high speed and secure HTTPS. In this project, CloudFront is used to distribute frontend applications from S3, improve performance and add HTTPS.

#### CloudFront in the Workshop
In this section, we will:

- Create a CloudFront distribution to deliver content from an S3 bucket.

- Configure Origin Access Control (OAC) to secure access to the bucket.

- Set basic settings such as HTTPS and HTTP methods.

#### Benefits of using CloudFront:

- **High speed**: Distribute content across global edge locations.

- **Security**: HTTPS support, AWS WAF integration (optional).

- **Low cost**: Free Tier provides 1TB of data transfer per month.

- **Integrated with S3**: Easily connect to a bucket containing static files.

#### Create CloudFront Distribution

1. **Go to AWS Management Console**
- Find and select **CloudFront** in AWS Console.

![search-cloudfront.png](/images/8-Configure-CloudFront/8.1-create-cloudfront/8.1.png)

2. **Create Distribution**
- In the **CloudFront** interface, select **Distributions** → **Create distribution**.

![create-distribution.png](/images/8-Configure-CloudFront/8.1-create-cloudfront/8.2.png)

3. **Distribution Configuration**
- **Origin domain**: Select the S3 bucket you created (e.g. `blog-workshop-<your-account-id>`).

- **Origin access**: Select **Origin access control settings (recommended)**.

- **Origin access control**: Select **Create new OAC**.

- In **Create new OAC**, keep the default and click **Create**.

- Go back and select the OAC you just created.

- **Viewer protocol policy**: Select **Redirect HTTP to HTTPS**.

- **Allowed HTTP methods**: Select **GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE**.

- **Web Application Firewall (WAF)**: Select **Do not enable security protections**.

- Keep the rest as default, scroll down and click **Create distribution**.

4. **Wait for Distribution to initialize**

- The initialization process takes about 4-5 minutes.

- Once completed, note down the **Distribution domain name** (e.g. `d1234567890abcdef.cloudfront.net`).

![distribution-created.png](/images/8-Configure-CloudFront/8.1-create-cloudfront/8.3.png)

{{% notice warning %}}
Make sure to select the correct S3 bucket as origin.
Save the **Distribution domain name** for checking and use in the next step.
OAC requires updating the bucket policy (will be done in step 8.2).
{{% /notice %}}

#### Done
- You have created a CloudFront distribution to distribute your frontend application.
