# AWS Serverless Project: Real-Time Data Processing Pipeline

![AWS](https://img.shields.io/badge/AWS-%23232F3E.svg?style=for-the-badge&logo=amazon-aws&logoColor=white) ![AWS Lambda](https://img.shields.io/badge/AWS%20Lambda-FF9900.svg?style=for-the-badge&logo=aws-lambda&logoColor=white) ![Amazon S3](https://img.shields.io/badge/Amazon%20S3-569A31?style=for-the-badge&logo=amazon-s3&logoColor=white) ![Amazon DynamoDB](https://img.shields.io/badge/Amazon%20DynamoDB-4053D6?style=for-the-badge&logo=amazon-dynamodb&logoColor=white) ![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)

---

> 🚀 **I've also documented this project on LinkedIn. Feel free to check out the post and connect with me!**
>
> **[View My LinkedIn Post](https://www.linkedin.com/posts/prateek-mani-tripathi-51935a259_aws-serverless-cloudcomputing-activity-7375049364181921792-s3JN?utm_source=share&utm_medium=member_desktop&rcm=ACoAAD-XD2UB3Q_7K3wzLRZFKaD5o7TxIPOLoF8)**

---

### ## 📖 Table of Contents
1.  [**Project Summary**](#-project-summary)
2.  [**Architecture Diagram**](#-architecture-diagram)
3.  [**Key Cloud Concepts Implemented**](#-key-cloud-concepts-implemented)
4.  [**Deployment Walkthrough**](#-deployment-walkthrough)
5.  [**Final Result & Verification**](#-final-result--verification)
6.  [**Project Cleanup**](#-project-cleanup)
7.  [**Potential Future Enhancements**](#-potential-future-enhancements)

---

### ## 📝 Project Summary

This repository contains a hands-on project that demonstrates the creation of a fully **serverless, event-driven data processing pipeline** on AWS. The architecture is designed to automatically extract and log metadata from files uploaded to an S3 bucket in real-time. This project serves as a practical showcase of fundamental serverless concepts, infrastructure automation, and best practices for building scalable, cost-effective cloud solutions.

---

### ## 🏛️ Architecture Diagram

The architecture is built on a serverless, event-driven model. An S3 bucket is configured to trigger a Lambda function whenever a new object is created. The function processes the event, extracts key metadata, and stores it in a DynamoDB table for persistent, queryable storage.

```
+--------------------------------------------------------------------------------------------------+
|                                        AWS Cloud Environment                                     |
|                                                                                                  |
|  +--------------------------------------------------------------------------------------------+  |
|  |                                  Event-Driven Serverless Pipeline                             |  |
|  |                                                                                            |  |
|  |    +--------------------------+        +---------------------------+                         |  |
|  |    |    ① User/Application    |        |    ② S3 Event Notification    |                         |  |
|  |    |     Uploads file (.jpg)  |------->|    (s3:ObjectCreated:*)   |                         |  |
|  |    +--------------------------+        +---------------------------+                         |  |
|  |                 |                                      |                                   |  |
|  |                 |                                      | Invokes                           |  |
|  |   +-------------v-------------+                        |                                   |  |
|  |   |    📦 Amazon S3 Bucket    |                        |                                   |  |
|  |   |    (Data Inlet/Storage)   |                        v                                   |  |
|  |   +---------------------------+        +------------------------------------------------+  |  |
|  |                                        |      ③ ⚡ AWS Lambda Function (Python)          |  |  |
|  |                                        |------------------------------------------------|  |  |
|  |                                        | 1. Receives event data (bucket, object key)    |  |  |
|  |                                        | 2. Fetches object metadata using S3 API        |  |  |
|  |                                        | 3. Formats data into a JSON object             |  |  |
|  |                                        | 4. Writes the object to DynamoDB               |  |  |
|  |                                        +-------------------------+------------------------+  |  |
|  |                                                                  |                           |  |
|  |                                                                  | .put_item()               |  |
|  |                                                                  v                           |  |
|  |                                        +-------------------------+------------------------+  |  |
|  |                                        |      ④ 🗂️ Amazon DynamoDB Table                 |  |  |
|  |                                        |        (Persistent Metadata Store)             |  |  |
|  |                                        +------------------------------------------------+  |  |
|  +--------------------------------------------------------------------------------------------+  |
|                                                                                                  |
+--------------------------------------------------------------------------------------------------+
```

---

### ## ✨ Key Cloud Concepts Implemented

* **Serverless Computing:** Utilized **AWS Lambda** to run code without provisioning or managing servers, paying only for the compute time consumed.
* **Event-Driven Architecture:** The entire pipeline is asynchronous and initiated by an event (`s3:ObjectCreated:*`) from **Amazon S3**, creating a loosely coupled and highly scalable system.
* **Managed Services:** Leveraged fully managed AWS services (**S3, Lambda, DynamoDB**) to reduce operational overhead and increase focus on application logic.
* **Infrastructure Security:** Implemented an **IAM Role** with a precisely scoped policy (Principle of Least Privilege) to ensure the Lambda function could only access the specific resources required for its task.

---

### ## 🚀 Deployment Walkthrough

1.  **Database Foundation:** Created a **DynamoDB** table (`file-metadata-log`) with a defined primary key to serve as the structured, NoSQL data store.
2.  **Storage Layer:** Deployed an **S3 bucket** to act as the entry point for the data pipeline where files are uploaded.
3.  **Secure Permissions:** Configured a custom **IAM Role and Policy** to grant the Lambda function the exact permissions needed: `s3:GetObject` and `dynamodb:PutItem`.
4.  **Compute Logic:** Developed and deployed an **AWS Lambda function** using Python and the Boto3 library. The code was designed to parse the S3 event, extract relevant metadata, and write it to DynamoDB.
5.  **Connecting the Services:** Configured an **S3 Event Notification** to act as the trigger, invoking the Lambda function automatically every time a new object is created in the bucket.

---

### ## ✅ Final Result & Verification

The success of the architecture was verified by uploading a file (e.g., `.jpg`, `.pdf`) to the configured S3 bucket. Within seconds, a new item automatically appeared in the DynamoDB table, containing the correct metadata for the uploaded file. This confirmed that the event-driven pipeline was functioning perfectly from end to end.

---

### ## 🧹 Project Cleanup

To adhere to best practices and avoid unnecessary costs, all resources were decommissioned and deleted in the correct dependency order: **S3 Bucket** (after emptying), **Lambda Function**, **DynamoDB Table**, and finally the **IAM Role & Policy**.

---

### ## 🔮 Potential Future Enhancements

* **Automation:** Rebuild the entire infrastructure using an **Infrastructure as Code (IaC)** tool like the AWS CDK or Terraform for repeatable, automated deployments.
* **Advanced Data Processing:** Enhance the Lambda function to generate image thumbnails or parse data from uploaded CSV/JSON files.
* **API Exposure:** Add **Amazon API Gateway** to create a REST API that could query the metadata stored in the DynamoDB table.
* **Notification System:** Integrate **Amazon SNS (Simple Notification Service)** to send an email or text message notification upon successful processing of a file.
