---
title: "Day10- Setting up a Kubernetes Cluster(Master+Worker Node) using kubeadm on AWS EC2…"
datePublished: Thu Oct 12 2023 15:52:37 GMT+0000 (Coordinated Universal Time)
cuid: clqnf91ba000o08iefyjxc3b6
slug: setting-up-a-kubernetes-cluster-master-worker-node-using-kubeadm-on-aws-ec2-instances-ubuntu-22-04-3432859b943b

---

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660065405/7b6658c2-e3a3-4543-b8eb-911b9f606cfe.png)

**Introduction**

Welcome to Day 10 of our #30DaysOfKubernetes journey! Today, we’ll guide you through the process of setting up a Kubernetes cluster on a master and worker node. Let’s embark on this exciting journey of creating a Kubernetes environment!

**Objectives**  
By the end of this tutorial, you will:

Create three EC2 instances with Ubuntu 22.04 and the necessary security group settings.  
Configure the instances to prepare for Kubernetes installation.  
Install Docker and Kubernetes components on all nodes.  
Initialize the Kubernetes cluster on the master node.  
Join the worker nodes in the cluster.  
Deploy an Nginx application on the cluster for validation.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660068741/72b9db96-ed71-41fc-bc0a-babe2b6516f3.png)

**Step 1:**

Create three EC2 Instances with the given configuration

> Instance type- t2.medium

> Ubuntu Version- 22.04

> Create the keypair so you can connect to the instance using SSH.

> Create the new Security group and once the instances are initialized/created then, make sure to add the Allow All traffic in inbound rule in the attached security group.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660070149/08ab92c5-e908-4a24-a6c4-021e3910555c.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660071655/c5754f7d-e51e-4024-9711-559f10cf113d.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660073496/7aee88b4-6806-410f-ad11-974ba00b2506.png)

Rename the instances according to you. Currently, I am setting up the One Master and two Workers nodes.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660075324/f417e596-76fa-41d6-984f-d62b07e0a817.png)

After creating the instances, we have to configure the all instances. Let’s do that and follow the steps carefully.

Commands need to run on all Nodes(Master and Worker)

Once we log in the all three instances, run the following command.

**Step 2:**

sudo su  
swapoff -a; sed -i '/swap/d' /etc/fstab

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660076922/57997ace-750d-4ab7-8b4a-e40bc7f842d0.png)

**Step 3:**

cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf  
overlay  
br\_netfilter  
EOF  
  
sudo modprobe overlay  
sudo modprobe br\_netfilter  
  
\# sysctl params required by setup, params persist across reboots  
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf  
net.bridge.bridge-nf-call-iptables  = 1  
net.bridge.bridge-nf-call-ip6tables = 1  
net.ipv4.ip\_forward                 = 1  
EOF  
  
\# Apply sysctl params without reboot  
sudo sysctl --system

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660078519/d5fdd13a-71a4-4c13-9442-98d7ec776f15.png)

**Step 4:**

apt update

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660080245/f625b4a7-471b-4711-a5e9-18ae33e54d5a.png)

**Step 5:**

Install dependencies by running the command

sudo apt-get install -y apt-transport-https ca-certificates curl

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660082198/3d64f334-8db4-4089-ae57-fd5ca6106f0b.png)

**Step 6:**

Fetch the public key from Google to validate the Kubernetes packages once it will be installed.

curl -fsSL https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-archive-keyring.gpg

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660084040/77720905-4a79-451d-b6c3-2c32dd373ca7.png)

**Step 7:**

Add the Kubernetes package in the sources.list.d directory

echo "deb \[signed-by=/etc/apt/keyrings/kubernetes-archive-keyring.gpg\] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660085484/41e7db4b-9407-4756-9106-872efbe17946.png)

**Step 8:**

Update the packages as we have added some keys and packages.

apt update

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660087129/1ee7d87d-8763-465e-adfa-528212795e60.png)

**Step 9:**

Install kubelet, kubeadm, kubectl and kubernets-cni

