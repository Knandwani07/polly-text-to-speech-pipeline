# 🏗️ Architecture Overview

This project follows a fully serverless, event-driven architecture using AWS managed services. The design ensures scalability, cost efficiency, high availability, and zero server management overhead.

The system automatically converts uploaded text files into MP3 audio files using Amazon Polly, triggered entirely through AWS events.

---

## 📐 Architecture Diagram
<img width="1376" height="644" alt="AWS Serverless Text-to-Speech Architecture-copy" src="https://github.com/user-attachments/assets/40606c90-2d0c-45b8-94df-346314130e08" />

> The diagram illustrates the complete data flow, event triggers, permission boundaries, and service interactions across the system.

---

## 🧩 Core Architecture Components

### 1. Amazon S3 (Source Bucket)
• Stores input `.txt` files uploaded by users  
• Acts as the event source for triggering the workflow  
• Configured with S3 event notifications  

Bucket Name:<br>
• amc-polly-source-bucket-0302  

---

### 2. Amazon S3 Event Notifications
• Automatically detects object creation events  
• Filters events based on `.txt` file suffix  
• Triggers the AWS Lambda function without polling  

This enables real-time, event-driven processing.

---

### 3. AWS Lambda (Text-to-Speech Processor)
• Acts as the core processing unit  
• Reads text content from the source bucket  
• Invokes Amazon Polly for speech synthesis  
• Stores generated MP3 output in the destination bucket  

Function Name:<br>
• TextToSpeechProcessor  

Runtime:<br>
• Python 3.12  

---

### 4. Amazon Polly (Text-to-Speech Service)
• Converts text input into natural-sounding speech  
• Uses managed neural text-to-speech models  
• Returns audio output in MP3 format  

This service eliminates the need for any custom audio processing logic.

---

### 5. Amazon S3 (Destination Bucket)
• Stores generated MP3 audio files  
• Provides durable, scalable storage  
• Allows users to download audio output  

Bucket Name:<br>
• amc-polly-destination-bucket-0302  

---

### 6. AWS IAM (Security & Access Control)
• Manages permissions using least-privilege access  
• IAM Role grants Lambda permission to:
  - Read objects from the source S3 bucket  
  - Write objects to the destination S3 bucket  
  - Invoke Amazon Polly  
  - Write logs to CloudWatch  

IAM Resources: <br>
• amc-polly-lambda-role  <br>
• amc-polly-lambda-policy  

---

### 7. Amazon CloudWatch (Logs & Monitoring)
• Captures Lambda execution logs  
• Aids in debugging and observability  
• Provides execution insights and error tracking  

---

## 🔄 End-to-End Workflow

1. User uploads a `.txt` file to the source S3 bucket  
2. S3 generates an object creation event  
3. Event triggers the AWS Lambda function  
4. Lambda reads the text file from S3  
5. Lambda sends the text to Amazon Polly  
6. Polly returns synthesized MP3 audio  
7. Lambda stores the MP3 file in the destination S3 bucket  
8. Logs are recorded in Amazon CloudWatch  
9. User downloads the generated audio file  

---

## ⚙️ Architecture Characteristics

• Fully serverless and event-driven  
• No infrastructure provisioning or management  
• Automatically scales based on workload  
• Secure by design using IAM least privilege  
• Highly available and fault-tolerant  
• Cost-efficient with pay-per-use pricing  

---

## ✅ Summary

This architecture demonstrates a clean, production-ready serverless design using AWS core services. By combining Amazon S3, AWS Lambda, Amazon Polly, and IAM, the system delivers an automated, scalable, and maintainable text-to-speech pipeline suitable for real-world applications.
