# AWS-Cloud-Cost-Optimization
AWS Cloud Cost Optimization - Identifying Stale EBS Snapshots

## Overview

This project automates AWS cloud cost optimization by identifying and deleting stale Amazon EBS snapshots that are no longer associated with active EC2 instances. It uses an AWS Lambda function written in Python and leverages the AWS SDK (Boto3) to interact with EC2 resources.

By removing unused EBS snapshots, organizations can reduce unnecessary storage costs and maintain a cleaner AWS environment.

---

## Features

- Retrieves all EBS snapshots owned by the AWS account.
- Fetches all active EC2 instances (Running and Stopped).
- Identifies snapshots whose associated volumes are no longer attached to any active EC2 instance.
- Deletes stale snapshots automatically.
- Helps reduce AWS storage costs.
- Can be scheduled using Amazon EventBridge (CloudWatch Events) for periodic execution.

---

## Architecture

```
          +----------------------+
          | Amazon EventBridge   |
          | (Scheduled Trigger)  |
          +----------+-----------+
                     |
                     v
          +----------------------+
          | AWS Lambda Function  |
          +----------+-----------+
                     |
      +--------------+--------------+
      |                             |
      v                             v
Describe Snapshots         Describe EC2 Instances
      |                             |
      +--------------+--------------+
                     |
                     v
      Identify Stale Snapshots
                     |
                     v
        Delete Unused Snapshots
```

---

## Technologies Used

- AWS Lambda
- Amazon EC2
- Amazon EBS
- Amazon EventBridge (Optional)
- Python 3.x
- Boto3 (AWS SDK for Python)
- IAM Roles & Policies

---

## Prerequisites

Before deploying the Lambda function, ensure you have:

- An AWS Account
- AWS CLI configured
- Python 3.x
- Boto3
- Appropriate IAM permissions:
  - `ec2:DescribeSnapshots`
  - `ec2:DescribeInstances`
  - `ec2:DescribeVolumes`
  - `ec2:DeleteSnapshot`

---

## Project Structure

```
AWS-Stale-EBS-Snapshot-Cleanup/
│
├── lambda_function.py
├── requirements.txt
└── README.md
```

---

## How It Works

1. The Lambda function retrieves all snapshots owned by the AWS account.
2. It fetches all EC2 instances that are currently in **Running** or **Stopped** state.
3. For each snapshot:
   - Finds the associated EBS volume.
   - Checks whether the volume is attached to an active EC2 instance.
4. If no active instance is using the volume:
   - The snapshot is considered stale.
   - The snapshot is deleted.
5. Logs are generated in Amazon CloudWatch for monitoring.

---


## Deployment Steps

1. Create a Lambda function.
2. Upload the Python source code.
3. Assign an IAM role with the required EC2 permissions.
4. Configure the Lambda runtime (Python 3.x).
5. (Optional) Create an Amazon EventBridge rule to run the function on a schedule.
6. Test the Lambda function.

---

## Sample Output

```
Fetching EBS snapshots...

Checking Snapshot: snap-0123456789abcdef

Snapshot is stale.
Deleting Snapshot...

Deleted Snapshot: snap-0123456789abcdef

Cleanup completed successfully.
```

---

## Benefits

- Reduces AWS storage costs.
- Removes unused resources automatically.
- Improves AWS resource management.
- Supports automated infrastructure maintenance.
- Easy to integrate into existing cloud environments.

---

## Future Enhancements

- Notify administrators before deleting snapshots.
- Add snapshot age threshold (e.g., delete only snapshots older than 30 days).
- Generate detailed cleanup reports.
- Send notifications using Amazon SNS.
- Support multi-region snapshot cleanup.
- Add tagging support to exclude critical snapshots.

---

## Learning Outcomes

Through this project, I gained hands-on experience with:

- AWS Lambda
- Amazon EC2 & EBS
- AWS IAM
- Boto3 SDK
- Cloud Cost Optimization
- Serverless Computing
- AWS Automation
- Event-Driven Architecture

---
