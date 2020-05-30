# Best practices for managing AWS account

## Securing the Root account

1. Login to the Amazon management console: https://<AWS_Account_ID>.signin.aws.amazon.com/console

   Note: Replace AWS Account ID with your actual account ID or DNS name.

2. Click on "Sign-in using root account credentials" -> specify the password for the Root account and click "Sign In"

3. From the main window, expand "Password" -> select "Click here" to replace the initial Root account password -> set a new complex password (minimum 8 characters, including upper case, lower case, at least one number and at least one non-alphanumeric character)

4. From the main window, expand "Multi-factor authentication (MFA)" -> click on Activate MFA -> select the MFA device to activate and follow the steps to active the MFA device

5. From the main window, expand "Access keys (access key ID and secret key)" -> make sure the Root account has no access or secrets keys (delete all previously assigned keys)

6. From the upper right pane, click on account name -> choose "My account":
   + Write down the "AWS Account ID" (it will be used in the next sections)
   + Make sure the Contact information is up to date
   + Under "Alternate Contacts" -> specify contact details for "Billing", "Operations" and "Security"
   + Configure "Security Challenge Questions"

7. From the left pane, click on "Cost Explorer" -> click on Enable Cost Explorer"
8. From the left pane, click on "Budgets" -> click on "Create budget" -> specify the budget details and notifications -> click "Create"
9. From the left pane, click on "Preferences" -> select the notifications you would like to receive via email



## Configuring the IAM policies and initial IAM administrator account

1. Login to the IAM console:

   https://console.aws.amazon.com/iam/

2. From the left pane, click on “Account settings” and set the following password policy:

   + Minimum password length: 8 characters
   + Require at least one uppercase letter (Selected)
   + Require at least one lowercase letter (Selected)
   + Require at least one number (Selected)
   + Allow users to change their own password (Selected)
   + Enable password expiration (Selected)
     + Password expiration period in days: 90
   + Prevent password reuse (Selected)
     + Number of passwords to remember: 24

3. Click on “Apply password policy”

4. From the left pane click on “Users” to create the first administrator IAM user and group -> click on “Add user” -> specify the user name -> leave “Programmatic access” unselected -> select “AWS Management Console access” -> select “Custom password” -> specify complex password -> unselect “User must create a new password at next sign-in” -> click “Next: Permissions” -> select “Add user to group” -> click on “Create group” -> on the “Group name” specify “AdministratorAccess” -> on the “Policy type” select “AdministratorAccess” -> click on “Create group”

5. Click on “Next: Review” -> click on “Create user” -> click on Close

6. From the left pane, click on Users -> click on the newly created admin account -> click on “Security credentials” tab -> click on the pencil icon near “Assigned MFA device” -> select the MFA device to activate and follow the steps to active the MFA device



## Configure S3 buckets for auditing and for billing reports

1. Login to the S3 console:

   https://s3.console.aws.amazon.com/s3/

2. Click on “Create bucket” -> specify bucket name <AWS_Account_ID>-auditlogs (Replace AWS Account ID with your actual account ID) -> select a region close to your location -> click Next

   + Click on “Server access logging” -> click “Enable” -> click Save
   + Click on “Tags” -> specify key: AccountName, Value – specify here the AWS account name or ID -> click Save
   + Click on "Default encryption" -> select "AES-256" -> click Save

3. Click Next

4. Leave the default settings “Do not grant public read access to this bucket” -> click Next -> click “Create bucket”

5. Click on “Create bucket” -> specify bucket name <AWS_Account_ID>-billing-reports (Replace AWS Account ID with your actual account ID) -> select a region close to your location -> click Next

   + Click on “Server access logging” -> click “Enable” -> click Save
   + Click on “Tags” -> specify key: AccountName, Value – specify here the AWS account name or ID -> click Save
   + Click on “Default encryption” -> select “AES-256” -> click Save

6. Click Next

7. Leave the default settings “Do not grant public read access to this bucket” -> click Next -> click “Create bucket”

8. Login to the AWS billing console:

   https://console.aws.amazon.com/billing/

9. From the left pane, click on “Preferences” -> select “Receive Billing Reports” -> billing reports S3 bucket previously created -> make sure the billing reports S3 bucket policy is configured according to the sample policy link -> when done configuring the billing reports S3 bucket policy, click on “Verify” -> select all type of reports -> click on “Save preferences”

10. Login to the AWS CloudTrail console:

    https://console.aws.amazon.com/cloudtrail/

11. From the left pane, click on “Dashboard” -> click on “Create trail” -> specify trail name <AWS_Account_ID>-audit-trail (Replace AWS Account ID with your actual account ID)

    + “Apply trail to all regions” should be set to “Yes”

    + “Read/Write events” should be set to “All”

    + Configure “Storage location”:

    + Create a new S3 bucket – No

      `S3 bucket – specify <AWS_Account_ID>-auditlogs`

      Note: Replace AWS Account ID with your actual account ID

12. Click on “Create”

    Note: AWS CloudTrail is not free. See the pricing information:

    https://aws.amazon.com/cloudtrail/pricing/



## Configure Trusted Advisor

1. Login to the Trusted Advisor management console:

   https://console.aws.amazon.com/trustedadvisor/

2. From the left pane, click on “Preferences” -> select all recipients and set email addresses for “Billing contact”, “Operations Contact” and “Security contact” (similar to the addresses you set up under “My Account” settings)