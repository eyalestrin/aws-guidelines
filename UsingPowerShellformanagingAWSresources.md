# Using PowerShell for managing AWS resources

## How to configure PowerShell for managing AWS resources (Windows platform)

1. Login to the machine using privileged account.

2. Follow the instructions below to install the latest build of PowerShell:

   https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell?view=powershell-7

3. From command prompt, run the command below to invoke PowerShell:

   `pwsh`

   Note: You need to run cmd.exe or pwsh.exe as administrator.

4. Run the command below to find out the current PowerShell version:

   `$PSVersionTable.PSVersion`

5. In-case you currently have version older than 5.1, follow the article below to locate the download URL for upgrading to the latest version of PowerShell:

   https://docs.microsoft.com/en-us/powershell/scripting/install/migrating-from-windows-powershell-51-to-powershell-7?view=powershell-7

6. Run the commands below to install AWS tools for PowerShell:

   `Install-Module -Name AWS.Tools.Common -Force`

   `Install-Module -Name AWS.Tools.Installer -Force`

8. Run the commands below to update to the latest AWS PowerShell module:

   `Update-Module -Name AWS.Tools.Common -Force`
   `Update-Module -Name AWS.Tools.Installer -Force`

9. To view the installed versions of AWS PowerShell module, run the command below:

   `Get-Module -Name AWS.Tools.* -List | select Name,Version`



## How to configure PowerShell for managing AWS resources (RHEL/CentOS platform)

1. Login to the machine using privileged account.

2. Run the command below to register the RedHat 7 or CentOS 7 repository:

   `curl https://packages.microsoft.com/config/rhel/7/prod.repo | sudo tee /etc/yum.repos.d/microsoft.repo`

3. Run the command below to install PowerShell:

   `sudo yum install -y powershell`

4. From command prompt, run the command below to invoke PowerShell:

   `sudo pwsh`

5. Run the command below to find out the current PowerShell version:

   `$PSVersionTable.PSVersion`

6. Run the commands below to install AWS tools for PowerShell:

   `Install-Module -Name AWS.Tools.Common -Force`

   `Install-Module -Name AWS.Tools.Installer -Force`

8. Run the commands below to update to the latest AWS PowerShell module:

   `Update-Module -Name AWS.Tools.Common -Force`
   `Update-Module -Name AWS.Tools.Installer -Force`

9. To view the installed versions of AWS PowerShell module, run the command below:

   `Get-Module -Name AWS.Tools.* -List | select Name,Version`



## How to configure PowerShell for managing AWS resources (Ubuntu 18.04 platform)

1. Login to the machine using privileged account.

2. Run the command below to register the Ubuntu 18.04 repository:

   `wget -q https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb`
   `sudo dpkg -i packages-microsoft-prod.deb`
   `sudo apt-get update`
   `sudo add-apt-repository universe`

3. Run the command below to install PowerShell:

   `sudo apt-get install -y powershell`

4. From command prompt, run the command below to invoke PowerShell:

   `pwsh`

5. Run the command below to find out the current PowerShell version:

   `$PSVersionTable.PSVersion`

6. Run the commands below to install AWS tools for PowerShell:

   `Install-Module -Name AWS.Tools.Common -Force`

   `Install-Module -Name AWS.Tools.Installer -Force`

8. Run the commands below to update to the latest AWS PowerShell module:

   `Update-Module -Name AWS.Tools.Common -Force`
   `Update-Module -Name AWS.Tools.Installer -Force`

9. To view the installed versions of AWS PowerShell module, run the command below:

   `Get-Module -Name AWS.Tools.* -List | select Name,Version`



## How to configure PowerShell for managing AWS resources (Debian 10 platform)

1. Login to the machine using privileged account.

2. Run the command below to register the Debian 10 repository:

   `wget https://packages.microsoft.com/config/debian/10/packages-microsoft-prod.deb`
   `sudo dpkg -i packages-microsoft-prod.deb`
   `sudo apt-get update`

3. Run the command below to install PowerShell:

   `sudo apt-get install -y powershell`

4. From command prompt, run the command below to invoke PowerShell:

   `pwsh`

5. Run the command below to find out the current PowerShell version:

   `$PSVersionTable.PSVersion`

6. Run the commands below to install AWS tools for PowerShell:

   `Install-Module -Name AWS.Tools.Common -Force`

   `Install-Module -Name AWS.Tools.Installer -Force`

