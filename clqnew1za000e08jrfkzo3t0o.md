---
title: "End-to-End Image Resizing Pipeline with AWS S3, Lambda, and SNS: A Step-by-Step Guide"
datePublished: Sun Jul 02 2023 14:41:42 GMT+0000 (Coordinated Universal Time)
cuid: clqnew1za000e08jrfkzo3t0o
slug: end-to-end-image-resizing-pipeline-with-aws-s3-lambda-and-sns-a-step-by-step-guide-94e42124ec0d
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703659515169/b1a54e2c-09a8-49b5-bd0a-4c6bc024c8ca.gif

---

AWS Infra Diagram

**Introduction:**

In today’s digital age, images play a crucial role in various applications and websites. However, managing and serving images of different sizes can be a challenging task. To overcome this challenge, we can leverage the power of cloud computing and automation to resize images on-the-fly. In this blog post, we will explore how to automate image resizing using AWS S3, Lambda, and SNS, enabling efficient image management and delivery.

**Prerequisites:**

Before diving into the implementation, let’s ensure we have the following prerequisites in place:

AWS Account: To follow along with the steps outlined in this blog post, you will need an AWS account. If you don’t have one, you can sign up for free on the AWS website.

Basic Knowledge of AWS Services: Familiarity with AWS S3 (Simple Storage Service), Lambda (Serverless Compute), and SNS (Simple Notification Service) will be helpful. If you are new to these services, don’t worry — we will cover the necessary concepts and steps in detail.

**Implementation Options:**

In this blog post, we will cover only implementation options: manual setup through the AWS Management Console and the other approach is infrastructure-as-code using Terraform.

1\. **Manual Setup**: We will guide you through the step-by-step configuration process using the AWS Management Console. This approach is suitable for users who prefer a hands-on approach and want to understand the individual components and their configurations.

2\. **Terraform**: For users who prefer infrastructure-as-code and want to automate the provisioning of resources, we will provide instructions for implementing the solution using Terraform. This approach allows for repeatable and version-controlled deployments of the Image Resize Automation.

To follow along with the manual setup implementation, please refer to the detailed instructions provided in this blog post. If you prefer the Terraform approach, you can access the complete Terraform code and instructions on our GitHub repository: [https://github.com/AmanPathak-DevOps/Terraform-for-AWS/blob/master/Non-Modularized/Resize-Image-Using-LambdaFunction-S3-SNS/](https://github.com/AmanPathak-DevOps/Terraform-for-AWS/blob/master/Non-Modularized/Resize-Image-Using-LambdaFunction-S3-SNS/lambda-function.tf). The repository includes the necessary Terraform scripts and configuration files to set up the Image Resize Automation.

By following the upcoming sections of this blog, you’ll gain insights into both manual and Terraform-based implementations of the automated AWS Image Resize Automation and email notification system. Whether you prefer a hands-on or automated approach, this solution will empower you to effortlessly stay resized your images and optimize your storage.

Now that we have our prerequisites in order, let’s dive into the implementation details and automate the image resizing process step by step. By the end of this blog post, you will have a fully functional system that can automatically resize images and store them in a separate bucket, while notifying a subscribed email address about the successful resizing process.

Let’s get started!

1.  In this project, we will be creating two buckets. The first bucket will be used to store the original images that we want to resize. The second bucket will be dedicated to storing the resized images. The resizing process will be automated through an AWS Lambda function, which will automatically upload the resized images to the second bucket.

Creating the first S3 bucket by clicking on **Create bucket**.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659464405/63cf89b5-5ec0-462d-87ff-a1d9fb1d4826.png)

2\. Enter the name of Bucket 1 and click on **Create bucket**.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659466099/ea556eab-f575-48dc-b880-25823cc8c8a3.png)

3\. Create the second S3 bucket by clicking on **Create bucket.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659467800/8e603af2-1f9b-4a86-ab43-f6e3ba009328.png)

4\. Enter the name of **Bucket2** and click on **Create bucket.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659469540/e942ed1d-1629-4090-97cd-7df89407c8a3.png)

