---
title: "Day 20 — Mastering Multi-Cluster Kubernetes with HAProxy"
datePublished: Tue Oct 24 2023 14:52:12 GMT+0000 (Coordinated Universal Time)
cuid: clqnfgbul000o08i8cvlkhqyj
slug: day-20-mastering-multi-cluster-kubernetes-with-haproxy-be63b08a4ec7
cover: https://cdn-images-1.medium.com/max/800/1*UDWAvojHCTmjd_i_iiQBcA.gif

---

### *Introduction*

Welcome to Day 20 of our exciting journey into the world of Kubernetes! Today, we’re diving deep into the realm of multi-cluster Kubernetes, a topic that’s gaining immense popularity for its ability to distribute workloads across different clusters. We’ll also explore the role of HAProxy, a powerful load balancer, in orchestrating our multi-cluster setup. So, let’s strap in and embark on this enlightening adventure.

### **Why Multi Cluster**

Multi-cluster Kubernetes setups are beneficial for various reasons, including high availability, disaster recovery, and geographical distribution. Having multiple clusters can ensure that if one cluster fails, your application remains available in other clusters. It also helps distribute workloads geographically, improving latency for users in different regions.

### **HAproxy**

HAProxy is used as a load balancer to distribute traffic across multiple Kubernetes clusters. It plays a crucial role in maintaining high availability by redirecting traffic to available clusters. In the provided setup, it acts as an entry point, routing requests to the appropriate Kubernetes cluster.

I have added the details for all Five servers. So, you will be able to understand the high overview of all the servers

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660375832/2ee90377-4b3a-4244-a304-931cbfeaf845.png)

We have to set up five servers where two will be Master Nodes, the other two will be Worker Nodes, and the last one HAproxy.

### Create HAproxy Server(EC2 Instance)

Click on **Launch instances**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660377371/84c4263b-eb86-49fb-875d-b758958b608c.png)

Enter the name of the instance and select Ubuntu 22.04(must).

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660379132/9c64816a-7e2f-40b7-8755-b6e76c2ed4c7.png)

The instance type will be **t2.micro** and click on **Create new key pair** for this demo.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660380871/fe641c97-acd5-4dab-b994-6033790b1303.png)

Enter the name, keep the things as it is, and click on **Create key pair**.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660382469/fe6c8d30-94b8-4680-94b1-c18efa5f369f.png)

Select the default vpc and select the subnet from the availability zone **us-east-1a** and create the new security group with **All Traffic type** where the **Source type** will be Anywhere.

Here we have configured everything for HAproxy, So click on **Launch Instance.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660384196/7d535843-4372-4a83-a883-9ef7fe44f346.png)

### Creating Master Nodes(EC2 Instance)

Here, we have to set up the two Master Nodes.

Enter the name of the instance and select Ubuntu 22.04(must) and in the right **number of instances** will be **2\.** So that, we will save us some time.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660385739/62d20855-ef43-4342-bcee-a663c935d1ed.png)

The master node needs **2CPU** that will get in the **t2.medium** instance type.

Provide the same key that we have provided for the **HAproxy server**.

Select the same **VPC** and same **Subnet** that we have provided for **the HAproxy server.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660387231/934897cc-c576-42f9-b236-4996c14bb9af.png)

Select the same **Security Group** that we have created for the **HAproxy server.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660388663/3d2e4973-9ee4-4f47-8917-fa4cebc9228f.png)

### Creating Worker Nodes(EC2 Instance)

Here, we have to set up the two Worker Nodes.

Enter the name of the instance and select Ubuntu 22.04(must) and in the right **number of instances** will be **2\.** So that, we will save us some time.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660390513/57909949-bf95-41b7-9c56-cb96b1b05c75.png)

The master node doesn’t need **2CPU** that’s why the **instance type** will be the same **t2.micro**

Provide the same key that we have provided for the **HAproxy server**.

