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

Having successfully deployed and configured two webservers, We will move on to the load balancer. As a prerequisite, we need to provision an EC2 instance running ubuntu 22.04, open port 80 to anywhere using the security group and connect to the load balancer via the terminal.

### Deploying and Configuring Nginx Load Balancer

Will be using shell script to implement load balancer with nginx using the code below:

```
#!/bin/bash

######################################################################################################################
##### This automates the configuration of Nginx to act as a load balancer
##### Usage: The script is called with 3 command line arguments. The public IP of the EC2 instance where Nginx is installed
##### the webserver urls for which the load balancer distributes traffic. An example of how to call the script is shown below:
##### ./configure_nginx_loadbalancer.sh PUBLIC_IP Webserver-1 Webserver-2
#####  ./configure_nginx_loadbalancer.sh 127.0.0.1 192.2.4.6:8000  192.32.5.8:8000
############################################################################################################# 

PUBLIC_IP=$1
firstWebserver=$2
secondWebserver=$3

[ -z "${PUBLIC_IP}" ] && echo "Please pass the Public IP of your EC2 instance as the argument to the script" && exit 1

[ -z "${firstWebserver}" ] && echo "Please pass the Public IP together with its port number in this format: 127.0.0.1:8000 as the second argument to the script" && exit 1

[ -z "${secondWebserver}" ] && echo "Please pass the Public IP together with its port number in this format: 127.0.0.1:8000 as the third argument to the script" && exit 1

set -x # debug mode
set -e # exit the script if there is an error
set -o pipefail # exit the script when there is a pipe failure


sudo apt update -y && sudo apt install nginx -y
sudo systemctl status nginx

if [[ $? -eq 0 ]]; then
    sudo touch /etc/nginx/conf.d/loadbalancer.conf

    sudo chmod 777 /etc/nginx/conf.d/loadbalancer.conf
    sudo chmod 777 -R /etc/nginx/

    
    echo " upstream backend_servers {

            # your are to replace the public IP and Port to that of your webservers
            server  "${firstWebserver}"; # public IP and port for webserser 1
            server "${secondWebserver}"; # public IP and port for webserver 2

            }

           server {
            listen 80;
            server_name "${PUBLIC_IP}";

            location / {
                proxy_pass http://backend_servers;   
            }
    } " > /etc/nginx/conf.d/loadbalancer.conf
fi

sudo nginx -t

sudo systemctl restart nginx

```
<img src="./images/img07.png">

To run the above code follow the following steps:
-  On your terminal, open a file nginx.sh using the command below:
```
$ sudo vi nginx.sh
```
-  Copy and Paste the script inside the file
-  Close the file using the command below:
```
type esc the shift + :wqa!
```
-  Change the file permission to make it an executable using the command below:
```
$ sudo chmod +x nginx.sh
```
-  Run the script with the command below:
```
./nginx.sh PUBLIC_IP Webserver-1 Webserver-2
```
<img src="./images/img03.png">

After running the code this is what you wil get to know if the configuration is successful. 
<img src="./images/img04.png">

## Verifying the setup
Screnshot of the first Server load balancing
<img src="./images/img05.png">
Screnshot of the second Server load balancing
<img src="./images/img06.png">