5\. So, Here we have created two Buckets.

You can check the below screenshot.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659470847/abde261b-009b-4880-aa13-1fc727ea793b.png)

6\. Now, let’s create an SNS **topic** and an SNS **subscription** before creating a lambda **function.**

Click on **Create topic.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659472326/346fc5dc-e8c8-4d68-beb0-6fb458a5c8bd.png)

7\. Select **Standard** type SNS topic and enter the suitable name for your SNS topic and click on **Create topic**.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659473742/27ac5f99-2ff1-45d6-b826-5850d825f0ff.png)

8\. As you can see in the below screenshot, we have configured the **SNS topic.** Now, click on **Create subscription** to add your email address.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659475510/75f1141c-d7f0-448d-ab8b-10e899b1733e.png)

9\. Select the **Protocol type** as **email** and add your mail address in **Endpoint** and click on **Create Subscription.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659477321/71b58a4b-ca2a-47c1-8279-138290679423.png)

10\. After creating **Subscription,** you will get a notification on your given mail address if not, please check your **spam box**. The mail will look like the below screenshot.

Click on **Confirm subscription** to get the notification for the resized image.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659479186/61175887-1b08-4db9-8305-f1a796c24508.png)

11\. Once you confirm your **subscription**. The **status** should be **Confirmed** from **Pending status.**

Here, we configured the **SNS topic** and **subscription.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659480738/0beabf72-55f9-46ce-9862-dfc6135ee59b.png)

12\. Now, Let’s create **a Lambda function** that will help us to resize the images and store the images.

Click on **Create function.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659482057/416e1a84-f2d9-44c1-ae69-21a68570364e.png)

13\. Enter the **function name, runtime(**I am using Python 3.9), and **architecture,** and for **role** select the first option, So we can configure from our own all the access for the services and click on **Create function**.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659483734/4e183021-7dc5-428e-8f73-11dcd9708bd8.png)

14\. First of all, replace the code with our code that will resize the image

Code link- [Lambda-Code](https://github.com/AmanPathak-DevOps/Terraform-for-AWS/blob/master/Non-Modularized/Resize-Image-Using-LambdaFunction-S3-SNS/CreateThumbnail.zip), After replacing the code, **deploy** it by clicking on **Deploy.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659485385/f7bd7202-9d89-4441-8204-6626151af78f.png)

15\. Now, we need to configure lots of things in the **lambda function.** Let’s start with **runtime settings,** that is, exactly under the code itself.

Click on Edit

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659486660/1b977777-213f-407f-8a4e-4340d40c3167.png)

16\. Now, add the **handler** name according to your **Python code name** and click on **Save.**

My **lambda function code** name is **CreateThumbnail** and **handler** name is **handler**. So, I have written **CreateThumbnail.handler**.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659488197/c7a83c44-33ec-4501-8a29-2769e9d448cd.png)

17\. In our **Python code,** we have to provide *Bucket2*\- **resized-bucket name** and **SNS topic Arn**. But instead of hardcoding it, we will use **Lambda environment variables and we will** pass our information.

To do that, Go to the **configuration** section, under click on **Environment variables,** and click on **edit.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659489464/531fca58-b80c-4ffd-9b70-ab0ccc8bcc11.png)

**18\. Add** the **SNS topic arn** and **S3 bucket name** of **Bucket2 according to your service name and on and click** on **Save.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659490959/a022f317-622e-4703-90fc-e40fd28efa37.png)

19\. In the lambda function, we have to increase the **timeout** and **Memory size** because the lambda function needs more time and memory to perform the resizing task.

To do that, Go to the **Configuration tab-> General configuration section -> click on edit.** Add the timeout 1 min and give memory 256MB. It would be enough to perform our task.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659492390/73d77246-5ceb-4742-8580-849d9f647dd7.png)

20\. Now, we need to add one **PIL** layer for our Python code.

Click on **Add a layer.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659494214/fd940785-b81b-4b71-9e0d-312fab6daeb4.png)

