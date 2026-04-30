# logsentinel-aws
Serverless log monitoring pipeline on AWS


# LogSentinel — Serverless Log Monitoring System

An automated log analysis pipeline built on AWS that monitors, 
classifies, and alerts on application errors in real time.

## Architecture
![image alt]( https://github.com/Sangamani/logsentinel-aws/blob/3e9fb45eef580e54e92b94cb52bb9001623f0efb/LogSentinel_Architecture%20(3).drawio%20(1).png)


## Features

- Auto-triggered log analysis using CloudWatch Subscription Filters
- Severity classification — Critical / High / Low / Info
- Structured JSON logs stored in S3 partitioned by date
- Real-time CloudWatch dashboard with custom metrics
- Automatic email alerts via SNS for errors
- Fully serverless — no servers to manage

## Tech Stack

| Service | Purpose |
|---|---|
| AWS EC2 | Log source (Python/Node.js app) |
| AWS CloudWatch | Log collection and metrics |
| AWS Lambda | Serverless log processing |
| AWS S3 | Structured log storage |
| AWS SNS | Email alerting |
| AWS Bedrock | AI-powered log classification |
| Python 3.11 | Lambda runtime |

## Project Structure

logsentinel-aws/
  ├── lambda_function.py    # Main Lambda handler
  ├── README.md             # Project documentation
  └── screenshots/          # Dashboard and architecture screenshots
      ├── dashboard.png
      ├── s3-logs.png
      └── sns-alert.png

## Setup Guide

### 1. Create Lambda Function
- Runtime: Python 3.11
- Handler: lambda_function.lambda_handler
- Memory: 256 MB
- Timeout: 30 seconds

### 2. Set Environment Variables
| Key | Value |
|---|---|
| S3_BUCKET | your-bucket-name |
| SNS_TOPIC_ARN | arn:aws:sns:ap-south-1:xxxx:nodejs-error-alerts |

### 3. IAM Permissions Required
- AmazonS3FullAccess
- AmazonSNSFullAccess
- CloudWatchFullAccess
- AmazonBedrockFullAccess

### 4. Create CloudWatch Subscription Filter
- Log group: /app/nodejs/production
- Filter pattern: (empty — captures all logs)
- Destination: Lambda function

### 5. Create SNS Topic
- Type: Standard
- Name: nodejs-error-alerts
- Protocol: Email
- Confirm subscription from email

## Severity Classification

| Severity | Triggers |
|---|---|
| Critical | FATAL, CRASH, UNHANDLED, OUT OF MEMORY |
| High | ERROR, EXCEPTION, TIMEOUT, CONNECTION REFUSED |
| Low | WARN, WARNING, DEPRECATED, SLOW |
| Info | Everything else |

## Sample S3 OutpuT

{
  "log_group": "/app/nodejs/production",
  "processed_at": "2026-04-25T17:25:11",
  "events": [
    {
      "timestamp": "2026-04-25T17:25:00",
      "message": "ERROR: Database connection failed",
      "severity": "High",
      "log_group": "/app/nodejs/production"
    }
  ]
}

## Sample SNS Alert Email

Subject: [ALERT] 1 error(s) in /app/nodejs/production

{
  "log_group": "/app/nodejs/production",
  "error_count": 1,
  "by_severity": {
    "High": ["ERROR: Database connection failed"]
  }
}

## Author

Sangamani -AWS Cloud Project
GitHub: https://github.com/Sangamani
