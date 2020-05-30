# How to configure MFA (Multi-Factor Authentication) for AWS IAM User

1. Install Virtual MFA application on your mobile device, as instructed:

   https://aws.amazon.com/iam/features/mfa/

2. Login to the AWS console:

   https://console.aws.amazon.com/iam/

3. From the left pane, click on Users

4. In the User Name list, select the target user

5. Choose the Security credentials tab -> click Next to Assigned MFA device, choose Manage

6. In the Manage MFA Device wizard, choose Virtual MFA device, and then choose Continue

   IAM generates and displays configuration information for the virtual MFA device, including a QR code graphic.

7. From the mobile device, open the virtual MFA application

   If the virtual MFA app supports multiple virtual MFA devices or accounts, choose the option to create a new virtual MFA device or account

8. In the Manage MFA Device wizard, in the MFA code 1 box, type the one-time password that currently appears in the virtual MFA device.

   Wait up to 30 seconds for the device to generate a new one-time password. Then type the second one-time password into the MFA code 2 box. Choose Assign MFA



## References

https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_enable_virtual.html#enable-virt-mfa-for-iam-user