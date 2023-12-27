---
title: "Day08- ReplicationController & ReplicaSet"
datePublished: Tue Oct 10 2023 15:54:31 GMT+0000 (Coordinated Universal Time)
cuid: clqnel6kh000808jremiudjux
slug: day08-replicationcontroller-replicaset-a0c6f9d98196
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703659008964/0efc5bdc-ce90-4717-bcd4-4016b8472b85.png

---

Welcome to Day 8 of our #30DaysOfKubernetes journey! Today, we dive into the world of ReplicationController and its more advanced counterpart, Replica Set. These powerful tools are essential for ensuring scalability and high availability in Kubernetes.

Before Kubernetes, other tools did not provide important and customized features like scaling and replication.

When Kubernetes was introduced, replication and scaling were the premium features that increased the popularity of this container orchestration tool.

Replication means that if the pod's desired state is set to 3 and whenever any pod fails, then with the help of replication, the new pod will be created as soon as possible. This will lead to a reduction in the downtime of the application.

Scaling means if the load becomes increases on the application, then Kubernetes increases the number of pods according to the load on the application.

ReplicationController is an object in Kubernetes that was introduced in v1 of Kubernetes which helps to meet the desired state of the Kubernetes cluster from the current state. ReplicationController works on equality-based controllers only.

ReplicaSet is an object in Kubernetes and it is an advanced version of ReplicationController. ReplicaSet works on both equality-based controllers and set-based controllers.

Let’s do some hands-on to get a better understanding of ReplicationController & ReplicaSet.

### YML file

apiVersion: v1  
kind: ReplicationController  
metadata:  
  name: myreplica  
spec:  
  replicas: 2  
  selector:  
    Location: India  
  template:  
    metadata:  
      name: testpod6  
      labels:  
        Location: India  
    spec:  
     containers:  
       \- name: c00  
         image: ubuntu  
         command: \["/bin/bash", "-c", "while true; do echo ReplicationController Example; sleep 5 ; done"\]

Create the replication controller by running the command

> kubectl apply -f myrc.yml

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658986827/ac02fac7-974c-4da7-b2cd-d98c96126ff1.png)

Now, you can see the replication controller that we created earlier and observe the desired state, current state, ready, and age.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658988669/b9f35223-9baf-4062-97a8-356745b76488.png)

If you list all the pods, you will see that my replica created two pods that are running.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658989883/c047ee92-3a39-4926-9ebf-eba00b8bbdb3.png)

If you try to delete the pods, you will see that the new pod will be created quickly. You can observe through the AGE of both pods.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658991602/aaf8fe3a-76a4-4129-ade8-72225a1d1add.png)

If you want to modify the replicas, you can do that by running the command

> kubectl scale — replicas=5 rc -l Location=India

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658993264/ca6660d4-36bc-42b3-829e-0b7b9c6d1b27.png)

If you try to delete pods, you will see again that the new pod is creating quickly.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658995072/a95d3a90-d627-4c70-a487-23480a186ec8.png)

Now, if you want to delete all the pods. You can do it by just deleting the replicationController by the given command.

kubectl delete -f myrc.yml

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658996325/fb3ac1d9-8ded-48d0-85ba-d0d42dd1d6cc.png)

### ReplicaSet HandsOn

### YML file

apiVersion: apps/v1                              
kind: ReplicaSet      
metadata:  
  name: myrs  
spec:  
  replicas: 2    
  selector:                    
    matchExpressions:                               
      \- {key: Location, operator: In, values: \[India, US, Russia\]}  
      \- {key: env, operator: NotIn, values: \[testing\]}  
  template:        
    metadata:  
      name: testpod7  
      labels:                
        Location: Russia  
    spec:  
     containers:  
       \- name: container1  
         image: ubuntu  
         command: \["/bin/bash", "-c", "while true; do echo ReplicaSet Example; sleep 5 ; done"\]

Create the replicaSet by running the command

> kubectl apply -f myrs.yml

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658998381/51b29e0f-9e6d-406b-bbc2-66b044f8c3dc.png)

Now, you can see the pods that have been created through replicaSet with labels

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658999963/f24e5fa2-a7a3-4699-84d8-b481aaa0298e.png)

If you try to delete the pods, you will see that the new pod is created with the same configuration.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659001168/a7adffa0-233c-4dcd-a284-85c9e0744f71.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659002508/4d325f97-9ebc-4530-981a-f3f7e644f159.png)

If you want to increase or decrease the number of replicas, you can do it by the given command.

> kubectl scale — replicas=5 rs myrs

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659003888/22e17e7b-60a3-41a8-b270-8f44194f9ad1.png)

If you want to delete all the pods, you can do it by deleting the replicaSet with the help of the given command.

> kubectl delete -f myrs.yml

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659005345/39bc5933-123a-4cd0-a277-9a8e7e7be89b.png)

### Conclusion

ReplicationController and ReplicaSet are essential tools in Kubernetes for achieving high availability and scalability. They provide the foundation for ensuring your applications run smoothly, even in the face of failures and increased demand.

In our hands-on exercises, you’ve witnessed their power to maintain the desired state of your pods and adapt to changing requirements seamlessly.

### Want to Know About Challenge?

If you’re eager to learn more and join our challenge through the GitHub Repository, stay tuned for the upcoming posts. Follow for more exciting insights into the world of Kubernetes!

**GitHub Repository**: [https://github.com/AmanPathak-DevOps/30DaysOfKubernetes](https://github.com/AmanPathak-DevOps/30DaysOfKubernetes)

#Kubernetes #ReplicationController #ReplicaSet #ContainerOrchestration #DevOps #K8sLearning

See you on Day **09** as we unravel more Kubernetes mysteries!

Stay connected on **LinkedIn**: [LinkedIn Profile](https://www.linkedin.com/in/aman-devops/)

Stay up-to-date with **GitHub**: [GitHub Profile](https://github.com/AmanPathak-DevOps)

Feel free to reach out to me, if you have any other queries.

Happy Learning

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659006611/82c98a16-ad0d-40eb-9594-4bc76d6b305f.jpeg)