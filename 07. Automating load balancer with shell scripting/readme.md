# Automate the Deployment of Webservers
In Devops automation is the heart of the work. Here in this project i will be automating a load balancer of two apache server using shell scripting. We will do that by writing a shell script that when ran, all that will did manually will be done for us automatically. **Automation** helps us speed the the deployment of services and reduce the chance of making errors in our day to day activity.

# Deploying and configuring the web server
- Provision an EC2 instance running ubuntu 20.04. 
-  Open port 8000 to allow traffic from anyhere using the security group. Again refer to the course mentioned above in step one for a refresher.
-  Connect to the webserver via the terminal using SSH client
-  Open a file
```
$ sudo vi install.sh
```
- paste the script below and close the file using the command below abd save with `shift` + `:wqa!`:
```
#!/bin/bash

####################################################################################################################
##### This automates the installation and configuring of apache webserver to listen on port 8000
##### Usage: Call the script and pass in the Public_IP of your EC2 instance as the first argument as shown below:
######## ./install_configure_apache.sh 127.0.0.1
####################################################################################################################

set -x # debug mode
set -e # exit the script if there is an error
set -o pipefail # exit the script when there is a pipe failure

PUBLIC_IP=$1

[ -z "${PUBLIC_IP}" ] && echo "Please pass the public IP of your EC2 instance as an argument to the script" && exit 1

sudo apt update -y &&  sudo apt install apache2 -y

sudo systemctl status apache2

if [[ $? -eq 0 ]]; then
    sudo chmod 777 /etc/apache2/ports.conf
    echo "Listen 8000" >> /etc/apache2/ports.conf
    sudo chmod 777 -R /etc/apache2/

    sudo sed -i 's/<VirtualHost \*:80>/<VirtualHost *:8000>/' /etc/apache2/sites-available/000-default.conf

fi
sudo chmod 777 -R /var/www/
echo "<!DOCTYPE html>
        <html>
        <head>
            <title>My EC2 Instance</title>
        </head>
        <body>
            <h1>Welcome to my EC2 instance</h1>
            <p>Public IP: "${PUBLIC_IP}"</p>
        </body>
        </html>" > /var/www/html/index.html

sudo systemctl restart apache2
```
-  Change the permissions on the file to make an executable using the command below:
```
$ sudo chmod +x install.sh
```
-  Run the shell script using the command below. Make sure you read the instructions in the shell script to learn how to use it.

```
$ ./install.sh PUBLIC_IP
```
<img src="./images/img01.png">

Your result should be this:

<img src="./images/img02.png">

# Deployment of Nginx as a Load Balancer using Shell script