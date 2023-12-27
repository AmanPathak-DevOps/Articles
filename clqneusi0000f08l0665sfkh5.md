---
title: "Deploying WordPress on AWS EC2: A Beginner’s Guide"
datePublished: Sat Jul 22 2023 05:55:07 GMT+0000 (Coordinated Universal Time)
cuid: clqneusi0000f08l0665sfkh5
slug: deploying-wordpress-on-aws-ec2-a-beginners-guide-3f3dff7d128f
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703659457015/eca332a7-f826-4c49-be50-b50d0c93030e.gif

---

Are you looking to host your WordPress website on the cloud but don’t know where to start? In this beginner’s guide, we will walk you through the step-by-step process of deploying WordPress on an Amazon EC2 (Elastic Compute Cloud) instance, giving you the freedom and scalability of cloud hosting. This is a beginner-level project, and we will configure MySQL on the same EC2 instance for simplicity. However, keep in mind that as you gain experience, you can explore more advanced configurations and cloud services. Let’s dive in!

**Prerequisites**:

Before we begin, make sure you have the following prerequisites in place:

1.  AWS Account: Sign up for an AWS account if you haven’t already.
2.  Domain Name: Purchase a domain name from a domain provider like get.tech.
3.  Basic Knowledge: Familiarize yourself with basic Linux commands and server configurations.

**Step 1: Launching the EC2 Instance**

Login to your AWS account and head over to the EC2 Dashboard.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659395519/a8f37ca4-02a5-4f1c-91c2-ca5ab1399c5e.png)

Click on ‘Launch Instances’ and choose the ‘Ubuntu’ machine with the 22.04 edition.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659397539/f3c30e8d-6dd5-4132-a81e-7939b9a19517.png)

Select ‘t2.micro’ as the instance type and add your PEM file (optional).

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659399109/ab68ebb8-e62a-4263-b444-c5f6ea332e06.png)

Configure the Security Group by opening ports 22 (SSH), 80 (HTTP), and 443 (HTTPS).

Click on ‘Launch instance’ to create the instance.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659400857/359f1f17-7305-4058-8d56-5b9a2c5e743f.png)

**Step 2: Allocating Elastic IP**

Click on ‘Allocate Elastic IP address’ to assign a static public IP to your instance.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659402303/dc64a010-03a9-4ef5-949d-cc10e4aa797d.png)

Without any additional configurations, click on ‘Allocate’.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659403962/ba63f573-6a26-4eb5-8d2f-4f41107ea1ca.png)

Associate the Elastic IP with your EC2 instance by clicking on ‘Actions’ and selecting ‘Associate Elastic IP address’.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659405464/c5d2531e-9bd9-4e8a-8a6e-40454eb88055.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659407193/e305651e-5453-453f-b3a6-36eb7097cda0.png)

Confirm that the Elastic IP has been attached by checking the ‘public IP’ in the EC2 dashboard.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659408682/de0fa191-37eb-4f3a-a1e9-0e58c40a7cb8.png)

**Step 3: Configuring the EC2 Instance**

Connect to your EC2 instance using ‘EC2 Instance Connect’ or an SSH client with the PEM file.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659410279/23c228f9-dcf0-43f6-b795-2022f3765f6b.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659412485/d998abb4-9d51-40ef-9ee4-ff0b4a7ea049.png)

Install the Apache2 web server using the following commands:

sudo apt-get update  
sudo apt install apache2  
sudo systemctl enable apache2  
sudo systemctl restart apache2

Verify if the server is running by entering the public IP address in your browser.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659414324/7b665e05-c9da-4794-aa17-704899a999a7.png)

**Step 4: Installing PHP and MySQL**

Install PHP and MySQL database on the EC2 server with the following commands:

sudo apt install php libapache2-mod-php php-mysql  
sudo apt install mysql-server

Log in to the MySQL database as the root user:

sudo mysql -u root

Set a password for the root account and create a new user and database:

ALTER USER 'root'@’localhost' IDENTIFIED WITH mysql\_native\_password by 'MySQL-1234';  
CREATE USER 'aman\_new'@localhost IDENTIFIED BY 'Aman@123DevOps';  
CREATE DATABASE aman;  
GRANT ALL PRIVILEGES ON aman.\* TO 'aman\_new'@localhost;

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659416125/d2fbefe6-735b-4cd0-ab3c-f96b7f691525.png)

Output of the above commands

**Step 5: Installing WordPress**

Download and install WordPress using the following commands:

sudo wget https://wordpress.org/latest.zip  
sudo apt-get install unzip  
unzip latest.zip  
mv wordpress/ /var/www/html/

Access your WordPress site by browsing the public IP/WordPress.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659417534/e30f09b4-3b15-459d-a0a1-0067dc0810d2.png)

Fill in the database details in the setup wizard using the database credentials created earlier.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659419316/ea35c312-7b4e-47ed-aa31-0dfb200b0aa8.png)

Now, you have to write the wp-config. php file.

Copy the content.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659420961/497daaf3-1b64-42cf-b236-1f1da8d8843d.png)

We have to create a wp-config.php file inside the /var/www/html/wordpress/ directory and paste the above content into the wp-config.php file. To do that, follow the below commands:

