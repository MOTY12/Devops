# Load Balancing:
Load balancing means distributing the work or tasks among several computers or servers so that no one computer gets overloaded with too much work. This helps to keep everything running smoothly and ensures that websites and apps work quickly and don't get too slow. It's like teamwork for computers!. 
To distribute the traffic evenly between the webservers, a load balancer is deployed. The load balancer stands in front of the webservers, all traffic gets to it first, it then distributes the traffic across the set of webservers. This is to ensure no webserver get over worked, consequently improving system performance.

## Setting Up a Basic Load Balancer

Let create two Ec2 instances running on ubuntu 22.04 and install apache web server in them.We will open port 8000 to allow traffic from anywhere, and finaly update the default page of the webservers to display their public IP address.

Next we will provision another EC2 instance running ubuntu 22.04, this time we will install Nginx and configure it to act as a load balancer distributing traffic across the webservers.

**Step 1:** Create EC2 instance. <a href=
"https://www.techtarget.com/searchcloudcomputing/tutorial/How-to-create-an-EC2-instance-from-AWS-Console">Click Here</a> to read more on how to create an Instance

**Step 2:** Open a terminal in your local, and connect to your virtual machine using ssh and .pem key Then install apache server on your instance.
```
$ sudo apt update -y &&  sudo apt install apache2 -y
```
<img src="./Images /Img01.png">

- Verify the apache is running
```
$ sudo systemctl status apache2
```
<img src="./Images /Img02.png">

**Step 3:** Configure Apache to server a page showing its public IP: 
we configure the Apache webserver to serve content on port 8000 instead of its default which is port 80. Then we will create a new index.html file. The file will contain code to display the public IP of the EC2 instance. We will then override apache webserver's default html file with our new file.
- Configuring Apache to Serve content on port 8000:
-- Using your text editor(eg vi, nano) open the file /etc/apache2/ports.conf
```
$ sudo vi /etc/apache2/ports.conf 
```
-- Add a new Listen directive for port 8000: First type i to switch the editor to insert mode. Then add the listen directive. Then save your file `esc` + `:wq`.

<img src="./Images /Img03.png">

-- Next open the file /etc/apache2/sites-available/000-default.conf and change port 80 on the virtualhost to 8000 like the screenshot below:

```
$ sudo vi /etc/apache2/sites-available/000-default.conf
```
<img src="./Images /Img04.png">

-- Restart apache to load the new configuration using the command below:
```
$ sudo systemctl restart apache2
```

- Creating our new html file:
-- Open a new index.html file with command below:

```
$ sudo vi index.html
```
- Copy the html code to in to the `vi` editor after changing it to insert mode by pressing `i`:
```
    <!DOCTYPE html>
    <html>
    <head>
        <title>My EC2 Instance</title>
    </head>
    <body>
        <h1>Welcome to my EC2 instance</h1>
        <p>Public IP: YOUR_PUBLIC_IP</p>
    </body>
    </html>

```

- Change file ownership of the index.html file with the command below:
```
$ sudo chown www-data:www-data ./index.html
```
- Overriding the Default html file of Apache Webserver:
-- Replace the default html file with our new html file using the command below:
```
$ sudo cp -f ./index.html /var/www/html/index.html
```
-- Restart the webserver to load the new configuration using the command below:
```
$ sudo systemctl restart apache2
```

<img src="./Images /Img05.png">


**Step 4:**  Configuring Nginx as a Load Balancer
- Provision a new EC2 instance running ubuntu 22.04. Make sure port 80 is opened to accept traffic from anywhere. Your can refer to step 1 through step 2 to refresh your memory

- Install Nginx into the instance using the command below:
```
sudo apt update -y && sudo apt install nginx -y
```
