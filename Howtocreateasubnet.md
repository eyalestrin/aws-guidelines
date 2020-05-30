# How to create a subnet

## Creating a Subnet

1. Login to the VPC console:

   https://console.aws.amazon.com/vpc

2. From the left pane, click on “Subnets” -> “Create Subnet”:

   + Name tag: Custom-subnet

     Note: Replace the Custom-subnet name according to your needs.

   + VPC: Select the relevant VPC name.

   + Availability Zone: Select an availability zone (for example: eu-west-1a)

   + IPv4 CIDR block: Specify 10.0.10.0/24

     Note: Replace the CIDR block according to your needs.

3. Click on "Yes, Create"

4. Logoff the Amazon console



## References

+ https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Subnets.html