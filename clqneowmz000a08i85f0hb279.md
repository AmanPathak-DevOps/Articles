---
title: "Empower Your Website: A Step-by-Step Guide to Building a Secure and Blazing-Fast AWS S3 Website"
datePublished: Fri Jul 28 2023 06:51:28 GMT+0000 (Coordinated Universal Time)
cuid: clqneowmz000a08i85f0hb279
slug: empower-your-website-a-step-by-step-guide-to-building-a-secure-and-blazing-fast-aws-s3-website-c49d37bff4d7
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703659182060/90cd924d-e89a-406d-accc-b1f31cd11f76.gif

---

AWS Infra Diagram

**Objective**

Welcome to an exciting journey into the world of AWS as we unveil the secrets to building a highly secure and incredibly fast static website using AWS S3, Route 53, CDN, and SSL. In this comprehensive guide, we’ll walk you through two approaches: one using Terraform for quick setup and another taking a deep dive into the manual approach. By the end of this tutorial, you’ll have a fully functional website deployed on AWS, delivering an unforgettable user experience!

**Prerequisites**

Before diving in, let’s ensure you have the following prerequisites in place:

*   An AWS account: To follow along, you’ll need access to an AWS account with the necessary permissions to create resources like S3 buckets, CloudFront distributions, and Route 53 hosted zones.
*   Basic familiarity with AWS services: While we’ll provide detailed instructions, some familiarity with AWS services will be beneficial.
*   A custom domain: Make sure you have a registered custom domain that you want to use for your website. This could be your personal blog domain or a new domain you wish to acquire.

**Step 1: Terraform Approach (Quick Setup)**

If you’re eager to get started quickly, the Terraform approach is your perfect match. We’ve prepared a Terraform module in our GitHub repository that simplifies the setup process. Simply grab the code, follow our brief instructions, and you’ll have your website up and running in no time!

Find the Terraform module here: [https://github.com/AmanPathak-DevOps/Terraform-for-AWS/tree/master/Non-Modularized/S3-Static-Website](https://github.com/AmanPathak-DevOps/Terraform-for-AWS/tree/master/Non-Modularized/S3-Static-Website)

**Step 2: Manual Approach (Deep Dive)**

For those seeking a thorough understanding of each component, the manual approach will be your treasure trove. We’ll embark on a step-by-step journey, starting from creating an S3 bucket for hosting, configuring static website hosting, setting up a CloudFront CDN, securing your website with SSL using ACM, and finally, configuring Route 53 to link your custom domain. By the end of this section, you’ll be an AWS pro, capable of customizing every aspect of your website!

Click on ‘Create bucket’.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659127409/09fb49f2-1366-417c-8a10-ebfe77a64d30.png)

Provide the bucket name(try to give the bucket name the same as the domain name with the www. prefix).  
It would be best practice for DevOps persons.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659128990/0e44dd4c-ee34-49ed-84cd-4b21243cebb6.png)

Keep the Bucket private by checking the below box and other things keep it as it is and click on ‘Create bucket’.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659130732/37417a4d-d189-4b42-9752-d8ba5ae4ce4b.png)

Here, We have uploaded our Static website content to the created bucket.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659132448/ff303973-e03c-4c75-b342-4ddaaaf35544.png)

To host the static website, you need to enable the ‘Static website hosting’ option.

Click on ‘Properties’.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659133998/9e79ea4d-1ca7-48bf-9be2-31c38c240b15.png)

Click on ‘Edit’.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659135517/5abafd87-520a-43ae-b1b6-c38bfafa23a1.png)

Select ‘Enable’ radio button and provide the files according to your website code.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659137205/34f5e692-5307-46bc-85a8-710168a20a4e.png)

Here, We have enabled the Static website hosting and We will try to check whether the hosting is successful or not by clicking on ‘Bucket website endpoint’.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659138744/f5e3e8e7-eef5-4400-a7f6-dd6d31446ec6.png)

Now, We have got an error message 403 Forbidden.

We are getting this error because our Bucket is private.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659140247/35ad6485-d120-48e0-b731-f63e79b62f3c.png)

Let’s move to another AWS Service Route53 to assign the domain name to our Static website.

Click on ‘Create hosted zone’.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659141520/e7f37800-bc7e-4ada-a8e4-bc9f75d0ec68.png)

Provide the domain name according to your domain name and keep the things remaining the same and click on ‘Created hosted zone’.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659143165/dfa936da-c557-4ae3-9a5e-33bdf6dd7197.png)

After creating, We will get two records only.

The NS type host will be needed to link our main domain which is from get.tech domain provider to Route53 AWS Service.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659144651/25967bd5-9306-4d1f-af14-bd384264c840.png)

Now, copy the NS type from Route53 Service paste it into your Name server section according to your domain account, and click to update them.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659146172/b7f5eb4a-d38a-4367-ab37-461d1a568224.png)

Now, We need to provide the SSL/TLS certificate to our Static website to make it secure(Connection Secure).

To do that, click on ‘Request’.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659147798/f4aa05d9-3b1b-44e8-97f0-c2d12b5911c2.png)

As we are hosting our website publicly, choose the first option and click on ‘Next’.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659149155/c82cd205-b5ab-4790-967b-99137b1e3096.png)

