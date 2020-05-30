# How to create a new security group

## Creating a new security group

1. Login to the VPC console:

   https://console.aws.amazon.com/vpc

2. From the left pane, click on “Security Groups” -> “Create Security Group”:

   + Name tag: Custom-security-group

     Note: Replace Custom-security-group with your own security group name

   + Group name: Specify here the same value as you specified in the Name tag field

   + Description: Specify here description for the new security group

   + VPC: Select the relevant VPC from the list

3. Click on "Yes, Create"

4. Select the newly created security group from the list -> from the lower pane, click on "Inbound Rules" -> click on Edit -> specify the example below for new inbound rule:

   + Type: SSH (22)
   + Protocol: TCP (6)
   + Source: Specify here your machine internet facing IP address or the internet facing Firewall IP address
   + Description: Specify a short description for the rule (for example: Inbound SSH access)

5. Click on Save

6. Logoff the Amazon console



## References

+ https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html