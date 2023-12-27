---
title: "Building Intelligent Image Recognition Systems with AWS Lambda, S3, Rekognition, and SNS: A…"
datePublished: Fri Jul 07 2023 18:19:29 GMT+0000 (Coordinated Universal Time)
cuid: clqnewptc000i08ie1ouce7mm
slug: building-intelligent-image-recognition-systems-with-aws-lambda-s3-rekognition-and-sns-a-b0902404a178
cover: https://cdn-images-1.medium.com/max/800/1*-Mudmql94hzKKOWZoVjYow.gif

---

AWS Infrastructure Diagram

**Introduction**

Image recognition technology has become increasingly vital across industries, enabling automated analysis of visual content. In this blog post, we will explore how to build intelligent image recognition systems using AWS services. By leveraging AWS Lambda, S3, Rekognition, and SNS, you can develop powerful solutions that automate visual insights. Let’s dive into the step-by-step guide to implementing this exciting technology.

**Problem Statement**

Imagine the need to analyze a large collection of images or videos to detect objects, identify faces, or moderate content. Manually processing such visual data is time-consuming, error-prone, and inefficient. This is where automated image recognition systems come into play. By harnessing the power of AWS services, you can create solutions that perform complex visual analysis with ease, saving time and enhancing accuracy.

**Prerequisites**

To implement the image recognition project, you’ll need the following prerequisites:

*   An AWS account: Sign up for an AWS account if you haven’t already.
*   Basic knowledge of AWS services: Familiarize yourself with AWS Lambda, S3, Rekognition, and SNS.
*   Programming concepts: Understand the basics of programming and how to work with APIs and SDKs.

**About the Approaches**

In this project, we will explore two approaches: a deployment using Terraform and a manual implementation.

**Terraform Approach**

For a more streamlined and automated deployment, you can utilize Terraform, an infrastructure-as-code tool. The project repository on GitHub contains the necessary Terraform code. Follow these steps to implement the solution using Terraform:

1.  Clone the Repository: Clone the [GitHub](https://github.com/AmanPathak-DevOps/Terraform-for-AWS/tree/master/Non-Modularized/Image-Recognistion-using-Amazon-Rekognition) repository to your local environment.
2.  Set up AWS Credentials: Configure your AWS credentials to allow Terraform to access your AWS account.
3.  Customize the Terraform Configuration: Modify the Terraform variables in the code to match your AWS environment and project requirements.
4.  Deploy the Infrastructure: Run the Terraform commands (`terraform init`, `terraform plan`, `terraform apply`) to deploy the AWS resources automatically.
5.  Monitor and Manage: Utilize Terraform to monitor and manage your infrastructure, easily make modifications, and maintain consistency.

**Manual Approach**: In situations where you prefer a more hands-on approach or need to customize the implementation further, the manual approach can be followed. The steps for the manual implementation are as follows:

1.  Create a bucket that will be stored and compared with the other images.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659523676/93fffb7e-e484-4682-af07-1843f3c2a725.png)

2\. Name the bucket name

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659525217/78e0da24-2f3f-412c-a05d-8a25464d714b.png)

3\. Click on **Create topic** to create an SNS topic.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659526974/de9ef607-20f4-449b-9d6c-7757f28e7d53.png)

4\. Topic type must be **Standard** and give the name to SNS topic.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659528527/1b45170e-728a-4d23-b8f2-7acd3e55b0a2.png)

5\. Click on **Create a subscription**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659529821/1f5690d4-f773-456d-be4e-26a3f420b92e.png)

6\. Give the TopicARN that we have created above and the protocol should be **Email** type and enter your email and click on **Create a subscription.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659531411/2f182755-5803-48c6-9cbe-cb98e888032b.png)

7\. After creating a subscription, you will get an email to the given mail id. Click on the highlighted link to confirm the subscription.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659532800/9cc83433-c5de-45d7-af7f-5d27e7a67524.png)

8\. Once you confirm the subscription. The status will be **confirmed** as per the below screenshot.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659534127/fe78f8af-8dfa-4a5d-85bb-1c7b10dd8069.png)

9\. Click on **Create a function**.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659535734/4beb6c2e-bedb-4212-b322-4cfec73cade0.png)

10\. Fill in the details, according to the below screenshot.

And replace the default code with the given code in the repository

[Python-Code-for-Amazon-Rekognition](https://github.com/AmanPathak-DevOps/Terraform-for-AWS/blob/master/Non-Modularized/Image-Recognistion-using-Amazon-Rekognition/ImageRekognition.zip)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659537329/37fde51b-c120-4b52-b385-f2fb5d21ceb7.png)

11\. Add the permission to the lambda function, **S3FullAccess** needs to read the files, **AmazonRekognitionFullAccess** needs to compare the files, and **SNSFullAccess** needs to send an email to the respective person.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659538952/e0d7e64c-70bc-4843-a95e-d6e2b79e56b9.png)

12\. We have to give the **BUCKET\_NAME** and **SNS\_TOPIC\_ARN** in the **python** code. But as it is not one of the DevOps best practices. We will use lambda environment variables to define the values of both(BUCKET\_NAME and SNS\_TOPIC\_ARN) and click on **Save.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659540532/d7bc791b-c30b-4f57-84fb-754cac191a77.png)

13\. Now, As part of the Automation, Whenever the user pushes the image to the bucket. Then, the lambda function will run and compare the pushed images with the other stored images.

To Add the trigger, click on **+Add trigger.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659541937/dfbe03f5-ba75-4100-bf11-d034df1983cb.png)

14\. Add the bucket name that we have created in the first step and click on **Add**.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659543997/3d34e28e-0db9-42e2-aca8-a43dda345a95.png)

15\. Now, As you can see in the below screenshot, the S3 trigger has been added.

**Note**: Increase the lambda timeout from 3sec to 10sec.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659545589/0b1a226a-3498-44ca-aaa3-7bf2e2b38b7c.png)

Now, I have pushed the image over the created S3 bucket and after that, I will get these kinds of emails.

This is our Goal which we have completed.

Conclusion: Building intelligent image recognition systems using AWS services empowers you to automate visual analysis and gain valuable insights from visual data. In this blog post, we explored the step-by-step process of implementing image recognition through both manual and Terraform approaches. By leveraging AWS Lambda, S3, Rekognition, and SNS, you can create robust solutions that enhance productivity and accuracy. Start building your own image recognition systems and unlock the potential of visual data today.

Thanks and Follow: Thank you for reading this blog post. I hope you found it informative and inspiring. If you have any questions or feedback, feel free to reach out. Stay connected for more exciting content and updates on AWS solutions for image recognition. Follow on [LinkedIn](https://www.linkedin.com/in/aman-devops/) to stay up to date with the latest developments.

GitHub Profile: [https://github.com/AmanPathak-DevOps](https://github.com/AmanPathak-DevOps)

Happy Learning!

Aman Pathak