Select the same **VPC** and same **Subnet** that we have provided for **the HAproxy server.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660392214/763126b1-cd89-444f-9050-6890e670278b.png)

Select the same **Security Group** that we have created for the **HAproxy server.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660393719/db164d48-d2b0-4769-8259-6752409121ef.png)

Now, both the master and worker nodes' names will be the same. So, you can modify the name of each by masternode1 and masternode2, the same for the worker node.

This is the total of five servers that we have created.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660395151/c060e928-6a85-46e0-bf6c-2f0b80c7ada7.png)

Now, we have to do the configurations in all the servers. Let’s do this and start with the **HAproxy** server.

**On HAproxy Server**

Before doing **SSH,** modify the permission of the **PEM** file that we will use to do SSH.

sudo su  
chmod 400 Demo-MultiCluster.pem

Now, use the command to **SSH** into the **HAproxy** server.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660397224/f407eaad-f5cc-4dd0-b513-7d1f491729e7.png)

To become a root user run the below command

sudo su

Now, update the **package** and install **haproxy** which will help us to set our Kubernetes multi-cluster

apt update && apt install \-y haproxy

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660398755/c5ad0b76-c748-42b5-8eff-e39a2239219b.png)

Here, we have to set the backend and frontend to set up Kubernetes Multi-Cluster.

Open the file haproxy.cfg and add the code snippets according to your **Private IPs**

vim /etc/haproxy/haproxy.cfg

Remember, in frontend block **HAproxy Private IP** needs to be present.

In the backend block, Both **Master Node IP** needs to be present.

frontend kubernetes\-frontend  
    bind 172.31.22.132:6443  
    mode tcp  
    option tcplog  
    default\_backend kubernetes\-backend  
  
backend kubernetes\-backend  
    mode tcp  
    option tcp\-check  
    balance roundrobin  
    server kmaster1 172.31.23.243:6443 check fall 3 rise 2  
    server kmaster2 172.31.28.74:6443 check fall 3 rise 2

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660400283/e5233f7f-9dc5-47bc-8d05-cb87c67e0d8c.png)

Once you add the frontend and backend, restart the haproxy service

systemctl restart haproxy

Now, check the status of whether the haproxy service is running or not

systemctl status haproxy

If you look at some bottom lines, the kmaster1 and kmaster2 nodes are down which is correct. But this indicates that the frontend and backend code are reflected.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660402029/baa6a0a4-b7c5-42a7-a928-a1da524dadbf.png)

Now, add the **hostnames** in the /etc/hosts files with all five servers' **Private IPs** like below

vim /etc/hosts

172.31.23.243 k8master1.node.com node.com k8master1  
172.31.28.74 k8master2.node.com node.com k8master2  
172.31.31.111 k8worker1.node.com node.com k8worker1  
172.31.22.133 k8worker2.node.com node.com k8worker2  
172.31.22.132 lb.node.com node.com lb

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660403864/1c4d0ea8-0b10-4142-921c-a47a05734686.png)

Now, try to **ping** all four servers(Master+Worker) from HAproxy. If your machine is receiving the packets then we are good to go for the next step which is configuring the **Master Nodes**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660405395/06c7864e-e063-4b85-9db7-fd18ec5af272.png)

### On Master Nodes

I have provided you the snippets of One Master Nodes only. But I **configured** both **Master nodes**. So, make sure to configure each and everything simultaneously on both **Master Nodes**.

Login to your both **Master Nodes**

Once you log into both machines, run the command that is necessary for both Master Nodes

Now, add the **hostnames** in the /etc/hosts files with all five servers' **Private IPs** like below

vim /etc/hosts

172.31.23.243 k8master1.node.com node.com k8master1  
172.31.28.74 k8master2.node.com node.com k8master2  
172.31.31.111 k8worker1.node.com node.com k8worker1  
172.31.22.133 k8worker2.node.com node.com k8worker2  
172.31.22.132 lb.node.com node.com lb

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660407020/576d6519-c0bf-4a8a-9d66-e8c61fb4a2c5.png)

