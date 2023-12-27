---
title: "Automated AWS Cost Report Generation and Email Notification for Daily and Monthly Insights"
datePublished: Sat Jul 01 2023 17:40:23 GMT+0000 (Coordinated Universal Time)
cuid: clqnenkvk000e08l67a4ebkon
slug: automated-aws-cost-report-generation-and-email-notification-for-daily-and-monthly-insights-130e98c26b25
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703659120442/33b281be-1535-42e7-85d0-c33248ff2371.gif

---

AWS Infrastructure Diagram

**Introduction:**

In today’s cloud-based infrastructure, managing costs is a crucial aspect for businesses. AWS offers a comprehensive set of tools and services to monitor and optimize costs, but keeping track of expenses can still be a daunting task. To simplify this process, automating the generation of cost reports and receiving regular email notifications can provide valuable insights and help businesses make informed decisions.

In this blog post, we will explore how to set up an automated AWS cost report generation and email notification system using AWS Lambda, Amazon EventBridge, and Amazon SES (Simple Email Service). This solution will enable you to receive daily and monthly cost insights directly in your inbox, empowering you with the information you need to manage your AWS costs efficiently.

**Prerequisites:**

Before diving into the implementation, there are a few prerequisites you should have in place:

1\. AWS Account: You will need an active AWS account to access the required services and resources.

2\. IAM Role: Create an IAM role with appropriate permissions to allow your Lambda function to access AWS Cost Explorer API and send emails using SES.

3\. AWS Lambda: Familiarity with AWS Lambda, a serverless compute service that allows you to run code without provisioning or managing servers.

4\. EventBridge (CloudWatch Events): Understand the basics of EventBridge, a serverless event bus service that allows you to schedule and trigger events based on time, application state changes, or custom events.

5\. SES (Simple Email Service): Basic knowledge of SES, an email sending and receiving service, used to deliver cost reports and notifications to your email address.

**Implementation Options:**

In this blog post, we will cover only implementation options: manual setup through the AWS Management Console and the other approach is infrastructure-as-code using Terraform.

1\. **Manual Setup**: We will guide you through the step-by-step configuration process using the AWS Management Console. This approach is suitable for users who prefer a hands-on approach and want to understand the individual components and their configurations.

2\. **Terraform**: For users who prefer infrastructure-as-code and want to automate the provisioning of resources, we will provide instructions for implementing the solution using Terraform. This approach allows for repeatable and version-controlled deployments of the cost management system.

To follow along with the manual setup implementation, please refer to the detailed instructions provided in this blog post. If you prefer the Terraform approach, you can access the complete Terraform code and instructions on our GitHub repository: [https://github.com/AmanPathak-DevOps/Terraform-for-AWS/tree/master/Non-Modularized/AWS-Cost-Reporting](https://github.com/AmanPathak-DevOps/Terraform-for-AWS/tree/master/Non-Modularized/AWS-Cost-Reporting). The repository includes the necessary Terraform scripts and configuration files to set up the cost management system.

By following the upcoming sections of this blog, you’ll gain insights into both manual and Terraform-based implementations of the automated AWS cost report generation and email notification system. Whether you prefer a hands-on or automated approach, this solution will empower you to effortlessly stay informed about your AWS costs and optimize your cloud expenditure.

Let’s get started with the step-by-step implementation of this cost management solution.

1.  We have to add two types of **SES** here, first- sender, and second receiver.  
    Let’s create first by going on **Amazon SES-> Verified Identities** and clicking on **Create an identity.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659074153/3cf3590b-cfed-48c6-bf13-ccf02e3059af.png)

2\. Choose Identity type as **Email address,** add your **Email address,** and click on **Create identity.**

**Don’t forget to create the receiver’s SES and do the same thing to create the receiver’s SES.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659075516/05402c43-d9ea-4fb8-b3b9-c7fd7fa4641f.png)

3\. After creating SES, you will get a notification on your given **email address.** Click on the link that highlighted the link in the Screenshots below.

After doing this, you will be able to receive or send mail.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659077140/6ad5bd49-b165-4d4d-8e27-75754ff88838.png)

4\. To confirm if you have agreed to accept emails in SES, you can check the identity status, which should be “**Verified**”.

After doing this, **SES** configuration will be completed and we will configure the **Lambda Function.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659078678/2b28256d-19ee-4b1b-aa6c-9974fdf8a830.png)

5\. Click on “**Create function”.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659080369/94306681-62b5-4a65-ac28-09b780ab0126.png)

6\. Take the details from the below screenshots.

About Runtime, I will be working with Python3.9 as this is not much older and quite stable.

And remember to **Create a new role with basic Lambda permissions** and click on **“Create”.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659081900/22ff22cf-1908-49e4-9ad5-040f9016a997.png)

7\. You can see in the screenshot below that we have created the lambda function.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659083251/f8f63b28-a35f-4244-b92d-f9a7ae35c705.png)

8\. Now, We need to add some permissions for the Lambda function. As it will require the data from **CostExplorer** and after that Lambda function will help to send an email using **Amazon SES service.**

Go to the IAM role that is attached to the **AWS-Cost-Reporting** Lambda function.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659084762/d623008a-4c82-4b40-b1cb-8acb4e33075c.png)

9\. Click on **“Add permissions”.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659086272/d6866876-c58b-4dfa-81c5-8cba458ffa43.png)

10\. We have added two new permissions(**Cost-Explorer-GetUsage** and **AmazonSESFullAccess**) in the role.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659087943/62da4a30-e377-477b-8bbe-c7a8d04a8912.png)