8. Run the commands below to update to the latest AWS PowerShell module:

   `Update-Module -Name AWS.Tools.Common -Force`
   `Update-Module -Name AWS.Tools.Installer -Force`

9. To view the installed versions of AWS PowerShell module, run the command below:

   `Get-Module -Name AWS.Tools.* -List | select Name,Version`



## How to configure AWS Account and Access Keys

1. Login to the IAM Console:

   https://console.aws.amazon.com/iam/

2. From the left pane, click on Users -> click on “Add user” -> specify the user name -> access type: “Programmatic access” -> do not select “AWS Management Console access” -> click “Next: Permissions”

3. From the “add user to group”, either select existing group or click on “Create group” -> click “Next: Review” -> click on “Create user”

4. Download the CSV file with the “Access key ID” and “Secret access key” and save the CSV file in a secure location

5. Click Close



## Managing Profiles

1. Login to the machine using privileged account.

2. From command prompt, run the command below to invoke PowerShell (Windows platform)

   `pwsh`

3. From command prompt, run the command below to invoke PowerShell (Linux platform)

   `pwsh`

4. Run the command below to add a new profile:

   `Set-AWSCredential -AccessKey <AWS_Access_Key> -SecretKey <AWS_Secret_Key> -StoreAs <Profile_Name>`

   Note 1: Replace **<AWS_Access_Key>** with the relevant value from the CSV file created above.

   Note 2: Replace **<AWS_Secret_Key>** with the relevant value from the CSV file created above.

   Note 3: Replace **<Profile_Name>** with your own profile name

5. By default, the credentials file is stored here:

   - On Windows: **C:\Users\username\.aws\credentials**
   - On Linux: **~/.aws/credentials**

6. List all available profiles:

   `Get-AWSCredential -ListProfileDetail`

7. Reference about configuration and credential file settings can be found at:

   https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html



## VPC related commands

+ List all available VPC’s in a specific region:

  `Get-EC2VPC -Region <Region_Name> -ProfileName <Profile_Name>`

  Note 1: Replace **<Region_Name>** with the target region, from the list below:

  https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.RegionsAndAvailabilityZones.html

  Note 2: Replace **<Profile_Name>** with your own profile name

  Example:

  `Get-EC2VPC -Region us-east-1 -ProfileName MyProfile`

+ Create a new VPC inside a specific region:

  `New-EC2VPC -CidrBlock <CIDR_Block> -Region <Region_Name> -ProfileName <Profile_Name>`

  Note 1: The above command should be written in a single line

  Note 2: Replace **<CIDR_Block>** with the IPv4 network range for the VPC, in CIDR notation. For example, 10.0.0.0/16.

  Note 3: Replace **<Region_Name>** with the target region, from the list below:

  https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.RegionsAndAvailabilityZones.html

  Note 4: Replace **<Profile_Name>** with your own profile name

  Example:

  `New-EC2VPC -CidrBlock 10.0.0.0/16 -Region us-east-1 -ProfileName MyProfile`



## Subnet related commands

+ List all available subnets inside a specific region:

  `Get-EC2Subnet -Region <Region_Name> -ProfileName <Profile_Name>`

  Note 1: Replace **<Region_Name>** with the target region, from the list below:

  https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.RegionsAndAvailabilityZones.html

  Note 2: Replace **<Profile_Name>** with your own profile name

  Example:

  `Get-EC2Subnet -Region us-east-1 -ProfileName MyProfile`

+ Get information about specific subnet:

  `Get-EC2Subnet -SubnetId <Subnet_ID> -Region <Region_Name> -ProfileName <Profile_Name>`

  Note 1: The above command should be written in a single line

  Note 2: Replace **<Subnet_ID>** with the relevant value

  Note 3: Replace **<Region_Name>** with the target region, from the list below:

  https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.RegionsAndAvailabilityZones.html

  Note 4: Replace **<Profile_Name>** with your own profile name

  Example:

  `Get-EC2Subnet -SubnetId subnet-101ad84c -Region us-east-1 -ProfileName MyProfile`



## Security Group related commands

+ List all security groups in a specific region:

  `Get-EC2SecurityGroup -Region <Region_Name> -ProfileName <Profile_Name>`

  Note 1: Replace **<Region_Name>** with the target region, from the list below:

  https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.RegionsAndAvailabilityZones.html

  Note 2: Replace **<Profile_Name>** with your own profile name

  Example:

  `Get-EC2SecurityGroup -Region us-east-1 -ProfileName MyProfile`

