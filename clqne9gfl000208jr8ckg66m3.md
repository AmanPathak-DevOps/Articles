---
title: "Day 29- Google Kubernetes Engine (GKE)"
datePublished: Sat Dec 09 2023 12:28:55 GMT+0000 (Coordinated Universal Time)
cuid: clqne9gfl000208jr8ckg66m3
slug: day-29-google-kubernetes-engine-gke-b65dec4fe504
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703658461428/939086e0-2b4c-4277-9d8f-636982753551.png

---

**Welcome to Day 29 of #30DaysOfKubernetes! 🚀**

Today, let’s unravel the wonders of GKE — Google Kubernetes Engine. 🌐✨

### What is GKE?

GKE stands for Google Kubernetes Engine which is a managed Kubernetes service to deploy your application easily without worrying about Control Plane, Scaling, or updating to your Kubernetes Cluster.

One of the main advantages of GKE is that Kubernetes is developed by Google itself.

### Features:

1.  **Multi-Versions**: GKE has the most available versions as Kubernetes developed Google itself.
2.  **Auto Upgrades**: The Control Plane and nodes will be automatically updated.
3.  **Auto Health Repair**: GKE provides automatic health repair for the nodes.
4.  **Security Enhancement**: GKE provides Containerized Optimised OS which will help you to enhance the security of your nodes.
5.  **Monitoring**: GKE provides monitoring support by integrating the monitoring services with GKE itself.
6.  **Scalability**: GKE provides scalability to your Kubernetes Clusters and Nodes according to the requirements.
7.  **Multi-Cloud Support**: You can run your applications anywhere without interruption using GKE on Google GKE, Azure AKS, or AWS EKS.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658408003/f6f9fa9a-c7c6-4e54-a519-6aec607c74de.png)

GKE Architecture- Source Google itself

### Let’s Dive into the Demo! 🛠️

If you have never created Kubernetes Cluster on Google Cloud before then, you have to enable it first to use it.

Click on **Enable.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658409616/70d0f0af-619c-449a-ac49-dac8ec6ad7c2.png)

Click on **CREATE**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658411454/8a4abd84-f370-4b6d-8774-97001192f941.png)

Now, we will create the Standard Kubernetes Cluster. To do that, click on **SWITCH TO STANDARD CLUSTER.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658413421/038c5899-b201-48f7-bbca-ff143b3d6079.png)

Provide the name to your cluster name and select the location type according to your credits remaining in the Google Cloud. Also, specify the node locations and click to expand **default-pool** which is showing on the left.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658415219/4b43d23b-8829-4712-9127-fd4155ce30ff.png)

Select the configuration as given in the below screenshot to reduce some costs.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658417044/ffe84bd4-0e82-4bb9-96d2-f4bb4c0c90e1.png)

Provide the Root disk size of 15 GB because it will be sufficient according to our demonstration.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658418755/49d2fb31-acb3-4960-8a5b-117614d79f29.png)

Click on the **Networking** and provide configurations as given below.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658420335/ba33682c-7051-407d-b3e1-776c609f5620.png)

We don’t need to modify anything in Security and other things. So you can skip it and click on **CREATE.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658422092/3351cd3d-b9fe-4992-9700-0c9c2ebbdcc9.png)

After clicking on **CREATE.** GCP will take some time to create the Kubernetes Cluster.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658423642/172d7eb0-6800-4e7e-91da-b9b383393cfd.png)

Once your Kubernetes Cluster is ready, then click on **CONNECT**.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658425121/323dcbc4-8042-496e-ad67-9b333c91e156.png)

You will get a command to configure it on your local machine. But there are two prerequisites that you have to follow which first is to configure gcloud-cli and install kubectl on your local machine. Let’s do this.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658426928/bd964e06-76e0-445d-b176-8cc7d97618c3.png)

sudo apt-get update  
sudo apt-get install apt-transport-https ca-certificates gnupg curl sudo  
curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo gpg - dearmor -o /usr/share/keyrings/cloud.google.gpg  
echo "deb \[signed-by=/usr/share/keyrings/cloud.google.gpg\] https://packages.cloud.google.com/apt cloud-sdk main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658428499/361c6996-abd0-456b-a013-1d47027430a8.png)

Update your local machine and install Google Cloud cli

sudo apt\-get update && sudo apt\-get install google\-cloud\-cli

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658429888/7888530b-3387-4505-9fa9-b62cd3b20856.png)

After installing gcloud cli, now we have to configure it with our account. To do that, run the below command and it will open your browser and select your main GCP account.

gcloud init

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658431725/3b42cd1a-7475-4d77-9d09-007235d60f8e.png)

Now, you need one plugin to complete the configuration.

gcloud components install gke-gcloud-auth-plugin

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658433343/0def4a3f-028d-4c38-a8a1-b0f4e21968a0.png)

