---
title: "Day 28- Azure Kubernetes Service(AKS)"
datePublished: Thu Dec 07 2023 18:14:25 GMT+0000 (Coordinated Universal Time)
cuid: clqnebm2c000908l30u0s8ucq
slug: day-28-azure-kubernetes-service-aks-353e09f2629c
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703658562456/a8238ec0-66d6-438f-be96-51ac8fd41c15.png

---

Welcome to Day 28 of our #30DaysOfKubernetes journey! 🚀

Today, we’ll explore Azure Kubernetes Service (AKS), a game-changer for deploying applications hassle-free! 🔧

### **What is AKS?**

AKS is a managed Kubernetes Service that helps us to deploy our applications without worrying about Control Plane and other things like regular updates support, Scaling, and high availability of your cluster.

### Key Features:

**Managed Control Plane:** AKS provides a fully managed control plane. So that, you don’t need to configure things like the upgradation of Kubernetes cluster, patching, and monitoring. This will help us to focus on main things like deployment of the application.

**Scalability:** AKS provides the scalability feature in which we can scale our worker nodes according to our application traffic which will help us to achieve high availability. AKS also enables the scale-up option for the AKS Clusters.

**Windows container support:** You can run Linux containers on the AKS, But you can run Windows containers as well which will help developers to work on Windows base applications on the Kubernetes cluster.

**Networking Configurations:** To create AKS, networking is required. So, you can configure the networking part according to your requirements which helps you to provide fine-grained control on the Cluster networking configurations.

**Integration with other services:** You can integrate AKS with multiple Azure services like Azure AD, Azure Policy, Azure Monitor, etc to provide solutions within the cloud for container management.

**Multi-worker nodes:** AKS can create multiple node pools according to the application’s requirements.

If you want to read more features in a detailed way refer to the below link:

[https://spot.io/resources/azure-kubernetes-service/](https://spot.io/resources/azure-kubernetes-service/)

### **Azure AKS Costing**

*   If you are new to AKS and want to explore the AKS service then, AKS comes under a free tier account. But the resources like computing, networking, and storage will have a cost. So use wisely. Remember, you can’t autoscale the clusters or nodes under the free tier.
*   Now, If you want to use AKS in your Organization It will cost you 0.10$ per cluster per hour similar to AWS EKS Cost but the AKS cluster will be in the Standard tier. You can auto-scale your clusters and worker nodes in the Standard tier.
*   There is one more tier which Premium and that will cost you 0.60$ per cluster per hour with advanced features. You can auto-scale your clusters and worker nodes in the Standard tier.

### Let’s Dive into the Demo! 🛠️

To create AKS, we need to configure VNet and other networking things. If you are not a beginner in the cloud feel free to skip the network configuration part. But if you are new to AKS or in the Azure Cloud, I would say to follow each step. So, it will help you to get a better understanding of each service that is related to Azure AKS.

First of all, create a separate Resource Group by clicking on **Create new.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658515399/f09c1ff2-0058-452c-b0d7-994fcb4d5238.png)

Provide the name of your Virtual Network and click on **Next.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658517007/e71a7070-3389-4ede-a678-67073caaa2d7.png)

Now, we have to create two public subnets for the high availability of our Azure Kubernetes.

Delete the default subnet and click on **Add a subnet** then add two subnets with your desired IP address range and click on **Next.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658518389/23e28b90-6d4b-46f0-bca9-c54e3222de83.png)

Click on **Review + create** to validate the error in the configurations. If there is no error feel free to click on **Create.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658519933/b58e3678-ef8a-4b53-868e-c089d02cc68d.png)

Once your deployment is done, in the search field enter **Kubernetes services** and click on the first one.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658521373/95794978-ff8c-4090-8bc8-2d99ec64992b.png)

Click on **Create** and select **Create a Kubernetes cluster.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658523296/0d30cfcc-6895-4f01-bc83-99d8308b6dc9.png)

Select the Same Resource Group that we have created while creating the Virtual Network. After that, provide the name to your AKS keep the things same as shown in the below snippet, and click on **Node Pools.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658524988/e7b46855-0f0e-4336-b24e-af1e91240b3a.png)

In the Node pools section, we have to add Worker Node. Remember one thing the default node(agentpool) is a system node. So you don’t have to do anything with that node.

Click on **Add node pool**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658527042/7bcb0d81-be1b-45c2-aa55-f2ed07ba28f0.png)

Provide the name to your worker node and keep the things same as shown in the below snippet such as Node size and Mode, etc.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658528952/5a68ea73-a183-4160-be85-b4769ff0402a.png)

Now, click on **Networking** section.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658530360/246ee39a-be05-4016-8264-57897ced1aca.png)

Select the kubenet as **Network configuration** then, keep Calico as **Network policy** and click on **Review + Create** by skipping integrations and Advanced sections**.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658531966/042003b7-9f13-46f1-badb-b5be8d732672.png)

Once the validation is passed, click on **Create.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658534091/3e9fca50-a882-4c27-a79a-2d7f7ca5f516.png)

Once your deployment is complete, click on **Go to resource.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658535882/661bb929-5415-4f7d-99c4-c2981db1bca8.png)

