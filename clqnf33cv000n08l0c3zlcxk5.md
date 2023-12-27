---
title: "Day13- Kubernetes Volumes and LivenessProbes"
datePublished: Sun Oct 15 2023 19:10:33 GMT+0000 (Coordinated Universal Time)
cuid: clqnf33cv000n08l0c3zlcxk5
slug: day13-kubernetes-volumes-and-liveness-probes-ea278ff9bb0f
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703659844403/e369587d-4a43-43e4-931c-1582721963dd.png

---

**Introduction**

Welcome to Day 13 of our #30DaysOfKubernetes journey! In today’s exploration, we dive into the world of Kubernetes volumes, unlocking the secrets to handling data in your pods and containers. While Kubernetes primarily works with short-lived data, we’ll unveil the power of volumes and their various types, including EmptyDir, hostPath, Persistent Volumes, and Persistent Volume Claims.

Discover how these storage mechanisms can help you retain and share data effectively within your Kubernetes environment, ensuring your applications have access to the data they need. But that’s not all; we’ll also delve into the fascinating realm of liveness probes, a crucial feature for monitoring the health of your applications. So, let’s embark on this knowledge-packed journey and make your Kubernetes experience even more robust!

The data is a very important thing for an application. In Kubernetes, data is kept for a short time in the applications in the pods/containers. There is no data persistent things available by default by Kubernetes. To overcome this issue, Kubernetes supports Volumes.

But before going into the types of Volumes. Let’s understand some facts about pods and containers' short live data.

*   The volumes reside inside the Pod which stores the data of all containers in that pod.
*   If the container gets deleted, then the data will persist and it will be available for the new container which was created recently.
*   Multiple containers within a pod can share one volume because the volume is attached to the pod.
*   If the Pod gets deleted, then the volume will also get deleted which leads to a loss of data for all containers permanently.
*   After deleting the pod, the new pod will be created with volume but this time volumes don’t have any previous data or any data.
*   There are some types of Volumes supported by Kubernetes:

### **EmptyDir**

*   This is one of the basic volume types that we have discussed earlier in the facts. This volume is used to share the volumes between multiple containers within a pod instead of the host machine or any Master/Worker Node.
*   EmptyDir volume is created when the pod is created and it exists as long as a pod.
*   There is no data available in the EmptyDir volume type when it is created for the first.
*   Containers within the pod can access the other containers' data. However, the mount path can be different for each container.
*   If the Containers get crashed then, the data will still persist and can be accessible by other or newly created containers.

### HandsOn

In this snippet, I have created one file in container1 and looking for the same file and content from container2 which is possible.

**EmptyDir YML file**

apiVersion: v1  
kind: Pod  
metadata:  
  name: emptydir  
spec:  
  containers:  
  \- name: container1  
    image: ubuntu  
    command: \["/bin/bash", "-c", "while true; do echo This is Day13 of 30DaysOfKubernetes; sleep 5 ; done"\]  
    volumeMounts:  
      \- name: day13  
        mountPath: "/tmp/container1"  
  \- name: container2  
    image: centos  
    command: \["/bin/bash", "-c", "while true; do echo Chak de INDIA!; sleep 5 ; done"\]  
    volumeMounts:  
      \- name: day13  
        mountPath: "/tmp/container2"  
  volumes:  
  \- name: day13  
    emptyDir: {}

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659816151/5956a248-5013-4672-a692-2e7017344b5d.png)

In this snippet, I am creating a file in container2 and looking for the file and the same content through container1 which is possible.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659817912/6610e846-ca1d-43ee-8a4d-64e572214c76.png)

### **2\. hostPath**

*   This volume type is the advanced version of the previous volume type EmptyDir.
*   In EmptyDir, the data is stored in the volumes that reside inside the Pods only where the host machine doesn’t have the data of the pods and containers.
*   hostpath volume type helps to access the data of the pods or container volumes from the host machine.
*   hostpath replicates the data of the volumes on the host machine and if you make the changes from the host machine then the changes will be reflected to the pods volumes(if attached).

### HandsOn

In this snippet, once the pod has been created the data directory is also created on the local machine(Minikube Cluster).

**hostPath YML file**

apiVersion: v1  
kind: Pod  
metadata:  
  name: hostpath  