After configuring Google Cloud CLI our first prerequisite is completed.   
Now, we have to install kubectl which is mandatory to perform the commands on the Google Kubernetes Cluster.

To install kubectl on your local machine follow the below commands.

curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.28.3/2023-11-14/bin/linux/amd64/kubectl

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658435084/81cafd3c-7a50-42e5-bc08-3a8fdab4ee2f.png)

curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.28.3/2023-11-14/bin/linux/amd64/kubectl.sha256

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658436579/fed159fe-7c26-4959-af78-4febb910756a.png)

sha256sum -c kubectl.sha256

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658437712/9cb57f17-6dc1-4612-a62d-1e24912fdd2e.png)

openssl sha1 -sha256 kubectl

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658439266/54bc1820-c4a2-429c-93b3-da0d5cd7eac7.png)

chmod +x ./kubectl  
mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$HOME/bin:$PATH  
echo 'export PATH=$HOME/bin:$PATH' >> ~/.bashrc

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658440746/6201c8ae-25c2-4bcc-826f-3a3d27932299.png)

kubectl version \--client

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658442090/3751c44f-a00d-4f80-b8eb-b82158737fe8.png)

Now, copy the command that is given by GCP to connect to the Kubernetes Cluster and paste it into your local machine.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658443208/561aa037-980f-44cf-a1cd-df862fa211d1.png)

To validate whether your cluster is working or not, list the nodes.

kubectl get nodes

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658444818/3313cf00-e758-41b9-8d6d-3288a7bb6001.png)

Now, let’s try to run the static application on nginx server with a GCP load balancer

apiVersion: apps/v1  
kind: Deployment  
metadata:  
  name: nginx-app-deployment  
  labels:   
    app: nginx-app  
spec:  
  replicas: 2  
  selector:  
    matchLabels:  
      app: nginx-app  
  template:   
    metadata:  
      labels:  
        app: nginx-app  
    spec:  
      containers:  
      \- name: nginx-container  
        image: avian19/nginx-ne:latest-1.0  
        ports:  
        \- containerPort: 80

kubectl apply -f deployment.yml

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658446296/c592368f-fc9c-4bbd-a494-c645864f3ec9.png)

Now, host the application outside of the Kubernetes Cluster by creating a service for the nginx application and observe the Public IP in the EXTERNAL-IP Column.

apiVersion: v1  
kind: Service  
metadata:  
  name: nginx  
spec:  
  selector:  
    app: nginx-app  
  type: LoadBalancer  
  ports:  
  \- protocol: TCP  
    port: 80

kubectl apply -f svc.yaml

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658447701/a6bed1c5-73ad-4f82-8d3d-ab1fbbd42f91.png)

Copy the Public IP and paste it into your favorite browser and see the magic.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658449236/5ae6f6c9-177d-43fb-bd77-a916d52d961e.png)

You can also decrease the number of nodes that are running in the below screenshot.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658451119/d11f5ecf-3db0-4261-a103-890a3c033a19.png)

After going to the Cluster, click on default-pool which is showing under **Node Pools.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658452685/57eefc5b-4519-462c-a6d1-2af8e408f8d4.png)

Click on edit

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658453966/53f3336b-d6b3-4164-a8d1-a9c9715d2fc4.png)

Replace 3 with the 1 to reduce the nodes then scroll down and click on Save.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658455524/1a70f633-2ad1-4179-9ddb-2fb0d4db6a0c.png)

Once you Save the configurations, then you will see the VM Instances will be deleted.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658456978/6b777e68-044e-49c4-80c8-6c91fad3bcbe.png)

Don’t worry your application will still running

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658458787/8f40b89e-7041-4de5-b71b-b833132aba72.png)

### *Conclusion*

GKE, the brainchild of Google, simplifies Kubernetes management, ensuring a smooth sail for your applications. Leverage its robust features and multi-cloud support to take your container orchestration to new heights! 🚢💻

### Want to Know About Challenge?

If you’re eager to learn more and join our challenge through the GitHub Repository, stay tuned for the upcoming posts. Follow for more exciting insights into the world of Kubernetes!

**GitHub Repository**: [https://github.com/AmanPathak-DevOps/30DaysOfKubernetes](https://github.com/AmanPathak-DevOps/30DaysOfKubernetes)

#Kubernetes #DaemonSet #StatefulSets #NetworkPolicy #Operators #ContainerOrchestration #DevOps #GKE #Google #GoogleCloud #K8sLearning

See you on Day 30 as we unravel more Kubernetes mysteries!

Stay connected on **LinkedIn**: [LinkedIn Profile](https://www.linkedin.com/in/aman-devops/)

Stay up-to-date with **GitHub**: [GitHub Profile](https://github.com/AmanPathak-DevOps)

Feel free to reach out to me, if you have any other queries.

Happy Learning