After closing the host's file, run the below commands.

sudo su  
ufw disable  
reboot

Now, log in again to your both machines after 2 to 3 minutes.

Run the below commands

sudo su  
swapoff -a; sed -i '/swap/d' /etc/fstab

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660408337/587b69ec-b254-49bb-8cbb-b006c6769160.png)

Run the below commands

cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf  
overlay  
br\_netfilter  
EOF  
sudo modprobe overlay  
sudo modprobe br\_netfilter  
\# sysctl params required by setup, params persist across reboots  
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf  
net.bridge.bridge-nf-call-iptables = 1  
net.bridge.bridge-nf-call-ip6tables = 1  
net.ipv4.ip\_forward = 1  
EOF  
\# Apply sysctl params without reboot  
sudo sysctl - system

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660411304/ade767ce-7f88-4169-bf33-0fd9775d8c79.png)

Install some dependencies packages and add the Kubernetes package

sudo apt-get install -y apt-transport-https ca-certificates curl  
curl -fsSL https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo gpg - dearmor -o /etc/apt/keyrings/kubernetes-archive-keyring.gpg  
echo "deb \[signed-by=/etc/apt/keyrings/kubernetes-archive-keyring.gpg\] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660413024/d1be8f3e-d193-40b9-8524-ccb41f18a71f.png)

As we have added the gpg keys, we need to run the update command

apt update

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660414651/69c40541-738e-44c8-bc55-7dd9ad6c91af.png)

Now, we have to install docker on our both master nodes

apt install docker.io -y

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660416194/419a1a62-c2bf-4db6-ba01-e40c583b2b69.png)

Do some configurations for containerd service

mkdir /etc/containerd  
sh -c "containerd config default > /etc/containerd/config.toml"  
sed -i 's/ SystemdCgroup = false/ SystemdCgroup = true/' /etc/containerd/config.toml  
systemctl restart containerd.service

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660417937/cffdf673-9b27-4a8f-8200-5be4c9234b3a.png)

Now, we will install our kubelet, kubeadm, and kubectl services on the Master node

apt-get install -y kubelet kubeadm kubectl kubernetes-cni

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660419430/d7bca034-ccb5-4802-9661-dcc1c4dbf884.png)

Now, restart the kubelet service, and don’t forget to enable the kubelet service. So that, if any master node will reboot then we don’t need to start the kubelet service.

sudo systemctl restart kubelet.service  
sudo systemctl enable kubelet.service

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660420677/d320d4b7-aed1-473a-937e-51f5696603dd.png)

**Only on Master Node1**

This command must need to run on **Master Node1**

We have to **init** the **kubeadm** and provide the endpoint which will be the **haproxy server Private IP** and in the end provide the **Master Node1 IP** only.

kubeadm init — control-plane-endpoint=”<hap-private-ip:6443" — upload-certs — apiserver-advertise-address=<master-node1-private-ip>

kubeadm init — control-plane-endpoint=”172.31.22.132:6443" — upload-certs — apiserver-advertise-address=172.31.28.74

Once you run the above command, scroll down.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660422490/5ff6a8a8-ef98-43bf-96d9-e646b7503fb0.png)

Once you scroll down, you will see Kubernetes control plan has initialized successfully which means Master node1 joined the HAproxy server. Now, we have to initialize Kubernetes Master Node2 as well. Follow the below steps:

*   Copy the Red highlighted commands, Blue highlighted commands and Green highlighted commands and paste them into your notepad.
*   Now, run the Red highlighted commands on the Master node1 itself.

mkdir -p $HOME/.kube  
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config  
sudo chown $(id -u):$(id -g) $HOME/.kube/config

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660423926/de6ffbd3-44a8-474b-b136-0466dfc619b5.png)

### On Master Node2

