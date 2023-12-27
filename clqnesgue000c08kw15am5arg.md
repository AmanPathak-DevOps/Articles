---
title: "AWS Codepipeline for EC2"
datePublished: Wed Feb 22 2023 11:30:18 GMT+0000 (Coordinated Universal Time)
cuid: clqnesgue000c08kw15am5arg
slug: aws-codepipeline-for-ec2-4d5a53e89d84
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703659348540/6ca7a48c-bbba-45b7-88f7-127d5b6a1e1a.png

---

Github Link for Source Code: [https://github.com/AmanPathak-dev/AWS-Codepipeline.git](https://github.com/AmanPathak-dev/AWS-Codepipeline.git)  
I’ve used Project1 for this tutorial.

**THE CONCEPT**

**CODE COMMIT**

1.  Create a Repository in the CodeCommit.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659256172/0e3aa56b-0db7-4c45-a07c-35e16bf512ea.png)

2\. Click on “Clone URL” to get the HTTP link of the repository.

But to clone the repo there is a pre-requisite that you should have the credentials of the code commit to clone the repository and other functionality regarding git(push, pull, etc).

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659258179/28742e3f-46da-4ffb-860a-afe951824739.png)

3\. So, Create the User by going into the IAM.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659259714/303bc07d-0d25-4d08-8371-a2ede245cadf.png)

4\. You don’t need to do anything like add a user to the group. But as you need this user’s credentials for code commit. So, there is one more thing to play with the code commit which is IAM Roles and policies which will be added in the next step.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659261314/1114a184-bfca-4656-a84f-0a9141dde5f5.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659263273/cb7a2b7a-9285-4cf2-af96-55ed15099c4c.png)

As you can see, the user has been created. Now click on it and you will get option of “credentials”.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659264696/9ea18b10-5a9f-49e2-9135-5bdb0bb87ae5.png)

5\. Now, click on the Generate credentials button.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659266076/f2122c05-50fc-4b97-bc9a-e8d82a87f872.png)

6\. Here you get the Credentials for the Code Commit. Also, download the .csv file for future reference because once this credentials will disappear, you can’t see it again.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659267665/f5eb0158-f894-4ac3-be4d-34395bdb1959.png)

7\. Here, you can see the credentials from the downloaded .csv file. Keep it for future reference.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659269174/24d2fb7d-22cd-4f7f-b364-ccda422acc77.png)

8\. Now, As the AWS CodeCommit is private So, we need to clone the repo in our local and then push the changes. But to do that, you must have the Policy which name “AWSCodeCommitPowerUser”.

This will help you into git commands authorization.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659270924/d780235e-8100-4456-a5a7-10895026ed2b.png)

9\. Now, When we will clone the repo in VSCode Editor. Then, there is an authentication will be required where you need to pass the username and password which we’ll take from the .csv file or if you don’t remote Step 6 Screenshot.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659272373/e9777add-dae5-4313-b619-0f6de5cb0a12.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659273793/cd8567ae-ef0e-4c9d-beab-588aa037a284.png)

*Here, we cloned the repo successfully.*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659275016/787e196d-6df8-44c2-9e46-e02a27aa8f9a.png)

10\. Now, I have selected a simple project from tooplate website and move it here.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659276322/ed4f42d3-949b-4699-bf02-3d140d48b848.png)

11\. Here, I’ve pushed the Project on Code Commit Repo.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659278161/ec3c385f-3eea-4e0c-b72f-50b9e65d9f7c.png)

**CODE BUILD**

12\. Now, The Next process is to Create the Code Build.

So, to do that We will provide the requirements as mentioned below.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659279954/5007017a-c21b-46a6-bc6c-12dd9d184687.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659281292/2eff0508-6439-41b6-a83d-b211b0c484e3.png)

*Here, I am creating the role for the Code Build because Code Build needs some access such as of CodeCommit, S3, and CodeDeploy.*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659282687/bf65e1ba-9cf6-4662-a29a-55fbe3e90f20.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659284545/ad489648-793e-4ebf-b617-6952313f2585.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659286025/ce2201ae-9da0-4631-b24a-6f5867b95335.png)

Here, I didn’t have provided the file name of buildspec because it will search for the file name builspec.yml by default. So, you don’t need to mention that.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659287904/6b3f5279-3d51-4838-938d-efceb2907660.png)

Here, We need to Configure the artifacts stored path. (Related to the last Screenshot).

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659290046/53658b83-0508-44a5-884b-9bab114ab797.png)

13\. Now, We will write the Code Build file named buildspec.yml and push it on the Code Commit Repository.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659291583/2409c951-9979-4cc5-93fa-423d05f832ba.png)

*Here, you can see that the buildspec.yml file has been pushed to the CodeCommit Repository.*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659293185/473cc76f-8ac0-40c4-87b9-bd87b9dba28b.png)

14\. Now, We are ready to start the build by clicking on “Start Build”.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659294572/b992dfb6-8cdd-43a7-b423-627db92d908b.png)

*Here you can see the build details in “Phase Details” where the build has been successful. So, we can proceed with CodeDeploy where the code will deploy to the EC2 Instance.*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659296102/eef44993-441c-4611-a7ee-5ef1224a026e.png)

**CODE DEPLOY**

15\. Go into the AWS CodeDeploy Service and click on “Create application”.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659298074/690be124-0663-4bf7-97b2-641ea9d4ee2e.png)

16\. Enter the application name to your application As we are deploying the Code on the EC2 Server. So we will select the EC2/On-premises option under the Compute Platform Section and click on Create application.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659299545/4cb34dd7-50bb-4d4a-ba96-0c62f6007e49.png)

