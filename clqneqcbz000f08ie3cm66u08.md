---
title: "Automating Infrastructure: Ansible Playbooks for Nginx and MongoDB Configuration"
datePublished: Tue Aug 01 2023 04:17:29 GMT+0000 (Coordinated Universal Time)
cuid: clqneqcbz000f08ie3cm66u08
slug: automating-infrastructure-ansible-playbooks-for-nginx-and-mongodb-configuration-7351c1f28580
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703659248691/072b673c-6a43-4051-9363-d5e5361cc3d8.gif

---

AWS Infra Diagram

**Introduction**

In today’s fast-paced world, the ability to quickly and efficiently deploy and manage infrastructure is crucial for the success of any project. In this blog, we will explore how to automate the configuration of Nginx and the deployment of applications using Ansible and Jenkins. Additionally, we will dive into the process of setting up a robust and secure MongoDB cluster using Ansible roles. By the end of this blog, you will have a comprehensive understanding of how to build a production-ready infrastructure that can be easily replicated and scaled.

**Objective**

The main objective of this blog is to demonstrate how automation can simplify the setup and management of critical components of a web application stack. By utilizing Ansible and Jenkins, we aim to achieve the following:

*   Configure Nginx Server: Learn how to automate the installation and configuration of Nginx, a high-performance web server, to serve as a reverse proxy for our web applications.
*   Deploy Applications with Ansible and Jenkins: Understand the integration of Ansible and Jenkins to automate the deployment process, ensuring consistent and error-free application releases.
*   Configure a Production-Ready MongoDB Cluster: Explore the steps to set up a MongoDB cluster with Ansible roles, ensuring high availability and fault tolerance.

**Prerequisites**

Before diving into the automation process, readers should have some familiarity with the following concepts:

*   Basic knowledge of Ansible: Understanding Ansible playbooks, roles, and inventory is beneficial for better comprehension.
*   Working knowledge of Jenkins: Familiarity with Jenkins and its CI/CD capabilities will be advantageous.
*   Web Servers: Basic knowledge of web servers like Nginx will help grasp the configuration process.
*   MongoDB fundamentals: Understanding the basics of MongoDB and database clusters will be beneficial when setting up the MongoDB cluster.

For more info about Ansible Roles, Please refer to the link: [Roles — Ansible Documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)

> Step by Step implementation guide:

**Ansible Master**: This Server will run the playbooks on the remote server through Jenkins jobs.

**Nginx Server(2)**: Both Nginx Servers are in different az for high availability purposes.

**MongoDB Server(2)**: Both Servers are in different az for high availability purposes with no public ip as this is the database.

To Create Ansible Master, Click on ‘Launch Instances’.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659188693/4242bf94-c0cd-4db2-99d3-6081a63f9c41.png)

Give the name to the server and I am using Ubuntu 22.04 version which is one of the prerequisites for this project.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659190281/f326ac7e-778f-4031-8eba-3a9d91a25fa1.png)

I am using Instance type t2.small because We have to install Ansible and Jenkins on the same machine. To get rid of the issue, select t2.small instead of t2.micro.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659191755/0f71b16f-d91c-43b3-8b9e-8410c7a3dac0.png)

Networking settings must be in your bucket.

Don’t open all the traffic. As this is the worst practice of being a DevOps Engineer.

Here, I want to connect with my Ansible-Master through my local machine only. So, According to that, I am providing my local machine's public IP address.

You can find your public IP by going to [https://whatismyipaddress.com/](https://whatismyipaddress.com/) or you can run the command ‘ curl ifconfig.me ‘ and click on ‘Launch Instance.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659193694/d47432d7-e266-44f1-8206-146ddb7e61d0.png)

Here, We need to configure our Security group for our Ansible-Master Server.

We need to open an 8080 port for Jenkins and 22 ports are already opened for SSH.

Make sure to provide your public IP only as a source instead of opening all traffic as we have already talked about this(security/networking) above.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659195160/28e91320-590e-4e7c-b45d-794074afa9e5.png)