spec:  
  containers:  
  \- name: container1  
    image: ubuntu  
    command: \["/bin/bash", "-c", "while true; do echo This is Day13 of 30DaysOfKubernetes; sleep 5 ; done"\]  
    volumeMounts:  
    \- mountPath: "/tmp/cont"  
      name: hp-vm  
  volumes:  
  \- name: hp-vm  
    hostPath:   
      path: /tmp/data

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659819153/d3173397-6816-4966-b2cc-9900102e0579.png)

In this snippet, I am creating a txt file inside the pod's mapped directory /tmp/cont which is mapped to the local directory /tmp/data and after that, I am looking for the same file and content on the local machine directory /tmp/data.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659821358/fffd0050-cdbb-4b71-9f7c-03f93d0faa3f.png)

### **3\. Persistent Volume**

*   Persistent Volume is an advanced version of EmptyDir and hostPath volume types.
*   Persistent Volume does not store the data over the local server. It stores the data on the cloud or some other place where the data is highly available.
*   In previous volume types, if pods get deleted then the data will be deleted as well. But with the help of Persistent Volume, the data can be shared with other pods or other worker node’s pods as well after the deletion of pods.
*   One Persistent Volume is distributed across the entire Kubernetes Cluster. So that, any node or any node’s pod can access the data from the volume accordingly.
*   With the help of Persistent Volume, the data will be stored on a central location such as EBS, Azure Disks, etc.
*   Persistent Volumes are the available storage(remember for the next volume type).
*   If you want to use Persistent Volume, then you have to claim that volume with the help of the manifest YAML file.

### **4\. Persistent Volume Claim(PVC)**

*   To get the Persistent Volume, you have to claim the volume with the help of PVC.
*   When you create a PVC, Kubernetes finds the suitable PV to bind them together.
*   After a successful bound to the pod, you can mount it as a volume.
*   Once a user finishes its work, then the attached volume gets released and will be used for recycling such as new pod creation for future usage.
*   If the pod is terminating due to some issue, the PV will be released but as you know the new pod will be created quickly then the same PV will be attached to the newly created Pod.

Now, As you know the Persistent Volume will be on Cloud. So, there are some facts and terms and conditions are there for EBS because we are using AWS cloud for our K8 learning. So, let’s discuss it as well:

*   EBS Volumes keeps the data forever where the emptydir volume did not. If the pods get deleted then, the data will still exist in the EBS volume.
*   The nodes on which running pods must be on AWS Cloud only(EC2 Instances).
*   Both(EBS Volume & EC2 Instances) must be in the same region and availability zone.
*   EBS only supports a single EC2 instance mounting a volume

### HandsOn

To perform this demo, Create an EBS volume by clicking on ‘Create volume’.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659822903/783cb4b4-76af-4769-bbb0-e9d327b361bc.png)

Pass the Size for the EBS according to you, and select the Availability zone where your EC2 instance is created, and click on Create volume.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659824599/c706c7fc-23a8-4cb0-bd39-47e70ce2a758.png)

Now, copy the volume ID and paste it into the PV YML file(12th line)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659825900/d884d1ca-7881-4d32-b37a-1fd51188176d.png)

**PV YML file**

In this snippet, we have created a Persistent Volume where the EBS volume is attached and created 1GB of capacity.

apiVersion: v1  
kind: PersistentVolume  
metadata:  
  name: myebsvol  
spec:  
  capacity:  
    storage: 1Gi  
  accessModes:  
    \- ReadWriteOnce  
  persistentVolumeReclaimPolicy: Recycle  
  awsElasticBlockStore:  
    volumeID: #Your\_VolumeID  
    fsType: ext4

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659827395/edebddfb-63f1-494c-a2ad-2941d2520761.png)

**PVC YML file**

apiVersion: v1  
kind: PersistentVolumeClaim  
metadata:  
  name: myebsvolclaim  
spec:  
  accessModes:  
    \- ReadWriteOnce  
  resources:  
    requests:  
      storage: 1Gi

In this snippet, we have created a Persistent Volume Claim in which PVC requests PV to get 1GB, and as you can see the volume is bound successful.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659828858/a210ee78-28ba-4ebc-859b-281605c4f4cd.png)

**Deployment YML file**

apiVersion: apps/v1  
kind: Deployment  
metadata:  
  name: pvdeploy  
spec:  
  replicas: 1  
  selector:       
    matchLabels:  
     app: mypv  
  template:  
    metadata:  
      labels:  
        app: mypv  
    spec:  
      containers:  
      \- name: shell  
        image: centos  
        command: \["bin/bash", "-c", "sleep 10000"\]  
        volumeMounts:  
        \- name: mypd  
          mountPath: "/tmp/persistent"  
      volumes:  
        \- name: mypd  
          persistentVolumeClaim:  
            claimName: myebsvolclaim

