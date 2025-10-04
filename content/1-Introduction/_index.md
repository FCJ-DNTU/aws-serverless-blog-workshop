+++
title = "Introduction"
date = 2020-05-14T00:38:32+07:00
weight = 1
chapter = false
pre="<b>1. </b>"
+++

# Workshop: Deploying a Serverless Blog Website on AWS With Lambda, API Gateway, DynamoDB, S3 & CloudFront

In this workshop, you will learn how to develop a **Serverless Blog Website** on AWS, using **AWS Lambda** to handle backend logic, **API Gateway** to create RESTful APIs, **DynamoDB** as a NoSQL database, **S3** to host static UI (React/Vite), and **CloudFront** to optimize content delivery performance.

![Workshop-Architecture](/images/workshop_architecture.png)

### Objectives:

- Learn how to design and develop serverless applications on AWS.
- Create and configure **Lambda Functions**, **API Gateway**, **DynamoDB**, **S3**, and **CloudFront**.
- Deploy a React/Vite frontend application on S3.
- Connect the frontend to the backend via API Gateway.
- Manage access between services using **AWS IAM**.
- Optimize the free tier and clean up resources.

### Requirements:

- AWS account with IAM access (Free tier: https://aws.amazon.com/free).
- Basic JavaScript skills (Node.js).
- Tools: Node.js, npm, AWS CLI, Git.
- (Optional) Postman for API testing.

### Architecture provided in the Workshop

**AWS services and costs:**

**1. AWS Lambda**
**Uses:**
- AWS Lambda is used to run auxiliary functions (`getPosts`, `createPost`) to process logic and create posts.
- Lambda operates on a serverless model, automatically scales and charges on demand.

**Cost:**
| Status | Type | Cost/request | Cost/month |
|----------|-------|-------|--------------|
| **Free Tier** | 1M free requests, 400,000 GB-sec | **$0.00** | **$0.00** |
| **Free Tier Ended** | Requests $0.20/1M, $0.0000167/GB-sec | ~$0.01 (100K requests) | ~$0.30 (100K requests) |

---

**2. Amazon API Gateway**
**What it does:**
- API Gateway creates HTTP endpoints (GET `/posts`, POST `/posts`) to connect the UI to Lambda.
- Supports RESTful API, request management, and integration with Lambda.

**Cost:**
| Status | Type | Cost/request | Cost/month |
|----------|-------|-------|--------------|
| **Free Tier** | 1M free requests | **$0.00** | **$0.00** |
| **Free Tier Ended** | $3.50/1M requests | ~$0.35 (100K requests) | ~$10.50 (3M requests) |

---

**3. Amazon DynamoDB**
**What it does:**
- DynamoDB is a NoSQL database for storing and managing post data (`BlogPosts` table).

- Supports fast reads/writes and auto-scaling.

**Cost:**
| Status | Type | Cost/day | Cost/month |
|----------|-------|--------------|--------------|
| **Free Tier** | 25 read/writes, 25GB storage | **$0.00** | **$0.00** |
| **Free Tier Ended** | $1.25/1M read, $0.25/1M write, $0.09/GB | ~$0.02 (10K read/write, 5GB) | ~$0.60 (10K read/write, 5GB) |

---

**4. Amazon S3**
**What it does:**
- S3 stores static files of React/Vite UI (HTML, CSS, JS, images).
- Supports static website hosting.

**Cost:**
| Status | Capacity | Cost/day | Cost/month |
|----------|----------|--------------|--------------|
| **Free Tier** | 5GB Storage, 2,000 GETs, 20,000 PUTs | **$0.00** | **$0.00** |
| **Free Tier Ended** | $0.023/GB, $0.005/1,000 GETs | ~$0.03 (5GB, GET 10K) | ~$1.00 (5GB, GET 10K) |

---

**5. Amazon CloudFront**
**What it does:**
- CloudFront is a CDN that delivers content from S3 at high speed and HTTPS.

- Offload directly from S3 and improve user experience.

**Cost:**
| Status | Capacity | Cost/day | Cost/month |
|----------|----------|--------------|--------------|
| **Free Tier** | 1TB bandwidth, 10M requests | **$0.00** | **$0.00** |
| **Free Tier Ended** | $0.085/GB, $0.01/10,000 requests | ~$0.10 (50GB, 100K requests) | ~$3.00 (50GB, 100K requests) |

---

**6. I AM**
**What it does:**
- IAM manages access to Lambda, API Gateway, and DynamoDB.

- Ensures security by granting specific permissions (e.g. `dynamodb:Scan`, `dynamodb:PutItem`).

**Cost:**
- **Free** under AWS.
---

**7. Cost Summary**

**7.1. Costs while still in Free Tier**
| Service | Type | Cost/day | Cost/month |
|---------|------|--------------|--------------|
| **AWS Lambda** | 1M requests | **$0.00** | **$0.00** |
| **Amazon API Gateway** | 1M requests | **$0.00** | **$0.00** |
| **Amazon DynamoDB** | 25 read/write units | **$0.00** | **$0.00** |
| **Amazon S3** | 5GB storage | **$0.00** | **$0.00** |
| **Amazon CloudFront** | 1TB bandwidth | **$0.00** | **$0.00** |
| **IAM** | Free | **$0.00** | **$0.00** |
| **Total** | | **$0.00** | **$0.00** |

---

**7.2. Cost after Free Tier expires**
| Service | Type | Cost/day | Cost/month |
|---------|-------|--------------|--------------|
| **AWS Lambda** | 100K requests | ~$0.01 | ~$0.30 |
| **Amazon API Gateway** | 3M requests | ~$0.35 | ~$10.50 |
| **Amazon DynamoDB** | 10K reads/writes, 5GB | ~$0.02 | ~$0.60 |
| **Amazon S3** | 5GB, 10K GETs | ~$0.03 | ~$1.00 |
| **Amazon CloudFront** | 50GB, 100K requests | ~$0.10 | ~$3.00 |
| **IAM** | Free | **$0.00** | **$0.00** |
| **Total** | | ~$0.51 | ~$15.40 |

---

**8. Conclusion**
- While in the Free Tier, the system operates at **$0** cost.

- When the Free Tier expires, the system maintenance cost will **fluctuate around $15 - $16/month** (assuming 100K Lambda requests, 3M API Gateway requests, 10K DynamoDB reads/writes, 5GB S3, 50GB CloudFront).

- If the system scales, the cost may increase depending on traffic and data.

### Activity flow:

**1. Client accesses website**
- User accesses blog via browser.
- Static content (React/Vite) is stored on S3 and delivered via CloudFront.

**2. CloudFront receives requests**
- CloudFront serves static content from S3 (HTML, CSS, JS).
- Forwards API requests (GET `/posts`, POST `/posts`) to API Gateway.

**3. API Gateway processes requests**
- API Gateway receives requests from CloudFront and passes them to Lambda functions (`getPosts` or `createPost`).
- Lambda handles backend logic.

**4. Lambda queries DynamoDB**
- Lambda function `getPosts` calls DynamoDB to get a list of posts.
- Lambda function `createPost` saves new posts to DynamoDB.

**5. Return results to client**
- Lambda returns results via API Gateway.
- API Gateway sends responses to CloudFront.
- CloudFront can cache responses to optimize speed.
- Responses are sent to the client via the browser.

**6. Manage access rights with IAM**
- IAM controls access rights for Lambda (e.g. `dynamodb:Scan`, `dynamodb:PutItem`).
- Ensure security between AWS services.