Now, log in to the Ansible-Master Server.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659196884/0f096ce1-f45d-4a03-9cbc-8bf7640f2460.png)

First thing, Install the Ansible by following the below commands.

#! /bin/bash  
apt install software-properties-common  
add-apt-repository ppa:deadsnakes/ppa  
apt install python3.11 -y  
sudo update-alternatives - install /usr/bin/python python /usr/bin/python3.11 1  
sudo apt-add-repository ppa:ansible/ansible  
sudo apt update  
sudo apt install ansible -y  
sudo apt update  
ansible - version

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659199050/d450ad2f-a744-43fc-bec5-03062f4bd05b.png)

After Installing Ansible, We have to automate our configurations by Jenkins.

To Install Jenkins, you can follow the below commands.

#! /bin/bash  
\# For Ubuntu 22.04  
\# Intsalling Java  
apt update -y  
apt install openjdk-11-jre -y  
java - version  
\# Installing Jenkins  
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \\  
/usr/share/keyrings/jenkins-keyring.asc > /dev/null  
echo deb \[signed-by=/usr/share/keyrings/jenkins-keyring.asc\] \\  
https://pkg.jenkins.io/debian binary/ | sudo tee \\  
/etc/apt/sources.list.d/jenkins.list > /dev/null  
sudo apt-get update -y  
sudo apt-get install jenkins -y

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659200725/4a5e531b-bd83-4560-ad9b-62a2f8804e09.png)

Let’s do the setup of Jenkins first.

Enter the password by going to the highlighted directory.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659202534/0c0f1e50-b7c7-4684-9445-06b7c800fa68.png)

We will go with ‘Install suggested plugins’ because we have to install our required plugins according to the project.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659204165/ea96b720-fc9a-44b2-8d5f-affa8404c400.png)

Install two plugins,

Ansible: To integrate with Ansible and make the configuration deployment automated.

Build Pipeline: To create a pipeline and make the configuration deployment pipeline.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659205752/b8247b75-acd8-4bd6-87f3-df9c0adb9b2f.png)

Now, Set the Credentials by following the below process:

On the Ansible-Master run command ‘ssh-keygen -t rsa -b 4096’.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659207489/aa33a690-ab2a-45f9-b74e-840adcc96233.png)

Go to the .ssh/ directory and copy the content of the private key which is named **id\_rsa.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659209141/f03d65fa-e9ae-42ec-ae8c-786a2332647e.png)

On Jenkins, Add the private key by creating global credentials. To do that, Go to Dashboard > Manage Jenkins and Go to Credentials

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659210586/4396ffb5-e72f-4ffb-acaf-53aec3f1e88f.png)

Click on System

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659212300/6fd660a0-1321-43d2-9587-f2197ad9aa30.png)

Click on ‘+ Add credentials’

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659213841/afc11a9e-5c70-46cf-beb3-004da7878034.png)

Select the Kind as ‘SSH Username with private key’, and provide the ID(mandatory for Jenkins's point of view) and username as well.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659215264/7ae0fdf2-91e1-40b2-a964-ec60ecb17819.png)

Paste the content of the private key of Ansibe-Master and click on Create.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659216626/97084c79-f7a4-405e-a459-670e2b50886b.png)

Here, you have added the global credentials.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659217943/cf13f4c2-7772-4d3a-88de-6d57cccb9658.png)

Let’s create a pipeline to automate the Ansible configuration deployment.

Give the name and select pipeline.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659220216/c7b44a41-29ca-415f-9ed1-869600d56882.png)

Here, As we don’t know the code that how to integrate Ansible with Jenkins then we will see through the Pipeline Syntax.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659222287/fe52108c-1166-4d21-8db5-78e3f9de8658.png)

Now, provide the parameters according to your need such as the playbook name and hosts files.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659223917/e241cf92-d502-4742-9f63-9bdf54a3a107.png)

Click on ‘Generate Pipeline Script’ to generate the script for it.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659225527/1827e304-b421-43ec-9386-366dc292aeaa.png)

Here, you can see the script that I have got according to my provided parameters.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659227155/95011ea4-2dd9-46f7-97b2-eee550c8f469.png)

