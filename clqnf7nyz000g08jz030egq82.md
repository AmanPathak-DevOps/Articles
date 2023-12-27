---
title: "Deploying Awesome App on AWS Serverless Services — Step-by-Step Guide"
datePublished: Fri Nov 17 2023 16:16:37 GMT+0000 (Coordinated Universal Time)
cuid: clqnf7nyz000g08jz030egq82
slug: deploying-awesome-app-on-aws-serverless-services-step-by-step-guide-54bc89e4d236
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703660056922/3d38d71c-347c-4b4e-b37e-9f6ef341bbed.gif

---

AWS Infra Diagram

Welcome to the comprehensive guide on deploying Awesome App, a full-stack AWS Serverless project. In this tutorial, we’ll walk through the process of storing the frontend app content in AWS CodeCommit, deploying it using AWS Amplify, setting up a DynamoDB table, creating Lambda functions, configuring API Gateway, and finally, adding a custom domain to the AWS Amplify Application.

### Introduction

Awesome App showcases the power of serverless architecture by seamlessly deploying frontend (HTML, CSS, JS), backend (Python), and a DynamoDB table on AWS Serverless Services. Follow these step-by-step instructions to deploy your own version of Awesome App and leverage the benefits of serverless computing.

### Prerequisites

Before we dive into the deployment process, ensure you have the following prerequisites:

*   An active AWS account with appropriate permissions.
*   Familiarity with AWS services such as CodeCommit, Amplify, DynamoDB, Lambda, and API Gateway.
*   Basic knowledge of Python for Lambda function configurations.

### Implementation Options

You have two implementation options to deploy Awesome App based on your preference:

### Manual Setup

If you prefer a hands-on approach and want to understand the individual components and their configurations, follow the steps outlined in the main guide. This approach is suitable for users who want to explore and learn the details of each service.

### Terraform Setup

For users who prefer infrastructure-as-code and want to automate resource provisioning, a Terraform implementation is available. The Terraform code and instructions can be found in the [GitHub repository](https://github.com/AmanPathak-DevOps/Terraform-for-AWS.git). Follow the provided instructions to set up the cost management system using Terraform.

*   Terraform Setup: [Link to Terraform Setup Guide](https://aman-pathak-devops.medium.com/terraform-deployment-of-awesome-app-on-aws-serverless-services-step-by-step-guide-7241b01770e0)

### Step-by-Step Guide

We have to store our front-end app content. To do that, we will use AWS CodeCommit.

Click on **Create repository.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659936856/c2a13410-820d-4332-ad85-b62bd8ee6531.png)

Enter the name of the repository and click on **Create.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659938467/094a8ba9-0865-4916-a99e-b134867407b8.png)

Now, we have some instructions on how to use the repository.

Let’s follow the instructions.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659940528/dfb6a323-6275-4c57-a650-48f94d609c14.png)

In Step 1, we have to generate our credentials to push and pull the repository.

Navigate to the IAM Services -> IAM Users.

Now, click on your user and click on the **Security Credentials** tab.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659942298/13be7b31-77e8-4163-a468-cc2cddb07382.png)

Click on **Generate credentials** to generate the credentials.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659943688/10a98916-c218-4eef-a1ac-7799e936392b.png)

Once you click on the **Generate credentials.** You will get a popup like the below screenshots in which the credentials have been given.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659944995/0bb2bb0b-edff-494b-8fa5-97fd80683ea9.png)

Now, go to the instructions page of the AWS CodeCommit service and copy the step 3 command to clone the repository.

You have to provide the credentials that you have generated in the above steps.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659946515/24b66e85-6de8-44c2-b369-5627deae5e8d.png)

You can validate by checking whether the directory is present or not.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659948326/6000b003-924d-4e8c-b556-a877cc25ec76.png)

Now, we need our frontend code to push it on the CodeCommit repository.

Download or Clone the entire code which includes frontend and backend both code.

Source Code Repo(Backend)- [https://github.com/AmanPathak-DevOps/Cloud-Serverless-Project/](https://github.com/AmanPathak-DevOps/Cloud-Serverless-Project/tree/master/Frontend)

So, copy all the files of the frontend directory in your CodeCommit repository and push it on the CodeCommit repository.  
You can refer to the below screenshot to push it on the CodeCommit repository.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659950238/fa364619-65fe-4c09-b003-f47a69583a16.png)