In this snippet, we have created a deployment for PV and PVC demonstration.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659830234/5f900476-85d7-4f81-aa17-2fb17e72d02c.png)

In this snippet, we have logged in to the created container and created one file with some text.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659831552/3ca57aa8-ae23-40d1-912e-8c7b21203c28.png)

In this snippet, we have deleted the pod, and then because of replicas the new pod was created quickly. Now, we have logged in to the newly created pod and checked for the file that we created in the previous step, and as you can see the file is present which is expected.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659833111/48989e97-8e30-46a8-9356-dda7700a526a.png)

The demonstration has been completed now feel free to delete the volume.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659834804/4622fa38-54cc-49c6-99f7-39a771f0bc2a.png)

### **LivenessProbe (HealthCheck)**

*   LivenessProbe is a rich feature in Kubernetes that is used to check the health of your application.
*   Kubernetes by default doesn’t check the health check of the applications.
*   If you want to use the livenessProve feature and want to check the health of your application, you have to mention it in the manifest file.
*   livenessProbe expects a 0 output which means the application is running perfectly. But if there is any other output except 0 then livenessProbe recreates the container and repeats the same process.
*   livenessProbe repeats the process after particular seconds or minutes(specified by you) to check the health of the application.
*   If there is a load balancer attached to multiple pods then, livenessProbe checks the health of the application and if the application’s health check status is not healthy then livenessProbe removes the particular pod from the load balancer and recreates the new pod and repeats the same process.

### HandsOn

In this snippet, we have created a pod, and if you observe there is a new thing added while describing the pod through the **kubectl describe pod** command in the bottom which comes under Containers liveness.

**livenessProbe YML file**

apiVersion: v1  
kind: Pod  
metadata:  
  labels:  
    test: liveness  
  name: mylivenessprobe  
spec:  
  containers:  
  \- name: liveness  
    image: ubuntu  
    args:  
    \- /bin/sh  
    \- \-c  
    \- touch /tmp/healthy; sleep 1000  
    livenessProbe:                                            
      exec:  
        command:                                           
        \- cat                  
        \- /tmp/healthy  
      initialDelaySeconds: 5            
      periodSeconds: 5                                   
      timeoutSeconds: 30 

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659836646/47b0520a-0f50-4d3a-b5c4-4e92db20e6cc.png)

If you see the YML file, there is one condition if the file healthy is not present in /tmp directory then livenessProbe will recreate the container. So, we have deleted that file.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659838302/d94bd96d-976b-4d21-b4e8-9986b191d277.png)

After deleting the file, if you run kubectl describe pod <pod-name> you will see in the last two lines that the container is failing because the condition is not meeting.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659840014/adb6c0a8-1b75-4cf3-8931-eae2646e06ed.png)

### Conclusion

As we conclude our exploration of Kubernetes volumes and liveness probes, you’ve gained insights into the key elements of managing data in your Kubernetes pods and containers. Remember, data is at the heart of every application, and with the right volume configurations, you can ensure its persistence and accessibility, even in dynamic container environments.

We’ve covered the fundamental volume types like EmptyDir, the powerful hostPath, and the game-changing Persistent Volumes and Persistent Volume Claims. These tools empower you to store and share data efficiently, making your applications more resilient.

Additionally, we’ve delved into liveness probes, an essential feature for maintaining the health of your applications. With liveness probes, you can automatically detect and recover from issues, ensuring your applications stay up and running without a hitch.

### Want to Know About Challenge?

If you’re eager to learn more and join our challenge through the GitHub Repository, stay tuned for the upcoming posts. Follow for more exciting insights into the world of Kubernetes!

**GitHub Repository**: [https://github.com/AmanPathak-DevOps/30DaysOfKubernetes](https://github.com/AmanPathak-DevOps/30DaysOfKubernetes)

#Kubernetes #LivenessProbe #Volume #ContainerOrchestration #DevOps #K8sLearning

See you on Day **14** as we unravel more Kubernetes mysteries!

Stay connected on **LinkedIn**: [LinkedIn Profile](https://www.linkedin.com/in/aman-devops/)

Stay up-to-date with **GitHub**: [GitHub Profile](https://github.com/AmanPathak-DevOps)

Feel free to reach out to me, if you have any other queries.

Happy Learning

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659841408/3f94e0c5-ab10-4ffe-841f-4408f6774690.gif)