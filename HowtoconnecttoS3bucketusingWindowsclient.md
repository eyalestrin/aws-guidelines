# How to connect to S3 bucket using Windows client

## Creating IAM user

1. Login to the IAM console:

   https://console.aws.amazon.com/iam/

2. From the left pane, click on Users -> click on “Add user” -> specify the user name -> access type: “Programmatic access” -> do not select “AWS Management Console access” -> click “Next: Permissions”

3. From the “add user to group”, click on “Create group” -> from the policy list, select “AmazonS3FullAccess” -> click “Next: Review” -> click on “Create user”

4. Download the CSV file with the “Access key ID” and “Secret access key” and save the CSV file in a secure location

5. Click Close



## Download and install S3 Browser

1. Download the installation file from:

   https://s3browser.com/download.aspx

2. Double click on the installation file and follow the instructions



## Configure the S3 Browser

1. Run the S3 Browser from the Windows client
2. From the “Add New Account page", specify the following details:
   + Account name – specify here the previously created IAM user
   + Account type – select “Amazon S3 Storage”
   + AWS Access Key ID – specify here the access key ID from the CSV file
   + AWS Secret Access Key – specify here the secret access key
   + Encrypt Access Keys with a password – select this option to specify password to protect the AWS credentials from been compromised
   + Use secure transfer (SSL / TLS) – select this option
3. Click on Add new account