Now, we need to do one more thing so that Master Node2 joins the HAproxy server as well. Follow the steps:

*   We have to use the Blue highlighted command, but we need to add one more thing with the command, refer to the below(add — apiserver-advertise-address=<PrivateIP-of-MasterNode2>

kubeadm join 172.31.22.132:6443 - token 0vzbaf.slplmyokc1lqland \\  
 - discovery-token-ca-cert-hash sha256:75c9d830b358fd6d372e03af0e7965036bce657901757e8b0b789a2e82475223 \\  
 - control-plane - certificate-key 0a5bec27de3f27d623c6104a5e46a38484128cfabb57dbd506227037be6377b4 - apiserver-advertise-address=172.31.28.74

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660425786/c9a48976-f17f-413c-9e69-ed388c075f03.png)

Once you followed the above steps, you can run the below commands on **Master Node2**

mkdir -p $HOME/.kube  
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config  
sudo chown $(id -u):$(id -g) $HOME/.kube/config

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660427210/3acd534c-9458-4fa7-b9ef-c4d2767e95fe.png)

Now, if you run the command ‘kubectl get nodes’ on Master Node1 to see the nodes. You will get both nodes but they are not in ready status because we did not configure the network. We will configure that once the Worker Nodes are configured.

**Note**: Copy the Green highlighted command, which we will use to connect with Worker Nodes

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660428631/497b685d-31a6-4617-b46d-fa2172fb21f3.png)

Now, if you run the command ‘kubectl get nodes’ on Master Node2 to see the nodes. You will get both nodes but they are not in ready status because we did not configure the network. We will configure that once the Worker Nodes are configured.

**Note**: Copy the Green highlighted command, which we will use to connect with Worker Nodes

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660429902/ca963141-06d1-437b-8f7e-06c60b3ef6ed.png)

### On Both Worker Nodes

Now, Let’s configure our Worker Nodes.

I have provided you the snippets of One Master Nodes only. But I **configured** both **Worker nodes**. So, make sure to configure each and everything simultaneously on both **Worker Nodes**.

Login to your both **Worker** **Nodes**

Once you log into your both machines, run the command that is necessary for both Master Nodes

Now, add the **hostnames** in the /etc/hosts files with all five servers' **Private IPs** like below

vim /etc/hosts

172.31.23.243 k8master1.node.com node.com k8master1  
172.31.28.74 k8master2.node.com node.com k8master2  
172.31.31.111 k8worker1.node.com node.com k8worker1  
172.31.22.133 k8worker2.node.com node.com k8worker2  
172.31.22.132 lb.node.com node.com lb

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660431467/9fd280a8-7277-405a-b9e1-f7ef928e31e1.png)

After closing the host's file, run the below commands.

sudo su  
ufw disable  
reboot

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660433257/2ea3a604-034c-46d5-9a6f-5527063194e1.png)

Now, log in again to both machines after 2 to 3 minutes.

Run the below commands

sudo su  
swapoff -a; sed -i '/swap/d' /etc/fstab

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660434533/690a2014-8667-4469-9002-3059ccc6c282.png)

Run the below commands:

cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf  
overlay  
br\_netfilter  
EOF  
sudo modprobe overlay  
sudo modprobe br\_netfilter  
\# sysctl params required by setup, params persist across reboots  
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf  
net.bridge.bridge-nf-call-iptables = 1  
net.bridge.bridge-nf-call-ip6tables = 1  
net.ipv4.ip\_forward = 1  
EOF  
\# Apply sysctl params without reboot  
sudo sysctl - system

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660436200/88f70ea7-0360-4331-bcf8-1b561ca46001.png)

Install some dependencies packages and add the Kubernetes package

sudo apt-get install -y apt-transport-https ca-certificates curl  
curl -fsSL https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo gpg - dearmor -o /etc/apt/keyrings/kubernetes-archive-keyring.gpg  
echo "deb \[signed-by=/etc/apt/keyrings/kubernetes-archive-keyring.gpg\] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660437931/ef4d5713-1fc2-4ae4-89a4-beb0c884f22e.png)