cd /var/www/html/wordpress  
sudo vim wp-config.php

Paste the above configuration which is given in the above screenshot and come back to the same screenshot and click on “Run the installation”.

Enter the details accordingly.

Now, WordPress needs to be configured, Enter the details according to the below screenshot, and remember the password to log in again.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659422764/be254603-5236-4249-b765-c4167702cee5.png)

WordPress is installed now! So click on “Log In”.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659424353/1b36314a-50dd-4adf-b03f-4f4000a1eb0a.png)

Now, the new window will ask you for your username and password. You can skip it by opening the incognito tab of your browser and paste “<public\_ip>/wordpress.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659426105/6267407b-0275-488b-a31c-5784bc0a4677.png)

**Step 6: Configuring Domain Name**

To avoid entering the public IP address for accessing WordPress, edit the “sites-available” file in “/etc/apache2/” and change the Document Root to “/var/www/html/wordpress”:

cd /etc/apache2/sites-available/  
sudo vim 000-default.conf

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659427345/ce410ac5-50be-4eee-88bc-8c5b04ce5fc9.png)

Restart the Apache service:

sudo systemctl restart apache2

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659428900/0cdd649b-e4b1-41a3-b4c1-ccb1789a846e.png)

Now, we need to add the domain name to our WordPress application.

To do that, there is one Prerequisite in which you must have one domain name.

I have one domain that is purchased from get.tech domain provider.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659430445/cb785060-c2c5-4727-99b8-56cd921a0cb6.png)

Now Go to the ‘Route53’ Service in AWS and click on ‘Create hosted zone’.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659432429/51028f48-0f54-4f1d-b941-dea8df336529.png)

Write your respective domain name Type should be ‘Public hosted zone’ and click on ‘Created hosted zone’.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659433778/a6c83dee-a39c-4b71-9446-3990e08b593d.png)

You will get two Records in your hosted zone.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659435353/ee8fd790-57a1-461d-bfea-91e003670c2d.png)

Now, we have to create two more records.

Click on ‘Create record select ‘Simple routing’ and click on ‘Next’.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659437030/079fe272-6eee-40c8-96ea-0c796060cb87.png)

Click on ‘Define simple word’.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659438490/09b0c8c4-2505-4a5b-bfe8-261829d7725f.png)

Create the first simple record

*   No need to add a subdomain
*   The record type should be A type
*   Value/Route traffic as IP Address
*   Add the Public IP of the instance and click on ‘Define simple record’.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659439985/ad3a1154-a053-42d8-a688-e1a785f7a616.png)

The record will be like this.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659441641/32d8232c-436c-43b1-87c7-6597aac42416.png)

Now, create a second simple record.

*   Add ‘www’ in the subdomain
*   The record type should be CNAME
*   Value/Route traffic will be ‘your domain name’ only.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659443099/f08b8491-50b7-4abe-b2f7-d8b3b8a467f1.png)

We have created both records. Don’t consider ‘www’ in the second ‘Value/Route traffic to’.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659445126/5a1a3798-b8cc-40c3-87be-af1fd0ae11f0.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659446739/6008a084-5cc7-4d57-912f-0a99298db0a6.png)

To follow the proper things with the help of video, kindly refer to the below link

[How to connect Godaddy Domain Name with AWS Route 53 | EC2 instance](https://www.youtube.com/watch?v=hRSj2n-XKGM&ab_channel=TechnicalBabaji)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659448227/f5ac968d-f919-4b23-b5b4-42cd89339188.png)

Now, we have to give the name to our Server that will be the same as our domain name. To do that,

*   cd /etc/apache2/sites-available/
*   sudo vim 000-default.conf

and add your domain in the ServerName and add ‘www’ as prefix in the ServerAlias and write the domain name as it is.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659450066/bb95b8ff-574a-44ca-9802-7c423be5f14e.png)

sudo systemctl restart apache2

Now, add the domain name by going to the WordPress admin.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659451895/47610586-75ee-46a8-af21-952942f71c6f.png)

**Step 7: Secure the Connection for HTTP and HTTPS**

Now, we need to secure the connection for HTTP and HTTPS.

Run the following commands on your web server

sudo apt install certbot python3-certbot-apache  
sudo certbot --apache

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659453648/b3735fa4-6bf9-48c6-9c99-a6a2dd286de9.png)

Here, you can see the connection has been secured for the web server.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659455173/625b585e-91cd-40dd-9627-cd5c76be8bd3.png)

**Conclusion:**

Congratulations! You have successfully deployed WordPress on an AWS EC2 instance, configuring MySQL on the same server for simplicity. As a beginner, this project provides an excellent introduction to cloud hosting and basic server configurations. As you progress, you can explore more advanced setups, including separating the database and web server, auto-scaling, and using managed database services. Cloud computing offers a wide array of possibilities, and this is just the beginning of your cloud journey. Happy exploring!

Keep exploring, keep innovating, and keep building amazing things!

GitHub Profile: [https://github.com/AmanPathak-DevOps](https://github.com/AmanPathak-DevOps)

Follow on [LinkedIn](https://www.linkedin.com/in/aman-devops/) to stay up to date with the latest developments.

Happy Learning!

Aman Pathak