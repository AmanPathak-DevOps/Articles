---
title: "Azure Pipeline for Azure App Service"
datePublished: Fri Mar 03 2023 05:58:53 GMT+0000 (Coordinated Universal Time)
cuid: clqned467000c08jo9j7whkzz
slug: azure-pipeline-for-azure-app-service-acfef3efa751
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703658632535/dee6bbfa-6ae9-4435-88e3-c49f306d5367.png

---

### **THE CONCEPT:**

High Overview

**Github Link for Project:**

[**DotNet Project**](https://github.com/AmanPathak-dev/DotNet-Project-Azure-Pipeline.git)

### **Azure App Service**

1.  We have to create three Azure App services for three different Environments Dev, QA, and Production.

Create Azure App Service first where we will be going to deploy our dotnet Project.

*For Configurations refer to the below Screenshots.*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658568881/d0698a71-f905-46dc-b93e-a00174bbb32b.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658570647/3cf56f9a-5107-472c-98da-170621305afa.png)

*Now, the Resource has been created.*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658572041/0d12d4c4-b5b2-4182-868d-464db2387d42.png)

*In the end, we will have three app services for three different environments dev, QA, and Production.*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658573584/e2278d16-be43-4597-8271-5076b72430e5.jpeg)

### **Azure Project & Repo**

2\. After Creating the Azure App services. Go to the devops.azure.com Where we have to create the Organization first. After Creating the Organization, we are going to create the project as you can see below.

***The project must be Private.***

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658575211/f55bc57d-5d8f-4500-8152-2f88827e8f61.png)

*The Project has been created.*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658576638/569a2876-bb9d-41e3-992f-da25e2d39f02.png)

3\. Now as per the flow of the Azure Pipeline, We are getting the source code from Azure Repo itself.

Now, Go to the Repos and follow the below steps:

*   Copy the repository link.
*   Generate the credentials for the repository to push the changes on the repository.
*   Clone the repository on your terminal.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658578095/dbddf5ae-6111-4c4c-adaa-f4a26c0fef67.jpeg)

4\. Here, We have cloned the repository on our terminal and added our project to the cloned repository, and pushed it to the repository.

I have used a demo project, you can also use it.

[Azure-Pipeline-Demo-Project](https://github.com/AmanPathak-dev/DotNet-Project-Azure-Pipeline.git)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658579520/62958262-dc49-4919-9d9e-1b31df119605.jpeg)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658581225/011fc37f-215c-47d9-b886-1fae385cb3f3.png)

5\. Here, We have uploaded the Code successfully.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658582685/ba7da39f-e11d-4eb9-8f79-ed7ece81f934.png)

### **Azure Build**

6\. Now, We will have to build our DotNet Project.

*   Click on “Pipelines”
*   Choose “Azure Repos Git”
*   Select the Repository which pushed to the Azure Repos recently.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658584229/fc5a7f83-f27a-48c5-b36b-ea224f16f4ea.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658585869/4d6d6a16-ac63-4117-9461-e06468df14d6.png)

7\. The buildspec file is the most important file to build any project where we write the process that how the project will be built and where to push the artifact as well.

*You can refer to my buildspec file. Also, Azure has a good*

*feature to create the buildspec file without making any major*

*complications as compared to AWS CodeBuild.*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658587388/16a11590-9841-4627-a565-57db695e4cc4.png)

8\. Now, run the build and watch the live logs where you will see the process of build and, in the end, the artifact must be created and stored in the given directory.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658589199/185ab278-1c24-4a40-bf33-003da352d78e.png)

*The build has been successful and you can validate the artifact by clicking on “artifact”.*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658591175/f12b7e77-87c3-491c-b0c2-8a6b510e3168.png)

### **Azure Release**

9\. Now, We are going to deploy our code to the Azure App Service which is easy as compared to Azure VM.

*   Click on **“Create release pipeline”.**
*   Select a template as given below.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658592891/7c32346c-c116-4d89-bc2d-e2d907214696.png)

*   To get the artifact, click on “Add an artifact” and select the “Source Alias” and click on add.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658594666/a1782865-87ef-4c2d-ba2a-71057d4ffaf3.png)

*   Now, you have to choose the app type and you can choose anything from Windows or Linux.
*   Add the app service which you have created.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658596211/f027bdd8-3604-4a58-900c-8e077c96b1c4.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658598110/3c238ffc-9993-4309-be39-5c3a523c87f0.png)

10\. Before going to the next step we will get one error while doing the deployment where we have to raise the request to run the Azure Pipeline. So If we get an error like this **“No hosted parallelism has been purchased or granted”** in the release logs. So. follow the below link and solve the issue.

[How to resolve “No hosted parallelism has been purchased or granted”](https://k21academy.com/microsoft-azure/how-to-resolve-no-hosted-parallelism-has-been-purchased-or-granted/)

11\. To start the deployment, Click on **“create a release”.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658599622/0a432bca-b6c7-48de-a08f-03ddfdd6679a.png)

