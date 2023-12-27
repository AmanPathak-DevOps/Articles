---
title: "Day 27- AWS Elastic Kubernetes Service(EKS)"
datePublished: Tue Dec 05 2023 15:12:17 GMT+0000 (Coordinated Universal Time)
cuid: clqnf1ek2000k08l066iu6ozl
slug: day-27-aws-elastic-kubernetes-service-eks-3b917cafbf2e
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703659765299/b879bbec-1c86-4f91-868b-61b264842c9f.png

---

### Welcome to AWS EKS Exploration! 🌐

Today, let’s dive into the world of Elastic Kubernetes Service (EKS) by Amazon Web Services. 🚀

### **What is AWS EKS?**

AWS EKS(Elastic Kubernetes Service) is a managed service that eliminates things like installation of the Kubernetes and maintaining the Kubernetes cluster.

Some basic benefits like you can focus on deployment for the applications. You don’t need to think about the availability of your cluster, AWS will take care of those things.

### **Key features of EKS:**

*   **Manage Control Plane**: In EKS, the Control Plane will be managed by AWS itself.
*   **Configure Node Groups**: In EKS, You can add multiple Worker Nodes according to the requirements in no time.
*   **Cluster Scaling**: AWS will take care of the Cluster scaling according to your requirements whether it’s upscaling or downscaling.
*   **High Availability**: AWS provides high availability of your Kubernetes cluster.
*   **Security**: AWS enhances the security by integrating IAM service with EKS.
*   **Networking**: AWS provides better control to manage the networking stuff for your Kubernetes cluster.

If you want to read more features in a detailed way refer to the following link:

[https://aws.amazon.com/eks/features/](https://aws.amazon.com/eks/features/)

### **AWS EKS Costing**

AWS will cost you 0.10$ per hour for each cluster. If you create EC2 for Node Groups then it will cost you separately according to the instance type and the same with ECS Fargate(depends on vCPU and memory resources).

### Let’s Dive into the Demo! 🛠️

To create EKS, we need to configure VPC and other networking things. If you are not a beginner in the cloud feel free to skip the network configuration part. But if you are new to EKS or the AWS Cloud, I would say to follow each step. So, it will help you to get a better understanding of each service that is related to AWS EKS.

Create VPC and select the desired IPv4 CIDR.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659692831/3d61e29b-bd12-47d8-bf22-949abfa79309.png)

We need to create at least two Public Subnets to ensure high availability.

Public-Subnet1

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659694577/c6b5c00c-9e00-492d-8263-f1ba2fbe3bb9.png)

Public Subnet2

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659696300/db5e1840-9d98-42b7-9327-9856a6c84c35.png)

We need an internet connection for our clusters and worker nodes. To do that, create an Internet Gateway.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659697580/0754afc8-dd4c-4e08-aeb6-f1ea610797d4.png)

Now, attach the above Internet Gateway to the VPC that we created in the earlier step.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659699062/83e84d2a-dac6-4ce4-93e4-acb1b50c72ea.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659700285/3052b16c-9189-4092-9d5e-db041afb61b6.png)

We need to create a route table as well for the internet access for each subnet.

Public Route table

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659702245/0382898c-4916-444f-a62f-efc80ed59ad7.png)

Select the Internet Gateway in the Target.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659703802/37558319-f0dd-4791-bfff-79de5b6d9301.png)

Once you add routes, then you have to add subnets for which purpose we are creating a Public Route table.

Click on **Edit subnet associations**.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659705372/32f8c057-fece-4b6a-b812-0078b73fad9d.png)

Select both subnets and click on **Save associations.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659706921/cdb2b682-769a-4a41-be04-aaaecbe564a3.png)

Once you associate the subnets. You will see your subnets look like the below snippets.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659708742/ce3e9639-be96-44d7-97a2-089562f93607.png)

Now, the EKS Cluster needs some access to the AWS Services like ec2, kms, and load balancer.

To do that, we will create an IAM Role and Policy for the EKS Cluster

Click on AWS service as a **Trusted entity type** and select the EKS as **Usecase** and in the below options, choose **EKS-Cluster**.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659710285/1ee8f739-c66e-43c4-a966-f5bac82553f6.png)

