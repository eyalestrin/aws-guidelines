# How to add permissions to AWS resources using AWS CLI on Windows client

## Creating IAM user

1. Login to the IAM console:

   https://console.aws.amazon.com/iam/

2. From the left pane, click on Users -> click on “Add user” -> specify the user name -> access type: “Programmatic access” -> do not select “AWS Management Console access” -> click “Next: Permissions”

3. From the “add user to group”, either select existing group or click on “Create group” -> click “Next: Review” -> click on “Create user”

4. Download the CSV file with the “Access key ID” and “Secret access key” and save the CSV file in a secure location

5. Click Close



## Install the AWS CLI

1. Login to the Windows client

2. Follow the instructions below on how to download and install the AWS CLI tools:

   https://docs.aws.amazon.com/cli/latest/userguide/awscli-install-windows.html



## Configuring AWS CLI credentials on the Windows client

1. Run the command below from CLI:

   `aws configure`

2. Fill in the following details:
   + AWS Access Key ID – specify here the access key ID from the CSV file
   + AWS Secret Access Key – specify here the secret access key
   + Region – specify here the default region close to your location (for example eu-west-1)