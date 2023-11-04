# LEMP stack Implementation
**LEMP** refers to a collection of open-source software that is commonly used together to serve web applications. The term LEMP is an acronym that represents the configuration of a Linux operating system with an nginx (pronounced engine-x, hence the **E** in the acronym) web server, with site data stored in a MySQL database and dynamic content processed by PHP.

## Prerequisites

Before i proceed i believe the reader have an account(AWS) and knowledge of how to create an EC2 instance if not go thorough my <a href="https://moty.hashnode.dev/deploy-lamp-stack-on-aws">LAMP article</a> to gain knowledge. Let get our hands dirty.

Firstly, we need to connect to the aws server.

<img src="./images/Screenshot 2023-11-03 at 13.52.45.png">

After connecting to the our remote server we need to install Nginx web server. NGINX is a high performance server

## Install NGINX server 

**Step 1:** We are installing the Nginx server using `apt` package manager to install this package

```
$ sudo apt update
$ sudo apt install nginx
```
<img src="./images/Screenshot 2023-11-04 at 16.44.38.png">

when prompted, enter `Y` to confirm the installation of nginx. Once the installation is finished, the server will be activated.
<img src="./images/Screenshot 2023-11-04 at 16.47.47.png">
To verify the server is install successfully 
```
$ sudo systemctl status nginx
```

<img src="./images/Screenshot 2023-11-04 at 16.50.03.png">

Before traffic can be received on our web server. we need to open a `TCP port 80` which is a default port for web browser used to access web pages in the internet. Port 22 is the default port created by ec2 machine for ssh access, so we need to add a rule to EC2 configuration to open inbound connection through port 80

<img src="./images/OpenPort80 (1).gif">

Our server is running and it can be accessed on locally and on the internet (Source 0.0.0.0/0 means 'from any IP address)
```
$ curl http://localhost:80
or
$ curl http://127.0.0.1:80
```
The code above is to access the Nginx server locally using domain name `localhost` and ip address.

**Resolution** is the process of converting a DNS name to IP address.

<img src="./images/Screenshot 2023-11-04 at 17.20.24.png">

Let test our nginx server on internet using a web browser.

```
http://<Public-IP-Address>:80
```

<img src="./images/Screenshot 2023-11-04 at 17.23.07.png">

Let install a mysql on our server.

## Installing Mysql
Now that we have a web server running, we need to install a **DBMS** Database management system to be able to store and manage data for site. **MySQL** is relational database management which uses structured query language to manage data inside a database. Let's install the database in the server

```
$ sudo apt install mysql-server
```

When prompted, enter `Y` to install. when the installation is done, log in by using the command below

```
$ sudo mysql
```
This will log into the database as the admin as user **root** and can set a password for the user , usinq **mysql_native_password** as default auth method. we're defining this user's password as `Password.1`

```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
```

exit the MySQL shell with

```
mysql> exit
```

Start the interactive script by running:

```
$ sudo mysql_secure_installation
```
This will ask if you want to configure the **VALIDATE PASSWORD PLUGIN**

**Note**: Enabling this feature is something of a judgment call. If enabled, passwords which don’t match the specified criteria will be rejected by MySQL with an error. It is safe to leave validation disabled, but you should always use strong, unique passwords for database credentials.

 Answer `Y` for yes, or anything else to continue without enabling.

`VALIDATE PASSWORD PLUGIN can be used to test passwords
and improve security. It checks the strength of password
and allows the users to set only those passwords which are
secure enough. Would you like to setup VALIDATE PASSWORD plugin?`

`Press y|Y for Yes, any other key for No:`

If you answer “yes”, you’ll be asked to select a level of password validation. Keep in mind that if you enter `2` for the strongest level, you will receive errors when attempting to set any password which does not contain numbers, upper and lowercase letters, and special characters, or which is based on common dictionary words e.g 
`PassWord.1`

```
There are three levels of password validation policy:

LOW    Length >= 8
MEDIUM Length >= 8, numeric, mixed case, and special characters
STRONG Length >= 8, numeric, mixed case, special characters and dictionary              file

Please enter 0 = LOW, 1 = MEDIUM and 2 = STRONG: 1

```

Regardless of whether you chose to set up the `VALIDATE PASSWORD PLUGIN` , your server will next ask you to select and confirm a password for the MySQL **root**  user. This is not to be confused with the **system root**. The **database root**  user is an administrative user with full privileges over the database system. Even though the default authentication method for the MySQL root user dispenses the use of a password, **even when one is set** , you should define a strong password here as an additional safety measure. We’ll talk about this in a moment. 

If you enabled password validation, you’ll be shown the password strength for the root password you just entered and your server will ask if you want to continue with that password. If you are happy with your current password, enter `Y` for “yes” at the prompt:

`Estimated strength of the password: 100 
Do you wish to continue with the password provided?(Press y|Y for Yes, any other key for No) : y
`

For the rest of the questions, press `Y` and hit the `ENTER` key at each prompt. This will prompt you to change the root password, remove some anonymous users and the test database, disable remote root logins, and load these new rules so that MySQL immediately respects the changes you have made.

When you’re finished, test if you’re able to log in to the MySQL console by typing:

```
$ sudo mysql -p
```

`-p` flag in this command, which will prompt you for the password used after changing the **root** user password.

To exit the MySQL console, type:

`mysql> exit
`

## Installing  PHP