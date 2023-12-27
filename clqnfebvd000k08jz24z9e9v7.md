---
title: "Deployed Java application using Maven, Sonarqube, Jfrog Artifactor, Jenkins, ArgoCD and finally…"
datePublished: Fri May 19 2023 07:01:44 GMT+0000 (Coordinated Universal Time)
cuid: clqnfebvd000k08jz24z9e9v7
slug: deployed-java-application-using-maven-sonarqube-jfrog-artifactor-jenkins-argocd-and-finally-ac25420dcdf4
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703660368274/fa25c6e7-2a1c-4ac0-a98f-d92e0d1d10db.png

---

**High Overview**

**Jenkins Server Setup**

1.  We have to configure **Jenkins**, For that We are going to create an **EC2** server where **Jenkins** will be installed. Also, We are going to install some other tools as well such as **Sonarqube** and **Docker** so, we need more **RAM** and **CPU.**

**Configurations:** t2.large, Ubuntu22.04, Security Group Port Open- 22, 9000, 8080, 8081, and 8082, Storage: 30GB gp2.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660265107/ea770873-94b6-4c5b-b95e-660851a0c1f0.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660266777/3e9a59f8-f4dd-4ea8-86bb-e1061ee878cf.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660268277/7370b5e3-a340-4ca5-b9e8-b1dc77d6d36f.png)

2\. After creating **EC2 Instance**, log in to the created machine using **ssh**.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660269793/feac6adf-543d-4d38-8484-70bfb1bad921.png)

3\. Install **Jenkins**, You can refer to the Github Repo where you just need to run the script or follow the command to install it on your machine.

**GitHub Repository**: [https://github.com/AmanPathak-DevOps/Scripts-Installation.git](https://github.com/AmanPathak-DevOps/Scripts-Installation.git)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660271426/27874e32-54a4-43fb-aaad-8ab92df96a6f.png)

You can see below **Jenkins** has been installed.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660272983/42352f4b-046d-435d-95ba-51a3aa80a12b.png)

4\. Now, browse the Jenkins server with the help of **public\_ip:8080.** Make sure, you have opened port number 8080 in the instance’s security group.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660274984/fdebfce4-7017-4584-85f7-933c2abe2a0f.png)

5\. Get the password using the command **/var/lib/jenkins/secrets/initialAdminPassword.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660276605/d9d26ff3-3a44-49b6-897d-9cf853a7ad99.png)

6\. After entering the password, Click on **Continue.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660278374/91ae4ebf-f9f9-414b-a719-42e552b504cf.png)

7\. Click on **“Install suggested plugins”**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660279750/f637724d-bb5f-4fdc-b24f-e24f78cf067c.png)

8\. For now, you can proceed by clicking on the **“skip and continue as admin”**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660281262/36c4a305-604f-4b37-86fa-db66c4edc0ea.png)

9\. The setup is completed.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660282957/ebe042ac-43c4-48ba-9107-5b68569bfc3c.png)

10\. Install **the Docker Pipeline** plugin

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660284677/d416bbcf-5f30-41b9-badd-838865c10132.png)

**Sonarqube Server Setup**

11\. Install the **Sonarqube Scanner** plugin

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660286236/8a88f0f3-23cc-4cf1-a55d-afb3ca93af01.png)

12\. Install **Sonarqube** on your EC2 Instance or the local machine.

You can refer to the Github Repo where you just need to run the script or follow the command to install it on your machine.

