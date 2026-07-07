# Project 3: Serverless Architecture (Lambda, API Gateway, S3, DynamoDB) using Terraform

## **🔹 Overview**

In this project, we will create:

✅ **AWS Lambda Function** (Python-based)

✅ **Amazon API Gateway** to expose the Lambda as a REST API

✅ **Amazon S3** to store static assets

✅ **Amazon DynamoDB** for data storage

## **📂 Folder Structure**

```bash
terraform-serverless/
│── main.tf
│── variables.tf
│── outputs.tf
│── lambda/
│   ├── lambda_function.py
│   ├── requirements.txt
│   ├── main.tf
│── api_gateway/
│   ├── main.tf
│── s3/
│   ├── main.tf
│── dynamodb/
│   ├── main.tf
```

## **Step 1: Create an S3 Bucket (s3/main.tf)**

This bucket will store static assets.

```hcl
resource "aws_s3_bucket" "my_bucket" {
  bucket = var.bucket_name
}

resource "aws_s3_bucket_public_access_block" "public_access" {
  bucket = aws_s3_bucket.my_bucket.id

  block_public_acls       = false
  block_public_policy     = false
  ignore_public_acls      = false
  restrict_public_buckets = false
}
```

## **Step 2: Create a DynamoDB Table (dynamodb/main.tf)**

This table will store application data.

```hcl
resource "aws_dynamodb_table" "my_table" {
  name           = var.table_name
  billing_mode   = "PAY_PER_REQUEST"
  hash_key       = "id"

  attribute {
    name = "id"
    type = "S"
  }
}
```

## **Step 3: Create a Lambda Function (lambda/main.tf)**

```hcl
resource "aws_lambda_function" "my_lambda" {
  filename      = "lambda_function.zip"
  function_name = "my_lambda_function"
  role          = aws_iam_role.lambda_exec.arn
  handler       = "lambda_function.lambda_handler"
  runtime       = "python3.9"

  environment {
    variables = {
      TABLE_NAME = aws_dynamodb_table.my_table.name
    }
  }
}
```

### **Lambda Function Code (lambda/lambda_function.py)**

This function writes data to DynamoDB.

```python
import json
import boto3
import os
import uuid

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table(os.environ['TABLE_NAME'])

def lambda_handler(event, context):
    item = {
        'id': str(uuid.uuid4()),
        'message': 'Hello from Lambda!'
    }
    table.put_item(Item=item)

    return {
        'statusCode': 200,
        'body': json.dumps(item)
    }
```

## **Step 4: Create an API Gateway (api_gateway/main.tf)**

This will expose the Lambda function as a REST API.

```hcl
resource "aws_api_gateway_rest_api" "my_api" {
  name        = "ServerlessAPI"
  description = "API Gateway for Lambda"
}

resource "aws_api_gateway_resource" "lambda_resource" {
  rest_api_id = aws_api_gateway_rest_api.my_api.id
  parent_id   = aws_api_gateway_rest_api.my_api.root_resource_id
  path_part   = "lambda"
}

resource "aws_api_gateway_method" "lambda_post" {
  rest_api_id   = aws_api_gateway_rest_api.my_api.id
  resource_id   = aws_api_gateway_resource.lambda_resource.id
  http_method   = "POST"
  authorization = "NONE"
}

resource "aws_api_gateway_integration" "lambda_integration" {
  rest_api_id = aws_api_gateway_rest_api.my_api.id
  resource_id = aws_api_gateway_resource.lambda_resource.id
  http_method = aws_api_gateway_method.lambda_post.http_method
  integration_http_method = "POST"
  type = "AWS_PROXY"
  uri = aws_lambda_function.my_lambda.invoke_arn
}
```

## **Step 5: Deploy the Terraform Code**

```
$ terraform init
$ terraform apply -auto-approve
```

## **✅ What We Have Built**

✔ **AWS Lambda function that writes data to DynamoDB**

✔ **API Gateway that exposes the Lambda function**

✔ **S3 Bucket for storing static assets**

✔ **DynamoDB Table for persistent storage**

Let's test our **Serverless API** to ensure everything is working as expected. 🚀

## **🔹 Step 1: Get the API Gateway URL**

After running `terraform apply`, Terraform will output the **API Gateway invoke URL**. You can also find it in the AWS Console under **API Gateway > Stages > Invoke URL**.

It should look something like this:

```bash
https://abcdefghij.execute-api.us-east-1.amazonaws.com/prod/lambda
```

## **🔹 Step 2: Send a Test Request**

We will send a **POST request** to trigger our Lambda function. You can use **cURL**, **Postman**, or any API testing tool.

### **Using cURL (Command Line)**

```bash
$ curl -X POST https://abcdefghij.execute-api.us-east-1.amazonaws.com/prod/lambda
```

### **Using Postman**

1. Open Postman
2. Set method to **POST**
3. Enter the API URL (`https://abcdefghij.execute-api.us-east-1.amazonaws.com/prod/lambda`)
4. Click **Send**

## **🔹 Step 3: Verify Data in DynamoDB**

After making the API request:

✅ You should get a JSON response similar to this:

```json
{
    "id": "f68f1d27-9b7d-423d-b3ab-7e1c784b4e79",
    "message": "Hello from Lambda!"
}
```

✅ Go to the **AWS Console > DynamoDB > Tables > my_table > Items**

✅ You should see a new record in your table with a unique `id` and message.