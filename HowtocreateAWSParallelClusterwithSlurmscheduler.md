# How to create AWS ParallelCluster with Slurm scheduler

## Environment preparation phase within AWS Management Console

1. Login the AWS IAM console:

   https://console.aws.amazon.com/iam/

2. From the left pane, click on Policies -> click on Users -> Add User -> specify the name **parallelcluster-user** -> Access type: Programmatic access -> click Next: Permissions -> Set permissions -> select a group with **“AdministratorAccess”** role -> click Next: Tags -> click Next: Review -> click on Create user -> click on Download .csv and keep it in a secured location -> click on Close

3. Follow the instructions below to create a key pair to access the cluster machines:

   https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html#having-ec2-create-your-key-pair

4. Follow the instructions below to create S3 bucket (with unique name) for storing data to export and import data to/from the FSx Lustre storage:

   https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html

   Note 1: Document the S3 bucket name for use inside the ParallelCluster config file

   Note 2: Create a folder called export (in small letters), inside the S3 bucket

5. In-case you wish to create a dedicate VPC and subnet for the HPC cluster, follow the instructions below:

   https://docs.aws.amazon.com/directoryservice/latest/admin-guide/gsg_create_vpc.html

6. Logoff the AWS console



## Python installation phase on Linux (Debian / Ubuntu)

1. Login to a Linux machine using SSH, and follow the instructions below to install Python 3:

   https://docs.aws.amazon.com/cli/latest/userguide/install-linux-python.html

   Note: In-case you already have Python 3 install, use the command below to upgrade to the latest build:

   `sudo apt-get upgrade python3`

2. To install pip3, run the command below:

   `sudo apt install python3-pip`



## Python installation phase on CentOS 7

1. Login to the CentOS machine using SSH, and follow the instructions below to install Python3 and Python3-PIP:

   https://www.rosehosting.com/blog/how-to-install-python-3-6-4-on-centos-7/



## Python installation phase on Windows

1. Login to a Windows machine using privileged account, and follow the instructions below to install Python 3 and PIP:

   https://docs.aws.amazon.com/cli/latest/userguide/install-windows.html



## AWS ParallelCluster installation phase

1. Run the commands below to install the AWS ParallelCluster:

   + Linux:

     `sudo pip install aws-parallelcluster`

   + Windows:

     `pip install aws-parallelcluster`

2. Run the command below to verify the installed version:

   `pcluster version`

3. Follow the instructions below to install the AWS CLI:

   https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html

4. Run the command below in-order to configure AWS CLI:

   `aws configure`

   + **AWS Access Key ID** – Specify the value from the CSV of the previously created IAM user **parallelcluster-user**

   + **AWS Secret Access Key** – Specify the value from the CSV of the previously created IAM user **parallelcluster-user**

   + **Default region name** – specify a region such as **eu-west-1**

     Full list: https://docs.aws.amazon.com/general/latest/gr/rande.html

   + Default output format: JSON

5. Run the command below to setup the initial configuration:

   `pcluster configure`

   + **Cluster Template**: Specify here a custom name for the HPC template (such as HPC Cluster)

   + **AWS Region ID**: Specify the same region you specified for the aws configure command (such as **eu-west-1**)

   + **VPC Name**: Specify the same name as the Cluster Template (such as **HPC Cluster**)

   + **Key Name**: Specify the name of the EC2 Key pair previously created

   + **VPC ID**: Specify the name of the target VPC ID to deploy the HPC cluster into

     Note: The full list of VPC’s can be found within the AWS management console:

     https://console.aws.amazon.com/vpc

   + **Master Subnet ID**: Specify here the name of the target subnet ID to deploy the HPC cluster into

     Note: The full list of subnets can be found within the AWS management console:

     https://console.aws.amazon.com/vpc

6. Edit the ParallelCluster **config** file:

   + Linux: The file is located inside **~/.parallelcluster/config**
   + Windows: The file is located inside **%UserProfile%\.parallelcluster\config**

7. Add the following parameters to the **[cluster]** section (for a large cluster):

   **base_os = centos7**

   **master_instance_type = c5n.xlarge**

   **compute_instance_type = c5n.18xlarge**

   **cluster_type = ondemand**

   **initial_queue_size = 2**

   **scheduler = slurm**

   **placement_group = DYNAMIC**

   **enable_efa = compute**

   **fsx_settings = fs**

   Note: For small cluster, add the following parameters to the **[cluster]** section:

   **base_os = centos7**

   **master_instance_type = m4.large**

   **compute_instance_type = m4.large**

   **cluster_type = ondemand**

   **initial_queue_size = 2**

   **max_queue_size = 3**

   **scheduler = slurm**

   **placement_group = DYNAMIC**

   **fsx_settings = fs**

8. Add the following entire section to the **config** file:

   **[fsx fs]**

   **shared_dir = /fsx**

   **storage_capacity = 3600**

   **imported_file_chunk_size = 1024**

   **export_path = s3://bucket/export**

   **import_path = s3://bucket**

   **weekly_maintenance_start_time = 1:00:00**

   Note 1: The storage_capacity is the size of the FSx Lustre storage in GB

   Note 2: Replace the value of bucket with the previously S3 bucket name

9. Run the command below to deploy the new cluster:

   `pcluster create mycluster`

   Note: Replace mycluster with your target HPC cluster name (without spaces)

10. Go to the CloudFormation console to view the deployment status:

    https://eu-west-1.console.aws.amazon.com/cloudformation/home

