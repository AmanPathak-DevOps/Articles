---
title: "Day 26 Chapter 2- Deploy Flask Application using Helm Chart and many more features- Kubernetes"
datePublished: Tue Nov 28 2023 16:15:35 GMT+0000 (Coordinated Universal Time)
cuid: clqnee7un000608jz9u6bdoxd
slug: day-26-chapter-2-deploy-flask-application-using-helm-chart-and-many-more-features-kubernetes-daf402b69e5c
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703658683978/568bcdc1-38c6-4348-aef0-4f4cfef9bd0f.png

---

### Introduction

🚀 Welcome to Day 26 of #30DaysOfKubernetes! 🎉 Today, we’re diving into Chapter 2 of the Helm Charts saga! 📊✨ Let’s unravel the magic of Helm, simplifying Kubernetes deployments.

Docker Project of Python Flask Repo- [https://github.com/AmanPathak-DevOps/Docker-Projects/tree/master/Python-Project-for-Helm-K8s](https://github.com/AmanPathak-DevOps/Docker-Projects/tree/master/Python-Project-for-Helm-K8s)

Kubernetes Manifest file Repo- [https://github.com/AmanPathak-DevOps/Kubernetes-files](https://github.com/AmanPathak-DevOps/Kubernetes-files)

In the Previous Chapter, we have covered some theory and basic hands-on. But today, we will deep dive and do more hands-on.

The topics that we will cover today:

✅ Deploy Python Flask Application using Helm Chart

✅ What is Helmfile?

✅ Demo of Helmfile(Deploy HelmCharts using declarative method)

✅ Test Cases for your Helm Chart

### Demonstration

Create a helm chart for the Python application by using the below command

helm create helm-deploy-rest\-api  
ls -lart Helm-Deploy-Rest\-API

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658638420/6d8f89e5-1be9-425a-b5f6-9f50345e07ca.png)

Comment Out the appVersion in the Chart.yaml file

vim helm-deploy-rest\-api/Chart.yaml

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658640070/b7411ef2-2288-4360-a010-47d537978e0d.png)

Replace the image repository with nginx

vim helm-deploy-rest-api/values.yaml

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658641878/77f84d78-3204-4da9-a6db-85acca73e90f.png)

Replace the service type from ClusterIP to NodePort in the same values.yaml file

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658643426/0f1a6cab-fd9f-4ea9-9726-00b60a63875e.png)

vim helm-deploy-rest-api/templates/deployment.yaml

Remove the appversion set and write only the image repository name

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658645029/b8a045a1-edd1-407d-ba9d-ed644be18981.png)

In the same deployment.yaml file.

Modify the port to 9001 according to our application

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658646555/0cdbaa50-9ed1-4942-bd58-4a59d2f7fcc0.png)

Now in the same deployment.yaml file comment out all the liveness probe and readiness probe function

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658647916/287d1412-50dc-4450-8ea6-ffb1a955694b.png)

Now, install your helm chart

helm install pythonhelm helm-deploy-rest\-api/

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658649173/5ad35ecc-3eae-4628-a788-f1aaa29da712.png)

Now, check whether the pod is running or not

kubectl get pods

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658650618/95887503-91d7-442c-9caa-1c52600f4cc2.png)

Check the service to get the port number

kubectl get svc

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658651933/660f9d6c-8f89-4f68-a4a8-a7cfd3feffd9.png)

Now, get the minikube ip using the minikube ip command and paste it on the browser to get the content of the application

Use the ‘/main’ because this is our path where content is present. Otherwise, you will get errors on different paths.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658653199/6d077fa7-916f-4a14-9dd8-cbcac92c0cf9.png)

You can uninstall the release for the helm chart

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658654682/cf2385f1-15c6-4f9f-ba30-de6730aaa32b.png)

### Helmfile

Earlier, we used to deploy helm charts in an imperative way. But if you want to deploy helm charts using a declarative way then we will use Helmfile. Helmfile also helps to deploy multiple charts in one go.

**Demo of Helmfile**

Install the file

https://github.com/roboll/helmfile/releases/download/v0.144.0/helmfile\_linux\_amd64

Rename the file name to helmfile

