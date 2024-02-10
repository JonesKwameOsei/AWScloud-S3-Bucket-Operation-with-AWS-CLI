# Automating Creating a Amazon Simple Storage Service (Amazon S3) with Bash Scripting and AWS CLI

## Overview 
In this project, we will remotely create **Amazon s3 bucket**,  load data and query the bucket without using the **Amazon Management Console**.  Amazon S3 allows users to store and retrieve data from anywhere on the web. It is commonly used to store files, images, videos, and other data with ease, high scalability, and good reliability.

## Why Create S3 Buckets Remotely?
Creating Amazon S3 buckets remotely offers several benefits:
1. **Accessibility**: By creating S3 buckets remotely, cloud engineers can access and manage the storage resources from anywhere with an internet connection, providing flexibility and convenience.

2. **Scalability**: Remote creation allows developers to easily scale storage capacity up or down based on business requirements without the need for physical hardware upgrades.

3. **Cost-Effective**: Using cloud storage like Amazon S3 can be cost-effective as users only pay for the storage used, without the need to invest in and maintain physical infrastructure.

4. **Reliability**: Amazon S3 is designed for durability and availability, ensuring that the data is stored securely and redundantly in multiple locations. Cloud engineers can remotely store and retrieve data across multiple AZ. 

5. **Security**: AWS provides robust security features for S3 buckets, including encryption options, access control mechanisms, and monitoring tools, to help protect your data from unauthorized access or breaches. One needs IAM credentials to perform activities only assigned to him. This makes it secured to perform operations remotely in s3. 

6. **Integration**: Remote creation allows seamless integration with other AWS services, enabling you to build scalable and efficient cloud-based applications.

These benefits make creating Amazon S3 buckets remotely a popular choice for organisations looking for reliable, scalable, and cost-effective storage solutions.
## Methods to Create Amazon S3 Buckets
Amazon S3 buckets can be created in various ways:

1. **AWS Management Console:**  S3 buckets can be created using the **AWS Management Console**, which provides a user-friendly interface for managing AWS services.

2. **AWS Command Line Interface (CLI):** We can use the **AWS CLI** to create S3 buckets from the command line. This method is useful for automating tasks and scripting.

3. **AWS SDKs:** Developers can use **AWS SDKs (Software Development Kits)** available for various programming languages like Python, Java, Node.js, etc., to create S3 buckets programmatically in your applications.

4. **AWS CloudFormation:** You can define S3 buckets in AWS CloudFormation templates to create and manage resources as a stack.

5. **AWS Cloud Development Kit (CDK):** With AWS CDK, you can define infrastructure as code using programming languages like TypeScript, Python, Java, and C#, allowing you to create S3 buckets along with other AWS resources.

These are some of the common ways you can create Amazon S3 buckets depending on the preference and business use case. In this hands-on project, we will utilise the AWS Command Line Interface to automate activities in S3 bucket. 
## Prerequisite
The following were used to achieved our desired goal:
1. AWS Account
2. Command line tools: We sued Gitpod in this exercise.
**Note**: We can also use 
- AWS Command Line Interface (AWS CLI)
	 - s3cmd
	- Boto3
	- Visual Studio Code
	- GitBash
- Install AWS CLI in Bash
- Install tree in Bash
- Invoke AWS autoprompt 
We can also configure the environment to install all the above simultaneously as an **Environment setup**.
![image](https://github.com/JonesKwameOsei/AWSCloud/assets/81886509/154fb406-cbba-4125-b6f5-d14afde070dc)
## Create S3 Bucket with Bash Script
We need to ensure that we do not have any bucket created already so we can create them from scratch.<p>
![image](https://github.com/JonesKwameOsei/AWSCloud/assets/81886509/c9ca50f0-751a-477c-91ba-fdcd88b21c5b)<p>
1. Let us configure our account to get access 
2. Now let us create a bucket named **jones-new-bucket** by running a script named createbucket:
```
#!/usr/bin/env bash

# Exit immediately if any command returns a non-zero status
set -e
echo "Bucket Created!"
# Check for bucket name
if [ -z "$1" ]; then
    echo "There needs to be a bucket name eg. ./bucket my-bucket-name"
    exit 1
fi
# Access the first argument (in this case) 
BUCKET_NAME=$1
# Use the input variable in a script
aws s3api create-bucket \
    --bucket $BUCKET_NAME \
    --region eu-west-2 \
    --create-bucket-configuration=LocationConstraint=eu-west-2 \
--query Location \
--output text
```
Our script was ran successfully:<p>
![image](https://github.com/JonesKwameOsei/AWSCloud/assets/81886509/39a808b8-391d-4437-ac68-8e0475032d06)<p>
Now lest us check if we have the bucket, jones-new-bucket, created.<p>
![image](https://github.com/JonesKwameOsei/AWSCloud/assets/81886509/46e30128-cbbf-4c87-ba85-1c4ce6042a0a)<p>
Indeed, our bucket has been created. 
3. Now, we will create multiple buckests by running this code:
```
#!/usr/bin/env bash

# Exit immediately if any command returns a non-zero status
set -e
# if bucket is created successfully, output:
echo "Bucket Created!"

# A for loop to create multiple buckets
for bucket_name in jones-beautiful-bucket bk-brass-bucket unique-bronze-bucket; do 
    aws s3api create-bucket \
        --bucket "$bucket_name" \
        --region eu-west-2 \
        --create-bucket-configuration=LocationConstraint=eu-west-2
done
```
It was ran successfully:<p>
![image](https://github.com/JonesKwameOsei/AWSCloud/assets/81886509/57d27703-b432-4e8c-8f70-709a35011f51)<p>
We will confirm if the three new buckets have been created. On the **Amazon S3** panel, select **Buckets** and then click on the **Refresh** button to populate the newly created buckets.<p>
![image](https://github.com/JonesKwameOsei/AWSCloud/assets/81886509/4058cf7f-52e2-430a-8ab3-4c8764e0a8f9)
