21\. Add the ARN of the layer **arn:aws:lambda:us-east-1:770693421928:layer:Klayers-p39-pillow:1** and click on **Save.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659495994/15ffc367-ce99-4dcc-b2f2-fd87317d3e91.png)

22\. Let’s do the IAM role configuration for our **Lambda function.** Go to **IAM -> roles -> ‘search for your lambda name role’** and click on the **role.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659497714/245a29e2-b944-47ab-bf26-bf96e8a4bd9f.png)

23\. Here we have only one **permission** that is related to see the **logs** of our **lambda** **function** in loggroup. But we need two more permissions which is **SNSFullAccess** to send the mail to the person and **S3FullAccess** to get the images and store the images in the respective buckets.

To do that, Click on **Add permissions** and select **Attach policy.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659499630/1acbedaa-d4b2-4ef8-998f-3c1de5c26561.png)

24\. Now, find the two permissions one by one **SNSFullAccess** and **S3FullAccess,** and check the box on the left of the permissions. After doing both, click on **Add permissions.**

The final IAM role has the below permissions list.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659501163/f5850855-5496-44b5-8e50-5039524ecea7.png)

25\. Finally, We have to add a trigger for the Lambda function. So whenever someone uploads an image to Bucket1. The lambda function will trigger and start the process of resizing the image and doing the other respective things.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659502744/8cf94889-92a8-491c-b330-7dcb60d8c81f.png)

26\. Add the **S3** as **source, Bucket1** which will trigger the **Lambda function,** and click on **Add.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659504361/e78c1ffb-02e4-47ce-8768-b1081f4bdfe9.png)

27\. Now, we have configured all the things related to the **Lambda function.** It’s time to test it.

To do that click on **Test -> Configure test event.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659505731/88021c8a-6aa3-4e29-a928-7cbb83630879.png)

28\. Add the **Event-name, the** template should be **s3 put** replace the few things that are given in the below screenshot, and click on **Save**.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659507359/8982a134-d18b-4523-9519-84a0cf2066da.png)

29\. Now, don’t run the lambda manually. Just upload an image to **Bucket1** and see the magic below.

**Outputs:**

*Uploading an Image to the Bucket1.*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659508981/d7e651b3-ab07-47fc-bee4-7c89bac96283.png)

The Image is of 1.9MB in the Bucket 1.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659510334/a45b81d4-d10c-4f82-adcc-0fb9031a4ddd.png)

The Image has been resized automatically from 1.9MB to 355.3KB.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659511766/aff65ef5-e49b-4e36-9083-3f1b23974507.png)

Hurray, We have received the mail as well.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659513361/5951c76e-e406-4d99-9a6f-8fe81e9174c9.png)

In conclusion, we have successfully implemented an automated image resizing system using AWS S3, Lambda, and SNS. This solution allows us to store the original images in one bucket and automatically resize and store them in another bucket. The integration of Lambda and SNS provides real-time notifications about the resizing process. By leveraging these powerful AWS services, we have simplified the image management workflow, improved efficiency, and enhanced user experience. Stay tuned for more exciting projects and follow us on social media for further updates and engaging content. Join us in the cloud revolution and unlock the potential of automation for your applications.

To stay updated with more exciting tutorials, AWS best practices, and cloud-related content, make sure to follow our blog and subscribe to our newsletter. By staying connected, you will unlock a wealth of knowledge and discover new ways to leverage cloud technologies for your projects.

Remember, the power of automation and cloud computing is at your fingertips. Embrace it, innovate, and unlock the full potential of your applications and services.

Feel free to reach out to me if you have any doubts or queries.

Stay tuned for more exciting content and happy cloud computing!

Follow us on:

LinkedIn- [https://www.linkedin.com/in/aman-devops/](https://www.linkedin.com/in/aman-devops/)

GitHub- [https://github.com/AmanPathak-DevOps/](https://github.com/AmanPathak-DevOps/)

Don’t miss out on future updates and exclusive content. Subscribe to our newsletter now and be part of the cloud revolution.

Together, let’s transform the way we build and deploy applications in the cloud!

Happy Learning!

Aman Pathak