17\. After creating the application, we have to create a deployment group by clicking on “create deployment group”.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659301115/6c3d0eba-4ac3-4644-8513-1c0bd1c865e1.png)

18\. But before creating a deployment group, we have to create a server where the code will be deployed.

So, for that, we will go into the AWS EC2 Service, and I’ve created Ubuntu 22.04 Server.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659302751/97d477f7-ecb9-41ed-af14-69103fcd53da.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659304219/f9ba3d34-0bf6-416d-9cf7-0261a9031711.png)

*Here I’ve passed user\_data to install the codedeploy-agent which is a must and the dependencies must be compatible with the selected server.*

*If you get any error at the start of your code deploy. So, the root cause of it will be your script where you didn’t have installed codedeploy-agent properly hence the codedeploy-agent is not running.*

*You can use this script if your server is Ubuntu22.04.*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659305897/f3a3660a-c3eb-4503-bf80-ccbddece2738.png)

19\. After Installing the codedeploy-agent, We will create the deployment group.

Here the other main thing is the service role where the Code deploy needs access to the selected service to deploy the code on the EC2 Server.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659307503/457d4b39-f2bb-47a1-bc6a-2290bdab722a.png)

*Here you can see the selected Policies name which is required to deploy the code on the Server.*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659309035/140eb00c-1cf4-44e3-8a1c-42b71e5d4e01.png)

20\. Now, select the EC2 Sever where you’ve installed the codedeploy-agent.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659310648/663b7819-7b9a-467c-84fc-81d1b5dded55.png)

*Here you’ve to choose Never for “Install AWS CodeDeploy Agent” because we have already installed the codedeploy-agent manually and disabled the load balancer as well.*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659312397/0af2081b-0abd-4108-a9ae-b633eebd68cc.png)

21\. After Creating the deployment group, We will create the last thing here which is deployment only.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659314354/bd896d55-9c9b-4ff6-b5dc-1706f544af42.png)

*Here, we will select the Deployment group which we have created already and the Revision Tye will be “My application is stored in Amazon S3” because we have uploaded the artifacts in our AWS S3 Bucket.*

*Also, we will get the S3 URL where the artifact has been uploaded and the zip file name as well.*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659315836/831ff1a6-6be4-4965-84e0-1c372fe65a21.png)

22\. To Deploy the Code using AWS Code Deploy, We need another file named appspec.yml where we write the script to deploy the code on the server.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659317393/4616a1c7-e509-4f0f-a617-6a56dc319a9f.png)

23\. Now, We’ll push the appspec file to the Code Commit Repository.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659318841/9b4c4774-457c-45a3-addc-967d30132ca7.png)

24\. But there is one more thing we have to do.

As you know that the Code Deploy needs access to the EC2 Server to deploy the code.

But we need to also add the IAM Role to the EC2 Server where EC2 will give permission to the Codedeploy to deploy the code.

So, to add the IAM role to the EC2 Instance.

Select the EC2 Instance -> Click on Security -> Manage IAM Role and add the role which we created in Screenshot 25.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659320930/2fa9d610-9e2e-4756-9b76-00fa0fded894.png)

25\. Here, we have created the ECInstanceRole for the deployment of the code.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659322472/efb4e308-020d-4ab0-a00c-6e141c27e677.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659323884/4112f970-8b80-4fa2-a00f-d513e73e9616.png)

26\. After creating the IAM role you’ve to restart the codedeploy-agent service again.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659325452/83d2f9ac-6f06-4eae-98df-47a9ed8bc5bf.png)

27\. Now, Before starting the Code deploy deployment. We need to Code Build again because the Code has been updated for appspec file. In the previous successful build, the appspec file has not been uploaded or included in the artifacts.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659327213/4003b2e0-6ea8-4fb4-bc0c-ca1139a461c2.png)

*Now, The new artifacts have been created in a zip file. So you can proceed with the CodeDeploy.*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659328707/6996bdd5-be2d-49f9-8f87-d0a15867098b.png)

28\. Here, When we start the deployment process. So As you can see, It is successfully completed.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659330448/8715aff3-1302-43ac-bebd-1d822a59f234.png)

29\. Now, Copy the Public IP of your EC2 Server where you’ve deployed the code. You’ll see your website or content as an output.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659331959/7b07e623-a910-4523-94fb-0837db1129b2.png)

**AWS CODE PIPELINE**

30\. But there is an issue, what If I want to automate the entire process?

So, to do that. We’ll create AWS Codepipeline and with the help of this, We’ll Automate the entire process.

To do that, Go on to the AWS CodePipeline Service and click on “Create Pipeline”.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659333319/99084970-c714-4e11-9d7f-3a5601d9ebb5.png)

31\. Now, Give the configuration given below for the Pipeline Settings.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659335608/e15d5cec-b28b-4135-9acf-3f6cb29517fb.png)

32\. Now Add the CodeCommit as the Source provider and provide the required configurations as given below.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659337320/8f5952ee-ba2f-4c39-a615-27c76ffa9e00.png)

33\. Now, Add the Code Build as a build provider and provide the required configurations as given below.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659339343/cc6e3523-fec5-45b2-b8c9-86a0072427da.png)

34\. Now, Add the AWS CodeDeploy as Deploy Provider and provide the required configurations as given below.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659341342/5dbb64dd-87ab-4b78-94b6-409fb49ba4ce.png)

35\. The Final Configuration will look like this.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659343161/4eaee143-293c-4116-9d62-6c2ed00d54bf.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659344756/347190ca-b02e-4479-96ea-5a1e8c4f034a.png)

36\. Now, you have to just run the pipeline and here you can see the pipeline has run successfully.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659346718/66e9c437-446e-42f7-8863-23efcc8ef377.png)

Hope, You’ve Understood Everything.