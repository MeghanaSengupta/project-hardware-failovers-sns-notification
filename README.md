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

Create Security group for RDS. Select the VPC we've created and give inbound rules MySQL/Aurora port 3306

![image](https://user-images.githubusercontent.com/109040029/199374483-d1e624b0-b698-4da7-a4c2-61e72f4c6e80.png)

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


