## **Amazon EC2 Instances (Challenge Lab)**

## **Overview**

This challenge demonstrates how to deploy a simple web application on an Amazon Linux EC2 instance inside a custom VPC.
You’ll create the VPC and subnet manually, configure networking components, launch an EC2 instance, install and start a web server automatically using user data, and deploy a sample HTML web page to confirm successful setup.

## **Objectives & Learning Outcomes**

After completing this lab, I will be able to:

Configure a custom VPC, subnet, and Internet Gateway

Launch an Amazon Linux EC2 instance with a General Purpose SSD (gp2)

Use user data to automatically install and start Apache (httpd)

Configure security groups for SSH and HTTP access

Deploy and test a static web page on your web server

Verify server setup using EC2 System Logs


## **Architecture**

<img width="1000" height="400" alt="8c29e5ed-ac68-419c-9ab8-5340d0b7e330" src="https://github.com/user-attachments/assets/0f9b7e06-7b0a-422f-9101-5dba9c5c7e2b" />


User → Internet → Internet Gateway → VPC → Public Subnet → EC2 Instance (Amazon Linux)


## **Key Components:**

VPC: Custom 10.0.0.0/16 network

Subnet: 10.0.1.0/24 (Public Subnet)

Internet Gateway: Allows outbound/inbound internet access

Route Table: Directs 0.0.0.0/0 traffic to Internet Gateway

Security Group: Allows ports 22 (SSH) and 80 (HTTP)

EC2 Instance: Amazon Linux 2, t3.micro, Apache installed via user data

## **Commands and Steps**

```bash
# ------------------------------------------
# Step 1: Create VPC and Networking
# ------------------------------------------
# In AWS Console: Create VPC (10.0.0.0/16), Subnet *********, Internet Gateway, and Route Table.
# Associate subnet with route table and add 0.0.0.0/0 → Internet Gateway route.

# ------------------------------------------
# Step 2: Launch EC2 Instance
# ------------------------------------------
# AMI: Amazon Linux 2
# Instance Type: t3.micro
# Storage: General Purpose SSD (gp2)
# Auto-assign Public IP: Enabled
# Security Group: Allow SSH (22) & HTTP (80)

# User Data Script
#!/bin/bash
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
chmod -R 777 /var/www/html
echo "<h1>Apache Web Server Installed Successfully</h1>" > /var/www/html/index.html

# ------------------------------------------
# Step 3: Verify Installation
# ------------------------------------------
# In EC2 Console → Monitor and Troubleshoot → Get System Log
# Confirm httpd installation lines and "Started httpd service" message.

# ------------------------------------------
# Step 4: Connect and Deploy Web Page
# ------------------------------------------
# Connect via EC2 Instance Connect
cd /var/www/html
sudo nano projects.html

# HTML content
<!DOCTYPE html>
<html>
<body>
<h1>Amarachi Emeziem's re/Start Project Work</h1>
<p>EC2 Instance Challenge Lab</p>
</body>
</html>

# Save and exit (Ctrl+O, Enter, Ctrl+X)
sudo chmod 777 /var/www/html/projects.html

# ------------------------------------------
# Step 5: Test in Browser
# ------------------------------------------
# Open: http://<public-ip>/projects.html

```


## **Screenshots**

System Log – httpd Installed	Verifies Apache installation from EC2 system log	
<img width="1571" height="428" alt="System log" src="https://github.com/user-attachments/assets/3202a36a-6f0c-4f09-b8fd-c8c5905e323e" />

Webpage Displayed in Browser	Shows successful HTML deployment
<img width="795" height="248" alt="2" src="https://github.com/user-attachments/assets/28c978f0-4bae-4bbb-b9a4-10b7efee9bb1" />

## **Tools Used**

Amazon VPC

Amazon EC2 (Amazon Linux 2)

EC2 Instance Connect

Apache HTTP Server (httpd)

AWS Management Console


## **What Actually Happened**

Created a custom VPC and public subnet with routing through an Internet Gateway.

Launched an EC2 instance with Amazon Linux 2, attached to the public subnet.

Used user data to automate Apache installation and web server startup.

Connected via EC2 Instance Connect and verified server setup.

Created and uploaded projects.html to /var/www/html.

Accessed the webpage through the instance’s public IPv4 address, confirming a successful setup.


## **Key Takeaways**

Always configure Internet Gateways and routes for public access.

Automate instance setup with user data for consistency.

Understand VPC architecture for secure and scalable environments.

Security groups act as your first firewall — always allow only required ports.


## **Author**

Amarachi Emeziem

Cloud Security & IT Support Specialist | AWS & Azure Certified

LinkedIn Profile: https://www.linkedin.com/in/amarachilemeziem/
