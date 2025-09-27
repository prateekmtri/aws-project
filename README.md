AWS Cloud Project: Serverless File Metadata Logger
🚀 This project is also documented in a post on my LinkedIn profile. You can view it and join the discussion here:

View My LinkedIn Post

## 📖 Project Summary
This repository contains a hands-on project that demonstrates the creation of a fully serverless, event-driven data processing pipeline on AWS. The architecture is designed to automatically extract and log metadata from files uploaded to an S3 bucket in real-time. This project serves as a practical showcase of fundamental serverless concepts, infrastructure automation, and best practices for building scalable, cost-effective cloud solutions.

## 🏛️ Professional Architecture Diagram
The architecture is built on a serverless, event-driven model. An S3 bucket is configured to trigger a Lambda function whenever a new object is created. The function processes the event, extracts key metadata, and stores it in a DynamoDB table for persistent, queryable storage.

+--------------------------------------------------------------------------------------------------+
|                                      AWS Cloud (Your Region)                                     |
|                                                                                                  |
|   +------------------------------------------------------------------------------------------+   |
|   |                                 Event-Driven Workflow                                    |   |
|   |                                                                                          |   |
|   |   +----------------------+      +-------------------------+      +-------------------+   |   |
|   |   |                      |      |  s3:ObjectCreated:* |      |                   |   |   |
|   |   |   User uploads a     |----->|       Event Trigger     |----->|   AWS Lambda      |   |   |
|   |   |   file to S3 Bucket  |      |                         |      |   (Python Code)   |   |   |
|   |   |                      |      +-------------------------+      |                   |   |   |
|   |   +----------------------+                                       +--------+----------+   |   |
|   |                                                                           |              |   |
|   |                                                                           | .put_item()  |   |
|   |                                                                           v              |   |
|   |                                                                    +------+-----------+   |   |
|   |                                                                    |                  |   |   |
|   |                                                                    |  Amazon DynamoDB |   |   |
|   |                                                                    |      Table       |   |   |
|   |                                                                    | (Stores Metadata)|   |   |
|   |                                                                    +------------------+   |   |
|   |                                                                                          |   |
|   +------------------------------------------------------------------------------------------+   |
|                                                                                                  |
+--------------------------------------------------------------------------------------------------+
## ✨ Key Cloud Concepts Implemented
Serverless Computing: Utilized AWS Lambda to run code without provisioning or managing servers, paying only for the compute time consumed.

Event-Driven Architecture: The entire pipeline is asynchronous and initiated by an event (ObjectCreated) from Amazon S3, creating a loosely coupled and highly scalable system.

Managed Services: Leveraged fully managed AWS services (S3, Lambda, DynamoDB) to reduce operational overhead and increase focus on application logic.

Infrastructure as Code (IaC) Principles: While manually configured, this project lays the foundation for automation with tools like the AWS CDK or Terraform.

Fine-Grained Security: Implemented an IAM Role with precisely scoped permissions (Principle of Least Privilege) to ensure the Lambda function could only access the specific resources required to perform its task.

## 🚀 Step-by-Step Deployment Walkthrough
Database Foundation: Created a DynamoDB table (file-metadata-log) with a defined primary key to serve as the structured data store for file metadata.

Storage Layer: Deployed an S3 bucket to act as the entry point for the data pipeline, where users can upload files.

Secure Permissions: Configured an IAM Role with a custom policy granting the Lambda function specific permissions to read from the S3 bucket and write to the DynamoDB table.

Compute Logic: Developed and deployed an AWS Lambda function using Python and the Boto3 library. The code is designed to parse the S3 event, extract relevant metadata, and write it to DynamoDB.

Connecting the Services: Configured an S3 Event Notification to act as the trigger, invoking the Lambda function automatically every time a new object is created in the bucket.

## ✅ Final Result & Verification
The success of the architecture was verified by uploading a file to the S3 bucket. Within seconds, a new item appeared in the DynamoDB table, containing the correct file_name, bucket, size_in_bytes, and upload_timestamp for the uploaded file. This confirmed that the event-driven pipeline was functioning perfectly from end to end.

## 🧹 Project Cleanup
To adhere to best practices and avoid incurring costs outside the AWS Free Tier, all resources created for this project (the S3 bucket, Lambda function, DynamoDB table, and IAM role/policy) were decommissioned and deleted after successful verification.

## 🔮 Potential Future Enhancements
Advanced Data Processing: Enhance the Lambda function to generate image thumbnails or parse data from uploaded CSV/JSON files.

API Exposure: Add Amazon API Gateway to create a REST API that could query the metadata stored in the DynamoDB table.

Notification System: Integrate Amazon SNS (Simple Notification Service) to send an email or text message notification upon successful processing of a file.

Full Automation: Rebuild the entire infrastructure using an IaC tool like AWS CDK or Terraform for repeatable, automated deployments.
