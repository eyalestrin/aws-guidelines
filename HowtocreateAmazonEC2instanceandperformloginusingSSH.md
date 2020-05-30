# How to create Amazon EC2 instance and perform login using SSH

## Login to the management console

1. Login to the EC2 management console:

   https://console.aws.amazon.com/ec2/

2. From the upper pane, it is strongly recommended to select a region close to your location

   There might be pricing differences between AWS regions. For more information, see:

   https://aws.amazon.com/ec2/pricing/

3. From the left pane -> Instances -> Instances -> click on “Launch Instance” -> select the relevant Amazon Machine Image.

4. From the “Choose an Instance Type”, select machine type according to your needs.

   For more information, see:

   https://aws.amazon.com/ec2/instance-types/

5. Click on “Next: Configure Instance Details”

6. Configure Instance Details page:

   + Number of instances – unless you need redundancy, leave the default value

   + Purchasing option - In case you do not need permanent EC2 machine for development / test environments, and would like to save money, consider using EC2 spot instance.

     For more information, see:

     https://aws.amazon.com/ec2/spot/

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

   + Enable termination protection – This option protect the instance content from been permanently removed when selecting “Terminate instance” from the management console. 

     Note: This option is not relevant for Spot instances

   + Monitoring – Allows you to monitor the performance of the instance; however, there is additional cost for using this service.

     For more information, see:

     https://aws.amazon.com/cloudwatch/pricing/

   + Tenancy – Leave the default settings (unless you have specific requirement, at additional cost, to choose a dedicated hardware without sharing it with other customers)

   + Network interfaces – Leave the default settings

   + Advanced Details – under “User data”, you can specify post-login script (for example, update security patches, install additional components, etc.)

     Example of using "User data" can be found here:

     http://progressivecoder.com/how-to-use-aws-ec2-user-data-to-create-ec2-instance/

7. Click on “Next: Add Storage”

8. From the “Add Storage” page, feel in the following details:

   + Specify the root device volume size

   + Select the Volume type according to your needs.

     For more information, see:

     https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSVolumeTypes.html

   + In case you need additional storage (for example, split between OS drive and data drives), click on “Add New Volume”

   + When adding additional volumes, it is recommended to encrypt all the additional volumes (there is no performance impact)

9. Click on “Next: Add Tags”

10. Click on “Add Tag”:

    + Key – Specify here ”Name”
    + Value – Specify here the instance host name

11. It is recommended to add additional tags (such as project name, environment, etc.).

    For more information, see:

    https://aws.amazon.com/answers/account-management/aws-tagging-strategies/

12. Click on “Next: Configure Security Group”

13. From the “Configure Security Group” page, select either to create a new security group or to select an existing security group.

    + When creating a new security group, specify an informative “Security group name” and description.
    + When configuring security groups for publicly accessible EC2 instances, it is highly recommended to avoid opening SSH / RDP access from the internet to the EC2 instances – restrict access to the EC2 instances from a static public IP address or your organization public address/subnet

14. Click “Review and Launch” -> click on Launch

15. On the “Select an existing key pair or create a new key pair” page:

    + “Create a new key pair” and specify key pair name – if this is your first EC2 instance in this specific region -> click on “Download Key Pair” to download the private key file -> save this key in a secure location, since it allows access to your EC2 instances
    + “Choose an existing key pair” – if you already created and download the private key file, select an existing key pair -> click on “I acknowledge”

16. Click on “Launch Instances” -> click on “View Instances”

17. Wait for the EC2 instance to switch its state to “running”



## Login to EC2 instance (from Windows machine)

1. Download puttygen.exe from:

   http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html

2. Run the puttygen.exe

3. Click on “Load” -> change the file extension from “Putty Private key files” to “All Files” -> locate the private key pair and click on Open -> click on OK -> click on “Save private key” -> click on “Yes” -> save the private key file with PPK extension -> close puttygen.exe

4. Download Putty from:

   https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html

5. Run putty.exe

6. From the left pane, under “Connection” -> expand SSH -> click on “Auth” -> from the main pane, under “Authentication parameters”, click on “Browse” -> locate the SSH private key generated by puttygen.exe

7. From the left pane, click on “Session” -> from the main pane, under “Host Name (or IP address)” specify the following:

   `ec2-user@IP_Address`

   Note: Replace IP_Address with the EC2 instance “IPv4 Public IP” or “Public DNS (IPv4)”

8. Under “Saved Sessions”, specify a name for this newly created connection.

9. Click on Save

10. Click on Open



## Login to EC2 instance (from Linux machine)

1. Login to the Linux machine console.

2. Copy the private key file into ~/.ssh of the currently running user

3. Run the following command:

   `ssh ec2-user@IP_Address -i ~/.ssh/[KEY_FILENAME]`

   Note 1: Replace IP_Address with the EC2 instance “IPv4 Public IP” or “Public DNS (IPv4)”

   Note 2: Replace KEY_FILENAME with the actual private key file name