+++
title = "Update and Test CloudFront"
date = 2025
weight = 9
chapter = false
pre = "<b>8.2 </b>"
+++

![cloudfront-test.png](/images/8-Configure-CloudFront/8.2-cloudfront-origin-website/8.4.png)

#### Update and Test CloudFront
In this section, we will:

- Update the bucket policy so that CloudFront can access S3 via Origin Access Control (OAC).

- Test the frontend application via CloudFront domain.

- Confirm that the website works with HTTPS.

#### Reasons to update and test:

- **Security**: OAC limits S3 access to CloudFront only, increasing security.
- **Verify Activity**: Make sure the website displays correctly and the API works over HTTPS.

- **Preparing to Finish**: Connect the entire system (frontend, backend, CDN).

#### Update and Test CloudFront

1. **Update Bucket Policy**
- Access the S3 bucket (`blog-workshop-<your-account-id>`), select the **Permissions** tab.

- In **Bucket policy**, select **Edit** and replace the current policy with the following policy (replace `<your-bucket-name>` and `<oac-id>` with the bucket name and OAC ID from step 8.1):
```json
{
"Version": "2008-10-17",
"Id": "PolicyForCloudFrontPrivateContent",
"Statement": [
{
"Effect": "Allow",
"Principal": {
"AWS": "arn:aws:iam::cloudfront:user/CloudFront Origin Access Identity <oac-id>"
},
"Action": "s3:GetObject",
"Resource": "arn:aws:s3:::<your-bucket-name>/*"
}
]
}
```
- Click **Save changes**.

![update-bucket-policy.png](/images/8-Configure-CloudFront/8.2-cloudfront-origin-website/8.5.png)

2. **Disable Public Access**

- In **Permissions**, select **Edit** under **Block public access**.

- Enable **Block all public access** and all sub-options, click **Save changes**.

- This ensures that only CloudFront can access the bucket.

![block-public-access.png](/images/8-Configure-CloudFront/8.2-cloudfront-origin-website/8.6.png)

3. **Test Website via CloudFront**

- Access the **Distribution domain name** from step 8.1 (e.g. `https://d1234567890abcdef.cloudfront.net`) in your browser.

- Check the post list and post creation form.

- Try creating a new post to confirm API Gateway works over HTTPS.

4. **Debug errors (if any)**

- If the website does not display:

- Check the bucket policy for the correct OAC ID and bucket name.

- Verify that the `index.html` file exists in the bucket.

- If the API does not work:

- Check the **Invoke URL** in `src/App.jsx` (step 6).

- Verify CORS in API Gateway (step 5.2).

- Check the browser console log or Lambda's CloudWatch Logs.

{{% notice warning %}}
Make sure the bucket policy uses the correct OAC ID and bucket name.
Only access the website via **Distribution domain name** (HTTPS), not using **Bucket website endpoint** (HTTP).
If you get a 403 error, check the OAC and bucket policy.
{{% /notice %}}

#### Done
- You have updated your bucket policy and tested your application over CloudFront with HTTPS.