mv helmfile\_linux\_amd64 helmfile

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658656368/7478399e-f3e3-460b-bf44-c6c261b0ba3e.png)

Change the permissions of the helmfile

chmod 777 helmfile

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658657725/ef21162e-65ad-417f-8744-d43b120ecedf.png)

Now, move the helmfile to the bin folder

mv helmfile /usr/local/bin

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658659544/7c81295b-3962-480c-adda-592cd22968ce.png)

Validate the version by the command

helmfile — version

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658660911/b49fb7aa-4179-49a3-a568-31bacb04921a.png)

Now, We will try to deploy our previous Python application using helmfile.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658662487/d4300b33-fd04-4e72-a820-77cdb3bf1822.png)

Once you run the command helmfile sync, your chart will be deployed.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658664030/28ec7af8-42ad-4910-8ee0-4be19ef90c8f.png)

If you want to uninstall the release then, go to the yaml file replace the true with false in the installed line, and run helm sync.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658665241/34ea8dfb-d6f4-46d6-9722-23c05f971741.png)

As we run the command to uninstall the deployment the pod is terminating now.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658667143/71ef7dd1-e9fb-4fe3-be52-7c9d771dc397.png)

### Helmfile using Git repository

Suppose your charts are present on the Git repository and you want to install them. So, Helm provides you with a feature in which you don’t need to clone the repo manually to deploy the chart. Instead of this, you just have to provide the git repository URL in the helmfile and helm will deploy the charts automatically.

Demo

To leverage this feature, you need to install one plugin for it. To install it just copy and paste the below command.

helm plugin install https://github.com/aslafy-z/helm-git — version 0.15.1

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658668794/2da6a6b7-ce65-4d03-9e1b-e4b7c53601ef.png)

Now, add your repo accordingly in the yaml file

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658670489/72780712-b70d-4e7a-a7e5-9c3627bea9e3.png)

Now, run the helmfile using the below command

helmfile sync

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658672408/174ba800-2fe2-44a1-9b67-d1fbec50b169.png)

### Install multiple charts using helmfile

You just need to add the charts in the previous helmfile below

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658674182/11841fd0-c66c-4a12-a4c3-1d4a1aa498a4.png)

Now, run the command to install the charts

helmfile sync

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658675956/1d162313-be41-4b66-9599-753b8f74496e.png)

### Test your helm chart

Once I deploy the chart, I want to test the chart whether it’s working or not.

So, you can define your test cases in the test-connection.yaml file which is presented in the tests folder of the chart itself.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658677586/f592ffaf-05de-428a-a9f7-40131270a2c9.png)

As we have deployed our charts in the previous demo. So, we will test those charts.

To test the particular chart use the below command.

helm test <chart>

As you can see in the below snippet. Our helloworld chart test is succeeded.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658679090/55801f95-abca-48b4-a1de-e3901e42275b.png)

🎩✨ Conclusion: Chapter 2 of our Helm adventure brought us closer to Helmfile and its powerful capabilities. We explored declarative deployments, and Git repository integration, and even tested the reliability of our Helm charts. As we continue our Kubernetes journey, Helm remains a key ally, streamlining the deployment process and enhancing automation. Ready for more Helm magic? Stay tuned! 🚀✨ #Kubernetes #HelmMagic #DevOps #Conclusion

### Want to Know About Challenge?

If you’re eager to learn more and join our challenge through the GitHub Repository, stay tuned for the upcoming posts. Follow for more exciting insights into the world of Kubernetes!

**GitHub Repository**: [https://github.com/AmanPathak-DevOps/30DaysOfKubernetes](https://github.com/AmanPathak-DevOps/30DaysOfKubernetes)

#Kubernetes #DaemonSet #StatefulSets #NetworkPolicy #Operators #ContainerOrchestration #DevOps #K8sLearning

See you on Day 27 as we unravel more Kubernetes mysteries!

Stay connected on **LinkedIn**: [LinkedIn Profile](https://www.linkedin.com/in/aman-devops/)

Stay up-to-date with **GitHub**: [GitHub Profile](https://github.com/AmanPathak-DevOps)

Feel free to reach out to me, if you have any other queries.

Happy Learning

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658680950/a0e1cd3b-de90-4e0f-a3c0-06815480a04b.gif)

Chapter 2 finished