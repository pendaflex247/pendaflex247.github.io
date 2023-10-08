---
title: AWS Ecommerce Website
author: Nomadstrides
date: 2023-06-09 20:55:00 +0800
categories: [Blogging, Demo]
tags: [ecommerce, aws, dynamodb, vpc]
pin: true
#img_path: '/assets/img/aws-s3-ecommerce-website'
---

Steps 1: Create VPC 
--
with minimum of 2 availability zones
-Select VPC and More

![Alt text](</assets/img/aws-ecommerce-website/20230625123219.png>)

![Alt text](</assets/img/aws-ecommerce-website/20230625123320.png>)

![Alt text](</assets/img/aws-ecommerce-website/20230625123605.png>)

![Alt text](</assets/img/aws-ecommerce-website/20230625123721.png>)

![Alt text](</assets/img/aws-ecommerce-website/20230625123919.png>)

This is what the out come should look like

![Alt text](</assets/img/aws-ecommerce-website/20230625124123.png>)

Steps 2: Create EC2 instance
--

![Alt text](</assets/img/aws-ecommerce-website/20230625124310.png>)

![Alt text](</assets/img/aws-ecommerce-website/20230625124445.png>)

##### Create key pair

![Alt text](</assets/img/aws-ecommerce-website/20230625124814.png>)

![Alt text](</assets/img/aws-ecommerce-website/20230625124951.png>)

##### Edith network settings

![Alt text](</assets/img/aws-ecommerce-website/20230625125042.png>)

-Select the ecom vpc
-enable auto assign public IP

![Alt text](</assets/img/aws-ecommerce-website/20230625125615.png>)

-Still under network add security group rules
HTTP & HTTPS

![Alt text](</assets/img/aws-ecommerce-website/20230625125921.png>)

![Alt text](</assets/img/aws-ecommerce-website/20230625130824.png>)

![Alt text](</assets/img/aws-ecommerce-website/20230625130900.png>)

![Alt text](</assets/img/aws-ecommerce-website/20230625130918.png>)

##### Connect to the EC2 Instance

![Alt text](</assets/img/aws-ecommerce-website/20230625131056.png>)

-Update apt cache
![Alt text](</assets/img/aws-ecommerce-website/20230625133330.png>)

Steps 3: Create MSQL RDS
--

![Alt text](</assets/img/aws-ecommerce-website/20230625133547.png>)

![Alt text](</assets/img/aws-ecommerce-website/20230625133807.png>)

![Alt text](</assets/img/aws-ecommerce-website/20230625133935.png>)

```
	database:

	name: ecom-database
	Username; admin123
	password: mypassword
```

![Alt text](</assets/img/aws-ecommerce-website/20230625134150.png>)

Select the VPC

![Alt text](</assets/img/aws-ecommerce-website/20230625205901.png>)

Note: Make sure the sure the ec2 instance  is on public subnet

![Alt text](</assets/img/aws-ecommerce-website/20230625205339.png>)

![Alt text](</assets/img/aws-ecommerce-website/20230625205526.png>)

To connect to the RDS subnet this is neccessary 
- Go to your EC2 instance 
	- click on Actions > Networking > connect RDS Database

![Alt text](</assets/img/aws-ecommerce-website/20230625205655.png>)

![Alt text](</assets/img/aws-ecommerce-website/20230625205755.png>)	

Steps 4: Setup EC2 (DB Server )
--

-Update cache
-Install lamp server

```bash
sudo apt update
sudo apt install lamp-server^
```

![Alt text](</assets/img/aws-ecommerce-website/20230625175040.png>)

-Install MSQL Client

```bash
sudo apt install mysql-client
```
![Alt text](</assets/img/aws-ecommerce-website/20230625175330.png>)	
	
-Verified that apache is installed

```bash
cd /var/www/html
```

![Alt text](</assets/img/aws-ecommerce-website/20230625175515.png>)	

##### Clone the ecom site from GitHub

```bash
sudo git clone https://github.com/Jhoode/Electronix-Website.git
```

![Alt text](</assets/img/aws-ecommerce-website/20230625195802.png>)		

Note: connection.inc .php is going to connect the ec2 and the rds

```bash
cd cd Electronix-Website/ecom/
```
![Alt text](</assets/img/aws-ecommerce-website/20230625200428.png>)	 
	 
##### Connect to the database
	
```bash
mysql -h ecom-database.cslgmy1hb3xc.us-east-2.rds.amazonaws.com -u admin123 -p
```

Note: if this does not connect you directly to the data base you need to go to the EC2 security group and add the mysql ports 3306 from anywhere.

![Alt text](</assets/img/aws-ecommerce-website/20230625200726.png>)	

Note: if you don't see the database (ecom) just create a new

```bash
create database ecom
```

![Alt text](</assets/img/aws-ecommerce-website/20230625200502.png>)				
				
```bash
use ecom
show tables;
```

![Alt text](</assets/img/aws-ecommerce-website/20230625202516.png>)
			
##### Create tables 
copy the ecom.sql from the git repo

![Alt text](</assets/img/aws-ecommerce-website/20230625201106.png>)

Copy everything from line 30 to 353

![Alt text](</assets/img/aws-ecommerce-website/20230625201217.png>)	

Paste in the ecom database 

![Alt text](</assets/img/aws-ecommerce-website/20230625201441.png>)		

```bash
show tables;
```
![Alt text](</assets/img/aws-ecommerce-website/20230625202711.png>)	
	
Exit the database

![Alt text](</assets/img/aws-ecommerce-website/20230625203837.png>)

Update the 

- Database endpoint
- Database username
- Database password
- EC2 instance name
- EC2 public IP


![Alt text](</assets/img/aws-ecommerce-website/20230625203707.png>)

![Alt text](</assets/img/aws-ecommerce-website/20230625204436.png>)

Go to the Url: 

```bash
3.145.170.165/Electronix-Website/ecom/
```
![Alt text](</assets/img/aws-ecommerce-website/20230625204547.png>)	

![Alt text](</assets/img/aws-ecommerce-website/20230625210743.png>)

Note: go into the appache to point the server to the index.apache