I have created a Jenkins pipeline according to my requirements. You can also do that according to your requirements and push it on GitHub or GitLab.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659228637/a3ed871e-a3d4-489a-8e98-b37e82d47178.png)

Here, I am providing the Jenkinsfile that is in the GitHub repository, and click on Save.

[https://github.com/AmanPathak-DevOps/CICD-Ansible.git](https://github.com/AmanPathak-DevOps/CICD-Ansible.git)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659230461/7d0e9b20-64dd-4aa1-adda-4dffcb05acac.png)

After Creating Pipeline, we need servers where we have to run our Ansible playbooks.

First of all, we will configure Nginx Servers.

To do that, Create an instance with the below configuration.

Make sure Both Nginx Servers should be created one by one and for different az to make high availability.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659232047/06dda634-3e4c-44da-ab21-6c697c826360.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659233801/cd335508-83cb-4da1-88b5-18b5f5edd33c.png)

Make sure the 22 port should be open for Ansible-Master Server. As We are following DevOps best practices and for port 80 you can open all the traffic because we are providing our content to the open world.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659235564/ac99b362-a13a-407d-8fa5-331e3617af18.png)

Local to Ansible Master

Now, we can connect with our Nginx Server through the Ansible-Master server as we have provided Ansible-Server Master private ip in the Nginx Server security group.

To do that send the pem file that you have selected while creating the Nginx Servers to the Ansible Server from the local machine.

You can use the below command to do that.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659237452/2ca26f95-60f1-456d-a47a-19f41be5d11b.png)

After doing that, we have to copy the public key that we generated earlier on Ansible-Master. To connect with the Nginx Servers, Ansible server should have something in common.

To do that, We are going to put the public key in the authorized\_keys of the Nginx Servers.

Copy the id\_rsa.pub file from the Ansible-Master server and paste it into the Nginx Servers inside the .ssh/authorized\_keys file.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659238818/fd2ffbcb-d458-40de-953f-091cc55f846c.png)

After doing this for both Nginx Servers. We have to provide the private IP to Ansible. So, whenever any playbook will start running. So, Ansible will get to know that on this server the playbook needs to run.

To do that, open the /etc/ansible/hosts file and write the following things according to your Private IPs and the rest of the things remain the same.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659240288/fb1f6e88-495d-4f59-b901-925da6ce9dde.png)

After doing all the setup, we run our Jenkins pipeline and it is successfully completed.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659242120/b155acfa-3f43-431e-83ea-8565dff143ba.png)

As you can see our web application is running perfectly on both servers without any issues.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659244074/112f96d2-2bc6-4fbd-849d-aa3259f0f695.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659245665/d881f35f-9497-40ab-80ba-5e9f2b44e7ae.png)

**Conclusion**

Automation is a game-changer in modern infrastructure management, and with the power of Ansible and Jenkins, we have successfully demonstrated how to build a production-ready infrastructure. By streamlining the configuration of Nginx, we can ensure a reliable and scalable environment for hosting web applications.

Through this blog, we hope to empower readers to harness the capabilities of automation and implement best practices in their own projects. By adopting automation, organizations can not only save time and effort but also reduce the likelihood of human errors in their infrastructure.

And the blog on setting up a production-ready MongoDB cluster can be accessed here:

[Automating Infrastructure: Ansible Playbooks for Nginx and MongoDB Configuration: Chapter 2](https://medium.com/@aman.pathak_51134/automating-infrastructure-ansible-playbooks-for-nginx-and-mongodb-configuration-chapter-2-6c9afddae22e)

Let’s meet there and complete our Project in the next blog.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659247355/f6ff8f0f-1962-4bc9-a0e1-fefb9906879f.gif)

Chapter 1 finished

**LinkedIn** Profile: [https://www.linkedin.com/in/aman-devops/](https://www.linkedin.com/in/aman-devops/)

**GitHub** Profile: [https://github.com/AmanPathak-DevOps](https://github.com/AmanPathak-DevOps)

Feel free to reach out to me, if any issues you were getting while implementing this.

Happy Learning