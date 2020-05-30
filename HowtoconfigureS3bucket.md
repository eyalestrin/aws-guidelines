# How to configure S3 bucket

## Creating S3 bucket

1. Login to the S3 console:

   https://s3.console.aws.amazon.com/s3/

2. Click on “Create bucket” -> specify bucket name -> make sure the bucket name is unique across all existing bucket names in Amazon S3 -> select a region close to your location -> click Next

   + Click on “Versioning” -> select “Enable versioning” -> click Save
   + Click on “Tags” -> specify key: AccountName, Value – specify here the AWS account name or ID -> click Save
   + Click on “Default encryption” -> select “AES-256” -> click Save

3. Click Next

4. Leave the default settings “Do not grant public read access to this bucket” -> click Next -> click “Create bucket”

5. For more information about S3 pricing model, see:

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
4. Specify policy name (for example: S3ReadWriteSpecificBucket)
5. Click on “Create policy”



## Configuring S3 access to specific IAM group

1. Login to the IAM console:

   https://console.aws.amazon.com/iam/

2. From the left pane, click on Groups -> click on “Create New Group” -> specify group name -> click “Next Step” -> from the policy list, search for the previously created policy (for example: S3ReadWriteSpecificBucket) -> select the policy name -> click “Next Step”-> click “Create Group”

3. In the future, in case you need to allow IAM users access to the specific S3 bucket, add them the newly created group



## Configuring S3 access to EC2 machines

1. Login to the IAM console:

   https://console.aws.amazon.com/iam/

2. From the left pane, click on Roles -> Create role

3. From the service list, select EC2 -> click “Next: Permissions” -> from the policy list, search for the previously created policy (for example: S3ReadWriteSpecificBucket) -> select the policy name -> click “Next: Review” -> specify role name -> click on “Create role”

4. In the future, in case you need to allow EC2 machines access to the specific S3 bucket, add them the newly created IAM role



## References

+ 28 Essential AWS S3 CLI Command Examples to Manage Buckets and Objects

  https://www.thegeekstuff.com/2019/04/aws-s3-cli-examples/