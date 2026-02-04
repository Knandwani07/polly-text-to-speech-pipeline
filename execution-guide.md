# 🛠️ Execution Guide: AWS Serverless Text-to-Speech Conversion System

This section provides a step-by-step guide to deploy and test the AWS Serverless Text-to-Speech Conversion System.

---

### I. AWS Account Setup & S3 Bucket Creation

1. Log in to the AWS Management Console using an account with administrative access.
2. Navigate to **Amazon S3** from the AWS services search bar.
3. Click **Create bucket**.

#### Source Bucket
• Bucket name: amc-polly-source-bucket-0302  
• Region: Default  
• All other settings: Default  
• Click **Create bucket**

#### Destination Bucket
• Click **Create bucket** again  
• Bucket name: amc-polly-destination-bucket-0302  
• Region: Same as source bucket  
• All other settings: Default  
• Click **Create bucket**

#### Verification
Ensure both buckets are visible in the S3 console:<br>
• amc-polly-source-bucket-0302  <br>
• amc-polly-destination-bucket-0302  

---

### II. IAM Policy Configuration

1. Navigate to **IAM** from the AWS services menu.
2. Select **Policies** from the left panel.
3. Click **Create policy**.
4. Switch to the **JSON** tab.
5. Paste the custom IAM policy JSON (located in `/iam/` directory).
6. Click **Next**.
7. Policy name: amc-polly-lambda-policy  
8. Add a short description explaining the policy purpose.
9. Review and click **Create policy**.

---

### III. IAM Role Configuration

1. In IAM, select **Roles** → **Create role**.
2. Trusted entity type: **AWS service**
3. Use case: **Lambda**
4. Click **Next**.

#### Attach Permissions
Attach the following policies: <br>
• amc-polly-lambda-policy  <br>
• AWSLambdaBasicExecutionRole  

5. Role name: amc-polly-lambda-role  
6. Review the configuration.
7. Click **Create role**.

---

### IV. AWS Lambda Function Setup

1. Navigate to **AWS Lambda**.
2. Click **Create function**.

#### Function Configuration
• Author from scratch  
• Function name: TextToSpeechProcessor  
• Runtime: Python 3.12  
• Execution role: Use an existing role  
• Role name: amc-polly-lambda-role  

3. Click **Create function**.

---

### V. Environment Variables Configuration

1. Open the Lambda function.
2. Navigate to **Configuration** → **Environment variables**.
3. Click **Edit** → **Add environment variable**.

#### Environment Variables
• SOURCE_BUCKET = amc-polly-source-bucket-0302  
• DESTINATION_BUCKET = amc-polly-destination-bucket-0302  

4. Click **Save**.

---

### VI. S3 Trigger Configuration

1. Open the Lambda function.
2. Click **Add trigger**.
3. Select **S3** as the trigger source.

#### Trigger Settings
• Bucket: amc-polly-source-bucket-0302  
• Event type: All object create events  
• Suffix filter: .txt  

4. Acknowledge the permission warning.
5. Click **Add**.

---

### VII. Lambda Function Code Deployment

1. Open the **Code** tab in the Lambda function.
2. Replace the default code with the provided Python implementation located in `/lambda/`.
3. Click **Deploy** to save changes.

---

### VIII. Testing the Solution

1. Navigate to **Amazon S3**.
2. Open the source bucket: amc-polly-source-bucket-0302.
3. Click **Upload** → **Add files**.
4. Upload a `.txt` file containing sample text.

#### Verification
• Lambda function triggers automatically  
• Navigate to amc-polly-destination-bucket-0302  
• Confirm an MP3 file is generated  

5. Download and play the MP3 file to verify successful text-to-speech conversion.

---

### IX. Resource Cleanup

To avoid ongoing AWS charges, delete the following resources after testing:

#### S3 Buckets
• amc-polly-source-bucket-0302  
• amc-polly-destination-bucket-0302  

#### Lambda Resources
• TextToSpeechProcessor function  

#### IAM Resources
• amc-polly-lambda-role  
• amc-polly-lambda-policy  

#### Monitoring
• CloudWatch log groups associated with the Lambda function  

---

### ✅ Conclusion

This execution guide demonstrates how to deploy a fully automated, serverless, event-driven text-to-speech pipeline on AWS. By integrating Amazon S3, AWS Lambda, Amazon Polly, and IAM, the system delivers scalable, cost-effective, and maintenance-free audio generation.
