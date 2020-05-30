# Best practices for AWS cost optimization

## Unused EBS Volumes

1.  Login to the AWS Management Console

   https://console.aws.amazon.com/ec2/

2. In the navigation panel, under Elastic Block Store, click Volumes.

3. To identify any unattached EBS volumes, check their status under State column. If the status is available, the volume is not attached to an EC2 instance and can be safely deleted.

4. Select the unused EBS volume -> click on Actions -> Delete volume

5. Change the AWS region from the navigation bar and repeat the process for the other regions.

6. Log off the AWS Management console.



## Low Utilization Amazon EC2 Instances

1. Sign in to the AWS Management Console

   https://console.aws.amazon.com/ec2/

2. In the left navigation panel, under INSTANCES section, choose Instances.

3. Select the EC2 instance that you want to examine.

4. Select the Monitoring tab from the dashboard bottom panel.

5. Within the CloudWatch metrics section, perform the following actions:

   + Click on the CPU Utilization (Percent) usage graph thumbnail to open the instance CPU usage details box. Inside the CloudWatch Monitoring Details dialog box, set the following parameters:
     + From the Statistic dropdown list, select Average.
     + From the Time Range list, select Last 1 Week.
     + From the Period dropdown list, select 1 Hour.
   + Once the monitoring data is loaded, verify the instance CPU usage for the last 7 days. If the average usage (percent) has been less than 2%, e.g. , the selected EC2 instance qualifies as candidate for an idle instance. Click Close to return to the dashboard.

6. Decide if you wish to terminate (delete) the EC2 instance.
7. To terminate the EC2 instance -> from Actions -> select Instance State -> select Terminate -> choose Yes, Terminate
8. Change the AWS region from the navigation bar and repeat the process for the other regions.
9. Log off the AWS Management console.

   

##  Unassociated Elastic IP Addresses

1. Sign in to the AWS Management Console

   https://console.aws.amazon.com/vpc/

2. In the left navigation panel, under Virtual Private Cloud section, choose Elastic IPs.

3. Select Unassociated from the Filter dropdown menu to filter all the available EIPs and return the unattached ones. The filtering process should return the Elastic IPs that are not currently associated with any running EC2 instances or Elastic Network Interfaces (ENIs). The unattached EIPs returned at this step can be safely released (see Remediation/Resolution section).

4. Change the AWS region from the navigation bar and repeat the process for the other regions.

5. Log off the AWS Management console.



## Underutilized Amazon Redshift Clusters

1. Login to the AWS Management Console

   https://console.aws.amazon.com/redshift/

2. In the left navigation panel, under Redshift Dashboard, click Clusters.

3. Choose the Redshift cluster that you want to examine then click on its identifier link listed in the Cluster column.

4. On the cluster settings page, select the Performance tab to access the monitoring panel.

5. On the monitoring panel displayed for the selected cluster, perform the following actions:

   + To verify the Redshift cluster Database Connections usage graph, follow the steps below:
     + From the Time Range dropdown list, select Last 1 Week.
     + From the Period list, select 1 Hour.
     + From the Statistic dropdown list, select Average.
     + And from the Metrics dropdown list, select DatabaseConnections.
   + Once the monitoring data is loaded into the Database Connections usage graph, check the number of database connections for the last 7 days. If the average usage (count) has been less than 1, e.g., the selected Redshift cluster qualifies as candidate for the idle cluster.

6. If you no longer need your RedShift cluster, you can delete it.

7. On the navigation menu, choose CLUSTERS.

8. Choose the cluster to delete.

9. For Actions, choose Delete. The Delete cluster page appears.

10. Choose Delete cluster.

11. Change the AWS region from the navigation bar and repeat the process for the other regions.

12. Log off the AWS Management console.

    