Click on **Connect.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658537421/dff0a1c2-3afc-42b0-9232-8aeaabb0464a.png)

You will have to run two commands that are showing on the right of the snippet to configure it on local or Cloud Shell.

But, we will configure it on our local. For that, we need to install Azure CLI and kubectl on our local machine.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658539307/8b44744d-2199-4b0e-95db-7393fa35ae90.png)

To install kubectl on your local machine follow the below commands.

curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.28.3/2023-11-14/bin/linux/amd64/kubectl

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658540695/658341ba-f582-4f69-8c3b-23a3e7e4ee8f.png)

curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.28.3/2023-11-14/bin/linux/amd64/kubectl.sha256

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658542019/fdde0ab7-965e-4828-ba51-bdec0946766f.png)

sha256sum -c kubectl.sha256

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658543504/3212d3a8-f9bd-4583-ba1e-6707e5d0b40e.png)

openssl sha1 -sha256 kubectl

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658544902/a45a5de7-da7f-47e9-b3d5-122c89fef3e7.png)

chmod +x ./kubectl  
mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$HOME/bin:$PATH  
echo 'export PATH=$HOME/bin:$PATH' >> ~/.bashrc

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658545993/256484cd-1d6a-4b72-9e39-b5f22b94a1b2.png)

kubectl version \--client

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658547395/372f1a58-088d-4430-ab0a-3445a66e643f.png)

Once the kubectl is installed, you can install azurecli on your local by following the below commands.

Install Azure CLI on the local machine

curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658549074/05dbffbb-4b91-4293-9dc0-e230763a372e.png)

To install AZ CLI on other OS, you can refer to the below link

[https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-linux?pivots=apt](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-linux?pivots=apt)

Now, login to your Azure Account by below command

az login

Once you run the below command, a new tab will open in your browser which will validate your account. If you have already logged into that account then, it will be automatically logged in on the terminal as well and you will see output like below snippet.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658550624/cc60a0d8-ef0e-418d-b970-4cee6cf7e110.png)

Once you log in, you can run the command that was given by Azure to connect with AKS

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658551990/d8b447b2-9eb6-4466-974d-692e3ccd497f.png)

Now, you can run the command to list the nodes which will help you to validate the connection as well.

kubectl get nodes

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658553510/dcc6ed0f-04e9-4709-8e9e-ddb51f3ec90a.png)

Now, let’s try to deploy the Apache application on AKS

apiVersion: apps/v1  
kind: Deployment  
metadata:  
  name: apache-app-deployment  
  labels:   
    app: apache-app  
spec:  
  replicas: 2  
  selector:  
    matchLabels:  
      app: apache-app  
  template:   
    metadata:  
      labels:  
        app: apache-app  
    spec:  
      containers:  
      \- name: apache-container  
        image: avian19/apache-ne:latest  
        ports:  
        \- containerPort: 80

kubectl apply -f deployment.yml

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658554842/7908117b-458f-414a-bca5-62f67b17ba29.png)

Now, host the application outside of the Kubernetes Cluster by creating a service for the Apache application and observing the public in the EXTERNAL-IP Column.

apiVersion: v1  
kind: Service  
metadata:  
  name: apache  
spec:  
  selector:  
    app: apache-app  
  type: LoadBalancer  
  ports:  
  \- protocol: TCP  
    port: 80

kubectl apply -f svc.yaml

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658556189/a52f9d16-e74f-4468-9208-62bcd7323aa2.png)

As our service type is LoadBalancer, AKS created one Load Balancer which will look like the below snippet.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658557963/6d084e61-230d-4b1a-83aa-b520d0c037ad.png)

Now, copy the EXTERNAL-IP that we get by running the list svc command in the previous step hit on the browser, and see the magic.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658559553/9405af56-33a9-46af-82d8-0bd4b572d475.png)

### Conclusion

Embark on your AKS journey with confidence! Today, we unraveled the simplicity and power of Azure Kubernetes Service, streamlining deployment and management. As we delve into diverse features and costing, remember, that AKS is your ally for scalable, hassle-free container orchestration. Ready to wield the Azure AKS magic? Explore the guide, and stay tuned for more Kubernetes wisdom on Day 29! 🚀 #AKS #KubernetesMastery #30DaysOfKubernetes

### Want to Know About Challenge?

If you’re eager to learn more and join our challenge through the GitHub Repository, stay tuned for the upcoming posts. Follow for more exciting insights into the world of Kubernetes!

**GitHub Repository**: [https://github.com/AmanPathak-DevOps/30DaysOfKubernetes](https://github.com/AmanPathak-DevOps/30DaysOfKubernetes)

#Kubernetes #DaemonSet #StatefulSets #NetworkPolicy #Operators #ContainerOrchestration #DevOps #AKS #Azure #K8sLearning

See you on Day 29 as we unravel more Kubernetes mysteries!

Stay connected on **LinkedIn**: [LinkedIn Profile](https://www.linkedin.com/in/aman-devops/)

Stay up-to-date with **GitHub**: [GitHub Profile](https://github.com/AmanPathak-DevOps)

Feel free to reach out to me, if you have any other queries.

Happy Learning