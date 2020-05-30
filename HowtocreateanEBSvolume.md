# How to create an EBS volume

1. Login to the EC2 management console:

   https://console.aws.amazon.com/ec2/

2. From the upper pane, select a region for creating the EBS volume

   Note: The volume must be in the same Amazon region as the target EC2 machine the volume will be attached to

3. From the left pane, under “Elastic Block Store”, click on Volumes

4. From the main pane, click on “Create Volume” -> select the Volume type according to your needs. 

   For more information, see:

   https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSVolumeTypes.html

5. Specify the volume size

6. Select the relevant availability zone.

   Note: The volume should be in the same availability zone as the EC2 machine the volume will be attached to

7. Select “Encrypt this volume” -> select Master key from the list (or use the default aws/ebs master key)

8. Click on “Add Tag”:

   + Key – Specify here “Name”
   + Value – Specify here the EBS volume name (It is recommended to include the target EC2 machine host name, as a prefix to the EBS volume name)

9. Click on “Create Volume”

10. From the main pane, select the newly created EBS volume

11. From the upper pane, click on Actions -> Attach Volume -> select the target EC2 machine name -> click on Attach

    