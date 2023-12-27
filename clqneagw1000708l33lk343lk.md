---
title: "Deploy Two-Tier Architecture on AWS using Terraform"
datePublished: Mon Dec 04 2023 14:56:39 GMT+0000 (Coordinated Universal Time)
cuid: clqneagw1000708l33lk343lk
slug: deploy-two-tier-architecture-on-aws-using-terraform-9a1e310811c0
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703658508478/a516688c-1d53-4e81-9c25-b9866687a3e2.gif

---

### Introduction

In the world of cloud computing, infrastructure as code (IaC) plays a pivotal role in automating the deployment and management of resources. This blog post provides a step-by-step guide on creating a Two-Tier architecture on AWS using Terraform. We’ll explore the essential services involved, ensuring high availability, security, and scalability for hosting a static website.

Also, we are adopting a modular approach with enhanced security measures. The infrastructure is organized into dedicated modules, ensuring a scalable, maintainable, and secure deployment.

### Directory Overview

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658468164/bade5a63-39d0-4b50-a7c7-dad30a6c09f7.png)

### Directory Overview

*   **bloody-sweet:**
*   `backend.tf`: Configuration for Terraform backend, specifying where to store the Terraform state.
*   `main.tf`: Main Terraform configuration orchestrating the deployment.
*   `variables.tf`: Definition of variables used in the main Terraform configuration.
*   `variables.tfvars`: Input values for the defined variables.
*   **modules:**
*   **alb-tg:**
*   `gather.tf`: Terraform script to gather information about the Application Load Balancer (ALB) and Target Group (TG).
*   `main.tf`: Main Terraform configuration for ALB and TG.
*   `variables.tf`: Definition of variables used in the ALB and TG module.
*   **aws-autoscaling:**
*   `deploy.sh`: Shell script for deploying the Auto Scaling Group.
*   `gather.tf`: Terraform script to gather information about the Auto Scaling Group.
*   `main.tf`: Main Terraform configuration for the Auto Scaling Group.
*   `variable.tf`: Definition of variables used in the Auto Scaling Group module.
*   **aws-iam:**
*   `iam-instance-profile.tf`: Terraform configuration for IAM instance profile.
*   `iam-policy.json`: JSON file containing the IAM policy.
*   `iam-policy.tf`: Terraform configuration for IAM policy.
*   `iam-role.json`: JSON file containing the IAM role.
*   `iam-role.tf`: Terraform configuration for IAM role.
*   `variables.tf`: Definition of variables used in the IAM module.
*   **aws-rds:**
*   `gather.tf`: Terraform script to gather information about the RDS cluster.
*   `main.tf`: Main Terraform configuration for the RDS cluster.
*   `variables.tf`: Definition of variables used in the RDS module.
*   **aws-vpc:**
*   `main.tf`: Main Terraform configuration for the Virtual Private Cloud (VPC) and other Networking Services like Public/Private Subnet, ElasticIP, etc.
*   `variables.tf`: Definition of variables used in the VPC module.
*   **aws-waf-cdn-acm-route53:**
*   `acm.tf`: Terraform configuration for ACM (Amazon Certificate Manager).
*   `cdn.tf`: Terraform configuration for CDN (Content Delivery Network).
*   `gather.tf`: Terraform script to gather information about WAF, CDN, ACM, and Route 53.
*   `route53.tf`: Terraform configuration for Route 53.
*   `variables.tf`: Definition of variables used in the WAF, CDN, ACM, and Route 53 modules.
*   `waf.tf`: Terraform configuration for AWS WAF (Web Application Firewall).
*   **security-group:**
*   `gather.tf`: Terraform script to gather information about security groups.
*   `main.tf`: Main Terraform configuration for security groups.
*   `variable.tf`: Definition of variables used in the security group module.

This modular approach enhances the project’s maintainability, making it easier to manage and scale as your infrastructure requirements evolve. Each module focuses on a specific aspect of the infrastructure, promoting reusability and clarity in configuration.

### Pre-requisites

Before diving into the infrastructure creation, make sure you have the following:

*   An AWS Account
*   Terraform installed on your local machine
*   AWS Access and Secret Access keys configured
*   Domain Name Configured manually and add the Name Servers to your Domain Provider

### Step-by-Step Guide

To get started, clone the repository using the following command:

git clone https://github.com/AmanPathak-DevOps/Terraform-for-AWS

Navigate to the project folder:

cd Non-Modularized/Two-Tier-Application/bloody-sweet

### Planning and Deployment

Execute the following Terraform commands to plan and deploy the infrastructure:

terraform plan -var\-file=variables.tfvars

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658469603/b7557f1d-41fd-43fe-b4e6-b328439853ba.png)

terraform apply -var-file=variables.tfvars --auto\-approve

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658471319/f67088c5-80ba-415f-abe6-10a8c268154e.png)

Once the deployment is complete, you can inspect the created services using the provided snippets for each service.

### VPC & Other Networking related Services

VPC

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658472876/eb821949-63d0-4214-be82-04c62cca12db.png)

Public and Private Subnets

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658474206/0ba024b2-441b-4069-a275-3c533b4fe827.png)

Public and Private Route tables

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658475935/8f289e9b-ee43-403c-b22e-23d33d19e1f6.png)

Internet Gateway

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658477257/c69515af-def3-447b-8065-ee97fc995a8c.png)

Elastic IP addresses

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658478977/63c3ebda-f8cc-4a99-90d7-f3885c581a08.png)

NAT Gateways

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658480287/0d108c5d-e095-4f36-b152-2e95905e1861.png)

Security Groups

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658481530/bd8bebf9-ccbf-45bb-8ee6-cfc8747b237c.png)

### EC2 & AutoScaling Group

Launch template

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658483133/57ea0a74-487e-4e1a-8117-4d690a9b5ed2.png)

AutoScaling Group

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658484378/357ede4b-22cf-4e11-a869-c28cfb1aeab9.png)

### Target Group & Load Balancer

Target Group

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658485783/dcadafd2-c832-4551-9e1d-6244130a739f.png)

Load balancer

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658487180/ac8eaefe-94d8-4712-b85d-f4a053d57a59.png)

### Database

Subnet Group for RDS

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658488944/56a5547b-84ce-4e78-a0be-8874e496d03b.png)

RDS Cluster

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658490399/8ca74788-e704-4ace-a059-57fa73a6a13a.png)

### After Core Service, Deploy Service on Server

Route53

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658491801/6332af1e-b61a-4702-9a8d-18ef76823538.png)

AWS Certificate Manager

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658493339/6c3d9e52-6bdb-4516-a4b6-4e07b22982d6.png)

AWS Web Application Firewall

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658494549/a52fe9f0-a28d-4abd-956a-9a6e89811ddc.png)

CloudFront

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658496073/09ef803e-26c0-418e-bda2-85075e9a0038.png)

IAM Role

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658497511/f67b76de-a003-4627-908e-87aca366c31a.png)

IAM Policy

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658498806/6d07c814-9e27-4b47-8329-5c81e502fac4.png)

IAM instance profile

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658500360/041f1465-6831-4fd6-a6c4-0ccb6eb1886d.png)

### TF State file and State lock

Backend- TF State file stored on S3

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658501952/f9c0bcf6-a6f4-4099-b07a-f35cb82b4981.png)

TF State lock file

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658503866/08b91707-0195-4edf-aa6f-2d3fbd4ab7bb.png)

Once the deployment is completed, you can enter your domain name in the browser to validate whether your servers are perfectly running or not.

As you can see in the below snippet, the Application is running

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658505458/ce10fcca-3262-4a16-9fc3-ef982b13df3c.png)

### Cleanup

When you’re done exploring the Two-Tier architecture and want to avoid incurring unnecessary costs, follow these steps to clean up the resources:

Run the following command to initiate the destruction of the infrastructure.

terraform destroy -var-file=variables.tfvars --auto\-approve

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658506956/ce37230c-1fb0-4fc7-bed2-b13cbb2b30fa.png)

Delete the Repository (Optional):

*   If you cloned the Git repository for this project and no longer need it, you can delete it locally.

rm -rf Terraform-for-AWS

This step is optional and depends on whether you plan to reuse the repository for future exploration.

By following these cleanup steps, you ensure that AWS resources are properly decommissioned, and you won’t incur unnecessary charges. Always exercise caution when performing destructive actions like `terraform destroy` to avoid unintended consequences.

### Conclusion

In this comprehensive guide, we embarked on a journey to deploy a Two-Tier architecture on AWS using Terraform. Embracing a modular approach with enhanced security measures, our infrastructure is organized into dedicated modules, offering scalability, maintainability, and robust security.

Happy coding!

**Feel free to reach out to me if you have any doubts or queries.**

**LinkedIn**\- [https://www.linkedin.com/in/aman-devops/](https://www.linkedin.com/in/aman-devops/)

**Github**\- [https://github.com/AmanPathak-DevOps/](https://github.com/AmanPathak-DevOps/)

Happy Learning!

Aman Pathak