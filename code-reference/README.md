## 📄 Code Reference – Lambda Function & IAM Policy

This section explains the purpose, logic, and security configuration of the
AWS Lambda function and the associated IAM policy used in the Serverless
Text-to-Speech Conversion System.

------------------------------------------------------------

## 🧠 AWS Lambda Function – TextToSpeechProcessor

### Purpose

The Lambda function acts as the core processing component of the system. It is
automatically triggered by an Amazon S3 object creation event whenever a `.txt`
file is uploaded to the source bucket. The function reads the text file,
invokes Amazon Polly for speech synthesis, and stores the generated MP3 file
in the destination S3 bucket.

---

### Key Responsibilities

• Read the uploaded text file from the source S3 bucket  
• Convert text into speech using Amazon Polly  
• Generate MP3 audio output  
• Upload the audio file to the destination S3 bucket  
• Log execution details and errors to CloudWatch  

---

### Environment Variables Used

The Lambda function relies on environment variables for configuration, avoiding
hard-coded values and enabling flexible deployments.

• SOURCE_BUCKET – Name of the S3 bucket containing input `.txt` files  
• DESTINATION_BUCKET – Name of the S3 bucket for generated `.mp3` files  

---

### Execution Flow

1. Lambda is triggered by an S3 `ObjectCreated` event.
2. The source bucket name and destination bucket name are read from environment variables.
3. The uploaded object key is extracted from the event payload.
4. The `.txt` file is fetched from the source S3 bucket.
5. File content is decoded from bytes to UTF-8 text.
6. Text is sent to Amazon Polly using the `SynthesizeSpeech` API.
7. Polly returns an MP3 audio stream.
8. The audio file is temporarily stored in `/tmp`.
9. The MP3 file is uploaded to the destination S3 bucket.
10. Execution status and logs are recorded in CloudWatch.

---

### Logging & Error Handling

• Informational logs track file retrieval, Polly invocation, and uploads  
• Errors are caught using a try-except block  
• Failure details are logged to CloudWatch for troubleshooting  
• Lambda returns HTTP-style status codes for observability  

------------------------------------------------------------

## 🔐 IAM Policy – amc-polly-lambda-policy

### Purpose

The IAM policy defines the minimum permissions required by the Lambda function
to perform text-to-speech conversion securely. It follows the principle of
least privilege.

---

### Permissions Breakdown

#### Amazon S3 Permissions
Allows the Lambda function to:
• Read text files from the source S3 bucket  
• Write MP3 audio files to the destination S3 bucket  

**Permissions**:<br>
• s3:GetObject  <br>
• s3:PutObject  

**Restricted to**:<br>
• amc-polly-source-bucket-0302  <br>
• amc-polly-destination-bucket-0302  

---

#### Amazon Polly Permissions
**Allows the Lambda function to:**<br>
• Invoke Amazon Polly for speech synthesis  

**Permission**:<br>
• polly:SynthesizeSpeech  

**Resource scope**:<br>
• All (required by Polly API)

---

### Security Considerations

• No wildcard S3 access is granted  
• Access is limited to specific buckets only  
• Lambda cannot delete or list bucket contents  
• No unnecessary AWS service permissions included  

------------------------------------------------------------

## ✅ Summary

The Lambda function and IAM policy together form a secure, scalable, and
efficient processing layer for the serverless text-to-speech pipeline. The
design ensures automation, least-privilege access, and production-ready
observability while keeping the implementation simple and maintainable.