Now, provide the normal domain name and if your domain name does not include www in the prefix(example.com), provide one more domain name with the www prefix([www.example.com](http://www.example.com)) keep the things as it is, and click on create certificate.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659151006/f03885dc-ef50-4648-86c3-11a77f704009.png)

Here, We got Pending validation status because we have to link our certificate with our domain name.

To do that, Click on ‘Certificate ID’.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659152566/2edc8b50-ae9b-427b-bf83-8aa4b2f95c05.png)

Click on ‘Create records in Route53’ which will create two more records in the created Route53 hosted zone.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659154119/1241607f-bad8-4530-9df1-f9e329c2e20b.png)

Click on ‘Create records’.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659155446/1e6073cf-bb97-41c2-b0c0-a15297151420.png)

As you can see here, the two Record names have been reflected in the Route53 Hosted zone.

Now, the two new records will validate the domain name with the help of NS type record because the Name servers are attached to the main domain provider.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659157301/8eb030ad-5c6a-482e-8827-876310249597.png)

Here, Amazon has issued the Certificate after completing the Validation of our domain.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659158670/67972f34-ebbd-4df9-9b4f-55143718f1e4.png)

Let’s create AWS CloudFront because we have to host our website with low latency. So anyone can access our website from any corner of the world with low latency.

Click on ‘Create distribution.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659160138/99c94687-de09-44cc-95ef-33f3c6b3c186.png)

In the ‘Origin domain’, We are providing the bucket name where our Static website content is stored.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659161401/353b8baf-f130-4ed3-816d-1d663326336c.png)

In origin access’, select Legacy access identities and click on ‘Create new OAI’ which will create a bucket policy to access the S3 bucket by CloudFront having a Public access block.

Don’t forget to select ‘Yes, update the bucket policy’. Otherwise, you have to update the bucket policy on your own which is not a beginner’s task.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659162843/99f5da7f-a79c-4c4b-83ed-f0752c40aaf5.png)

For the AWS WAF, disable it for now.

You can set the class according to you, I have selected all edge locations one for the least latency.

Add the domain name with the www prefix and without the www prefix as well in ‘Alter domain name(CNAME)’.

Now, Provide the Custom SSL certificate that we have created through AWS Certificate Manager.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659164300/bd07bd2c-fbad-4d9d-b3ad-0ab2fc53ad87.png)

Now, provide the main content file of your website in ‘Default root object’. I have added index.html according to my website content and clicked on ‘Create distribution’,click

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659165833/399c112f-dbc1-420e-835c-51a59879ecc2.png)

Once you try to access your Static website through CloudFront domain name. You will get this error.

If yes, then give it 3 to 5 minutes. If it’s still getting the same, then try to create a CloudFront distribution again because you have misconfigured something.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659167306/8358a168-a4bf-431e-b9b5-367e27b550c5.png)

After some time, My Static website hosting was done.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659168965/d73f6eb2-044b-480e-bf89-c4173ab6e9f7.png)

Now, I am expecting the same content from my main domain as well.

To do that, you have to route the application from your domain to the Cloudfront domain.

Click on ‘Create record’.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659170758/56f4ecfd-2890-44ba-a1ce-739c4da0421a.png)

Here, we are routing traffic to CloudFront distribution. So provide the CloudFront distribution domain name and keep the things that remain the same according to your domain name and click on ‘Define simple record’.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659172193/b90d9645-37a5-4beb-bd79-7f1a8b1e27e4.png)

Now, I have created a new record for my domain name with www prefix(www.example.com). So do the same thing except Record name and click on ‘Define simple record’.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659173857/7770db1b-a98b-4e1e-ae53-6426960ee205.png)

Click on ‘Create records’.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659175431/d2807dfc-9e57-4298-b857-231be9379cbc.png)

You have to wait some minutes or an hour according to your domain provider.

Here, I am browsing my website without the www prefix(example.com) domain name and getting the content of my website.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659176892/d23a2e56-4918-4cde-be46-872b1403d6a3.png)

Here, I am browsing my website www prefix(www.example.com) domain name and getting the content of my website.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659178910/79941a50-7261-4367-ba69-566681362a55.png)

Congratulations!!!! You have successfully deployed your Static Website on the AWS S3 bucket by providing the Connection secure and domain name. Also, don’t forget that your application can be accessed faster than the other 69% of applications.

Just kidding, I don’t know but the application provides the content with extremely low latency.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659180487/9b194d1a-779a-4d1b-9fec-9cf7afe790ae.gif)

Dancing After Deployment

**Conclusion**

Congratulations on mastering the art of building a lightning-fast, secure AWS S3 website! With this newfound knowledge, you’re now empowered to share your stories, products, or ideas with the world. Whether you prefer the quick Terraform approach or the in-depth manual journey, your website is now ready to shine.

Remember to regularly update your website with fresh content and maintain its security with SSL certificate renewals. If you encounter any issues along the way, don’t hesitate to explore AWS’s vast documentation or seek help from the AWS community.

Happy website building! Feel free to share your masterpiece with us, and remember to keep spreading the knowledge.

Follow us for more exciting AWS guides and tutorials!

*Disclaimer: The implementation guide for both approaches can be found in the provided GitHub repository. The blog focused on the overall project, prerequisites, and the reasoning behind the choices made. For detailed steps, refer to the repository.*

If you have any other queries, feel free to reach out to me on

LinkedIn Profile: [https://www.linkedin.com/in/aman-devops/](https://www.linkedin.com/in/aman-devops/)

GitHub Profile: [https://github.com/AmanPathak-DevOps](https://github.com/AmanPathak-DevOps)

Happy Learning

Aman Pathak