11. Wait for the cluster deployment to complete.

12. Document the **MasterPublicIP** and **ClusterUser**.



## Increase EC2 service limit

In-case you need to increase the EC2 service limit (for example number of EC2 instances from a specific instance type), follow the instructions below:

https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-resource-limits.html#request-increase



## Connecting to the HPC cluster (from Linux Machine)

1. Run the command below to connect using SSH to the master server:

   `pcluster ssh mycluster -i /path/to/keyfile.pem`

   Note 1: Replace **mycluster** with the previously create cluster name

   Note 2: Replace **/path/to/keyfile.pem** with the actual path and key file name

2. Run the command below to verify the state of the cluster:

   `sinfo`



## Connecting to the HPC cluster (from Windows Machine)

1. Download **puttygen.exe** from:

   http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html

2. Run the **puttygen.exe**

3. Click on “Load” -> change the file extension from “Putty Private key files” to “All Files” -> locate the private key pair and click on Open -> click on OK -> click on “Save private key” -> click on “Yes” -> save the private key file with PPK extension -> close **puttygen.exe**

4. Download **Putty** from:

   https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html

5. Run **putty.exe**

6. From the left pane, under “Connection” -> expand SSH -> click on “Auth” -> from the main pane, under “Authentication parameters”, click on “Browse” -> locate the SSH private key generated by **puttygen.exe**

7. From the left pane, click on “Session” -> from the main pane, under “Host Name (or IP address)” specify the following:

   `user@IP_Address`

   Note 1: Replace **user** with the previously documented **ClusterUser** value

   Note 2: Replace **IP_Address** with the previously documented **MasterPublicIP**

8. Under “Saved Sessions”, specify a name for this newly created connection.

9. Click on Save

10. Click on Open

11. Run the command below to verify the state of the cluster:

    `sinfo`



## Common actions to control the cluster

+ Displays a list of stacks that are associated with AWS ParallelCluster:

  `pcluster list`

+ Displays a list of all instances in a cluster:

  `pcluster instances mycluster`

  Note: Replace **mycluster** with the previously create cluster name

+ View the current status of the cluster:

  `pcluster status mycluster`

  Note: Replace **mycluster** with the previously create cluster name

+ Updates a running cluster by using the values in the configuration file:

  `pcluster update mycluster -c ~/.parallelcluster/config`

  Note 1: Replace **mycluster** with the previously create cluster name

  Note 2: Replace **~/.parallelcluster/config** with the target config file location

+ Stops the compute fleet, leaving the master node running:

  `pcluster stop mycluster`

  Note: Replace **mycluster** with the previously create cluster name

+ Starts the compute fleet for a cluster that has been stopped:

  `pcluster start mycluster`

  Note: Replace **mycluster** with the previously create cluster name



## Delete AWS ParallelCluster

1. In-case you wish to keep the AWS ParallelCluster master node static IP, login to the AWS console:

   https://console.aws.amazon.com/ec2/

2. From the left pane, click on Elastic IPs -> select the public IP of the master node -> Actions -> Disassociate address

3. From command prompt (the same machine you used the pcluster commands), run the command below to delete the cluster:

   `pcluster delete mycluster`

   Note: Replace **mycluster** with the previously create cluster name



## Important notes regarding shared storage

+ Long term data must be stored inside S3 bucket
+ The Amazon FSx for Lustre storage (mount **/fsx**) will be used for the duration of the compute job



## References

+ Getting started with AWS ParallelCluster:

  https://aws-parallelcluster.readthedocs.io/en/latest/getting_started.html#

+ Setting Up AWS ParallelCluster

  https://docs.aws.amazon.com/parallelcluster/latest/ug/getting_started.html

+ Install AWS ParallelCluster in a Virtual Environment

  https://docs.aws.amazon.com/parallelcluster/latest/ug/install-virtualenv.html

+ A Scientist's Guide to Cloud-HPC: Example with AWS ParallelCluster, Slurm, Spack, and WRF

  https://jiaweizhuang.github.io/blog/aws-hpc-guide/

+ Launch your first sample HPC environment on AWS and review important concepts along the way

  https://aws.amazon.com/getting-started/use-cases/hpc/

+ AWS ParallelCluster Wiki:

  https://github.com/aws/aws-parallelcluster

+ Deploying an Elastic HPC Cluster

  https://d1.awsstatic.com/Projects/P4114756/deploy-elastic-hpc-cluster_project.pdf

+ Scale HPC Workloads with Elastic Fabric Adapter and AWS ParallelCluster

  https://idk.dev/scale-hpc-workloads-with-elastic-fabric-adapter-and-aws-parallelcluster/

+ Best Practices for Running Ansys Fluent Using AWS ParallelCluster

  https://aws.amazon.com/blogs/opensource/best-practices-running-ansys-fluent-aws-parallelcluster/

+ AWS ParallelCluster with AWS Directory Services Authentication

  https://aws.amazon.com/blogs/opensource/aws-parallelcluster-aws-directory-services-authentication/

+ Adding support for FSx for Lustre:

  https://aws-parallelcluster.readthedocs.io/en/develop/configuration.html#fsx-section

+ Getting Started with Amazon FSx for Lustre

  https://docs.aws.amazon.com/fsx/latest/LustreGuide/getting-started.html

+ Amazon FSx for Lustre Lustre User Guide

  https://docs.aws.amazon.com/fsx/latest/LustreGuide/LustreGuide.pdf