You can validate whether the code is pushed or not on the CodeCommit repository.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659951826/fcbfcf5d-6dae-4d98-afa0-e01cb09c2c78.png)

Now, we have to deploy our frontend code to the AWS Amplify App.

Click on the **New app**.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659953289/781c87da-77aa-4b86-8cca-08847a2d80ba.png)

As our source code is on CodeCommit then select it and click on **Continue.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659954798/c15ec212-d7e1-47c2-9293-a8bd4fdc1587.png)

Select the repository that we have created earlier and click on **Next.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659956029/2df78ac7-0db5-4eec-9ef0-33a327805984.png)

Enter the name of the **App name** then, **check** the ‘Allow AWS Amplify to automatically deploy all files hosted in your project directory’ and keep the other things remaining the same, and click on **Next.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659957491/17277fac-5165-41be-bd10-f7e8b7ea45df.png)

Click on **Save and deploy.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659959241/9614f3c5-922a-498f-ba77-9120fe4e625d.png)

As you can see in the below screenshot, our application is deploying.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659960706/b88abf00-ace1-44b2-8ab3-67ebb0ce811b.png)

Once it is deployed, you can validate the content by clicking on the **URL** which is showing on the left side under the window icon.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659962177/f0f96eac-98eb-4688-b59a-8da4201f2dcc.png)

But, you won’t be able to add the records in the database because the backend and database are not configured yet.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659964082/4d8ee093-ea7a-4276-be76-cdf51138503d.png)

Let’s create the DynamoDB table.

Click on **Create table.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659965952/ad0f792a-b0b0-4ca2-9c8b-4317397576b8.png)

Enter the name of your database table then **ID** in the Partition key and click on **Create.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659967638/c1dedb8f-dc27-4de0-9a95-aedbdb983346.png)

You can see our Table is in **Active** status.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659969385/c82442f5-b230-491a-95f9-0f0c7d848711.png)

Now, we have to create two Lambda functions one for Get data and the second for Post data.

Click on the **Create function.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659970921/1903c5c0-5dfa-44f7-9325-91e16acddf6c.png)

As we are creating lambda function for Post data. Hence, enter the name of lambda function like addStudent-function then select **Python3.10** as runtime and click on **Create function.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659972181/efa59e54-eb9a-4f00-89ec-9919aaf10917.png)

Now, paste the code of the backend for addStudent.

In the below backend code, take care of the DynamoDB table name. If your table name is different from mine then kindly change the table name accordingly and click on the **Deploy** button.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659973615/8ce82b8d-dbe6-46a8-958e-8f622a212b66.png)

Now, click on **Test** and provide the parameter like the below screenshot.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659975308/7389b90c-a95d-4e60-b0c5-81b0408aa783.png)

Once you save the test parameters and then test the lambda function, you will get an error like the below screenshot. Because the lambda function doesn’t have permission to perform any task with the dynamodb table.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659976610/0fb207a7-2f6e-4d99-856c-199cb9b32b06.png)

To provide the permission to the lambda function.

Click on the **Configuration** tab then select **Permissions** from the Lambda function and click on **Role name.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659978411/51617205-8a4d-4274-adde-f0573762e537.png)

Now, click on **Add permissions** then **Create inline policy**.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659980044/b167762d-e98c-4c88-9d47-4764640e02d6.png)

Select **JSON** format copy the below Permission and paste it into your Policy editor.

[https://github.com/AmanPathak-DevOps/Cloud-Serverless-Project/blob/master/DynamoDB-Policy/db-policy.json](https://github.com/AmanPathak-DevOps/Cloud-Serverless-Project/blob/master/DynamoDB-Policy/db-policy.json)

Make sure to provide the **DynamoDB** **ARN** according to your table and click on **Next.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659981700/8ab6a8b7-a200-47e1-a260-873b8a32ec7e.png)

Provide the name of our policy and click on **Create policy.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659983531/5128b3b3-d2da-4683-910d-eee75828d12f.png)

If you test the lambda function again by clicking on the **Test** button. You will see that you got a 200 success code which means our data is stored in the DynamoDB table.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659985204/4ed9853b-8bc5-42ff-b843-ad8a99f8fa4d.png)