As we have added the gpg keys, we need to run the update command

apt update

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660439596/315c0cc2-b92b-4b2c-adc6-15871382f1d8.png)

Now, we have to install docker on our both worker nodes

apt install docker.io -y

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660441265/bd4adba4-7a5a-4c2d-a2cc-d1a48db2ed51.png)

Do some configurations for containerd service

mkdir /etc/containerd  
sh -c "containerd config default > /etc/containerd/config.toml"  
sed -i 's/ SystemdCgroup = false/ SystemdCgroup = true/' /etc/containerd/config.toml  
systemctl restart containerd.service

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660442909/ce54b664-6b14-4ad2-9059-d36c2543dff9.png)

Now, we will install our kubelet, kubeadm, and kubectl services on the **Worker node**

apt-get install -y kubelet kubeadm kubectl kubernetes-cni

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660444670/aa63453a-55e0-49da-8313-3cbf73d751d3.png)

Now, restart the kubelet service, and don’t forget to enable the kubelet service. So that, if any worker node will reboot then we don’t need to start the kubelet service.

sudo systemctl restart kubelet.service  
sudo systemctl enable kubelet.service

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660446287/14e18e1f-de45-465b-807a-d152f754e838.png)

If you remember, I told you to copy the **Green highlighted command.**  
Paste that command, on both Worker Node1 and Worker Node2.

Once you do that, you will see the output like the below snippet.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660447582/65950b42-fd23-47a3-8cae-0fc4f956ff4c.png)

Run on any Master Node

Let’s validate whether both Worker Nodes are joined in the Kubernetes Cluster by running the below command.

kubectl get nodes

If you can see four servers then, Congratulations you did 99% work.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660449069/36db6c01-6234-4110-a76a-2b3b5f7ac83b.png)

As you know, our all nodes are not in ready status because of network components.

Run the below command to add the Calico networking components in the Kubernetes Cluster.

kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.25.0/manifests/calico.yaml

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660450585/430324b8-ed20-4c72-bb6e-42ace4888076.png)

After 2 to 3 minutes, if you run the command ‘*kubectl get nodes*’. You will see that all nodes are getting ready.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660452266/3054a8f4-05a2-4e65-af51-3a2b336d6720.png)

Let’s deploy the Nginx Container on Worker Node1 and the Apache Container on Worker Node2

To achieve this, you have to perform the commands on Master Nodes only.

> Add a label on both worker nodes

For **WorkerNode1**

kubectl label nodes <WorkerNode1-Private-IP\> mynode=node1

For **WorkerNode2**

kubectl label nodes <WorkerNode2-Private-IP\> mynode=node2

You can also validate whether the labels are added to both Worker Nodes or not by running the below command

kubectl get nodes — show\-labels

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660453911/3854b085-9513-400a-8b1f-df6a7ae60ae7.png)

**Let’s create two Container on both different Worker nodes from different Master nodes**

I am creating an Nginx Container on Worker Node1 from Master node1

Here is the deployment YML file

apiVersion: apps/v1  
kind: Deployment  
metadata:  
  name: nginx-deployment  
spec:  
  replicas: 1  
  selector:  
    matchLabels:  
      app: nginx  
  template:  
    metadata:  
      labels:  
        app: nginx  
    spec:  
      nodeSelector:  
        mynode: node1  \# This deploys the container on node1  
      containers:  
      \- name: nginx  
        image: nginx:latest  
        ports:  
        \- containerPort: 80

Here is the service YML file

apiVersion: v1  
kind: Service  
metadata:  
  name: nginx-service  
spec:  
  selector:  
    app: nginx  
  ports:  
  \- protocol: TCP  
    port: 80  
    targetPort: 80  
  type: LoadBalancer  \# Use LoadBalancer for external access

