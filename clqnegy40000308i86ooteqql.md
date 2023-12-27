---
title: "Day 04: Setting up Minikube on Your Local Machine or AWS Instance"
datePublished: Fri Oct 06 2023 16:51:38 GMT+0000 (Coordinated Universal Time)
cuid: clqnegy40000308i86ooteqql
slug: day-04-setting-up-minikube-on-your-local-machine-or-aws-instance-620a4cb57abc
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703658811414/e85ca44a-92db-4084-bddf-780d1ba9ea99.png

---

Welcome to Day 04 of the #30DaysOfKubernetes challenge! Today, we’ll dive into the practical aspect of working with Kubernetes. We’re going to set up Minikube, a tool that enables you to run a Kubernetes cluster on your local machine or an AWS instance.

### Why Set Up Minikube?

Before we begin, you might be wondering why it’s essential to set up Minikube. Well, Minikube provides an excellent environment for learning and experimenting with Kubernetes without the need for a full-scale cluster. It’s perfect for developers and enthusiasts who want to get hands-on experience with Kubernetes in a controlled environment.

### Prerequisites

To follow along with this tutorial, you’ll need the following:

*   An AWS account (if you’re setting up on an AWS instance).
*   Basic knowledge of AWS and Linux terminal commands.

Let’s get started!

### Setting Up Minikube on AWS Instance

Here, I am creating an EC2 Instance to set up minikube on the server. If you are comfortable setting up minikube on your local then feel free to jump on the minikube setup.

> Enter the name of the machine and select the Ubuntu22.04 AMI Image.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658783544/efd1731e-cbde-4342-9745-0a8056f31b23.png)

> Make sure to select the t2.medium instance type as Master node 2CPU cores which is present in the t2.medium instance type.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658785205/d8a3308c-cce4-4847-a94c-045cb11a68a4.png)

> Create a new key pair and select the Private key file format according to your OS(For Windows select .ppk or for Linux select .pem).

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658786631/357c131d-45f8-4c5e-8f36-a9f2f4210e50.png)

> Open port 22 and rest you can leave it.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658788110/c5bb91ba-86a8-4713-9809-74c880c3cd63.png)

> Now, go to your Downloads folder or where you have downloaded your pem file and change the permission by running the command ‘chmod 400 <Pem\_file\_name>

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658789653/3be4e080-46be-4f4f-bc9d-29beb07b1f31.png)

> Now, connect your instance by copying the given command below.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658791014/32eaa5b1-23a7-41d7-a251-ed0722f0a3bc.png)

> As you can see I logged in to the Instance.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658792611/bd4d23f5-8a74-4dfe-87c5-236d08b2dd06.png)

Now, run the following commands to install minikube on your local machine or AWS machine.

sudo apt update \-y && sudo apt upgrade \-y  
sudo reboot

After 3 to 4 minutes, reconnect with the instance through ssh

sudo apt install docker.io  
sudo usermod -aG docker $USER && newgrp docker  
sudo apt install -y curl wget apt-transport-https  
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64  
sudo install minikube-linux-amd64 /usr/local/bin/minikube  
minikube version

I have given the line break in the below curl command, Kindly avoid the break and any whitespaces after \`. You can refer to the below screenshot.

curl -LO https://storage.googleapis.com/kubernetes-release/release/\`  
curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt\`/bin/linux/amd64/kubectl

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658794437/3e1901da-73b9-44cf-8701-28024c57a8f7.png)

chmod +x kubectl  
sudo mv kubectl /usr/local/bin  
kubectl version -o yaml  
minikube start - vm-driver=docker

Now, to verify the installation you can run the given command and if you get the result in the snippet then your installation is completed.

minikube status

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658796094/b6ca0ba5-bc03-46cd-938d-e5b68fa55b62.png)

You can also validate your kubectl version by running the command.

kubectl version

Now, run the given command after 4 to 5 minutes which will show the nodes.

kubectl get nodes

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658797517/8550d124-f333-47ff-b94b-9cd63799e335.png)

### Creating Your First Pod

To get some hands-on experience, let’s create a simple manifest file and deploy a pod on your Minikube cluster. Don’t worry if you don’t understand everything in the manifest file; we’ll cover that in later days.

Create a new file and copy the given content to your file without editing anything. While running the manifest file, if you get any error then it must be related to the indentation of the file. So, check the file again.

vim Day04.yml

kind: Pod                                
apiVersion: v1                       
metadata:                             
  name: testpod                    
spec:                                      
  containers:                        
    \- name: c00                       
      image: ubuntu                
      command: \["/bin/bash", "-c", "while true; do echo Hello-Kubernetes; sleep 5 ; done"\]  
    \- name: container2  
      image: ubuntu  
      command: \["/bin/bash", "-c", "while true; do echo Second Container is still running; sleep 3 ; done"\]  
  restartPolicy: Never

**Deploy the Pod:**

Run the following command to deploy the pod:

kubectl apply -f Day04.yml

**List Pods:**

To list all the pods, use this command:

kubectl get pods

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658798743/6814b7b5-dd0a-4456-ab9b-7dac4b266f79.png)

**Check Logs:**

You can check the logs of the primary container with:

kubectl logs -f pod1

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658800595/c2489a44-7e0a-494a-9ba0-213bc0ca5bfd.png)

To check the logs of the primary container, specify the container name:

kubectl logs \-f pod1 \-c container1

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658801879/aa4611a4-0311-4277-92eb-562bbf2aabbb.png)

To check the logs of the second container, specify the container name:

kubectl logs \-f pod1 \-c container2

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658803523/c1f642cd-0d01-427f-a6d6-731bb9dd160a.png)

**Delete the Pod:**

To delete the pod, use this command:

kubectl delete pod pod1

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658804748/a0918a50-861c-4663-803e-3f264a050613.png)

To list the IP of the pod, use the below command.

kubectl pod pod1 -c container1 — hostname -i

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658806145/160e94be-0bf3-4fc4-82c7-ea85ec344038.png)

To delete the pod by specifying the manifest file name

kubectl delete -f Day04.yml

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658807429/984a72e3-ca64-4407-adcb-077aed71702b.png)

### Conclusion

Congratulations! You’ve successfully set up Minikube on your local machine or an AWS instance and created your first pod. In the coming days, we’ll dive deeper into Kubernetes concepts, so stay tuned.

Feel free to explore more and share your progress. Happy learning, Kubernetes enthusiasts!

### Want to Know About Challenge?

If you’re eager to learn more and join our challenge through the GitHub Repository, stay tuned for the upcoming posts. Follow for more exciting insights into the world of Kubernetes!

**GitHub Repository**: [https://github.com/AmanPathak-DevOps/30DaysOfKubernetes](https://github.com/AmanPathak-DevOps/30DaysOfKubernetes)

#30DaysOfKubernetes #Kubernetes #MasterNode #ControlPlane #K8s

See you on Day **05** as we unravel more Kubernetes mysteries!

Stay connected on **LinkedIn**: [LinkedIn Profile](https://www.linkedin.com/in/aman-devops/)

Stay up-to-date with **GitHub**: [GitHub Profile](https://github.com/AmanPathak-DevOps)

Feel free to reach out to me, if you have any other queries.

Happy Learning

![](https://cdn-images-1.medium.com/max/800/0*rAOjEZ8VFrY14MMw.gif)