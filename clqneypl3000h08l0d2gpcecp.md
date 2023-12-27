---
title: "Deploy Static Website on AWS S3 + AWS Cloudfront + Route53 + AWS Certificate Manager using…"
datePublished: Mon Jun 12 2023 07:24:38 GMT+0000 (Coordinated Universal Time)
cuid: clqneypl3000h08l0d2gpcecp
slug: deploy-static-website-on-aws-s3-aws-cloudfront-route53-aws-certificate-manager-using-a30e1fa0e1b8
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703659639994/5437134e-a1eb-4d94-989a-11f95ccadb58.jpeg

---

Overall Architecture

**Introduction:**

In today’s digital landscape, having a fast and reliable website is essential for businesses and individuals alike. AWS (Amazon Web Services) offers a powerful combination of services that can help you deploy and scale your static website with ease. By utilizing services such as S3, CloudFront, Route 53, and ACM, you can ensure that your website is highly available, globally distributed, and secure. In this blog post, we will explore how to deploy a static website on AWS using Terraform, an infrastructure-as-code tool that allows for efficient and reproducible deployments. So, let’s dive in and learn how to harness the power of AWS services to deploy your static website seamlessly.

1.  **Setting Up AWS S3 Bucket:** We’ll start by creating an S3 bucket to store our website’s static assets. S3 provides highly scalable storage for hosting static files, and it integrates seamlessly with other AWS services. With Terraform, we can define the bucket’s configuration, such as its name, region, and permissions, in a declarative manner.
2.  **Content Delivery with AWS CloudFront:** To distribute our website’s content efficiently, we’ll leverage AWS CloudFront, a globally distributed content delivery network (CDN). CloudFront caches our website’s files in edge locations worldwide, reducing latency and improving the overall performance for visitors across the globe. With Terraform, we can configure CloudFront to serve our S3 bucket’s content and take advantage of its advanced caching and security features.
3.  **DNS Management with AWS Route 53:** To connect our custom domain name to our CloudFront distribution, we’ll use AWS Route 53, a highly scalable and reliable domain name system (DNS) service. Route 53 enables us to manage DNS records and route traffic to our CloudFront distribution seamlessly. By configuring the appropriate DNS settings, we can ensure that our website is accessible using our custom domain name.
4.  **Securing the Website with AWS Certificate Manager (ACM):** To provide a secure browsing experience for our website visitors, we’ll obtain an SSL/TLS certificate from AWS Certificate Manager (ACM). ACM simplifies the process of provisioning, managing, and renewing SSL certificates. With Terraform, we can automate the certificate provisioning and attach it to our CloudFront distribution, enabling HTTPS encryption for our website.

**Github Repository:** [https://github.com/AmanPathak-DevOps/Terraform-for-AWS/tree/master/Non-Modularized/S3-Static-Website](https://github.com/AmanPathak-DevOps/Terraform-for-AWS/tree/master/Non-Modularized/S3-Static-Website)

**Prerequisites:**

1.  **AWS Account:** Before getting started, you’ll need an AWS account. If you don’t have one already, you can create a free account at [https://aws.amazon.com.](https://aws.amazon.com.)
2.  **Terraform Installed:** Ensure that Terraform is installed on your local machine. You can download Terraform from the official website: [https://www.terraform.io/downloads.html.](https://www.terraform.io/downloads.html.) Follow the installation instructions provided for your operating system.
3.  **AWS CLI Installed and Configured:** Install the AWS Command Line Interface (CLI) on your local machine. You can download the AWS CLI from the official website: [https://aws.amazon.com/cli/.](https://aws.amazon.com/cli/.) Once installed, configure the AWS CLI by running aws configure and providing your AWS access key, secret access key, default region, and output format.
4.  **Domain Name:** Obtain a domain name that you want to use for your static website. You can register a new domain or use an existing one. Ensure that you have access to manage the DNS records for your domain.
5.  **Valid SSL/TLS Certificate:** If you want to enable HTTPS for your website, you’ll need a valid SSL/TLS certificate. You can obtain a certificate from a trusted certificate authority (CA) or use AWS Certificate Manager (ACM) to generate a free certificate.
6.  **Static Website Assets:** Prepare the static website assets (HTML, CSS, JavaScript, images, etc.) that you want to deploy. Organize them in a directory structure that matches the desired URL structure of your website.
7.  **Basic Knowledge of Terraform:** Familiarize yourself with the basics of Terraform, including how to define resources using HashiCorp Configuration Language (HCL) and how to execute Terraform commands. You can refer to the official Terraform documentation and tutorials to get started.

By fulfilling these prerequisites, you’ll be ready to follow the tutorial and deploy your static website on AWS using S3, CloudFront, Route 53, and ACM with Terraform.

Before we proceed with writing the Terraform code to create the infrastructure, let’s set up our domain name on **AWS Route53**. Please refer to the images and comments below for detailed instructions:

1.  Sign in to the AWS Management Console and open the Route53 service.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659608889/9fa217ca-c05b-4c05-8d5c-106b0d53a050.png)

2\. Click on the “Create hosted zone” button.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659610576/3d39cd80-d8d5-48c5-b8bf-fdd351f7f395.png)

3\. Enter your domain name (e.g., example.com) in the designated field.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659612177/fa256edd-f8fc-41ef-b197-c3be44efdf1c.png)

4\. In the screenshot below, you can see that there are four records present in the hosted zone.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659613870/9ac4955b-4abb-4a1c-9627-0dd215ace915.png)

