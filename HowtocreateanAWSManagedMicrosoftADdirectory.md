# How to create an AWS Managed Microsoft AD directory

1. Login to the AWS Directory Service console:

   https://console.aws.amazon.com/directoryservicev2/

2. Click on "Set up directory"

3. Select "AWS Managed Microsoft AD" -> click Next

4. Directory Information page:

   + Directory type: Select "Standard Edition"
   + Directory DNS name: Specify target directory DNS name
   + Admin password: Specify complex password for the managed directory, and document the password in a secured location
   + Confirm password: Specify the above password

5. Click on Next

6. Networking page:

   + VPC: Select the relevant VPC name from the list
   + Subnets: Select the relevant subnets from the list

7. Click Next

8. Click Create directory

9. Wait for the directory creation process to complete

10. Begin working the new managed AD

11. Logoff the AWS console

    