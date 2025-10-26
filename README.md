# Personal-File-Sharing-with-API-Gateway-Lambda-S3

### Description:

The Serverless file sharing platform allows users to securely upload and download files via a simple HTTP API. It leverages AWS Lambda for serverless compute, API Gateway for restfull API management, Amazon S3 for durable and scalable object storage. Users can interact with the platform using any HTTP client, making it versatile for various use cases that involve file sharing and storage.

### Architecture:

!<img width="1132" alt="Screenshot 2024-07-09 at 10 38 15 PM" src="https://github.com/yeshwanthlm/Serverless-File-Sharing-Platform/assets/66474973/702f29d8-8eca-4d17-9842-e291ff945801">

### Steps to Build the Project:

## Prerequisites
1.	AWS Account with appropriate permissions to create Lambda functions, API Gateway, and S3 buckets.

* Step 1: Create an S3 bucket to store uploaded files:
my-file-sharing-bucket-pnm

![Created Bucket][image-1.png]

* Step 2: Create the Lambda function to handle file uploads (UploadFunction):
Name: UploadFunction
Runtime: Python 3.x
Execution role: IAM role with S3 write permissions
Code: Use the UploadFunction Python code.
![Upload Function][image-2.png]

* Step 3: Create the Lambda function to handle file downloads (DownloadFunction):
Name: DownloadFunction
Runtime: Python 3.x
Execution role: IAM role with S3 write permissions
Code: Use the DownloadFunction Python code.
![alt text][image-3.png]

* Step 4: Create an API Gateway
Name: my-file-sharing-api-pnm
Create two resources: /files with POST and GET methods.
For each method, configure Lambda integration with UploadFunction and DownloadFunction respectively.
![alt text][image-4.png]
![alt text][image-5.png]
![alt text][image-6.png]
![alt text][image-7.png]

* Step 5: Configure GET Method:
Method Request --> Edit --> Request validator --> Validate Query String Parameters and Headers
Method Request --> Edit --> Request Body --> text/plain
Integration Request --> Edit --> Mapping Templates --> Content Type: application/json --> Content Body:
{
  "queryStringParameters": {
      "fileName": "$input.params('fileName')"
  }
}

* Step 6: Configure POST Method:
Integration Request --> Edit --> Mapping Templates --> Content Type: text/plain --> Content Body: \
{
  "body" : "$input.body",
  "queryStringParameters" : {
      "fileName" : "$input.params('fileName')"
  }
}

* Step 7: Deploy API Gateway:
Deploy the API to a stage (e.g., dev):
Click on "Actions" > "Deploy API".
Choose your stage (e.g., dev) and deploy.

* Step 8: Testing \
Upload a File
Use Postman or another HTTP client:
POST to https://.execute-api..amazonaws.com/dev/files?fileName=test.txt
Set request body to raw and content type to text/plain.
Enter file content (e.g., Hello World!) and send the request.
or
Use CURL Utility:
curl --location 'https://<api-id>.execute-api.<region>.amazonaws.com/dev/files?fileName=test.txt' \
--header 'Content-Type: text/plain' \
--data 'Hello World from A Monk in Cloud!'
Download a File
Use Postman or another HTTP client:
GET to https://.execute-api..amazonaws.com/dev/files?fileName=test.txt
Verify file content is returned correctly.
or
Use CURL Utility
curl --location 'https://<api-id>.execute-api.<region>.amazonaws.com/dev/files?fileName=test.txt'


[def]: image.png
[def2]: image-1.png
[def3]: image-2.png
[def4]: image-3.png