5\. To add the four records to the Name Servers, follow these steps:

*   Sign in to the domain registrar where you purchased your domain (e.g., get.tech, or any other registrar of your choice).
*   Navigate to the DNS management or domain settings section.
*   Locate the Name Servers configuration or DNS settings for your domain.
*   Replace the existing Name Servers with the four records shown in the screenshot below.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659615453/15771260-e51f-499d-952b-67ac3b2928e4.png)

Please note that the specific steps and terminology may vary depending on the domain registrar you are using. Make sure to save the changes after updating the Name Servers.

With the setup completed for Route53, let’s proceed to create the infrastructure. We will create an S3 bucket to store the website, set up CloudFront for content delivery, and utilize Certificate Manager for SSL/TLS certificates.

During this process, we will use Terraform to define and provision the necessary resources. Terraform allows us to write infrastructure-as-code, making it easier to manage and reproduce the infrastructure configuration.

Let’s begin by writing the Terraform code to create the S3 bucket, CloudFront distribution, and Certificate Manager resources. Once deployed, you will have a scalable and secure infrastructure for hosting your static website on AWS.

**provider.tf**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659617171/7e6e36ab-5ef2-4af8-a2f2-2de01780e7c8.png)

**s3-bucket.tf**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659618970/e98253c7-6686-4f5a-ab72-ca54371401fb.png)

**cloudfront-distribution.tf**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659620867/384ddbc9-2617-4fcb-bd33-e123fa502628.png)

**certificate.tf**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659622700/6b326902-3dbd-48e3-8abb-28807b6431f5.png)

**route53.tf**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659624966/50051615-c990-4ef3-8ed3-b011cb96c2ba.png)

**variables.tf**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659627124/4f6acde6-129a-483e-bec1-befb6c753331.png)

*After running the command,*

**terraform fmt**

**terraform validate**

**terraform plan**

**terraform apply -auto-approve**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659628874/cd64342b-162b-44bb-982e-41c0d2504a21.png)

**We have successfully deployed our static website using the CloudFront domain with a secure connection and certificate.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659631247/134218df-6995-4112-bc2c-04324b8b2a7e.png)

### **Outputs**

**S3 Bucket**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659633250/02814305-e066-40a5-80e9-e147577df951.png)

**Cloudfront distribution**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659634851/c7649205-6c6a-4e70-a62a-e756c94db002.png)

**Certificates**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659636825/f10e0574-a564-40dc-9fbc-4eef1ecba10c.png)

**Route53**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659638538/39b27a02-c314-4d2b-9a3f-4df4e70d5ba6.png)

In conclusion, deploying a static website on AWS using S3, CloudFront, Route53, and ACM with Terraform provides a scalable and secure solution. By following the step-by-step guide in this blog, you have learned how to set up the necessary infrastructure components and configure them for hosting your static website.

However, this is just the beginning of your AWS journey. There is so much more you can explore and learn. Here are a few suggestions to continue your AWS exploration:

1.  **Dive deeper into Terraform:** Learn more about Terraform’s capabilities and how to automate the deployment and management of complex infrastructure on AWS.
2.  **Enhance website functionality:** Explore additional features such as custom error pages, content caching, and CDN behaviors to optimize the performance and user experience of your static website.
3.  **Implement CI/CD pipeline:** Integrate your Terraform configuration with a CI/CD pipeline to automate the deployment and updates of your infrastructure.
4.  **Secure your website further:** Explore AWS security best practices and consider implementing measures such as WAF (Web Application Firewall) or AWS Shield to protect your website from potential attacks.

Remember, AWS provides a vast array of services and resources to build and scale your applications. Continuously exploring and learning will help you leverage the full potential of AWS and enhance your skills as a cloud practitioner.

**Happy exploring and happy coding!**

Feel free to reach out to me, if you are getting an issue to implement the above demo.

LinkedIn: [https://www.linkedin.com/in/aman-devops/](https://www.linkedin.com/in/aman-devops/)