12\. If you want to automate the deployment of a specific environment then, click on your stage name if the border turns blue then, it will automate else you can choose the Stage name from the drop-down menu and click on create to start the deployment.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658601016/e7ea6203-aea9-49ac-a4b1-a06b50c60949.png)

13\. As we can see below, the deployment for the dev environment has succeeded.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658602742/0c9c9972-02ea-415c-9eed-c85394ee59da.png)

14\. But to check whether the deployment of the dev environment has succeeded or not use the **“default domain”** link on your browser.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658604761/50505921-121d-4323-83cf-105ed047bdd9.png)

*The deployment has succeeded for the Dev environment.*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658606434/71d1beb2-b0af-47ae-8640-c67e8e4c0b1e.png)

15\. Now, We have to deploy our code in a QA environment where all things will remain the same as in the dev environment. And get the Azure App Service for QA environment **“default domain”** link and check the deployment.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658607865/942205f6-3b25-4dd1-af68-2a1ba5c64299.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658609651/2555b391-2686-45da-9734-6eba12a8e1f4.png)

*The deployment has succeeded for the QA environment.*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658611306/1b992460-e58e-4b4e-ad21-b8d7f7ed2960.png)

If the deployment is for dev and QA environments then, the downtime will not be a problem but in the Production environment, there should be no downtime.

So to get rid of this downtime, we will use the Deployment slot feature which is available in the Azure app service which will help to minimize the probability of downtime.

To get a better understanding of the Deployment Slot feature:

\-> [Deployment Slot](https://learn.microsoft.com/en-us/azure/azure-functions/functions-deployment-slots#:~:text=Azure%20Functions%20deployment%20slots%20allow,to%20a%20slot%20on%20demand.)

*Azure Functions deployment slots* ***allow your function app to run different instances called “slots”****. Slots are different environments exposed via a publicly available endpoint. One app instance is always mapped to the production slot, and you can swap instances assigned to a slot on demand.*

16\. Go to the Azure App Service for Production environment -> Deployment slots, and add the slot.

Here, I’ve created a new slot named **staging**.

So, when the deployment of Production with the new version of the code is in progress then the application will not have downtime because the **staging** slot will be handling the previous version of the code.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658613082/85ed83c9-2da1-4af2-96b4-f495ee8f3cf0.png)

17\. Now, We will create the deployment for the Production environment.

Choose the App service for the Production environment.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658614584/271f4301-73f9-4e0f-af48-69753e5d2899.png)

*In a Production environment, We don’t want to do Continuous deployment without Human Intervention. So before starting the Code deployment for a Production environment, the authorized user would have to approve the request for the Deployment.*

*Add “****Agentless Job”*** *by clicking on three dots in the line of* ***“Production Deployment Process”***

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658616233/1bf698e2-c5a9-4671-8918-e1628e15ad9d.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658617887/da0f3388-91e4-41b2-a26b-039f4c4112da.png)

*Now, We have to create two jobs, first will be normal where we will deploy our code on the Production slot in the Production environment.*

*Don’t forget to check the* ***“Deploy to Slot or App Service Environment”*** *and add the first slot which created automatically.*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658619741/8f532396-0ceb-4e5f-8052-eb4eafdac2d6.png)

*Now, We need to add the Swap slot by selecting the template name* ***“Azure App Service manage”****.*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658621263/831a3a7b-b5cf-4792-8e29-c3a743b8e060.png)

*Add the slot we created in the Production App Service named Staging slot.*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658623043/e4023569-8dce-4250-be8c-f99728227868.png)

*The Final Pipeline will look like this for three different Environments.*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658624522/8c1a94bd-e742-4946-821b-c22ec695c1bc.png)

*Let’s run the entire pipeline for all environments.*

*As you can see the pipeline has succeeded.*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658626181/33e0aec4-e2cb-47b2-bb46-5d1b41ab35e4.png)

*Now, check the final output of all the different environments.*

**Development Environment**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658627702/6c6c9108-eb79-4659-a856-2d4e33e29c6f.png)

**QA Environment**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658629542/7eb01e75-8116-4cef-8cc5-33fd1d43685d.png)

**Production Environment**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658631066/bb5b01f7-6eb5-4d31-b5dd-09c2c10324de.png)

### Stackademic

*Thank you for reading until the end. Before you go:*

*   *Please consider* ***clapping*** *and* ***following*** *the writer! 👏*
*   *Follow us on* [***Twitter(X)***](https://twitter.com/stackademichq)*,* [***LinkedIn***](https://www.linkedin.com/company/stackademic)*, and* [***YouTube***](https://www.youtube.com/c/stackademic)***.***
*   *Visit* [***Stackademic.com***](http://stackademic.com/) *to find out more about how we are democratizing free programming education around the world.*