To check whether the data is stored in your DynamoDB table or not.

Click on **Explore table items.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659986835/3a44adf0-91bb-4de7-9195-4a824c49a6c5.png)

You will be able to see that our data is present in the table.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659988318/3b6a5675-20ee-487a-89e9-10d9075e509d.png)

Now, we have to create one more lambda function to get all the records from the DynamoDB table.

Click on the **Create function.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659989614/c99f3df0-0809-42ca-86bc-365b45cad8b7.png)

As we are creating lambda function to Get data. Hence, enter the name of the lambda function like getStudent-function, select **Python3.10** as runtime then select the same execution role that we used for the first lambda function and click on **Create function.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659991376/933b2121-c973-4464-a9b9-90106e49a219.png)

Now, paste the code of the backend to getStudent and click on the **Deploy** button.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659992915/b7ed7ce9-8897-4667-a01a-55786e7fad65.png)

Don’t do anything and click on **Save.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659994650/6a2e2742-84d3-4830-8642-9881457443dd.png)

Once you click on **Test** you will be able to see the details from the DynamoDB table.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659996300/2da731de-0511-4ef4-bf9b-6977e42c3081.png)

Now, we have to create an API Gateway. So, that our frontend application can access the data from the backend.

Click on **Create API.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659998090/52ed43fc-329e-4689-acb7-eca594022a3e.png)

Select the **REST API** and click on **Build.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659999435/b861045e-38db-47e9-bcb7-ba3bf8f0474d.png)

Now provide the name to the API name and click on **Create API.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660000527/774b33b0-4ad7-4fa9-ab6d-1e33ff9282b7.png)

Now, we have to create the resource.

To do that, select the default ‘/’ and click on **Create resource.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660001894/146d6905-228c-44cb-aefd-d9589c2135b8.png)

In the **Resource name** enter the path like we are creating for addStudent and click on **Create resource.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660003261/221cbc3f-36d7-4f07-b09d-d1727a0c5c41.png)

You can see in the below screenshot that we have created an **addStudent resource.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660004614/625b0dda-8aa0-4d74-a2c3-7cb5bfbec579.png)

Now, we have to create another resource for getStudent.

To do that, provide the Resource name and click on **Create resource.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660005931/599a9b58-73d2-423e-a36d-cfa910ffd3d1.png)

In the below screenshot, you can see that both resources have been created.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660007213/44b27ea6-c0e8-4ed5-9be7-77b98c2da66f.png)

Now, we have to create two methods for both resources GET and POST.

As we have to create the POST method, select **/addStudent** resource and click on **Create method.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660008584/a1ee66b4-4fde-4a74-8de9-d781eb22f791.png)

Select the **POST** method with **Integration type** Lambda function and select the addStudent-function lambda function then click on **Create method**.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660010190/7b1be726-aa48-4a32-a1a0-bb47d180c6f4.png)

Now, we have to create a **GET** method.

To do that, Select the **GET** method with **Integration type** Lambda function and select the getStudent-function lambda function then click on **Create method**.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660011849/5e042dda-5797-4724-9868-67cfd7fa7645.png)

You can validate in the below screenshot that we have created both methods for both resources.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660013474/473490fa-698c-483f-8181-2c9f6c84b005.png)

Now, click on **Deploy API**.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660015075/85fb65c2-fbd9-4e43-938b-d74d8b7430b0.png)

To deploy your API you need to create the new stage.

Select New stage and enter the stage name according to you then click on **Deploy.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660016576/007f3d6d-554d-470c-92fc-14b4a940d1b6.png)

Copy the API URL like the below screenshot.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660018118/39af8ab4-24cc-4eb1-be52-59a52e15a301.png)

Now, open the index.html file from the frontend folder in the editor and paste your copied API URL.

If you observe that you copied only the parent API URL. But you have to provide the resource name for the GET method. In our case, that is getStudent.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660019815/91190fee-6f38-4ec2-869b-385224d08704.png)

Scroll down and you will see the second endpoint.

Paste the same copied API URL. But you have to provide the resource name for the POST method. In our case, that is addStudent.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660021256/06ad28e2-c7f2-4dd4-a1eb-c38deaced630.png)

As our frontend code has some changes, that’s why we have to push our changes to the CodeCommit repository.

