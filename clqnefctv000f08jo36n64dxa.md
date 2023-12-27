---
title: "Day 26- Kubernetes Helm & Helm Charts Chapter 1"
datePublished: Fri Nov 24 2023 15:49:43 GMT+0000 (Coordinated Universal Time)
cuid: clqnefctv000f08jo36n64dxa
slug: day-26-kubernetes-helm-helm-charts-chapter-1-46e23aba25e6
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703658736385/8c4ceb76-91eb-40eb-b3b0-96ed5554fc10.png

---

### Introduction

Welcome to Day 26 of our #30DaysOfKubernetes journey! 🚀 Today, we’re diving into the magic of Helm, the Kubernetes package manager. Let’s simplify your deployments and embrace the power of Helm charts! 🎩💡

### Helm

Helm is a Kubernetes package manager in which the multiple numbers of YAML files such as the backend, and frontend come under one roof(helm) and deploy using helm.

**Let’s understand with the help of a simple example.**

Suppose you have an application where frontend, backend, and database code needs to deploy on Kubernetes. Now, the task becomes hectic if you have to deploy frontend and backend codes because your application will be Stateful. For frontend, backend, and database you will have to create different YAML files and deploy them too but it will be complicated to manage. So, Helm is an open-source package manager that will help you to automate the deployment of applications for Kubernetes in the simplest way.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658690874/1a6c7a9b-11c3-461a-a353-3eb13d6ead14.png)

**Let’s understand again with the help of the above architecture.**

**Normal deployment**

As you know, to deploy your code you need to write a minimum of two YAML files which is deployment and service file. Those files will be deployed with the help of the kubectl command. These files act differently from each other but you know that the files are dependent on each other.

**Helm deployment**

In the help deployment, all the YAML files related to the application deployment will be in the helm chart. So, if you want to deploy your application you don’t need to deploy each YAML file one by one. Instead, you can just write one command helm install <your-chart-name> and it will deploy your entire application in one go. Helm is the Package manager for the Kubernetes which helps to make the deployment simple.

Benefits

1.  Because of the helm, the time will be saved.
2.  It makes the automation more smoothly
3.  Reduced complexity of deployments.
4.  Once files can be implemented in different environments. You just need to change the values according to the environmental requirements.
5.  Better scalability.
6.  Perform rollback at any time.
7.  Effective for applying security updates.
8.  Increase the speed of deployments and many more.

### Hands-On

Install Helm

curl -fsSL -o get\_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3  
chmod 700 get\_helm.sh  
./get\_helm.sh  
helm version

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658692352/b2a3a1d1-1b14-4afc-8deb-4a5e9cbff096.png)

To list the all repositories

helm list

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658693823/a552b333-dd9e-42ed-ae78-319c8711d246.png)

Create a stable repository using the helm

helm repo add stable https://charts.helm.sh/stable

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658695368/d72bfc64-9b47-4b44-9817-473ba72f732e.png)

List the repo, again

helm repo list

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658696933/aa2ad71c-28c0-4ae4-b4f9-f0c2816fb4d9.png)

To remove the repo

helm repo remove stable

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658698421/11a7638c-5f4b-41a1-aebe-db531829b01e.png)

To create our own repo

helm create my-repo

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658699808/767f3e09-9b1d-435f-8c57-97c5fe2e75ec.png)

To see the files inside the created repo

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658701202/fecd49b0-496a-4004-8499-a5a9c6ad207b.png)

Let’s do a simple demo where we will host the nginx page on Kubernetes using the helm chart.

Create a chart using the command

helm create helloworld

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658702663/d914c9ac-d15e-4227-b024-62217f8cf5a9.png)

The file structure should look like the below snippet.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658704343/f79ae179-0f9a-4a0f-afd6-cc143dfe2d93.png)

Go to the Helm Chart directory helloworld and edit the values.yaml file

cd helloworld  
vim values.yaml

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658706245/229cd223-25e4-4d92-9963-cc875bd6826c.png)

Replace the ClusterIP with NodePort

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658707939/6c1c075b-87d9-410a-bd17-e982190d0d8e.png)

Now deploy your helm chart

To do that, you have to be present in the directory where the chart is present.

In my case, my helloworld chart is in the Helm directory. So, I have to be in the Helm directory and run the below command to install the Helm Chart.

helm install thehelloworld hellowolrd  
helm install <custom-chart-release-name\> <given-chart-name\>

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658709336/cbd0bb0a-d110-47b4-a650-1c12e1b1564d.png)

