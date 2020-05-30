# How to set up Amazon FSx for Windows File Server

## Pre-requirements

+ AWS Directory service configured

+ Windows based EC2 machine installed and joined to the AWS directory service

  For more information, see:

  https://docs.aws.amazon.com/fsx/latest/WindowsGuide/walkthrough01-prereqs.html

+ AWS EC2 installed on the same VPC, subnet and Security Group as the target Amazon FSx file system



## Creating an Amazon FSx File System

1. Login to the Amazon FSx console:

   https://console.aws.amazon.com/fsx/

2. Click on Create file system

3. Select Amazon FSx for Windows File Server -> click Next

4. Create file system page:

   + File system details:
     + File system name – specify a name for the new file system
     + Storage capacity – specify the size of the new file system (minimum 300GB)
   + Network & security:
     + Virtual Private Cloud (VPC) – select the same VPC as the AWS Directory service previously created and the same VPC as the EC2 machine
     + Availability zone – select one of the availability zones were the AWS Directory service previously created and the same availability zone as the EC2 machine
     + Subnet – select one of the availability zones were the AWS Directory service previously created, and on the same subnet where the target EC2 instance is connected to
   + Windows authentication:
     + Microsoft Active Directory ID – select the previously created AWS Directory service
   + Encryption:
     + Encryption key – select either the default aws/fsx encryption key or specify the ARN for the AWS KMS fsx encryption key (for customer managed key scenarios)
   + Maintenance preferences:
     + Daily automatic backup window – select start time, and specify off-hours time
     + Automatic backup retention period – leave the default (7 days) or specify your own value
     + Weekly maintenance window – unless you have maintenance window (such as patching time), leave the default “No preference”

5. Click Next -> click Create file system

6. Wait for the new file system to be created

7. From the Amazon FSx console, locate the newly created file system -> Click on the newly created file system -> Network & security tab -> write down the DNS name (for future use)

8. Logoff the AWS console



## Mounting the AWS FSx file system

1. Login to the Windows based EC2 machine using RDP

2. Open File Explorer

3. Right click on This PC -> Map network drive

4. Map Network Drive page:

   + Drive: Select a drive letter

   + Folder: Specify the path to the network drive in the following format:

     `\\DNS_Name\Share`

     Note: Replace **DNS_Name** with the DNS name value written before

5. Select “Connect using different credentials”

6. Click Finish

7. Enter network credentials page:

   + Username: Admin

   + Password: The password for the admin account specified during the creation of the AWS Directory service

     Note: Instructions for resetting the admin password:

     https://docs.aws.amazon.com/directoryservice/latest/admin-guide/ms_ad_manage_users_groups_reset_password.html

8. Select “Remember my credentials” -> click OK

9. Begin using the managed file system