Apply both files by the below commands

kubectl apply -f deployment.yml  
kubectl apply -f service.yml

Validate whether the deployment is complete or not by running the below commands

kubectl get deploy  
kubectl get pods  
kubectl get svc

Now, to check whether the application is running or not from outside of the cluster. Copy the worker node1 public ip and then use the port that is showing when you run the ‘kubectl get svc’ command that i have highlighted in the snippet.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660455406/6c02d9e9-8be4-4788-b3a6-1794da5c050a.png)

Here, you can see our nginx container is perfectly running outside of the Cluster.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660456974/1fd65365-e192-4f32-8281-4b27e5e9eed0.png)

The second Container from the Second Master Node on the Second Worker Node

I am creating Apache Container on Worker Node2 from Master node2

Here is the deployment YML file

apiVersion: apps/v1  
kind: Deployment  
metadata:  
  name: apache-deployment  
spec:  
  replicas: 1  
  selector:  
    matchLabels:  
      app: apache  
  template:  
    metadata:  
      labels:  
        app: apache  
    spec:  
      nodeSelector:  
        mynode: node2  \# This deploys the container on node1  
      containers:  
      \- name: apache  
        image: httpd:latest  
        ports:  
        \- containerPort: 80

Here is the service YML file

apiVersion: v1  
kind: Service  
metadata:  
  name: apache-service  
spec:  
  selector:  
    app: apache  
  ports:  
  \- protocol: TCP  
    port: 80  
    targetPort: 80  
  type: LoadBalancer  \# Use LoadBalancer for external access

Apply both files by the below commands

kubectl apply -f deployment.yml  
kubectl apply -f service.yml

Validate whether the deployment is complete or not by running the below commands

kubectl get deploy  
kubectl get pods  
kubectl get svc

Now, to check whether the application is running or not from outside of the cluster. Copy the worker node2 public IP and then use the port that is showing when you run the ‘kubectl get svc’ command corresponding to port 80.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660458683/02030ced-420e-456c-ba55-da81a83f40f0.png)

Here, you can see our Apache container is perfectly running outside of the Cluster.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660459927/9aeead88-eeab-4a77-90d6-2b51406cded8.png)

### *Conclusion*

In conclusion, Day 20 has taken us on an incredible journey through the intricacies of multi-cluster Kubernetes with HAProxy. We’ve set up a multi-cluster environment, configured HAProxy for load balancing, and even deployed applications across different clusters. This newfound knowledge is invaluable, as it equips us to build scalable, resilient, and highly available Kubernetes setups.

As we continue to explore the vast landscape of DevOps, Kubernetes, and more, remember that learning is a journey. Each day brings us closer to mastering the art of container orchestration and the world of cloud-native technologies. So, keep the curiosity alive, stay eager to learn, and watch as your expertise in these domains continues to grow.

Stay tuned for Day 21, where we’ll delve even deeper into the fascinating world of DevOps. Until then, keep practicing, keep exploring, and keep embracing the wonderful world of Kubernetes.

### Want to Know About Challenge?

If you’re eager to learn more and join our challenge through the GitHub Repository, stay tuned for the upcoming posts. Follow for more exciting insights into the world of Kubernetes!

**GitHub Repository**: [https://github.com/AmanPathak-DevOps/30DaysOfKubernetes](https://github.com/AmanPathak-DevOps/30DaysOfKubernetes)

#Kubernetes #ReplicationController #ReplicaSet #ContainerOrchestration #DevOps #K8sLearning

See you on Day **21** as we unravel more Kubernetes mysteries!

Stay connected on **LinkedIn**: [LinkedIn Profile](https://www.linkedin.com/in/aman-devops/)

Stay up-to-date with **GitHub**: [GitHub Profile](https://github.com/AmanPathak-DevOps)

Feel free to reach out to me, if you have any other queries.

Happy Learning