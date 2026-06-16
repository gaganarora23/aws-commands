# AWS CLI Commands Reference

A comprehensive reference guide for commonly used AWS CLI commands, organized by service and functionality.

---

## Table of Contents
- [Authentication & Account](#authentication--account)
- [EC2](#ec2)
- [S3](#s3)
- [IAM](#iam)
- [CloudFormation](#cloudformation)
- [RDS](#rds)
- [Lambda](#lambda)
- [VPC & Networking](#vpc--networking)
- [Monitoring & Logs](#monitoring--logs)

---

## Authentication & Account

### Check AWS CLI Version
```bash
aws --version
```
**Description:** Displays the installed version of the AWS CLI and the Python version it's using. Useful for troubleshooting compatibility issues.

### Get AWS Account Identity
```bash
aws sts get-caller-identity
```
**Description:** Displays the AWS account ID, user ID, and ARN of the currently authenticated AWS principal. Helps verify which AWS account and credentials are active.

**Response**

```json
{
    "UserId": "ABCAVC:ConsoleSession",
    "Account": "5959625",
    "Arn": "arn:aws:sts::5959625:assumed-role/ConsoleAccessRoleEDA/ConsoleSession"
}
```

---

## EC2

### List All EC2 Instances
```bash
aws ec2 describe-instances
```
**Description:** Lists all EC2 instances in the current region with detailed information.

### List Running Instances
```bash
aws ec2 describe-instances --filters "Name=instance-state-name,Values=running"
```
**Description:** Lists only running EC2 instances.

### Start an Instance
```bash
aws ec2 start-instances --instance-ids <instance-id>
```
**Description:** Starts a stopped EC2 instance.

### Stop an Instance
```bash
aws ec2 stop-instances --instance-ids <instance-id>
```
**Description:** Stops a running EC2 instance.

---

## S3

### List All Buckets
```bash
aws s3 ls
```
**Description:** Lists all S3 buckets in your AWS account.

### List Bucket Contents
```bash
aws s3 ls s3://<bucket-name>
```
**Description:** Lists objects in a specific S3 bucket.

```bash
aws s3 ls s3://<bucket-name> --recursive
```
**Description:** Lists objects in a specific S3 bucket.

### Upload File to S3
```bash
aws s3 cp <local-file> s3://<bucket-name>/<key>
```
**Description:** Uploads a file from local machine to S3 bucket.

### Download File from S3
```bash
aws s3 cp s3://<bucket-name>/<key> <local-file>
```
**Description:** Downloads a file from S3 bucket to local machine.

### Read File from S3
```bash
aws s3 cp s3://<bucket-name>/<key> -
```
**Description:** Read a file from S3 bucket without downloading to local machine.


---

## IAM

### List All Users
```bash
aws iam list-users
```
**Description:** Lists all IAM users in the AWS account.

### List All Roles
```bash
aws iam list-roles
```
**Description:** Lists all IAM roles in the AWS account.

### Get User Info
```bash
aws iam get-user --user-name <username>
```
**Description:** Displays detailed information about a specific IAM user.

---

## CloudFormation

### List All Stacks
```bash
aws cloudformation list-stacks
```
**Description:** Lists all CloudFormation stacks.

### Describe Stack
```bash
aws cloudformation describe-stacks --stack-name <stack-name>
```
**Description:** Shows detailed information about a specific stack.

### Create Stack
```bash
aws cloudformation create-stack --stack-name <name> --template-body file://<template.yaml>
```
**Description:** Creates a new CloudFormation stack from a template.

---

## RDS

### List All DB Instances
```bash
aws rds describe-db-instances
```
**Description:** Lists all RDS database instances.

### Get DB Instance Details
```bash
aws rds describe-db-instances --db-instance-identifier <instance-id>
```
**Description:** Shows detailed information about a specific database instance.

---

## Lambda

### List All Functions
```bash
aws lambda list-functions
```
**Description:** Lists all Lambda functions in the current region.

### List Lambda Functions and list their names.

```bash
aws lambda list-functions --query "Functions[].FunctionName"                                                                                                                                                                                                  
[
    "student92-populate-bucket-lambda",
    "student92-hello-world-lambda",
    "student92-empty-bucket-lambda"
]
```

### Get Lambda Function 

```bash

aws lambda get-function \
  --function-name student92-hello-world-lambda \
  --query 'Configuration.[FunctionName, Runtime, MemorySize, Timeout]' \
  --output table

----------------------------------
|           GetFunction          |
+--------------------------------+
|  student92-hello-world-lambda  |
|  python3.12                    |
|  128                           |
|  30                            |
+--------------------------------+

```

### Invoke Function
```bash
aws lambda invoke --function-name <function-name> response.json

aws lambda invoke --function-name student92-hello-world-lambda output.json
{
    "StatusCode": 200,
    "ExecutedVersion": "$LATEST"
}
```
~ $ cat output.json

```json
{
    "statusCode": 200,
    "message": "Hello from AWS Lambda!",
    "timestamp": "2025-06-16T20:59:34.606560",
    "function_name": "student92-hello-world-lambda",
    "event_received": {},
    "tips":
        [
            "You successfully invoked this Lambda function using AWS CLI!", "Check CloudWatch Logs to see the function execution logs next"
        ]
}
```

**Description:** Invokes a Lambda function synchronously and saves the response.

---

## VPC & Networking

### List VPCs
```bash
aws ec2 describe-vpcs
```
**Description:** Lists all VPCs in the current region.

### List Security Groups
```bash
aws ec2 describe-security-groups
```
**Description:** Lists all security groups in the current region.

### List Subnets
```bash
aws ec2 describe-subnets
```
**Description:** Lists all subnets in the current region.

---

## Monitoring & Logs

### Get CloudWatch Metrics
```bash
aws cloudwatch get-metric-statistics --namespace AWS/EC2 --metric-name CPUUtilization --start-time <start> --end-time <end> --period 300 --statistics Average
```
**Description:** Retrieves CloudWatch metrics for monitoring resource performance.

### List Log Groups
```bash
aws logs describe-log-groups
```
**Description:** Lists all CloudWatch log groups.

---

## Tips

- Replace `<placeholder>` with actual values
- Use `--region <region-name>` to specify a different AWS region
- Use `--profile <profile-name>` to use a different AWS CLI profile
- Add `--output json|text|table` to format output