apt-get install -y kubelet kubeadm kubectl kubernetes-cni

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660088658/353d0091-fce0-4166-86ef-7ded4aca4554.png)

**Step 10:**

This is one of the important dependencies to setting up the Master and Worker nodes. Installing docker.

apt install docker.io -y

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660090484/4532d367-2993-4308-a3e3-22db1376251f.png)

**Step 11:**

Configuring containerd to ensure compatibility with Kubernetes

sudo mkdir /etc/containerd

sudo sh -c "containerd config default > /etc/containerd/config.toml"

sudo sed -i 's/ SystemdCgroup = false/ SystemdCgroup = true/' /etc/containerd/config.toml

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660092204/3c3d4b8d-8ba4-4b81-abda-1ddd93e03a6d.png)

**Step 12:**

Restart containerd, kubelet, and enable kubelet so when we reboot our machine the nodes will restart it as well and connect properly.

systemctl restart containerd.service

systemctl restart kubelet.service

systemctl enable kubelet.service

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660093954/584b453a-d77d-4a48-a829-08c16f60394e.png)

Now, we have completed the installation of the things that are needed on both nodes (Master and Worker). But in the next steps, we have to configure things only on the Master Node.

### Only on the Master Node

**Step 13:**

Initialize the Kubernetes cluster and it will pull some images such as kube-apiserver, kube-controller, and many other important components.

kubeadm config images pull

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660095261/2a62768b-747b-4990-a4c1-7084d67e8ee2.png)

**Step 14:**

Now, initialize the Kubernetes cluster which will give you the token or command to connect with this Master node from the Worker node. At the end of this command, you will get some commands that need to run and at the bottom, you will get the **kubeadm join** command that will be run from the **Worker Node** to connect with the Master Node. I have highlighted the commands in the second next snipped. Please keep the **kubeadm join** command somewhere in the notepad.

kubeadm init

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660096638/7d824e28-8b94-417a-b400-83abd6657e61.png)

Keep the **kubeadm join** command in your notepad or somewhere for later.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660098555/284f0c35-3908-4217-8603-326d851dcb89.png)

**Step 15:**

As you have to manage the cluster that’s why you need to create a .kube file copy it to the given directory and change the ownership of the file.

mkdir -p $HOME/.kube

sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config

sudo chown $(id -u):$(id -g) $HOME/.kube/config

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660100128/a2b7f821-e9fd-4512-9cc0-9117f3ca1f7f.png)

**Step 16:**

Verify the kubeconfig by kube-system which will list all the pods. If you observe, there are starting twopods that are not ready status because the network plugin is not installed.

kubectl get po \-n kube\-system

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660101502/e5315f88-8e88-447b-a08a-ef8f13ba1e2a.png)

**Step 17:**

Verify all the cluster component health statuses

kubectl get --raw='/readyz?verbose'

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660103723/3a726bea-02be-40c3-ad6e-30960ceeb575.png)

**Step 18:**

Check the cluster-info

kubectl cluster-info

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660105229/453001e3-bd52-45ae-b7aa-6a854d1d3d28.png)

**Step 19:**

To install the network plugin on the Master node

kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.25.0/manifests/calico.yaml

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660107070/8d7b2742-8e2f-42a5-821c-b65471263434.png)

**Step 20:**

Now, If you run the below command, you will observe the two remaining pods are in ready status which means we are ready to bootstrap by our Workers Node or connect to the Master node through the Worker Node.

kubectl get po \-n kube\-system

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660108718/145cf523-e05c-4c83-a80d-1aa31ceb1624.png)

**Step 21:**

Now, you have to run the command on **Worker Node1.** If you remember, I told you to keep the command somewhere in **Step 14**. Here you have to use your command because your token is different as well as mine.

**Worker Node1**

kubeadm join 10.0.0.15:6443 — token 6c8w3o.5r89f9cfbhiunrep — discovery-token-ca-cert-hash sha256:eec45092dc7341079fc9f2a3399ad6089ed9e86d4eec950ac541363dbc87e6aa

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660110199/26fd3644-9e6c-4ee1-8e2d-b7ce862cd4de.png)