**GitHub Repository**: [https://github.com/AmanPathak-DevOps/Scripts-Installation.git](https://github.com/AmanPathak-DevOps/Scripts-Installation.git)

*apt install unzip*

*adduser sonarqube*

*wget* [*https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.4.0.54424.zip*](https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.4.0.54424.zip)

*unzip \**

*chmod -R 755 /home/sonarqube/sonarqube-9.4.0.54424*

*chown -R sonarqube:sonarqube /home/sonarqube/sonarqube-9.4.0.54424*

*cd sonarqube-9.4.0.54424/bin/linux-x86–64/*

*./*[*sonar.sh*](http://sonar.sh) *start*

13\. Set up the Sonarqube for the EC2/Local server.

Username and password: **admin**

In the next step, you have to reset your password.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660287908/006ba205-5736-4905-bb54-77df92a13865.png)

14\. Your Sonarqube server is up and running which will look like this.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660289368/dce6759a-60b5-4015-a9c5-99eb5e7c6431.png)

15\. Now, We have to connect the Jenkins **Sonarqube scanner** with this **Sonarqube server**. To do that, click on the profile which is right of the search box, and go to **My Account.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660290867/f2a7e4c6-b52c-417b-873d-1b057dcf5034.png)

16\. To generate the toke for connecting with Jenkins **Sonarqube Scanner** with the **Sonarqube Server.** Click on the **Security tab,** give the names to the **token,** and click on **Generate.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660292304/a9029e9f-f59e-4cf6-a8ea-a62312f83f59.png)

17\. Now, Add the **Generated Token** in **Jenkins.** By **Dashboard -> Manage Jenkins -> Credentials -> System -> Global credentials.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660294004/e97f830b-08e1-475e-bb2d-8f586fff9303.png)

**Jfrog-Artifactory Server Setup**

18\. Create a new **EC2** Instance **t2.medium** and install **jfrog-artifactory**.

You can refer to the Github Repo in which there is a script called [**Install-Jfrog-Artifactory.sh**](http://Install-Jfrog-Artifactory.sh) and run it which installed **Jfrog Artifactory** on your machine or you can just copy and paste.

GitHub Repository: [https://github.com/AmanPathak-DevOps/Scripts-Installation.git](https://github.com/AmanPathak-DevOps/Scripts-Installation.git)

You can also refer youtube video link to integrate **Jfrog Artifactory** with **Jenkins** [How to Configure Artifactory in Jenkins](https://www.youtube.com/watch?v=fj_TD9pufFM&t=1s&ab_channel=CloudBeesTV)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660295492/af848e48-a56f-46ea-adfb-776902a24ace.png)

19\. Now, set up the Artifactory, to upload the artifacts after Code Analysis Successfully completed.

Click on **Get Started.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660297145/78442cb1-18f4-459a-9ef1-bcd1573f627e.png)

20\. Enter the new **password** and click on **next.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660298677/ee03108b-04a6-4c87-b7d5-2a5a5bb707f7.png)

21\. Click on **Skip**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660300423/a3870a98-792e-48ee-9b65-66fd36cba94e.png)

22\. Click on **Skip.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660302098/00eecc0f-c541-4c3f-bf02-2d240e62c7f6.png)

23\. Create the **repo** by giving the name to it, make sure the **repo should be generic** or **maven** as we have to upload the **artifacts** for the **java.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660303988/67a6a9a0-8cfd-4115-a64f-e2b09f36529d.png)

24\. Name the **repo** and click on **save and finish.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660305545/78c31387-cb59-4355-b185-27dec23bcf07.png)

25\. To integrate **Jfrog-Artifactory** with **Jenkins**, we have to **generate token** in the **Jfrog-Artifactory.**

Click on **Generate Access token.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660307315/cd0c7631-bd0c-4e40-ba62-e5284c160c6f.png)

26\. Click on **Generate.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660309146/b039aeb6-4d67-4542-9c52-d74e770b1edf.png)

**27\. Copy the token.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660310943/fd3bdb73-0216-4bca-855f-4a17537f1dcc.png)

28.Go to **Jenkins** and **Dashboard-> Manage Jenkins -> Credentials** and paste the **token** that you have copied from the **Jfrog-Artifactory** and do the rest thing as per the below screenshot and click on **Create**.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660313000/7a724602-5028-4b40-9817-00d7c1a47cef.png)

29.Install **Docker** on **EC2 Server** where **Jenkins is** Installed. You can refer to the below command or You can refer to the Github Repo where you just need to run the script and install it on your machine.

**GitHub Repository**: [https://github.com/AmanPathak-DevOps/Scripts-Installation.git](https://github.com/AmanPathak-DevOps/Scripts-Installation.git)

sudo apt update

sudo apt install docker.io

sudo su -

usermod -aG docker jenkins

usermod -aG docker ubuntu

systemctl restart docker

30\. Install **minikube** on your local machine.

Refer to the given **link:** [https://minikube.sigs.k8s.io/docs/start/](https://minikube.sigs.k8s.io/docs/start/)

*You can refer to the Github Repo where you just need to run the script or follow the command to install it on your machine.*

***GitHub Repository****:* [*https://github.com/AmanPathak-DevOps/Scripts-Installation.git*](https://github.com/AmanPathak-DevOps/Scripts-Installation.git)

*sudo apt update*

*sudo apt upgrade*

*wget* [*https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64*](https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64)

*chmod +x minikube-linux-amd64*

*sudo mv minikube-linux-amd64 /usr/local/bin/minikube*

31\. Install ArgoCD by going on to [https://operatorhub.io/operator/argocd-operator](https://operatorhub.io/operator/argocd-operator) and clicking on **install.** You will get the commands that you have to use to install on your local machine where **minikube** is installed.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660314860/1b0f3a08-c1fd-4312-baaf-1f2ff5fd1ee6.png)

**32\. Command** ran and **argoCD operator** has been installed

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660316630/dd223892-462e-4ff8-925a-e5e4512d63fc.png)

33\. Store **Docker credentials.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660318471/3cb40ba6-6e00-4bda-8860-424a56689d94.png)

34\. Store **Github Credentials** as well(Personal access token).

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660320419/9aac503f-f82f-47bc-9b2d-157b2e6c2689.png)

**Deploy on Kubernetes cluster**

35\. Go to [operatorhub.io](http://operatorhub.io) and enter on **argoCD** then click the **operator Documentation** link

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660322041/9e5b3a8e-ec90-4d07-81f9-2ab9d3040d30.png)

36\. Click on **usage -> basics** and copy the **selected script.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660323668/663bc1bd-7b58-4a2c-8857-9f800bfb6763.png)

37\. Create a **yml file** and **paste the script into the file.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660325233/0ea207e0-3643-4d8b-a125-5be36470f558.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660326460/42f295e5-c6ba-4eda-8d8d-3bb582c8df03.png)

38\. We are using this **script** to **download** the **argoCD controller.**

Now, the last step to download the argoCD controller which will done by the help of **command: *kubectl apply -f argocd-basics.yml***

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660327820/29bbdad1-7bda-400c-a0c6-a3c5b0e77896.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660329124/90112dd1-bed1-4343-bb13-1c3479cb4a71.png)

39\. Here, you can see that the **argoCD controller** has been **created.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660330643/1e51dc71-f1f9-4119-a7fd-250cfda6c30f.png)

40\. We need the fourth controller because this is responsible for **argoCD UI.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660332271/3c2b00b7-98b4-4ec7-a71d-a76da70ad179.png)

41\. We want to run this controller on our local machine. So, to do that I will edit the fourth controller where I will change **Type: ClusterIP** to **NodePort.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660333621/616e3469-88e3-4671-b409-1979f0453d90.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660335020/d2285813-7d4f-46ac-9fbb-eb007a0e75c7.png)

42\. We are running the same command to get the **services** but this time there is a difference where we can see the **NodePort.**

Now, to generate the **browser URL,** write the **command *minikube service argocd-server.***

And to get the **browser URL,** write the **command *minikube service list,*** *which* you can check in the below screenshot.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660336481/a1f528c9-1b4e-48dd-8759-02f4e81d2235.png)

43\. I have clicked on the link and you will get this **UI,** click on **advance** and then click on **<your\_IP>**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660337996/b7d06d1f-f870-4e08-8671-64cdf45ef4dd.png)

44\. You can see the **ArgoCD UI login Page.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660339679/e4fc1cda-665a-4798-9b3a-ca73a740d3bd.png)

45\. Here, you have to enter the **username** and **password.**

The **username** will be **admin** and to get the password, refer to the below screenshots.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660341378/133e9f67-21c0-4e82-b4fb-1aafd9b4768a.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660342918/b7d3a50c-b8ed-4279-8d35-9d60eb645ba6.png)

46\. I have got the encrypted password and will copy it.

Now, I have decrypted the password by using the below command

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660344184/a5e4ff8d-2022-4049-b8a1-504b85bc48de.png)

47\. Now, I have entered the **username** and **password** and **logged** the **argoCD UI.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660345411/f2c54194-46dd-469a-b11d-949565126e1a.png)

48\. Click on **CREATE APPLICATION.** You can see this in the above screenshot.

Now, To create the application, you have to enter some configurations, you can check it out in below screenshot and in the end click on **CREATE.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660347048/5b82f4c3-8931-4fac-82a2-308f4e223f87.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660348536/64100ae1-f93f-4a6d-ba4b-4c9842e84f27.png)

49\. Now, wait for some minutes. **argoCD** will deploy the application automatically.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660350169/448e27d0-80e1-4937-9f2a-c137b287af86.png)

50\. Here, I was getting an **error** because I have written the **namespace** in the **namespace** parameter which should be the **default.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660351641/07b2812f-23f2-4a6e-bb3b-37ffac0febe8.png)

51\. So, let’s correct it.

Final Configuration.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660353295/4578417c-4e20-408e-8e55-e2f71a4bd211.png)

52\. After clicking on **Save,** click on **Refresh,** and it will start the process again to deploy the application.

And My application has deployed successfully, as you can see in the below screenshot.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660355173/7cf01b61-3056-4bca-a0fc-0dc7839fe072.png)

53\. If you run the command, **kubectl get pods.** You can see the last **two pods** that are running which ensures that **deployment** is **successful.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660356737/ccb4ed3b-5619-4cc4-88d3-29596bdbb288.png)

54\. So, there were some issues that I have faced and resolved as well.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660358462/352765f9-6a97-49e2-92c8-90c255cf24ba.png)

**Final Output of Every Service and tool**

**The Jenkins Pipeline**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660360525/734e4c1f-b0d7-4775-969d-571369ceae75.png)

**Sonarqube Code Analysis**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660362522/3ac71573-3f71-4418-8ac3-28b466a0d34b.png)

**Uploaded Artifacts to Jfrog Artifactory**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660364403/f483a5b8-aefb-4cf0-90c4-e66c1452afc6.png)

**Deployment using argoCD Controller on Kubernetes Cluster**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660366550/7c8931c3-3f44-461b-9610-dd8b3c30a6d2.png)

References:  
**GitHub Link:**

**Jenkins Pipeline script, deployment file, Docker file, and the source code link are given below:  
**[**End-to-End-CICD-Project**](https://github.com/AmanPathak-DevOps/Springboot-End-to-End-CICD-Project)

Youtube Video Link

[**https://youtu.be/JGQI5pkK82w**](https://youtu.be/JGQI5pkK82w)

If you have any queries on your mind, feel free to ask, whether they are related to Jenkins Pipeline scripts or any other process. Follow me for projects and more content on DevOps.

**Happy Learning**