+ Create a new security group inside a specific VPC:

  `$groupid = New-EC2SecurityGroup -VpcId <VPC_Name> -GroupName <Security_Group_Name> -GroupDescription <Group_Description> -Region <Region_Name> -ProfileName <Profile_Name>`

  Note 1: The above command should be written a single line

  Note 2: Replace **<VPC_Name>** with the relevant value

  Note 3: Replace **<Security_Group_Name>** with a unique value (up to 255 characters), as mentioned below:

  https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html

  Note 4: Replace **<Group_Description>** with relevant value, as mentioned below:

  https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html

  Note 5: Replace **<Region_Name>** with the target region, from the list below:

  https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.RegionsAndAvailabilityZones.html

  Note 6: Replace **<Profile_Name>** with your own profile name

  Example:

  `$groupid = New-EC2SecurityGroup -VpcId "vpc-64c0c61f" -GroupName "myPSSecurityGroup" -GroupDescription "EC2-VPC from PowerShell" -Region us-east-1 -ProfileName cliuser`

+ Additional examples can be found here:

  https://4sysops.com/archives/create-and-view-ec2-security-groups-with-powershell/



## Key Pair related commands

+ Create a new key pair:

  `$myPSKeyPair = New-EC2KeyPair -KeyName <Key_Pair_Name> -Region <Region_Name> -ProfileName <Profile_Name>`

  Note 1: The above command must be written as a single line

  Note 2: Replace **<Key_Pair_Name>** with your own key pair name

  Note 3: Replace **<Region_Name>** with the target region, from the list below:

  https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.RegionsAndAvailabilityZones.html

  Note 4: Replace **<Profile_Name>** with your own profile name

  Example:

  `$myPSKeyPair = New-EC2KeyPair -KeyName MyKeyPair01 -Region us-east-1 -ProfileName MyProfile`

+ View a key pair fingerprint:

  `Get-EC2KeyPair -KeyName <Key_Pair_Name> -Region <Region_Name> -ProfileName <Profile_Name> | format-list KeyName, KeyFingerprint`

  Note 1: The above command must be written as a single line

  Note 2: Replace **<Key_Pair_Name>** with your own key pair name

  Note 3: Replace **<Region_Name>** with the target region, from the list below:

  https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.RegionsAndAvailabilityZones.html

  Note 4: Replace **<Profile_Name>** with your own profile name

  Example:

  `Get-EC2KeyPair -KeyName MyKeyPair01 -Region us-east-1 -ProfileName MyProfile | format-list KeyName, KeyFingerprint`

+ Storing a private key to a file:

  `$myPSKeyPair.KeyMaterial | Out-File -Encoding ascii <Private_Key_Name>.pem`

  Note: Replace **<Private_Key_Name>.pem** with your own value

+ Additional examples can be found here:

  https://docs.aws.amazon.com/powershell/latest/userguide/pstools-ec2-keypairs.html

  https://docs.aws.amazon.com/powershell/latest/userguide/pstools-ec2.html



## Storage related commands

+ List all S3 buckets inside a specific AWS account:

  `Get-S3Bucket -ProfileName <Profile_Name>`

  Note: Replace **<Profile_Name>** with your own profile name

+ Get S3 bucket location:

  `Get-S3BucketLocation -BucketName <S3_Bucket_Name> -ProfileName <Profile_Name>`

  Note 1: Replace **<S3_Bucket_Name>** with the relevant S3 bucket name

  Note 2: Replace **<Profile_Name>** with your own profile name

+ Create a new S3 bucket:

  `New-S3Bucket -BucketName <S3_Bucket_Name> -Region <Region_Name> -ProfileName <Profile_Name>`

  Note 1: The above command must be written as a single line

  Note 2: Replace **<S3_Bucket_Name>** with a unique value between 3-63 characters (numbers and lower-case letters)

  Note 3: Replace **<Region_Name>** with the target region, from the list below:

  https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.RegionsAndAvailabilityZones.html

  Note 4: Replace **<Profile_Name>** with your own profile name

  Example:

  `New-S3Bucket -BucketName mys3bucket001 -Region us-east-1 -ProfileName MyProfile`

+ Remove S3 bucket:

  `Remove-S3Bucket -BucketName <S3_Bucket_Name> -ProfileName <Profile_Name>`

  Note 1: Replace **<S3_Bucket_Name>** with the relevant S3 bucket name

  Note 2: Replace **<Profile_Name>** with your own profile name

  