Now, check the services whether your application is running or not

kubectl get svc

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658710687/50878f61-966a-4044-b77e-e76f4987713a.png)

Now, access your application via browser by copying the port number(in my case 30738) with minikube private IP in starting.

To get the IP, you can simply run the command on the terminal minikube ip and use it to view the content.

As you can see in the below snippet, our application is successfully deployed with the help of Helm Chart.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658712074/06acb2d4-13f0-4b8a-a561-fc0e1adc7d61.png)

If you want to see the minikube dashboard GUI then run the below command on the terminal.

minikube dashboard

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658713766/d869291f-6f1e-4d06-94dd-8a11a654910c.png)

Once you run the command, a new tab will open in your browser that will look like the below snippet.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658715376/e3464859-35e3-4e12-af21-8c04b55a2c96.png)

Now, if you want to uninstall your deployment. You can simply run the below command.

helm uninstall thehelloworld

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658716774/f2a64556-7382-4c9f-a837-d4e012f97f84.png)

Once you uninstall the deployment you will see nothing on your K8s dashboard because the deployment has been deleted.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658718438/7ab0966d-228e-4352-ab09-18f3b1e24ee7.png)

### Demo of Helm Cheat Sheets

Create the helm chart

helm create helloworld

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658719963/28f5b5ba-c262-4551-82bd-349f0a66886a.png)

Create the release and deploy the helm chart

helm install thehelloworld helloworld

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658721297/e5bef47b-b6b2-4af6-909e-b58cd08ccc1a.png)

Now, if you want 2 replicas instead of 1. So you will make the changes in the values.yaml file and replace the replica 2 with 1. But you have to redeploy the changes which will be done by the below command after doing the changes.

helm upgrade thehelloworld helloworld

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658723019/82c07c08-ffc9-4126-bab1-aa0100495e72.png)

Now, you think that you did something wrong or unexpected and you want to switch to the previous deployment. To do that, first of all, you need to know that to which revision number you have to switch.

To check the current revision use the below command.

helm list -a

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658724244/62ca8b95-2801-4d30-953e-56802bbeadcb.png)

Now, I want to switch to the 1 Revision number. So, I will roll back the changes by the below command.

helm rollback thehelloworld 1

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658725737/1f2a3f06-6960-45ce-8c60-26ba254fd60f.png)

If you want to dry-run the chart before installation use the below command.

helm install thehelloworld — debug — dry-run helloworld

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658727171/ce0fc752-7c4f-4fed-b9a6-f3c5d1eb1b24.png)

If you want to validate your YAML files within the helm chart then, use the below command.

helm template helloworld

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658728616/3482279e-42ff-4120-aeeb-3cb90bb9f750.png)

If you want to validate your Charts then use the below command.

helm lint helloworld

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658729881/2f8298df-cd3f-4747-a877-5cbb2be6cbc8.png)

If you want to delete or release the deployment then, use the below command.

helm uninstall thehelloworld

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658731702/11868b13-c995-40df-a2cd-16c7dbb44b55.png)

### Conclusion

Cheers to unlocking the Helm magic on Day 26! 🎉 Dive into Helm charts for simplified Kubernetes deployments. We will cover some advanced things related to Helm in the next Chapter(**Not on Next Day**).

Stay tuned for more Kubernetes insights on our #30DaysOfKubernetes journey! 🚀🌐

### Want to Know About Challenge?

If you’re eager to learn more and join our challenge through the GitHub Repository, stay tuned for the upcoming posts. Follow for more exciting insights into the world of Kubernetes!

**GitHub Repository**: [https://github.com/AmanPathak-DevOps/30DaysOfKubernetes](https://github.com/AmanPathak-DevOps/30DaysOfKubernetes)

#Kubernetes #DaemonSet #StatefulSets #NetworkPolicy #Operators #ContainerOrchestration #DevOps #K8sLearning

See you in the Next Chapter as we unravel more Kubernetes mysteries!

Stay connected on **LinkedIn**: [LinkedIn Profile](https://www.linkedin.com/in/aman-devops/)

Stay up-to-date with **GitHub**: [GitHub Profile](https://github.com/AmanPathak-DevOps)

Feel free to reach out to me, if you have any other queries.

Happy Learning

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658733062/2fcb16c3-9a84-4e1f-b1aa-25dc5d4cbf4d.gif)

Chapter 1