To push the code, refer to the below screenshot.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660023032/b4d197b8-1e93-406a-b680-167dc2ad7566.png)

Once you push your code, AWS Amplify is triggered automatically. But in your case it doesn’t then you can deploy by clicking on redeploy the version.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660024552/69e97dd2-ed2e-4e0d-9229-842a960303d6.png)

Now, we have to enable the CORS to get access to the API Gateway.

Select the resource /addStudent and click on **Enable CORS.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660026187/860c44b2-b316-4afc-9132-a4f797d35d9a.png)

Check the **POST** method and click on **Save.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660027480/7861dca1-2f9b-49a3-80c7-56e4b9a83743.png)

After enabling the CORS of /addstudent resource, you will observe that the new method is created named **OPTIONS.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660028721/5b0b3040-354f-403d-a48c-725e5243eef2.png)

Now, we have to enable the CORS to get access to the API Gateway for /getStudent access.

Select the resource /getStudent and click on **Enable CORS.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660029978/f6ad328a-6608-471c-b649-77af2bb922ed.png)

Check the **GET** method and click on **Save.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660031603/eb2f0031-0847-45de-8793-00b2d0493888.png)

After enabling the CORS of /getstudent resource, you will observe that the new method is created named **OPTIONS.**

After enabling CORS for both resources, we have to deploy our API again by clicking on **Deploy API.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660033640/0abbb085-9973-492c-9ee9-6166cb1f8a76.png)

Now, wait for some minutes and then, click on the AWS Amplify app URL.

Then, you will see that our application getting data from DynamoDB.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660035585/91cc90e6-6202-4183-8e27-ebc347902ff0.png)

Now, we have to add our custom domain to our AWS Amplify Application.

To do that, we will use AWS managed service Route53.

Click on **Created hosted zone.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660037323/b5e4d01f-ac2e-4e9c-baf7-236fe057f65d.png)

Provide the domain name that you have and click on **Create hosted zone**.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660038635/88a0e7dc-daf0-4a71-8729-b7554cd1019c.png)

Once the hosted zone is created you will get Name servers.

Now, copy all the name servers.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660040192/faec90cc-2b76-4a98-9885-a2f9728c4f91.png)

Go to your domain provider and click on NS.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660041627/b712aa08-9261-4f30-a9cd-3819f177b1ab.png)

Now, paste the Name Servers that we have copied in the earlier steps into your domain provider NS.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660043114/22c9f138-c3e9-45da-8f0b-adf6026050dd.png)

Click on **Domain management** which is showing in the left pane.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660044738/562540f1-59f2-4899-849c-7cb856cff325.png)

Click on **Add domain.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660046158/a8a7f8d1-b3f5-4327-9ba8-65977654433e.png)

Select the domain name that you have created through Route53 and click on **Configure domain.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660047396/a90c8a6c-dc6b-41b5-be1c-eba952935770.png)

Click on Save.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660049138/74a4a0df-6d56-445b-aa96-af4a0a5648a8.png)

Now, AWS Amplify will do the configuration itself to add the SSL certificate and domain to the application.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660050477/a2386396-f016-4a75-8b03-b1a60545ec29.png)

If you check in Route53, you will see AWS Amplify created three more records to add a domain to the application.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660052057/69d1478d-4852-4776-8170-a76ac12d4962.png)

Now, if you hit your domain from the browser. Then, you will be able to see the content with the SSL certificate.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660053634/af38a3af-6162-4971-9417-1c12fba094b9.png)

I have added one more record which means to validate whether the application is perfectly running and responding.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660055419/8840c7fa-6b5c-4f28-a77b-ea38cdb67d7c.png)

### Conclusion

Congratulations! You have successfully deployed Awesome App on AWS Serverless Services using the chosen implementation option. Your application is now live with serverless infrastructure, providing a scalable and cost-effective solution.

Feel free to explore further customization and optimization options based on your project requirements. If you encounter any issues or have suggestions for improvements, refer to the Contributing section in the README.

Happy coding!

**Feel free to reach out to me if you have any doubts or queries.**

**LinkedIn**\- [https://www.linkedin.com/in/aman-devops/](https://www.linkedin.com/in/aman-devops/)

**Github**\- [https://github.com/AmanPathak-DevOps/](https://github.com/AmanPathak-DevOps/)

Happy Learning!

Aman Pathak