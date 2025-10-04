+++
title = "Workshop: Building an End-to-End Machine Learning Pipeline on AWS with Lambda, API Gateway, S3, SageMaker & DynamoDB"
date = 2025
weight = 1
chapter = false
+++

# Workshop: Building an End-to-End Machine Learning Pipeline on AWS with Lambda, API Gateway, S3, SageMaker & DynamoDB

### Overview

In this workshop, you will learn how to build and deploy an **end-to-end machine learning pipeline** on AWS.  
We will use **AWS Lambda** for preprocessing and inference, **API Gateway** to expose RESTful endpoints, **S3** for data storage, **Amazon SageMaker** for model training and hosting, **DynamoDB** for metadata storage, and **CloudWatch** for monitoring and logging.

![Workshop Architecture](/images/ml_pipeline_architecture.png)

### Objectives:

- Understand how to design and deploy a complete ML pipeline on AWS.  
- Build and configure data ingestion, preprocessing, and model training workflows.  
- Deploy and manage machine learning models with Amazon SageMaker.  
- Expose inference endpoints through API Gateway and Lambda.  
- Integrate DynamoDB for model metadata and monitoring with CloudWatch.  
- Learn best practices for IAM permissions and cost optimization.

### Requirements:

- AWS account with IAM access (Free Tier: https://aws.amazon.com/free)  
- Basic knowledge of Python or Go (for Lambda functions and ML scripts)  
- Familiarity with REST APIs and JSON  
- Tools: AWS CLI, Git, Docker (optional), and a web browser  
- (Optional) Postman for testing inference endpoints

### Contents

1. [Introduction](1-Introduction/)  
2. [Set Up AWS Account and IAM Permissions](2-Set-Up-AWS-Account-and-IAM-Permissions/)  
3. [Create S3 Bucket for Data Storage](3-Create-S3-Bucket/)  
4. [Implement Lambda Preprocessing Function](4-Implement-Lambda-Preprocessing/)  
5. [Train and Register Model with Amazon SageMaker](5-Train-Model-with-SageMaker/)  
6. [Deploy SageMaker Endpoint for Inference](6-Deploy-SageMaker-Endpoint/)  
7. [Build Lambda Inference Function and API Gateway](7-Build-Lambda-Inference-and-API/)  
8. [Integrate DynamoDB and CloudWatch](8-Integrate-DynamoDB-and-CloudWatch/)  
9. [Clean Up Resources](9-Clean-Up-Resources/)  