Click on Next.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659711689/f705385d-c6fb-49d9-977c-9de0745fe541.png)

Provide the **Role name**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659712910/6e37282b-b7ea-487d-ac14-3193c7be6af7.png)

Once we created the roles for the EKS Cluster. Now, we have to create a role for the Worker Nodes which is also a necessary part.

Click on AWS service as a **Trusted entity type** and select the EC2 as **Usecase** and in the below options, choose **EC2**.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659714588/f30984ea-5d5a-44a8-a0ed-48d35928e3e3.png)

When you will get a popup to add the Policy for the Worker Nodes.

Select the below three policies for our Worker Nodes.

[AmazonEC2ContainerRegistryReadOnly](https://us-east-1.console.aws.amazon.com/iam/home?region=us-east-1#/policies/details/arn%3Aaws%3Aiam%3A%3Aaws%3Apolicy%2FAmazonEC2ContainerRegistryReadOnly), [AmazonEKS\_CNI\_Policy](https://us-east-1.console.aws.amazon.com/iam/home?region=us-east-1#/policies/details/arn%3Aaws%3Aiam%3A%3Aaws%3Apolicy%2FAmazonEKS_CNI_Policy), and [AmazonEKSWorkerNodePolicy](https://us-east-1.console.aws.amazon.com/iam/home?region=us-east-1#/policies/details/arn%3Aaws%3Aiam%3A%3Aaws%3Apolicy%2FAmazonEKSWorkerNodePolicy)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659716188/281b1967-b1a2-47b9-b595-a59170b4a301.png)

Provide the name of the Role and click on next.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659717495/84547e33-35b5-4dc5-ba19-f12ceecf30ac.png)

Now, Prerequisites are completed. Let’s create the EKS

Navigate to the AWS EKS and click on **Add cluster**.

Select Create.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659718786/9fa96653-33aa-4c4b-bd32-52315cb3d66f.png)

Provide the name of your EKS Cluster then select the Cluster role that we have created for EKS Cluster and rest of the things will be as they as and click on **Next.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659720696/fc453402-0ede-41ed-b581-3f8d18bd7a32.png)

In the network configuration,

Select the vpc that we created earlier with both subnets. Apart from that, others will be as it is, and click on **Next.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659722140/c5656e27-615e-44fe-b0ab-000c2ad0732e.png)

Keep the default things as it is and click on **Next.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659723736/6e339e10-fe8b-4c06-86c9-c18c3bbe2019.png)

Keep the default things as it is and click on **Next.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659725205/5cff6c99-812c-4aa8-9cdf-d9f656077919.png)

Keep the default things as it is and click on **Next.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659726710/d782e1de-7ca4-4ee8-9176-50c6c484109f.png)

Keep the default things as it is and click on **Create.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659728266/5c966de5-a280-4232-9ad1-4e27132e8ef8.png)

After clicking on **Create**, AWS will take around 4 to 5 minutes to create the EKS. Meanwhile, let’s install Kubectl to work on AWS EKS.

curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.28.3/2023-11-14/bin/linux/amd64/kubectl

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659729609/a3317699-bb3b-4365-878d-c543e1419b4f.png)

curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.28.3/2023-11-14/bin/linux/amd64/kubectl.sha256

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659731169/2039d6c9-77f2-4e65-9880-49944e569c3b.png)

sha256sum -c kubectl.sha256

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659732846/8d4275a7-bf95-4358-b8f0-a01f766d2984.png)

openssl sha1 -sha256 kubectl

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659734256/d09cfffb-ece9-426b-a02e-286170c16465.png)

chmod +x ./kubectl  
mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$HOME/bin:$PATH  
echo 'export PATH=$HOME/bin:$PATH' >> ~/.bashrc

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659735590/5d08f470-c541-4b6d-abc8-5674f35e3b35.png)

kubectl version — client

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659736717/c3da846e-8a78-48f5-b8fe-19c1a2663032.png)

Install eksctl on the local machine (*Optional)*

ARCH=amd64  
PLATFORM=$(uname -s)\_$ARCH  
curl -sLO "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl\_$PLATFORM.tar.gz"  
curl -sL "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl\_checksums.txt" | grep $PLATFORM | sha256sum - check  
tar -xzf eksctl\_$PLATFORM.tar.gz -C /tmp && rm eksctl\_$PLATFORM.tar.gz  
sudo mv /tmp/eksctl /usr/local/bin

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659738113/41551e78-adef-4875-b997-3798e8b2fd1b.png)

