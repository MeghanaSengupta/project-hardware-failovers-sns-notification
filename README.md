# project-withstand-hardware-failovers-and-get-notified-using-sns

sign in to console and select VPC service

Click on Create VPC only

Give the VPC name project-withstand-hardware-failovers-and-get-notified-using-sns 

IPv4 CIDR 10.0.0.0/16 

![image](https://user-images.githubusercontent.com/109040029/199369049-122908be-096c-4cda-886b-7b7e86a4f71e.png)

Create Public and 2 Private Subnets

Select the VPC created in previous step, give Public-Subnet-withstand-hardware-failovers-and-get-notified-using-sns, Avaibility Zone = 2a, CIDR block = 10.0.1.0/24

Private-Subnet-1-withstand-hardware-failovers-and-get-notified-using-sns, Avaibility Zone = 2a, CIDR block = 10.0.2.0/24

Private-Subnet-2-withstand-hardware-failovers-and-get-notified-using-sns, Avaibility Zone = 2b, CIDR block = 10.0.3.0/24

![image](https://user-images.githubusercontent.com/109040029/199369895-af2c8531-8cb2-422a-ace5-d4c73e3b3858.png)

![image](https://user-images.githubusercontent.com/109040029/199370036-37ff5ab0-5a41-4f11-98f3-5e63762c404d.png)

Now create Subnets

![image](https://user-images.githubusercontent.com/109040029/199370189-f9dde7a8-6896-4891-a59e-139612214081.png)

Click on Internet Gateways from the left menu and click Create internet gateway

Name tag : Enter IGW--withstand-hardware-failovers-and-get-notified-using-sns

![image](https://user-images.githubusercontent.com/109040029/199370675-02de18d6-f493-400f-8b87-1d1e2ab66189.png)

Select the Internet gateway you created from the list

Click on Actions

Click on Attach to VPC button

![image](https://user-images.githubusercontent.com/109040029/199370906-0ac39f30-369f-4f08-83ad-5851cd3b2715.png)

![image](https://user-images.githubusercontent.com/109040029/199370968-d2b84658-4558-43fe-bfed-207ffd285a8f.png)

Select the VPC created in the first step and attach it here.

![image](https://user-images.githubusercontent.com/109040029/199371104-34264f45-f74b-4e83-acb6-d0c1400b53e5.png)

Create a Public and Private Route Table

Go to Route Tables from the left menu and click on 

Name Tag: Enter PublicRT

VPC: Select the VPC created from the list.

Click on Create route table

![image](https://user-images.githubusercontent.com/109040029/199371761-1a7cebf1-e7f0-4dbf-a98f-acf2de9aba95.png)

Select PublicRT.

Go to the Routes tab, click on the Edit routes button and on the next page, click on Add route.

![image](https://user-images.githubusercontent.com/109040029/199371891-fd051486-4d3f-4252-8613-b8243e80d2dd.png)

![image](https://user-images.githubusercontent.com/109040029/199372095-2c466082-ed25-4fac-ac33-25b78a91c177.png)

![image](https://user-images.githubusercontent.com/109040029/199372199-c13daa0a-2f94-42b0-add9-df6bcc4038fa.png)

Now in PublicRT , select the Subnet Associations tab and Edit Subnet Associations

![image](https://user-images.githubusercontent.com/109040029/199372382-3911e2b4-877d-472a-a8b2-27be0edcaf8c.png)

![image](https://user-images.githubusercontent.com/109040029/199372725-e4411821-d073-4bd4-acb2-c455a3fb907f.png)

now select Public-Subnet and save associations

![image](https://user-images.githubusercontent.com/109040029/199372828-dfc6e54e-2e7e-42da-bc18-9748b3de7bb8.png)

Now create Private Route Tables

Click on Create Route tables and give name Tag PrivateRT

![image](https://user-images.githubusercontent.com/109040029/199373129-9f670004-df71-402b-a013-fb024393eeac.png)

Now associate both private subnet to this PrivateRT. Go to PrivateRT and select Subnet Associations. 

![image](https://user-images.githubusercontent.com/109040029/199373229-cd670bb8-16b3-44cd-9141-6d5bcba3b2bf.png)

Now click on Edit Subnet Associations

![image](https://user-images.githubusercontent.com/109040029/199373506-989de370-82f7-4538-b7dc-87e9b8b7a4fc.png)

and save associations

Create Security group for EC2. Select the VPC we've created and give inbound rules SSH port 22

![image](https://user-images.githubusercontent.com/109040029/199374343-4ac7cc55-c389-4ed7-b007-4a9ec6299b06.png)

click on create Security Group

Create Security group for RDS. Select the VPC we've created and give inbound rules MySQL/Aurora port 3306 & allow from EC2-SG

![image](https://user-images.githubusercontent.com/109040029/204158952-7a5d7b7b-42d6-4e93-a7ca-bb521077d691.png)

click on create security group

now select RDS services and go to subnet groups and create subnet groups

Give name RDS-SNS, Select VPC we've created, Select Availability Zone 2a and 2b, and select both the private subnets we have created

![image](https://user-images.githubusercontent.com/109040029/199375307-2feb9819-cc80-4ef6-8aa7-a4923c54ae9b.png)

click on create

Now go to RDS Dashboard and click on DB Instances. Then click on create database

Select Standard create, MySQL, Free Tier templates, 

![image](https://user-images.githubusercontent.com/109040029/199377772-ddfdfdc9-70d0-4ef8-9132-ec8487e2b559.png)

DB instance identifier: Enter RDS-EC2-SNS

Master username: Enter admin

Master password: Enter admin123

Confirm password: Enter admin123

select Burstable classes and select db.t3.micro

Allocated storage 20 GiB.

Uncheck Enable storage autoscaling option

![image](https://user-images.githubusercontent.com/109040029/199377912-6a0a791a-1b93-4c3c-bee9-7fc289b1746c.png)

In Connectivity, select the VPC we've created, No Public access and select rds-sns DB subnet group,choose exiting security group = RDS-SG and remove default

![image](https://user-images.githubusercontent.com/109040029/199378060-9e1de09a-272c-49b1-b7a0-73c58cb0f7df.png)

Now click on create database and keep rest of the things default.

Now select EC2 service and click on launch instance

AMI = Linux, free tier eligible, instance type = t2.micro

create a new key pair Key pair name = RDS-EC2-SNS 

Select RSA and .ppk

![image](https://user-images.githubusercontent.com/109040029/199379490-4fb7517a-2858-4ca3-aa51-6548624217a0.png)

Netowrk setting = select the VPC we have created. Subnet = public subnet, enable auto assign public IP, select existing securtiy group, we created, EC2-SG

Now go to advanced details and in IAM instance profile, create new IAM role

![image](https://user-images.githubusercontent.com/109040029/199380093-d3d3e225-c87d-4056-be5d-ea732b57ee1a.png)

Click on Create Policy and  select JSON tab and write below mentioned script (it will give full cloudwatch access)

{

    "Version": "2012-10-17",
    
    "Statement": [
    
        {
        
            "Sid": "CloudWatchAccess",
            
            "Action": "cloudwatch:*",
            
            "Effect": "Allow",
            
            "Resource": "*"
            
        }
        
    ]
    
}

go to next and if you wish give tag or else select Review

now give name  CloudWatchRole-EC2-RDS-SNS and click on Create policy

![image](https://user-images.githubusercontent.com/109040029/199382271-af9d1fe4-1c39-4fde-95c9-25ca9943c462.png)

attach this policy to the role

![image](https://user-images.githubusercontent.com/109040029/199627366-b1a879fd-1aa3-4185-b6d4-c6ea4a059e51.png)

![image](https://user-images.githubusercontent.com/109040029/199627416-c218df58-5d87-4a90-b03f-ca03d20a86b7.png)

![image](https://user-images.githubusercontent.com/109040029/199627497-b6f88e82-f624-4e66-a636-5b66b25d2565.png)

Now click on Launch instance 

Select CloudWatch service. Select Alarms, then All alarms and click on create Alarm

![image](https://user-images.githubusercontent.com/109040029/199628491-8d9a81a7-6745-418c-a50b-05710e97b978.png)

![image](https://user-images.githubusercontent.com/109040029/199628877-90c72bd6-c8af-41ec-bb24-ae47562cc90e.png)

click on select metrics

click on EC2

![image](https://user-images.githubusercontent.com/109040029/199628945-d8bc00c4-2015-438f-97cd-b45e36e00549.png)

click on Per-instance Metrics

![image](https://user-images.githubusercontent.com/109040029/199629007-7bb01a0c-be9c-48e8-b2f1-5e03018d27ca.png)

select StatusCheckFailed_System

![image](https://user-images.githubusercontent.com/109040029/199629162-9694bb60-418c-4e4e-a979-99d165de4ac3.png)

Click on select Metrics and it will lead you to  below mentioned screen 

Make couple of changes, In Metric: period = 1 minute and In Condition, Greater/Equal and select next 

![image](https://user-images.githubusercontent.com/109040029/199629428-b8573612-9268-4110-a314-b61c7a36e3e2.png)

select Create new topic and give name EC2-RDS-SNS-failure and enter the email address you want to receive the notification and click on create topic

Now click on EC2 action, and select Recover this instance

![image](https://user-images.githubusercontent.com/109040029/199632420-dbe77c8e-c9db-4d9d-8a53-88098034e0bf.png)

Now click next

give Alarm name and click next

![image](https://user-images.githubusercontent.com/109040029/199632446-b36b4bed-a85f-4d4c-a39b-2323e2bf35eb.png)


![image](https://user-images.githubusercontent.com/109040029/199632725-bf51f119-b970-4dcc-87e7-73a152334ce2.png)

Go to email and confirm your subscription to receive the notification and it will lead to 

![image](https://user-images.githubusercontent.com/109040029/199633180-d634e3b0-a61f-4e1e-88e0-6181c2812fec.png)

Now SSH EC2 instance

![image](https://user-images.githubusercontent.com/109040029/200683126-4215544d-83fb-4955-add2-5cb41d02d83a.png)

give below command

aws configure

And give Access Key and Secret Key. Give region = eu-west-2 & output format = JSON  

Now give below command

aws cloudwatch set-alarm-state --alarm-name "EC2-RDS-SNS-failover" --state-value ALARM --state-reason "Simulate Hardware Failure"






