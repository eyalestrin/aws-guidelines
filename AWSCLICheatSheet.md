# Using AWS CLI for managing AWS Resources

## Installing AWS CLI

1. Login to the machine using privileged account.

2. Download the latest build of AWS CLI.

   * Windows download instruction and location:

     https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-windows.html

   * Linux download instruction and location:

     https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-linux.html



## How to configure AWS Account and Access Keys

1. Login to the IAM Console:

   https://console.aws.amazon.com/iam/

2. From the left pane, click on Users -> click on “Add user” -> specify the user name -> access type: “Programmatic access” -> do not select “AWS Management Console access” -> click “Next: Permissions”

3. From the “add user to group”, either select existing group or click on “Create group” -> click “Next: Review” -> click on “Create user”

4. Download the CSV file with the “Access key ID” and “Secret access key” and save the CSV file in a secure location

5. Click Close



## Configuring the AWS CLI

Run the command below in-order to configure AWS CLI:

`aws configure –profile <profile_name>`

* Profile name – set relevant target profile name

* AWS Access Key ID – Specify the value from the CSV of the previously created IAM user

* AWS Secret Access Key – Specify the value from the CSV of the previously created IAM user

* Default region name – Specify a region such as eu-west-1

  Full list:

  https://docs.aws.amazon.com/general/latest/gr/rande.html

* Default output format: JSON



Note: By default, the credentials file is stored here:

* On Windows: C:\Users\username.aws\credentials
* On Linux: ~/.aws/credentials



Reference about configuration and credential file settings can be found at:

https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html



## Storage related commands

+ Create a new S3 bucket:

  `aws s3 mb <BucketName>`

* Remove S3 bucket:

  `aws s3 rb s3://<BucketName> --force`

* List all S3 buckets:

  `aws s3 ls`

* List of S3 buckets using additional AWS IAM account:

  `aws s3 ls --profile [profile_name]`

* List the content of specific S3 bucket:

  `aws s3 ls s3://<BucketName>`

* Copy file from S3 bucket to local directory:

  `aws s3 cp s3://<BucketName>/<FileName> /<LocalFolderName>`



## References

+ Installing the AWS Command Line Interface

  https://linuxacademy.com/blog/tutorials/installing-the-aws-command-line-interface/

+ 28 Essential AWS S3 CLI Command Examples to Manage Buckets and Objects

  https://www.thegeekstuff.com/2019/04/aws-s3-cli-examples/

* 15 AWS Configure Command Examples to Manage Multiple Profiles for CLI

  https://www.thegeekstuff.com/2019/03/aws-configure-examples/

