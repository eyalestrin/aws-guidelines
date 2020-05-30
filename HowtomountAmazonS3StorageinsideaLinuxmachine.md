# How to mount Amazon S3 Storage inside a Linux machine

## Creating IAM user

1. Login to the IAM console:

   https://console.aws.amazon.com/iam/

2. From the left pane, click on Users -> click on “Add user” -> specify the user name -> access type: “Programmatic access” -> do not select “AWS Management Console access” -> click “Next: Permissions”
3. From the “add user to group”, click on “Create group” -> from the policy list, select “AmazonS3FullAccess” -> click “Next: Review” -> click on “Create user”
4. Download the CSV file with the “Access key ID” and “Secret access key” and save the CSV file in a secure location
5. Click Close



## S3FS installation

1. Login using SSH to the target Linux machine using privileged account.

2. Follow the instructions below to install the S3FS Fuse adapter:

   https://github.com/s3fs-fuse/s3fs-fuse/wiki/Installation-Notes

3. Run the command `sudo vi /etc/passwd-s3fs` to create config file with the following content:

   `bucketName:accessKeyId:secretAccessKey`

   Note 1: Replace **bucketName**, with the target bucket name

   Note 2: Replace **accessKeyId**, with the relevant value from the credentials created on the IAM console

   Note 3: Replace **secretAccessKey**, with the relevant value from the credentials created on the IAM console

4. Change the permissions of the passwd-s3fs file (Amazon Linux):

   `sudo chown ec2-user:ec2-user /etc/passwd-s3fs`

   `sudo chmod 600 /etc/passwd-s3fs`

5. Change the permissions of the passwd-s3fs file (Ubuntu):

   `sudo chown ubuntu:ubuntu /etc/passwd-s3fs`

   `sudo chmod 600 /etc/passwd-s3fs`



## Mount phase

1. Run the commands below to mount the Amazon S3 storage (Amazon Linux):

   `sudo mkdir -p /dev/cloudstorage`

   `sudo chown -R ec2-user:ec2-user /dev/cloudstorage/`

   `sudo chmod 777 /dev/cloudstorage`

   `/usr/bin/s3fs MyBucket /dev/cloudstorage -o passwd_file=/etc/passwd-s3fs`

   Note: Replace **MyBucket**, with the target bucket name

2. Run the commands below to mount the Amazon S3 storage (Ubuntu):

   `sudo mkdir -p /dev/cloudstorage`
   `sudo chown -R ubuntu:ubuntu /dev/cloudstorage/`
   `sudo chmod 777 /dev/cloudstorage`
   `/usr/bin/s3fs MyBucket /dev/cloudstorage -o passwd_file=/etc/passwd-s3fs`

   Note: Replace **MyBucket**, with the target bucket name

3. To view the content of the Amazon S3 bucket, switch to the new mount point:

   `cd /dev/cloudstorage`



## Unmount phase

1. When completing the work on the bucket, from the Linux SSH console, and run the commands below to unmount the Amazon S3 Storage:

   `cd ~`
   `sudo fusermount -u /dev/cloudstorage`

2. Logoff the Linux machine



## Additional references regarding permanent mount using FSTAB

+ https://github.com/s3fs-fuse/s3fs-fuse/wiki/Fuse-Over-Amazon
+ https://github.com/s3fs-fuse/s3fs-fuse
+ https://www.systutorials.com/docs/linux/man/1-s3fs/