**Step 22:**

Follow the Step 21.

**Worker Node2**

kubeadm join 10.0.0.15:6443 \--token 6c8w3o.5r89f9cfbhiunrep --discovery-token-ca-cert-hash sha256:eec45092dc7341079fc9f2a3399ad6089ed9e86d4eec950ac541363dbc87e6aa

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660111614/b5a2687d-c7be-4fa5-993a-e6183a1dc84d.png)

**Step 23:**

**Now, from here all the commands will be run on Master Node only.**

If you run the below command you will see that the Worker Nodes are present with their respective Private IPs and it is in Ready. status

kubectl get nodes

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660112880/73a2bf43-26c7-4a0d-9889-5287bbcdb17a.png)

Here, we have completed our Setup of Master and Worker Node. Now let’s try to deploy a simple nginx pod on both worker nodes.

**Step 23:**

Run the command, which includes the deployment file to deploy nginx on both worker Node.

cat <<EOF | kubectl apply \-f \-  
apiVersion: apps/v1  
kind: Deployment  
metadata:  
  name: nginx-deployment  
spec:  
  selector:  
    matchLabels:  
      app: nginx  
  replicas: 2   
  template:  
    metadata:  
      labels:  
        app: nginx  
    spec:  
      containers:  
      \- name: nginx  
        image: nginx:latest  
        ports:  
        \- containerPort: 80        
EOF

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660114317/02a6efac-c302-4824-82cd-5c04b1dd5bf9.png)

**Step 24:**

To expose your deployment on NodePort 32000 which means you can access your nginx application on port 32000 through your browser easily.

cat <<EOF | kubectl apply \-f \-  
apiVersion: v1  
kind: Service  
metadata:  
  name: nginx-service  
spec:  
  selector:   
    app: nginx  
  type: NodePort    
  ports:  
    \- port: 80  
      targetPort: 80  
      nodePort: 32000  
EOF

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660115769/a5edefd6-5acf-4bf0-a1c2-42885a8d5671.png)

**Step 25:**

Now, check the pods by the below command and you can see that your pods are in running status.

kubectl get pods

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660117269/053c54b4-2d0c-4465-a491-90816abb3500.png)

**Step 26:**

**On Worker Node1**

You can validate your deployment by copying the Public IP of your WorkerNode1 and adding a colon(:) with port(32000).

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660118699/44e8210a-7a10-4757-a085-646b619024d9.png)

**On Worker Node2**

You can validate your deployment by copying the Public IP of your WorkerNode2 and adding a colon(:) with port(32000).

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660120043/7fc5f628-2d26-487c-975b-465bb812e37f.png)

### **Conclusion:**

Today, you’ve successfully set up a Kubernetes cluster with a master and two worker nodes. This foundational step is crucial for your Kubernetes journey. You’ve paved the way for deploying and managing containerized applications effectively. Keep exploring, and learning, and enjoy your progress in the #30DaysOfKubernetes challenge! #Kubernetes #ContainerOrchestration #30DaysOfK8s

### Want to Know About Challenge?

If you’re eager to learn more and join our challenge through the GitHub Repository, stay tuned for the upcoming posts. Follow for more exciting insights into the world of Kubernetes!

**GitHub Repository**: [https://github.com/AmanPathak-DevOps/30DaysOfKubernetes](https://github.com/AmanPathak-DevOps/30DaysOfKubernetes)

#Kubernetes #ReplicationController #ReplicaSet #ContainerOrchestration #DevOps #K8sLearning

See you on Day **11** as we unravel more Kubernetes mysteries!

Stay connected on **LinkedIn**: [LinkedIn Profile](https://www.linkedin.com/in/aman-devops/)

Stay up-to-date with **GitHub**: [GitHub Profile](https://github.com/AmanPathak-DevOps)

Feel free to reach out to me, if you have any other queries.

Happy Learning

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660121282/f79122fa-a544-498f-ad57-241c0d2ba021.jpeg)