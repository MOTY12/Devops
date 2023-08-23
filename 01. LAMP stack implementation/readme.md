## WEB STACK IMPLEMENTATION (LAMP STACK) IN AWS
<p>In this article, you will learn how to implement LAMP stack on aws</p>

## Overview
LAMP is an acronym of four software technologies that developers used for building website and web application. It is denoted from <b>LINUX</b> an operating system, <b>Apache</b> a web server, <b>mySQL</b> a database server, and <b>PHP, python, perl</b> a server-side programming language. LAMP is a stack 

AWS means Amazon Web Services (AWS) is a secure cloud services platform, offering compute power, database storage and it helps Running  web and application servers in the cloud to host dynamic websites. I'll be using a cloud server called <b>Ec2 server </b>  which will enables us to use a virtual server and it offers a free tier account that we are going to leverage on for this projects.

EC2 means elastic compute cloud. it is a virtual server that provides secure, resizable compute capacity in the cloud.

Apache server

Security group acts as a firewall that controls the traffic allowed to and from the resources in your virtual private cloud (VPC). 

dbms

## Prerequisites
In order to proceed, I believe the reader have a AWS account because you will need an AWS account and a virtual server with Ubuntu Server OS else <a href="https://portal.aws.amazon.com/billing/signup">click here</a> to create an account. Stay calm and Let makes our hands dirty.

## Create a Ec2 instance
Let create a virtual server and install a ubuntu OS on it. To create an instance below is the process.

Firstly, we log on to our account, create an ec2 instance, install ubuntu OS and save your key pair(.pem) securely. <a href="https://www.techtarget.com/searchcloudcomputing/tutorial/How-to-create-an-EC2-instance-from-AWS-Console">click here</a> for more details. or follow this below

<img src="./Images/Screenshot 2023-08-23 at 18.17.07.png">

After,  a Ec2 instance is created then let connect to the instance.
if you are using windows you connect using a tools called putty and mac users use there terminal to connect into their instance using SSH command. I'm a mac user <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html">Click here</a> for windows user. 

## Connect to Ec2 instance
a. Open terminal and locate the downloaded key pair file using this linux command `cd` and your file directory.

```bash
$ cd Downloads
```

b. Change premissions for the private key file, otherwise you can get an error

```bash
$ chmod 400 <private_file.pem>
```

c. Connect to the instance
```bash
$ ssh -i <private-key-name>.pem ubuntu@<Public-IP-address>
```

## Install Apache and update firewall

To install apache server we will use the Ubuntu package manager `apt`

```bash
#update a list of packages in package manager
sudo apt update

#run apache2 package installation
sudo apt install apache2
```
To ensure the apache2 is installed, use

```bash
sudo systemctl status apache2
```
If the below image is your result then you are on the track. You have launched your web server. 

<img src="./Images/Screenshot 2023-08-23 at 20.15.22.png">

Let update the firewall by configuring our security group inbound rules on ec2 instance.

## Configure EC2 instance  security inbound rules

When the instance is created, we have a default TCP rule on port 22 opened which is useful for SSH connection to a terminal. In order to ensure that our webpage are being accessed on the internet, we need to open a TCP port 80 inbound rule.

<img src="./Images/OpenPort80.gif">

To check the accessiblity of our web server on the internet, we curl the IP address/DNS name of our localhost.

```bash
 curl http://localhost:80
or
 curl http://127.0.0.1:80 
```

<img src="./Images/Screenshot 2023-08-23 at 22.22.20.png">

To see if our web application server can respond to requests , use the public ip address of our instance on a web browser. http://<Public_IP_Address>:80 or without the port 'http://<Public_IP_Address>'

<img src="./Images/Screenshot 2023-08-23 at 22.26.15.png">

## Installing mysql
To install mysql server we use the command below

```bash
$ sudo apt install mysql-server
```

## Installing PHP
To install PHP, these 3 package need to be installed together
```bash
$ sudo apt install php libapache2-mod-php php-mysql
```
once the installation is done used the command below to ensure it is installed and check the version 

```bash
$ php -v
```
<img src="./Images/Screenshot 2023-08-23 at 22.58.25.png">