Now, check the status of EKS whether it is Active or Not.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659739651/ad83d3da-b149-4020-9972-06eda007fc5c.png)

Once the status of EKS is Active, run the below command.

aws eks update\-kubeconfig — region us\-east\-1 — name EKS\-Cluster\-Demo

If you are getting the error which is showing below in the snippet. Then, don’t worry. Let’s solve it in the next step.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659741483/54bf925e-2c23-4003-b07b-84352d18ccfd.png)

Replace the ‘alpha’ with ‘beta’ like below snippet

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659742867/f354b7b6-1f6e-46fe-9526-dcabb5d02f2e.png)

Now, run the command again to update the config

aws eks update\-kubeconfig — region us\-east\-1 — name EKS\-Cluster\-Demo

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659744032/cbcd294f-35e8-49cd-bd9b-15024264a5b8.png)

It’s working

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659745449/f3ae38d3-f124-4a9e-b26d-e0df11d7a9b7.png)

Trying to deploy the pod but it is in pending status because there is no worker node present where the pod can be created.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659746655/c65dd5c3-a01a-47d5-84a6-bf2c80592237.png)

To create a worker node, Select the EKS Cluster and navigate to the **Compute** section.

Click on **Add node group.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659748728/bc3673cd-6c53-4add-a17e-96ed238e1e54.png)

Provide the name of your worker node and select the Worker Node role that we created earlier.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659750044/f4446151-6e23-4fe6-a1d0-8312faf84046.png)

You can modify things according to your requirements. But the instance type t3.medium will be good because Kubernetes needs at least 2CPU.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659751474/f323a4ed-dfce-4735-83cd-f13ee181344c.png)

Select the Subnets of the VPC that we have created above and click on **Next**.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659752778/751fca00-6ad8-471b-b4dc-3abf331765d3.png)

Once the node is in **Active** status. Then, you can follow the next step.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659754592/d4a2ef27-b602-4adf-b883-7084e1cccfc9.png)

Run the below command and you will see that our pending pod is now in running state.

kubectl get pods

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659756059/2e4f5fc1-1a38-47ba-8b38-dded69bd51f4.png)

Now, delete the previous, and let’s try to run the static application on the nginx server with the AWS load balancer

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

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659757670/c0609a5c-7412-485d-ae45-641dcc02700a.png)

Now, host the application outside of the Kubernetes Cluster by creating a service for the nginx application and observing the load balancer dns in the EXTERNAL-IP Column.

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

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659759379/1447252b-de04-4d56-9f78-716a0ae7048f.png)

Now, navigate to AWS Console and go to the Load Balancer section.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659760975/8179d202-9edf-4d91-a631-0f5a340d7863.png)

Copy the Load balancer DNS then, hit on the browser and see the magic.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659762518/f014afae-2bb5-4fb8-b4aa-a6deaf818c44.png)

### Conclusion

AWS EKS simplifies Kubernetes management, allowing you to focus on application deployment. Today’s journey introduced the basics, key features, costing, and a hands-on demo. Explore the power of EKS and stay tuned for more Kubernetes adventures! 🚀💡

### Want to Know About Challenge?

If you’re eager to learn more and join our challenge through the GitHub Repository, stay tuned for the upcoming posts. Follow for more exciting insights into the world of Kubernetes!

**GitHub Repository**: [https://github.com/AmanPathak-DevOps/30DaysOfKubernetes](https://github.com/AmanPathak-DevOps/30DaysOfKubernetes)

#Kubernetes #DaemonSet #StatefulSets #NetworkPolicy #Operators #ContainerOrchestration #DevOps #EKS #AWS #K8sLearning

See you on Day 28 as we unravel more Kubernetes mysteries!

Stay connected on **LinkedIn**: [LinkedIn Profile](https://www.linkedin.com/in/aman-devops/)

Stay up-to-date with **GitHub**: [GitHub Profile](https://github.com/AmanPathak-DevOps)

Feel free to reach out to me, if you have any other queries.

Happy Learning