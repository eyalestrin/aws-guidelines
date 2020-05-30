# AWS CLI Cheat Sheet

+ Configuring the AWS cli environment (using IAM roles and access/secret keys):

  `aws configure`

+ Configure a profile for additional AWS IAM account:

  `aws configure --profile [profile_name]`

+ Create a new S3 bucket:
`aws s3 mb <BucketName>`
+ Remove S3 bucket:
`aws s3 rb s3://<BucketName> --force`
+ List all S3 buckets:
`aws s3 ls`
+ List of S3 buckets using additional AWS IAM account:
`aws s3 ls --profile [profile_name]`
+ List the content of specific S3 bucket:
`aws s3 ls s3://<BucketName>`
+ Copy file from S3 bucket to local directory:
`aws s3 cp s3://<BucketName>/<FileName> /<LocalFolderName>`



## References

+ Installing the AWS Command Line Interface
https://linuxacademy.com/blog/tutorials/installing-the-aws-command-line-interface/
+ 28 Essential AWS S3 CLI Command Examples to Manage Buckets and Objects
https://www.thegeekstuff.com/2019/04/aws-s3-cli-examples/