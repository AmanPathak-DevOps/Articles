---
title: "Day09- Deployment Object in Kubernetes"
datePublished: Wed Oct 11 2023 15:47:56 GMT+0000 (Coordinated Universal Time)
cuid: clqnekcv5000608i87n6j3gz0
slug: day09-deployment-object-in-kubernetes-30b0022bc4ae
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703658970065/1daef7d1-6c5d-489b-9571-83b5e81df086.png

---

Welcome to Day 9 of our #30DaysOfKubernetes journey! Today, we’ll delve into the fascinating world of Kubernetes Deployment Objects. These versatile tools are the key to managing application updates, and rollbacks, and keeping your Kubernetes applications running smoothly. So, let’s get started on our journey to master the Deployment Object in Kubernetes! 🚀🐱‍🏍 #Kubernetes #K8sLearning #DeploymentObject

*   Replication Controller and Replica Set don’t provide the updates and rollback for the application in the cabinets cluster. But in a deployment, the object does.
*   The deployment object works as a supervisor for the pods which gives granular control over the pods. Deployment objects can decide how and when the pods should be deployed, rollback or updated.
*   In Deployment, we define the desired state and the deployment controller will help to achieve the desired state from the current state.
*   You can achieve this by declarative(manifest) method only.
*   A Deployment provides declarative updates for Pods and ReplicaSets.
*   The deployment object supports updates which means if there is any update in the applications that needs to be deployed in the new version then, the deployment object helps to achieve it.
*   Deployment object supports rollback which means if the app is crashing in a new update then you can easily switch to the previous version by rollback.
*   The deployment object doesn’t work directly with the pods. Under the deployment object, there will be a replica set or replica controller that manages the pods and helps to manage the desired state.

**Use Cases for the Deployment Object**

*   With the help of deployment, the replica set will be rolled out, which will deploy the pods and check the status in the background whether the rollout has succeeded or not.
*   If the pod template spec is modified then, the new replica set will be created with the new desired state and the old replica set will still exist and you can roll back according to the situation.
*   You can roll back to the previous deployment if the current state of the deployment is not stable.
*   You can scale up the deployment to manage the loads.
*   You can pause the deployment if you are fixing something in between the deployment and then resume it after fixing it.
*   You can clean up those replica sets which are older and no longer needed.

### HandsOn

YML file

apiVersion: apps/v1  
kind: Deployment  
metadata:  
   name: thedeployment  
spec:  
   replicas: 3  
   selector:       
    matchLabels:  
     name: deploy-pods  
   template:  
     metadata:  
       name: ubuntu-pods  
       labels:  
         name: deploy-pods  
     spec:  
      containers:  
        \- name: container1  
          image: ubuntu  
          command: \["/bin/bash", "-c", "while true; do echo This is Day09 of 30DaysOfKubernetes; sleep 5; done"\]

Creating the deployment by running the command ‘*kubectl apply -f da-deployment.yml’.*

If you observe that the replica set has 3 desired states and same as the current state.

Also, all three pods are in ready and running status.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658948362/e0b4b655-b6d2-4d02-8f9a-6d4850904097.png)

Now, if you try to delete any pod, because of a replica set, the new pod will be created quickly.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658949950/e6d7118f-f74b-4c06-8383-89cc459b38ca.png)

Here, we have increased the number of replicas set from 3 to 5 by the command

> ‘*kubectl scale — replicas=5 deployment <deployment\_name>’.*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658951256/ab4bf383-cce3-47d4-a564-e54132cac646.png)

Here, we are checking the logs for the particular pod.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658952759/7bbb0122-a134-4f2a-8522-03af91869865.png)

Here, I made some changes in the previous YML file as you can see in the file. I have updated the image and command for the needed YML file.

apiVersion: apps/v1  
kind: Deployment  
metadata:  
   name: thedeployment  
spec:  
   replicas: 3  
   selector:  
    matchLabels:  
     name: deploy-pods  
   template:  
     metadata:  
       name: ubuntu-pods  
       labels:  
         name: deploy-pods  
     spec:  
      containers:  
        \- name: container1  
          image: centos  
          command: \["/bin/bash", "-c", "while true; do echo DevOps is a Culture; sleep 5; done"\]

After updating the file, I have applied the updated file and as you can see, ‘**thedeployment’** has 3 replicas which were previously 5. This happened because, in the YML file the replicas are 3.

Also, if you observe that the previous rs is still present with 0 desired and current state.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658954368/afe01a59-726f-40f7-848b-706f847ed963.png)

Now, if you will check the logs of the new pods. You will see the updated command running that we have written in the YML file.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658956027/7e7dcd6d-43bf-4392-9019-9de954c85888.png)

Also, We have updated the image for the OS and you can see that we are getting the expected result for the image.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658957412/2e039be8-f63f-44fe-baf5-e255b238de6f.png)

Here, we have increased the number of replicas set from 3 to 5 by the command ‘*kubectl scale — replicas=5 deployment <deployment\_name>’.*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658959343/e7607154-41d3-4d21-b3ec-1b92ada7edf8.png)

Now, if you want to switch to the previous deployment you can do it by the given command.

kubectl rollout undo deployment *<deployment\_name>*

If you compare the first **kubectl get rs** command with the second **kubectl get rs,** then the desired state shifts to the other deployment.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658960979/7e190f4d-3f01-4f53-b70a-ef94f216d990.png)

Now, if you see the logs of the running pods. You will see that the previous command is running because we have switched to the previous deployment.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658962623/94575ee5-6ffe-4a9e-819b-744e9b49832c.png)

If you see the OS Image, you will understand that in our previous deployment, we used an Ubuntu image which is expected.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658964454/cfac54c8-5204-4598-8c27-cdb180144a3e.png)

If you want to delete all those running pods and replica sets, then use the given command.

kubectl delete -f <deployment\_file>

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658966030/f140ad5c-d4cc-440c-af0e-8528f344ca3b.png)

### Conclusion

The Deployment object in Kubernetes is your trusted companion for managing application updates, rollbacks, and more. Its declarative approach, support for rollbacks, and seamless updates make it an indispensable tool for Kubernetes enthusiasts.

So, the next time you need to roll out a new version of your application or swiftly revert to a stable state, think of Deployment objects. They’re here to make your Kubernetes journey smoother and more efficient.

Happy Deploying!

### Want to Know About Challenge?

If you’re eager to learn more and join our challenge through the GitHub Repository, stay tuned for the upcoming posts. Follow for more exciting insights into the world of Kubernetes!

**GitHub Repository**: [https://github.com/AmanPathak-DevOps/30DaysOfKubernetes](https://github.com/AmanPathak-DevOps/30DaysOfKubernetes)

#Kubernetes #ReplicationController #ReplicaSet #ContainerOrchestration #DevOps #K8sLearning

See you on Day **10** as we unravel more Kubernetes mysteries!

Stay connected on **LinkedIn**: [LinkedIn Profile](https://www.linkedin.com/in/aman-devops/)

Stay up-to-date with **GitHub**: [GitHub Profile](https://github.com/AmanPathak-DevOps)

Feel free to reach out to me, if you have any other queries.

Happy Learning

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658967873/e4c62126-e073-4850-b8de-f45a2110a6be.jpeg)