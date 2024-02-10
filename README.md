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
We can also configure the environment to install all the above simultaneously as an **Environment setup**.<p>
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
![image](https://github.com/JonesKwameOsei/AWSCloud/assets/81886509/4058cf7f-52e2-430a-8ab3-4c8764e0a8f9)<p>
We have achieved our desirable aim. 

## Listing Buckets
It is relevant to do Amazon S3 bucket profiling. This will give an update on bucket **visibility**. Thus, Listing buckets allows you to see all the buckets you have in your AWS account. This provides an overview of the storage resources you are using. By listing buckets, you can verify the access control settings and permissions on each bucket. This is important for ensuring that only authorized users have access to specific buckets. When working with scripts or applications that interact with S3, listing buckets can be useful for automation tasks, such as creating backups, syncing data, or managing resources programmatically. Lastly, Listing buckets can help in troubleshooting issues related to bucket names, permissions, or configurations. It provides a starting point for diagnosing problems within your S3 environment.<p><p>
Overall, listing buckets in Amazon S3 is a fundamental operation that provides essential information about storage infrastructure and helps developers effectively manage and utilise your cloud storage resources.

1. Let us arscertain the number of S3 buckets we have created in our storage. We will ru this script:
```
#!/bin/env bash
echo "== list newest buckets"

# list busckts from decending order. Thus, from the laste created bucket to the oldest. 
aws s3api list-buckets | jq -r '.Buckets | sort_by(.CreationDate) | reverse | .[0:5] | .[] | .Name' 
echo "..."
```
Scripts are executed successfully and the scripts returned a liat of 4 buckets:<p>
![image](https://github.com/JonesKwameOsei/AWSCloud/assets/81886509/fa39e236-771d-42c6-b3ef-63f002321523)<p>

2. Sometimes, we may need to get data from only the newest created bucket. In such circunstance, we will run:
```
#!/bin/env bash
echo "== list newest bucket"

# return the newest or lasted created bucket
aws s3api list-buckets | jq -r '.Buckets | sort_by(.CreationDate) | reverse | .[0] | .Name'
```
The bucket, **unique-bronze-bucket**, was returned since it was the last bucket we created. We can affirm this by checking the bucket in the s3 panel in our AWS account.<p>
![image](https://github.com/JonesKwameOsei/AWSCloud/assets/81886509/89a790e7-35c2-4dcd-ad88-b4fdf917a583)<p>

## Adding Objects to S3 Bucket
1. We will first add a single object. First create a file named with the **touch command**, then put this object into the **jones-new-bucket**. Next create a bashcript with:
```
#!/bin/env bash

# Exit immediately if any command returns a non-zero status
set -e
# Check for bucket name
if [ -z "$1" ]; then
    echo "There needs to be a bucket name. Example: ./bucket my_bucket_name"
    exit 1
fi
# Check for filename prefix
if [ -z "$2" ]; then
    echo "There needs to be a filename prefix. Example: ./bucket my-bucket-name my_filename-prefix"
    exit 1
fi

BUCKET_NAME="$1"
FILENAME="$2"

OBJECT_KEY=$(basename "$FILENAME")

aws s3api put-object \
    --bucket $BUCKET_NAME \
    --body $FILENAME \
    --key $OBJECT_KEY
```

![image](https://github.com/JonesKwameOsei/AWSCloud/assets/81886509/d3a1a5fe-4bc1-4a5f-8b0e-be3488e3eb97)<p>
The file has been added to the the bucket. let's check the acount to confirm. <p>
![image](https://github.com/JonesKwameOsei/AWSCloud/assets/81886509/7267b935-e55f-4f92-b416-ff05671b09f1)<p>

2. Adding Multiple Objects. The scripts beloow create multiple objects.
```
#!/bin/env bash

# Exit immediately if any command returns a non-zero status
set -e

# Check for AWS CLI installation
if ! command -v aws &> /dev/null; then
    echo "AWS CLI is not installed. Please install it first."
    exit 1
fi

# Check for bucket name
: <<COMMENTS
if [ -z "$1" ]; then
    echo "Usage: $0 <bucket-name>"
    exit 1
fi
COMMENTS

BUCKET_NAME="$1"
FILE_COUNT=10

# Check if the bucket exists, if not, create it

if ! aws s3api head-bucket --bucket "$BUCKET_NAME" 2>/dev/null; then
    echo "Creating S3 bucket: $BUCKET_NAME"
    aws s3api create-bucket \
    --bucket $BUCKET_NAME \
    --region eu-west-2 \
    --create-bucket-configuration=LocationConstraint=eu-west-2 \
--query Location \
--output text
fi
# Create a temporary directory to store random files
TEMP_DIR=$(mktemp -d)

# Remove folder if it already exits
rm -r $TEMP_DIR

# We will create a folder to store our output files
mkdir -p $TEMP_DIR

# Generate a random number to manage the number of files created
FILE_COUNT=$((RANDOM % 6 + 5))

# Generate and upload random files
for ((i=1; i<=$FILE_COUNT; i++)); do
    FILE_NAME="random_file_$i.txt"
    FILE_PATH="$TEMP_DIR/$FILE_NAME"

    # Generate random content (in this example, 1 KB of random data)
    dd if=/dev/urandom of="$FILE_PATH" bs=1024 count=$((RANDOM % 1024 + 1)) 2>/dev/null 
done

tree $TEMP_DIR

# Clean up temporary directory
rm -r "$TEMP_DIR"
```
Our files have been created:<p>
![image](https://github.com/JonesKwameOsei/AWSCloud/assets/81886509/41985c1a-7ac8-4423-987d-2aa81bb6fe61)<p>
we will sync these files created later. We ca also created and upload files from the CLI. Let's run this code:
```
#!/bin/env bash

# Exits immediately if any command returns a non-zero status
set -e

# Checks for AWS CLI installation
if ! command -v aws &> /dev/null; then
    echo "AWS CLI is not installed. Please install it first."
    exit 1
fi
# Checks for bucket name
if [ -z "$1" ]; then
    echo "Usage: $0 <bucket-name>"
    exit 1
fi
BUCKET_NAME="$1"
FILE_COUNT=10
# Check if the bucket exists, if not, create it
if ! aws s3api head-bucket --bucket "$BUCKET_NAME" 2>/dev/null; then
    echo "Creating S3 bucket: $BUCKET_NAME"
    aws s3api create-bucket \
    --bucket $BUCKET_NAME \
    --region eu-west-2 \
    --create-bucket-configuration=LocationConstraint=eu-west-2 \
--query Location \
--output text
fi
# Creates a temporary directory to store random files
TEMP_DIR=$(mktemp -d)
# Generate and upload random files
for ((i=1; i<=$FILE_COUNT; i++)); do
    FILE_NAME="random_file_$i.txt"
    FILE_PATH="$TEMP_DIR/$FILE_NAME"

    # Generates random content (in this example, 1 KB of random data)
    dd if=/dev/urandom of="$FILE_PATH" bs=1024 count=1 status=none

    # Upload the file to the S3 bucket
    aws s3 cp "$FILE_PATH" "s3://$BUCKET_NAME/$FILE_NAME"

    echo "Uploaded $FILE_NAME to s3://$BUCKET_NAME/"
done
tree $TEMP_DIR
# Clean up temporary directory
rm -r "$TEMP_DIR"
```
Ten files are created and uploaded. Let's check this below.<p>
![image](https://github.com/JonesKwameOsei/AWSCloud/assets/81886509/a32c18b5-860e-417d-ada4-651d726afc1f)<p>
Let's have a look at the bucket in the S3 panel to asrcetain whether the files have loaded in the bucket, **jones-shiny-bucket**. 10 objects have been loaded into the right destination bucket.<p>
![image](https://github.com/JonesKwameOsei/AWSCloud/assets/81886509/8513e627-aaa4-4b26-b40f-b545cbf357ef)<p>
Now, let's go back to the 6 files files we created and **sync** them into the **jones-new-bucket**.
The Bascript is:
```
#!/bin/env bash

echo "==sync"

# Exit immediately if any command returns a non-zero status
#set -e

# Check for AWS CLI installation
if ! command -v aws &> /dev/null; then
    echo "AWS CLI is not installed. Please install it first."
    exit 1
fi

# Check for bucket name
if [ -z "$1" ]; then
    echo "There needs to be a bucket name. Example: ./bucket my_bucket_name"
    exit 1
fi

BUCKET_NAME="$1"

# Check for filename prefix
if [ -z "$2" ]; then
    echo "There needs to be a filename prefix. Example: ./bucket my-bucket-name my_filename-prefix"
    exit 1
fi

FILENAME_PREFIX="$2"

# Check if the bucket exists, if not, create it
if ! aws s3api head-bucket --bucket "$BUCKET_NAME" 2>/dev/null; then
    echo "Creating S3 bucket: $BUCKET_NAME"
    aws s3api create-bucket \
    --bucket "$BUCKET_NAME" \
    --region eu-west-2 \
    --create-bucket-configuration=LocationConstraint=eu-west-2 \
    --query Location \
    --output text
fi

# Create a temporary directory to store random files
TEMP_DIR=$(mktemp -d)

# Remove folder if it already exists
rm -r "$TEMP_DIR"

# We will create a folder to store our output files
mkdir -p "$TEMP_DIR"

# Generate a random number to manage the number of files created
NUM_FILES=$((RANDOM % 6 + 5))

# Generate and upload random files
for ((i=1; i<=$NUM_FILES; i++)); do
    FILE_NAME="$TEMP_DIR/${FILENAME_PREFIX}_$i.txt"
    #FILE_PATH="$TEMP_DIR/$FILE_NAME"

    # Generate random content (in this example, 1 KB of random data)
    dd if=/dev/urandom of="$FILE_NAME" bs=1024 count=$((RANDOM % 1024 + 1)) 2>/dev/null 
done

tree "$TEMP_DIR"
aws s3 sync "$TEMP_DIR" "s3://$BUCKET_NAME/files"
# Clean up temporary directory
rm -r "$TEMP_DIR"
```
![image](https://github.com/JonesKwameOsei/AWSCloud/assets/81886509/40b7d7fe-e649-4161-85c5-028b0f3eabfc)<p>

The folder and files have be populated in the **jones-new-bucket** as shown below:<p>
![image](https://github.com/JonesKwameOsei/AWSCloud/assets/81886509/5e723f6e-c2f3-4729-ba5f-5bf747720009)
![image](https://github.com/JonesKwameOsei/AWSCloud/assets/81886509/eb214252-a16e-4137-aa5b-c167b8ee9572)<p>

## Delete Files, Folders and Buckets
Having created, listed and uplaoded objects into the Amazon S3 bucket by automating, we will employ bashscripts to again automate the deletion operations. 
1. Deleting Files: We will utilise **aws s3api delete-objects** to the delete objects from the the bucket.
Running the scripts bellow deletes object stored in the buckets. We will first call the list of objects before deleting them:
```
#!/usr/bin/env bash
echo "Objecet Deleted!"

# Exit immediately if any command returns a non-zero status
set -e

# Check for bucket name
if [ -z "$1" ]; then
    echo "There needs to be a bucket name eg. ./bucket my-bucket-name"
    exit 1
fi
# Access the first argument (in this case) 
BUCKET_NAME=$1

# Use the input variable in a script

aws s3api list-objects-v2 \
    --bucket $BUCKET_NAME \
    --query 'Contents[].Key' \
    | jq -n '{Objects: [inputs | .[] | {Key: .}]}' > /tmp/delete_objects.json
    #--region eu-west-2 \
aws s3api delete-objects --bucket $BUCKET_NAME --delete file:///tmp/delete_objects.json
```
Objects deleted:
![image](https://github.com/JonesKwameOsei/AWSCloud/assets/81886509/7117d058-e3d7-421a-a08f-61a6b0f53012)<p>
Let us follow up in the AWS account to see if they have been deleted there. Before clicking the **Refresh** button, the files were still there:<p>
![image](https://github.com/JonesKwameOsei/AWSCloud/assets/81886509/fed7f8df-172d-4ea8-b92e-5ab1cbaff178)<p>
Now let us refresh the page. The files have been deleted:
![image](https://github.com/JonesKwameOsei/AWSCloud/assets/81886509/6c765d76-b0a1-44ac-a9cc-ce7e30da5ab2)<p>

2. Delete Bucket: The following script deletes bucket by providing the bucket name as an argument.
```
#!/usr/bin/env bash

# Exit immediately if any command returns a non-zero status
set -e

echo "Bucket Deleted!"

# Check for bucket name
if [ -z "$1" ]; then
    echo "There needs to be a bucket name eg. ./bucket my-bucket-name"
    exit 1
fi
# Access the first argument (in this case) 
BUCKET_NAME=$1

# Use the input variable in a script
aws s3api delete-bucket \
    --bucket $BUCKET_NAME \
    --region eu-west-2 
```
Let us delete the bucket named, **unique-bronze-bucket**.
Bucket deleted!:
![image](https://github.com/JonesKwameOsei/AWSCloud/assets/81886509/2e3f35fd-25bc-41f5-a196-b45f8bf84126)<p>
Also deleted from the AWS account when the s3 bucket page is refreshed.<p>
![image](https://github.com/JonesKwameOsei/AWSCloud/assets/81886509/504419ad-08c1-4f5d-8c21-07d43fde944c)
![image](https://github.com/JonesKwameOsei/AWSCloud/assets/81886509/cab66a98-8c6c-49e7-9745-f3e2077cb408)<p>

we can also have an interactive deleting session. This isa mopre secured way of removing onbjects since it prompts you to be certain of what to delete. 

```
#!/bin/env bash

echo "=== Hi, welcome to S3 Cleanup ==="

# Exit immediately if any command returns a non-zero status
set -e 
 
# Check for AWS CLI installation
if ! command -v aws &> /dev/null; then
    echo "AWS CLI is not installed. Please install it first."
    exit 1
fi

# Check for AWS CLI configuration
if [ -z "$(aws configure get aws_access_key_id)" ] || [ -z "$(aws configure get aws_secret_access_key)" ]; then
    echo "AWS CLI is not configured. Please run 'aws configure' to set up your credentials."
    exit 1
fi

# Ask for the S3 bucket name
read -p "Enter the name of the S3 bucket you want to delete: " BUCKET_NAME

# Confirm deletion with the user
read -p "Are you sure you want to delete the bucket '$BUCKET_NAME' and all its contents? (yes/no): " CONFIRMATION

# Check if the user confirmed
if [ "$CONFIRMATION" != "yes" ]; then
    echo "Deletion canceled. Exiting script."
    exit 0
fi

# Delete all objects in the bucket
echo "Deleting all objects in the bucket..."
aws s3 rm "s3://$BUCKET_NAME" --recursive --include "*" #--exclude ".gitkeep"

# Delete the bucket
echo "Deleting the bucket..."
aws s3 rb "s3://$BUCKET_NAME"

echo "Deletion complete. S3 Bucket Empty!"
```
Interactivly cleaneed and deleted **jones-new-bucket**.<p>
![image](https://github.com/JonesKwameOsei/AWSCloud/assets/81886509/07d3fb68-467d-4f99-9fdc-ac931c66a09d)
![image](https://github.com/JonesKwameOsei/AWSCloud/assets/81886509/c50224e4-fa1b-4509-9a75-4339e0cfc5c8)<p>
We will go haed and delete all buckets with their contents:<p>
![image](https://github.com/JonesKwameOsei/AWSCloud/assets/81886509/8e28f52e-e371-4868-8f10-dc02808a57e8)
![image](https://github.com/JonesKwameOsei/AWSCloud/assets/81886509/4d191bad-5450-485c-8a06-f2b2feb38203)
![image](https://github.com/JonesKwameOsei/AWSCloud/assets/81886509/8ee47513-7735-47bd-b930-b5ca4ba95839)<p>

### commit and push all changes to the project repo
We will push all changes to the main project folder. Let us see the modifications have to to commit with 
```
git commit
```
![image](https://github.com/JonesKwameOsei/AWSCloud/assets/81886509/780141cd-f45c-4f96-a473-b43462f0388e)<p>
Now, we will commit with:
```
git add
git commit -a
```
![image](https://github.com/JonesKwameOsei/AWSCloud/assets/81886509/e977bad8-7f15-41ed-9bf2-7aea7b57e99b)<p>
Now, we will the project:


















