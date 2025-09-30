# Deploying a Serverless Blog Website on AWS

This is a Serverless Blog Website built using Node.js, React.js, and Express.js, deployed on AWS using **Lambda**, leveraging **DynamoDB** as a NoSQL database, and optimizing performance with **CloudFront** and **S3**.

## Features
**Client**
  - [x] User Account Creation (Optional)
  - [x] Post Categories
  - [x] Pagination with Page Numbers
  - [x] Commenting on Posts (Only Available to Signed-in Users)
  - [x] Dark and Light Theme Settings

**Admin**
  - [x] Account Creation
  - [x] Email Verification with OTP
  - [x] Create Posts, Delete Posts, Enable or Disable Posts
  - [x] Dashboard Analytics
  - [x] Content and Views Analytics (7 days, 28 days, 90 days & 365 days)
  - [x] Ability to delete user comments on a blog post
  - [x] Dark and Light Theme Settings

## Tech Stack
- Node.js, React.js, Express.js
- AWS Lambda
- AWS DynamoDB
- AWS S3
- AWS CloudFront
- AWS API Gateway

## Setup

1. Clone the repository:
   ```bash
   git clone https://github.com/<username>/aws-serverless-blog-workshop.git
   cd aws-serverless-blog-workshop

