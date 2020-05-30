# How to create Windows based Amazon EC2 instance from the AWS Marketplace

## How to create Windows based Amazon EC2 instance from the AWS Marketplace

1. Login to the EC2 management console:

   https://console.aws.amazon.com/ec2/

2. From the upper pane, it is strongly recommended to select a region close to your location

   Note: There might be pricing differences between AWS regions.

   For more information, see:

   https://aws.amazon.com/ec2/pricing/

3. From the left pane -> Instances -> Instances -> click on “Launch Instance” -> from the left pane, click on “AWS Marketplace” -> select an image from the marketplace (for example “SQL Server 2016 Express on Windows Server 2016”) -> click Select -> click Continue

4. From the “Choose an Instance Type”, select machine type according to your needs.

   For more information, see:

   https://aws.amazon.com/ec2/instance-types

5. Click on “Next: Configure Instance Details”

6. Configure Instance Details page:

   + Number of instances – unless you need redundancy, leave the default value

   + Network – Select the relevant VPC (Virtual Private Cloud).

     For more information on creating a VPC, see:

     https://docs.aws.amazon.com/AmazonVPC/latest/GettingStartedGuide/getting-started-ipv4.html

   + Subnet – Select the relevant subnet and availability zone.

     For more information on creating subnets, see:

     https://docs.aws.amazon.com/AmazonECS/latest/developerguide/create-public-private-vpc.html

   + Auto-assign Public IP – Leave the default settings

   + IAM role - Choose the relevant Amazon IAM role.

     For more information on creating IAM roles, see:

     https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create.html

   + Shutdown behavior – Choose either Stop (the instance will be shut down, but it’s content will remain) or Terminate (the instance content will be permanently removed)

   + Enable termination protection – This option protect the instance content from been permanently removed when selecting “Terminate instance” from the management console

   + Monitoring – Allows you to monitor the performance of the instance; however, there is additional cost for using this service.

     For more information, see:

     https://aws.amazon.com/cloudwatch/pricing/

   + Tenancy – Leave the default settings (unless you have specific requirement, at additional cost, to choose a dedicated hardware without sharing it with other customers)

7. Click on “Next: Add Storage”

8. From the “Add Storage” page, feel in the following details:

   + Specify the root device volume size

   + Select the Volume type according to your needs.

     For more information, see:

     https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSVolumeTypes.html

   + In case you need additional storage (for example, split between OS drive and data drives), click on “Add New Volume”

9. Click on “Next: Add Tags”

10. Click on “Add Tag”:

    + Key – Specify here ”Name”
    + Value – Specify here the instance host name

11. It is recommended to add additional tags (such as project name, environment, etc.). 

    For more information, see:

    https://aws.amazon.com/answers/account-management/aws-tagging-strategies

12. Click on “Next: Configure Security Group”

13. From the “Configure Security Group” page, select either to create a new security group or to select an existing security group.

    + When creating a new security group, specify an informative “Security group name” and description.
    + When configuring security groups for publicly accessible EC2 instances, it is highly recommended to avoid opening SSH / RDP or database (SQL / MySQL, Oracle, etc.) access from the internet to the EC2 instances – restrict access to the EC2 instances from a static public IP address or your organization public address/subnet

14. Click “Review and Launch” -> click on Launch

15. On the “Select an existing key pair or create a new key pair” page:

    + “Create a new key pair” and specify key pair name – if this is your first EC2 instance in this specific region -> click on “Download Key Pair” to download the private key file -> save this key in a secure location, since it allows access to your EC2 instances
    + “Choose an existing key pair” – if you already created and download the private key file, select an existing key pair -> click on “I acknowledge”

16. Click on “Launch Instances” -> click on “View Instances”

17. Wait for the EC2 instance to switch its state to “running”



## Login to the EC2 instance using RDP client

1. Login to the EC2 management console:

   https://console.aws.amazon.com/ec2/

2. From the upper pane, select the relevant region

3. From the left pane, click on Instances

4. From the main pane, select the relevant EC2 instance -> Actions -> “Get Windows Password”

5. Click on Browse and locate the private key of the EC2 instance previously created -> click Open to copy the content of the file into the “Contents” field

6. Click on “Decrypt Password”

7. Document the administrator password

8. Click Close

9. Run Microsoft “Remote Desktop Connection” client

10. On the computer field, specify either the EC2 instance “IPv4 Public IP” or “Public DNS (IPv4)”

11. Click Connect

12. On the username field specify Administrator

13. On the password field specify the initial password documented above

14. After first time login, replace the initial password and document it

    