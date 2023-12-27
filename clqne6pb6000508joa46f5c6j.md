---
title: "Automating Infrastructure: Ansible Playbooks for Nginx and MongoDB Configuration: Chapter 2"
datePublished: Tue Aug 01 2023 04:19:29 GMT+0000 (Coordinated Universal Time)
cuid: clqne6pb6000508joa46f5c6j
slug: automating-infrastructure-ansible-playbooks-for-nginx-and-mongodb-configuration-chapter-2-6c9afddae22e
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703658332375/eff45694-ad6b-4909-b82b-2047b203238d.gif

---

AWS Infrastructure Diagram

**Introduction**

In our previous blog

[**Automating Infrastructure: Ansible Playbooks for Nginx and MongoDB: Chapter1**](https://medium.com/@aman.pathak_51134/automating-infrastructure-ansible-playbooks-for-nginx-and-mongodb-configuration-7351c1f28580)**,**

we explored the power of automation with Ansible and Jenkins to configure Nginx and automate application deployments. Continuing on our journey to building a production-ready infrastructure, this blog will focus on setting up a robust MongoDB cluster using Ansible roles. As data becomes the backbone of modern applications, ensuring high availability and fault tolerance in the database layer is crucial. With Ansible’s flexibility and simplicity, we can streamline the process of configuring a MongoDB cluster, making it easier to maintain and scale.

**Objective**

The main objective of this blog is to demonstrate how to create a highly available MongoDB cluster using Ansible roles. By the end of this blog, readers will have a clear understanding of the steps involved in setting up a production-ready MongoDB environment, ensuring data integrity and scalability.

**Prerequisites**

To get the most out of this blog, readers should have some familiarity with the following:

*   Basic knowledge of Ansible: Familiarity with Ansible playbooks, roles, and inventory is essential, as we will be using Ansible to automate the MongoDB cluster setup.
*   Nginx Configuration and Basic Ansible: Knowledge of Nginx and basic Ansible concepts is beneficial, as we will refer to them throughout the blog. If you need a refresher, you can find our previous blog \[Link to Previous Blog\] on Nginx configuration and basic Ansible usage.

> Step by step guide

Now, We have to configure the Production-Ready MongoDB cluster through Ansible Roles.

But before going to jump on this. We have to make our MongoDB servers with proper security.

We have to keep our database server private.

To do that follow the following steps:

Create a NAT gateway and provide the subnet that you will be going to use while creating the MongoDB server creation and add the elastic IP as it is mandatory.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658310646/68aa6b19-a7dc-422a-89aa-1a11765b7d41.png)

Now, Create a private route table. Select the VPC that you will be going to use while creating the database server.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658312017/c05e5dc7-cd2a-4e3c-b108-18d9f32d2590.png)

Make sure in the routes the Target should be the NAT gateway that we have created above and the destination will be 0.0.0.0/0 as the server should have access to the internet.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658313602/c9dc1e72-9e44-4fb7-9f09-0e8f9d351d94.png)

Now, add the subnet that you will be going to use while creating the database servers.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658315299/8d7fc991-cdb5-4fe2-9fd8-dc9fab5d3461.png)

Let’s create MongoDB servers.

Add the below details, and make sure you create Mongodb servers with different az subnets.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658316949/f7e998cc-3a8d-4491-828d-97e8a4d8fdc8.png)

Make sure that Auto-assign public IP should be disabled as we are creating our database server private.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658318660/6a0e8e9a-fd59-48a3-91d0-98a7018e7f87.png)

SSH port will open for the Ansible-Master private Ip only and As MongoDB runs on the 27017 port. So we have to open that port for the Ansible-Master server only and click on Create.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658320091/42e8ce15-7cb4-4276-a39d-d1d6ef867bea.png)

As our goal is to configure the Production-Ready mongoDB cluster through Ansible Role. We will create two roles, one for Mongo installation and the second for Mongo users configurations.

ansible-galaxy init mongo  
ansible-galaxy init mongo\_users

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658321798/d764f287-818e-4679-93cf-a9819b413a76.png)

The file structure will look like this.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658323566/5f4c2e96-2ba5-4e50-beac-9aac25fc15d3.png)

Now, I have given the GitHub repo link of the playbook for the Mongodb configuration with proper comments or justification.

Here, Our Jenkins job to configure the MongoDB has been successfully completed.

GitHub Repository: [https://github.com/AmanPathak-DevOps/CICD-Ansible.git](https://github.com/AmanPathak-DevOps/CICD-Ansible.git)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658325148/57a146c8-d1ea-45e9-8217-e36b70d8e4af.png)

Here, you can also validate according to our playbook tasks.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658326465/bb99766c-34e2-4276-9949-d825dd7d726f.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658328026/0c0f3e33-f26a-4469-9503-a20ec9a628af.png)

**Conclusion**

In this blog, we have explored the process of creating a production-ready MongoDB cluster using Ansible roles. By automating the deployment and configuration of the MongoDB replica set and shared cluster, we ensure high availability, data redundancy, and scalability. We have also highlighted the importance of monitoring and backup strategies to safeguard critical data.

Combined with the insights from our previous blog on Nginx configurations and basic Ansible usage, readers now possess the knowledge to build a comprehensive and automated infrastructure for hosting web applications.

We hope this blog inspires readers to embrace automation and unlock the potential of modern infrastructure management. By adopting Ansible’s best practices and methodologies, organizations can create reliable, efficient, and scalable environments to support their growing applications.

For a comprehensive understanding of Nginx configurations and basic Ansible usage, refer to our previous blog here: \[Link to Previous Blog\].

As always, we welcome your valuable feedback and encourage you to explore further automation possibilities to enhance your infrastructure.

Happy automating, and may your MongoDB clusters be resilient and highly available!

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658330121/b15e662d-8f00-40ea-b695-45cafc6fa254.gif)

Chapter 2 finished

**LinkedIn** Profile: [https://www.linkedin.com/in/aman-devops/](https://www.linkedin.com/in/aman-devops/)

**GitHub** Profile: [https://github.com/AmanPathak-DevOps](https://github.com/AmanPathak-DevOps)

Feel free to reach out to me, if any issues you were getting while implementing this.

Happy Learning