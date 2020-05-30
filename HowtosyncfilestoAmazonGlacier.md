# How to sync files to Amazon Glacier

## Creating S3 bucket

1. Login to the S3 console:

   https://s3.console.aws.amazon.com/s3/

2. Click on “Create bucket” -> specify bucket name -> make sure the bucket name is unique across all existing bucket names in Amazon S3 -> select a region close to your location -> click Next

   + Click on “Versioning” -> select “Enable versioning” -> click Save
   + Click on “Tags” -> specify key: AccountName, Value – specify here the AWS account name or ID -> click Save
   + Click on “Default encryption” -> select “AES-256” -> click Save

3. Click Next

4. Leave the default settings “Do not grant public read access to this bucket” -> click Next -> click “Create bucket”

   For more information about S3 pricing model, see:

   https://aws.amazon.com/s3/pricing/



## Configuring IAM policy with read/write access to specific S3 bucket

1. Login to the IAM console:

   https://console.aws.amazon.com/iam/

2. From the left pane, click on Policies -> Create policy:

   + Service: S3
   + Actions: List, Read, Write
   + Resources: Specific
     + Bucket: click on “Add ARN” -> specific the bucket name -> click Add
     + Object: Select Any

3. Click on Review Policy

4. Specify policy name **S3ReadWriteSpecificBucket**

5. Click on “Create policy”



## Configuring S3 access to specific IAM group

1. Login to the IAM console:

   https://console.aws.amazon.com/iam/

2. From the left pane, click on Groups -> click on “Create New Group” -> specify group name -> click “Next Step” -> from the policy list, search for the previously created policy **S3ReadWriteSpecificBucket** -> select the policy name -> click “Next Step”-> click “Create Group”
3. In the future, in case you need to allow IAM users access to the specific S3 bucket, add them the newly created group



## Configure user for accessing the storage bucket using CLI

1. Login to the IAM console:

   https://console.aws.amazon.com/iam/

2. From the left pane, click on Users -> click on “Add user” -> specify the user name -> access type: “Programmatic access” -> do not select “AWS Management Console access” -> click “Next: Permissions”

3. From the “add user to group”, select **S3ReadWriteSpecificBucket** -> click “Next: Review” -> click on “Create user”

4. Download the CSV file with the “Access key ID” and “Secret access key” and save the CSV file in a secure location

5. Click Close



## Configuring the AWS CLI environment

Configure a profile for additional AWS IAM account:

`aws configure --profile [profile_name]`

Note: Replace [profile_name] with your target AWS IAM user previously created



## Perform Backup to Glacier

 Run from command prompt:

`aws s3 sync /MySourceFolder s3://MyS3Bucket --storage-class GLACIER --profile profile_name`

Note 1: Replace **MySourceFolder** with the source folder to copy/sync files from

Note 2: Replace **MyS3Bucket** with the target S3/Glacier bucket name

Note 3: Replace **profile_name** with the previously AWS configured profile