# aws-efs-multi-az
**How to access the Elastic Filesystems between multiple EC2 instances**

**Step -1: To create a security group for EFS**

Navigate to EC2 console - Security group and create a new security group

I just added outbound rule only. we are good to go.

![image](https://github.com/kohlidevops/aws-efs-multi-az/assets/100069489/7b593645-3080-42f5-8878-d0ead73b29aa)

**Step -2: To create a Elastic Filesystem**

Navigate to EFS console and create a new filesystem - select - customize option to explore lot

![image](https://github.com/kohlidevops/aws-efs-multi-az/assets/100069489/e8cedf78-aeb6-4256-a329-8093eea162b6)

By default, automatic backup has been enabled. If you are using in production then go with automatic backup.

Transition into IA - 30 days since last access - The data's are moving from standard to Infrequent Acces (IA) which are not accessed by last 30 days.

Transition out of IA - On first access - The data's are back to Standard when first access

You can use own KMS to encrypt the data. Otherwise AWS will encrypt the data using aws managed keys.

![image](https://github.com/kohlidevops/aws-efs-multi-az/assets/100069489/f23fab65-266c-4cd6-98b2-e2ac9aa06f8a)

Select your VPC, public subnets in all availability zone and security group for EFS (which is created in step-1)

I don't use any policy as of now.

![image](https://github.com/kohlidevops/aws-efs-multi-az/assets/100069489/48ab6155-3d8b-4f3f-b15f-520d2c3332d9)

Then review and create a file system

**Step -3: Create a Security group for EC2 instances**

I allowed SSH with open to world. Because I'm going to access the EC2 instance through EC2 Instance connect.

![image](https://github.com/kohlidevops/aws-efs-multi-az/assets/100069489/7413f9d7-9acf-42fb-893a-0cc70636538c)

**Step -4: Launch EC2 instance A**

Navigate to EC2 console - Launch instance with Amazon Linux2

![image](https://github.com/kohlidevops/aws-efs-multi-az/assets/100069489/f7226b9f-741f-4695-98f5-2497bd673d57)

Select your VPC, Public subnet A, and security group (which is created in step-3)

![image](https://github.com/kohlidevops/aws-efs-multi-az/assets/100069489/f1e960d5-cf82-41a8-8696-3e74c16b4b7e)

To add a shared filesystem using EFS

![image](https://github.com/kohlidevops/aws-efs-multi-az/assets/100069489/fe0f6a26-013e-465a-a74e-b14281b0911f)
![image](https://github.com/kohlidevops/aws-efs-multi-az/assets/100069489/9fb6209b-25e8-4006-bec3-1eb1ba6077a5)

This option will automatically assign inbound rules in EFS security group and mount the files system in target EC2 instance.

Then review and launch Instance A.

**Step -5: Launch EC2 Instance B**

Launch one more EC2 Instance as Instance B with same VPC, select Public subnet B, will configure same things as we did in Instance A.

So we have 2 EC2 Amazon Linux instance as of now.

![image](https://github.com/kohlidevops/aws-efs-multi-az/assets/100069489/654cda3e-df2a-409e-a9cd-9e65b550b2f1)

**Step -6: Connect EC2 Instance through EC2 Instance Connect**

Select your EC2 instance - connect - EC2 Instance Connect - to access the EC2 machine in browser mode.

![image](https://github.com/kohlidevops/aws-efs-multi-az/assets/100069489/076e4c42-91f3-470e-8c4e-ebee2cccb305)

I just connected to Instance A, and i can able to see the mount point which is automatically created. Then I created one file inside the mount point in Instance A

![image](https://github.com/kohlidevops/aws-efs-multi-az/assets/100069489/b1643ea9-58bb-4bb0-9712-ce1e343be7e1)

Now I will connect to Instance B and i can able to see the file (which is created in Instance A) and insert some data in mounted file from Instance B.

![image](https://github.com/kohlidevops/aws-efs-multi-az/assets/100069489/2addf7c5-ad64-45a3-8b1c-a8175f2110fc)

Go back to Instance A and check the data.

![image](https://github.com/kohlidevops/aws-efs-multi-az/assets/100069489/db706d47-667f-4c92-9fdb-4220e2bfe4be)

So EFS file systems allow us to share the files between multiple EC2 instances in multiple AZ's.

**Step -7: Clean up the Resources**

Remove the EC2 Instances
Remove the Elastic Filesystem
Remove the Security group















