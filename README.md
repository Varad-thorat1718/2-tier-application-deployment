🏗️ 2-Tier Application Deployment on AWS (Amazon Linux)
📋 Project Overview

This project demonstrates the deployment of a 2-Tier Architecture Application on AWS using Amazon Linux instances.
The architecture separates the Web Tier (Frontend) and Database Tier (Backend) to ensure scalability, performance, and security.
User --> Web Server (Amazon Linux + Apache/PHP/HTML)
               |
               v
       Database Server (Amazon Linux + MySQL)
🚀 Technologies Used
Component	Technology
OS	Amazon Linux
Web Server	Apache HTTP Server
Backend	PHP
Database	MySQL
Cloud Platform	Amazon Web Services (AWS EC2, Security Groups, EBS)
⚙️ Project Setup Steps
Step 1: Create Two EC2 Instances

Web Server Instance

OS: Amazon Linux

Security Group: Allow HTTP (80) and SSH (22) inbound traffic.

Database Server Instance
Step 2: Configure Web Server

Connect to the web server:

ssh -i your-key.pem ec2-user@<web-server-public-ip>


Install Apache, PHP, and MySQL client:

sudo yum update -y
sudo yum install httpd php php-mysqlnd -y


Start and enable Apache:

sudo systemctl start httpd
sudo systemctl enable httpd


Deploy your web application:

cd /var/www/html
sudo nano index.php


Example PHP file:

<?php
$conn = mysqli_connect("DB-Private-IP", "username", "password", "database");
if ($conn) {
    echo "Connected to Database Successfully!";
} else {
    echo "Connection Failed!";
}
?>
Step 3: Configure Database Server

Connect to the DB instance:

ssh -i your-key.pem ec2-user@<db-server-public-ip>


Install MySQL server:

sudo yum update -y
sudo yum install mysql-server -y


Start and enable MySQL:

sudo systemctl start mysqld
sudo systemctl enable mysqld


Secure the MySQL installation:

sudo mysql_secure_installation


Create database and user:

CREATE DATABASE mydb;
CREATE USER 'webuser'@'%' IDENTIFIED BY 'Pass@123';
GRANT ALL PRIVILEGES ON mydb.* TO 'webuser'@'%';
FLUSH PRIVILEGES;


Update MySQL config to listen on all IPs:

sudo nano /etc/my.cnf


Add or update:

bind-address = 0.0.0.0


Then restart MySQL:

sudo systemctl restart mysqld
Step 4: Connect Web Server to Database

Edit your PHP code with the Database Private IP and MySQL credentials.

Test by visiting:

http://<web-server-public-ip>/index.php


You should see:

Connected to Database Successfully!

🔒 Security Configuration

Web Server SG:

Inbound: HTTP (80), SSH (22)

DB Server SG:

Inbound: MySQL (3306) from Web Server Private IP only

Outbound: Allow all (default)
📦 Folder Structure Example
2-tier-app/
│
├── index.php
├── db_config.php
├── README.md
└── screenshots/
    ├── architecture-diagram.png
    ├── web-success.png
    └── db-config.png

🧠 Key Learning

How to design and deploy a 2-Tier architecture on AWS.

Secure communication between web and database tiers.

Setting up and configuring Apache, PHP, and MySQL on Amazon Linux.

Managing AWS Security Groups effectively.
✨ Author

Varad Thorat
OS: Amazon Linux

Security Group: Allow MySQL (3306) only from the Web Server’s private IP.
