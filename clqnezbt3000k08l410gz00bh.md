---
title: "How to Automate ReactJS Deployment with CircleCI and AWS S3?"
datePublished: Sat May 06 2023 11:18:41 GMT+0000 (Coordinated Universal Time)
cuid: clqnezbt3000k08l410gz00bh
slug: how-to-automate-reactjs-deployment-with-circleci-and-aws-s3-8bb1d6b72b26
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703659669208/f90c6aee-b2b0-403c-b2fe-aaf5cf358949.png

---

**What is CircleCI?**

CircleCI is a cloud-based continuous integration and continuous deployment (CI/CD) platform that helps developers automate their software development process. It provides a user-friendly interface to set up automated testing and deployment pipelines.

**How CircleCI is better than Jenkins?**

Compared to Jenkins, CircleCI is easier to set up and manage as it is a cloud-based solution that doesn’t require any server setup or maintenance. CircleCI also provides a more intuitive user interface and better integration with modern technologies like Docker and Kubernetes.

**CircleCI Advantages-**

The advantages of CircleCI include its ease of use, scalability, and ability to integrate with various tools and technologies. It also provides fast build times and supports parallel testing.

**CircleCI Disadvantage-**

However, its cloud-based nature can be a disadvantage for companies that require on-premise solutions or have strict security requirements.

**How Jenkins is better than CircleCI?**

Jenkins, on the other hand, is a more mature and widely used CI/CD tool that offers more customization options and flexibility. It provides an open-source nature, a vast plugin ecosystem, and the ability to work with any programming language and platform. Jenkins also provides a higher degree of control over the CI/CD process, making it suitable for complex projects or organizations with specific requirements.

**Jenkins Advantages-**

Its advantages include its open-source nature, vast plugin ecosystem, and ability to work with any programming language and platform.

**Jenkins Disadvantage-**

However, its complex setup can also be a disadvantage for smaller teams or organizations with limited resources.

**How to deploy ReactJS application using CircleCI**

To create a new IAM user, Click on **Add user.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659646238/95f3b379-3d5f-43f8-969e-2b407680d3dc.png)

Add the **User name** and Click on **next**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659648038/b7e9db59-f6b7-4041-a40a-d5b14b86cfde.png)

Here, we have to create the **user group** for the created user.

We will deploy our reactJS application on the AWS S3 bucket. So, we need only **AmazonS3FullAccess**.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659649603/4dc60e35-4706-49fa-96e1-d6306c4e40f5.png)

Select the Create Group name and click on **Next.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659651420/37f5daeb-7cd1-4655-a778-d3114f670b21.png)

Navigate to the IAM user->**Security Credentials** tab and click on **Create access key** to generate the access and secret access keys.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659653081/8320ab8b-dbe6-4251-9a60-9c56fd9702d4.png)

**Let’s do the CircleCI Setup**

If you are new, click on SignUp with Github so that the repository will appear automatically for you to deploy its code.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659654531/e17300fb-500e-4f3c-a059-108841c45062.png)

Click on **Signup with Github.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659656043/b06ef1c6-6124-4586-86b1-147562eb2db0.png)

Here, you will see your Github Account username or profile.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659657531/b9cb7d79-fd7b-41aa-8199-9aeb6f44b1a4.png)

This is a dashboard of CircleCI. You can see here all the repository that is present in your GitHub Account.

My code is present in the **CircleCI-ReactJS** repo. So, I will click on the **Set up Project** in front of the **CircleCI-ReactJS** repo.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659659086/c175e6ff-21c6-44b1-aab4-b4dc478f1ba5.png)

My config file is present in the master branch. So, I mentioned it here on the **master** branch and click on the **Set up Project.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659660852/c7fe5cca-3426-44a5-be22-e5a93c6e28d6.png)

But, before going to build the Project, I need to add **Environment Variables** of the AWS account where I had generated the AWS access key and secret access key.

So, don’t forget to add the **Environment Variables** in your project otherwise the deployment will not get done.

You can add **Environment Variables** by clicking on the **Project Settings-> Environment Variables.**

Before building the project, it is important to add the **Environment Variables** for the AWS account that generated the AWS access key and secret access key. Make sure to add the **Environment Variables** in your project, otherwise, the deployment will fail. To add environment variables, go to the **Project Settings** and then click on **Environment Variables**.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659662503/fd9e7b48-d694-495f-8dfc-38f0c2e5edec.png)

The .circleci directory in my GitHub repository for the [CircleCI-ReactJS Project](https://github.com/AmanPathak-dev/CircleCI-ReactJS.git) Project contains the config.yml file. This file specifies the build process and deployment steps that CircleCI will use.

version: 2.1  
jobs:  
\# Builing the project is going to start  
  build:  
    machine:  
      image: ubuntu-2004:202010-01  
      docker\_layer\_caching: true  
    steps:  
      \- checkout  
      \- run:  
          name: Installing AWS CLI  
          command: |  
            sudo apt-get update  
            sudo apt install python3-pip  
            sudo pip3 install awsebcli --upgrade  
      \- run: cd ./app && npm install && npm run build  
      \- persist\_to\_workspace:  
          root: .  
          paths:  
            \- .  
\# Deploying the code to AWS S3 Bucket  
  deploy:   
    machine:  
      image: ubuntu-2004:202010-01  
      docker\_layer\_caching: true  
    steps:  
      \- attach\_workspace:  
          at: .  
      \- checkout  
      \- run:   
          name: Configuring AWS  
          command: |  
            ls  
            if \[ $CIRCLE\_BRANCH = 'master' \]; then  
              aws s3 sync ./app/build s3://${AWS\_S3\_BUCKET}  
            fi  
  
workflows:  
  version: 2  
  execute\_bulk:  
    jobs:  
      \- build  
      \- deploy:  
          requires:  
            \- build  
          filters:  
            branches:  
              only:  
                \- master 

If you have followed all the steps correctly, you should now be able to run your project and deploy it successfully.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659664138/2a715e0d-9eca-451f-95dd-366fb1a39a26.png)

To confirm that your build file has been uploaded to the S3 bucket, check the contents of the bucket. If you can see the files present in the bucket, then the upload was successful.

Next, go to the properties of the bucket and look for the “**Bucket website endpoint**” option at the bottom of the page.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659665840/3525e1c5-7bc5-4709-b862-a7a807064474.png)

After copying the **Bucket website endpoint**, my ReactJS application is now up and running.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659667673/239728ad-7e2c-4890-85f6-0ea881b3c7ca.png)

That’s all for now. I hope you have gained a good understanding of CircleCI and its advantages and disadvantages. You can use CircleCI to deploy projects for various technologies such as Angular, Java, and more.

Remember to keep practicing and exploring, and don’t hesitate to reach out if you have any questions

Github: [**https://github.com/AmanPathak-DevOps**](https://github.com/AmanPathak-DevOps)

LinkedIn: [**https://www.linkedin.com/in/aman-devops/**](https://www.linkedin.com/in/aman-devops/)

Happy Learning