11\. Now, we need some library packages(boto3, DateTime, SDKPandas, etc). To do that, we will add a layer in which all the libraries related to our code will be there.

Click on **“Layers”** highlighted in the screenshot below**.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659089643/38942e9b-c583-415f-a7ae-709514b567e2.png)

12\. Click on **“Add a layer”.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659091297/a0e81077-acf9-4756-8278-ea32a313941f.png)

13\. First layer is already given by **AWS itself.** Do the same according to the given screenshot below.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659092634/edf33258-4e81-477c-9117-b71c8bb4978e.png)

14\. Now, we need the Python DateTime package, which is not added in the previous layer.

To do that, firstly we will install the **DateTime** package and move the **DateTime** package to the file **layer/python/lib/python3.9/site-packages/.** Make sure that the file sequence must be like that and to follow the entire process, refer to the given screenshot below.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659094120/1b1932c9-7b22-4f7e-975a-a6f57d6cd70c.png)

15\. Click on **“Add a layer”** by going to the **“AWS-Cost-Reporting”** lambda.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659095796/79687da7-6f81-47a5-ae75-fd511f98458e.png)

16\. This time click on the highlighter “**Layers”** in the given screenshot below.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659097091/558e52c6-67e6-4808-b5ff-04c7f57888c4.png)

17\. Now, Create the layer first by adding the zip file that you have created in step/screenshot 14 and keep the rest of things the same.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659098424/2e173e9b-cf4e-406e-bda3-d6815b887c05.png)

18\. Now, Add a layer. We have a custom layer. So, click on **“Custom layers”** and add the created **layers.**

After doing this, main dependencies configurations have been completed for the **Lambda function.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659099982/d55dea7b-2b61-427d-b7b9-5fcc6d7df07e.png)

19\. Now, we will add our Python code to fetch the Cost Usage from CostExplorer and send the mail with the Cost Usage details to the given mail ids.

**Note:** *In the provided code, there are two places where you need to update your email addresses. Please make the necessary changes at those locations.*

Python Code link: [https://github.com/AmanPathak-DevOps/Terraform-for-AWS/blob/master/Non-Modularized/AWS-Cost-Reporting/costusage.zip](https://github.com/AmanPathak-DevOps/Terraform-for-AWS/blob/master/Non-Modularized/AWS-Cost-Reporting/costusage.zip)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659101459/5f029a6c-e3dd-4d67-95a0-0eeffe887035.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659103421/2cc1764c-fb6a-466a-8b45-6ad57c4c8e5c.png)

20\. To run the **lambda function.** Click on **Test** and do the following things by referring to the given screenshot below.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659104741/815b06ff-1ff6-4624-85f6-9c6b7403a293.png)

21\. Here, We have **run** the code and it has given the expected results related to the cost.

Now, we have to check whether the mail has been received or not.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659106428/b6f5d597-6f3c-48bd-bc72-94bd26b0c487.png)

22\. In the below screenshot, you can see that the mail has been received.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659107968/5b4ad489-e1eb-4969-b35e-2f4791d914c4.png)

23\. Now, We can’t run the lambda function manually on a daily basis.

So, to automate this. We will add an Eventbridge rule that has a scheduler that will trigger the lambda function at the given time in the scheduler.

To do this, Click on **Create rule** by selecting **EventBridge Rule.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659109306/e55bf69e-0bae-451f-b30c-a997d7078d30.png)

24\. Select Rule type as **Schedule** and click on **Continue to create rule.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659110994/59e78b44-6e3b-41ad-ad84-97d68c2931bc.png)

25\. Here, We will schedule the time to trigger the lambda function.

In the below screenshot, We have provided the UTC time 11:20 IST which will be IST 4:50 PM and It will trigger on the 1st day of the month every year at 4:50 PM IST.  
You can schedule the time according to you.

For testing purposes try to schedule time while you implement it.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659112669/d951f99e-ec5d-4de4-9038-e17f2925b2f8.png)

26\. Now, add the **Lambda function** as Target which will trigger the **AWS-Cost-Reporting** Lambda function, and click on **Next**.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659114084/c5d13ae8-1325-48ea-972e-085185ca38fe.png)

Now, you can see the rule that we have created in the given screenshot below.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659115592/9c01c8fb-89e0-4c39-be24-b081791ff15c.png)

According to the EventBridge rule, the Lambda function will trigger at 4:50 PM IST.

Currently, it is 4:40 PM on my clock. Let’s wait for 10 minutes and see if we receive the expected email or not.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659117363/17e2193f-181f-47a8-93b3-a118be66e1f2.png)

Hurray, we received the message at the expected time.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659119274/b2aac9ee-97fa-4d62-adea-f8b62be67623.png)

**Conclusion:** Automating AWS cost reporting and email notifications is a valuable step towards effective cost management in the cloud. In this blog post, we have discussed two implementation options: manual setup through the AWS Management Console and infrastructure-as-code using Terraform. You can choose the approach that best suits your needs and preferences.

Whether you opt for the manual setup or the Terraform implementation, following the instructions provided will enable you to automate the generation of cost reports and receive timely email notifications. This will empower you to make informed decisions and optimize your AWS costs efficiently.

Feel free to explore the blog post and the accompanying resources, and start implementing the automated AWS cost report generation and email notification system today.

Feel free to reach out to me if you have any doubts or queries.

**LinkedIn**\- [https://www.linkedin.com/in/aman-devops/](https://www.linkedin.com/in/aman-devops/)

**Github**\- [https://github.com/AmanPathak-DevOps/](https://github.com/AmanPathak-DevOps/)

Happy Learning!

Aman Pathak