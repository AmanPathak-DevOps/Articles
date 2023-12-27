---
title: "Day11- Kubernetes Networking(Services)"
datePublished: Fri Oct 13 2023 15:56:55 GMT+0000 (Coordinated Universal Time)
cuid: clqnejr06000j08joc8mx3cxj
slug: day11-kubernetes-networking-services-6fb913b059d0
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703658941794/f55adb20-0f44-472a-8057-13388e131d14.png

---

Welcome to Day 11 of our #30DaysOfKubernetes journey! Today, we dive into the intricate world of Kubernetes networking, an essential aspect of managing containers in a cluster. Kubernetes networking ensures that your containers can communicate within the cluster and with the outside world. Let’s unravel the complexities of networking in Kubernetes.

**Objectives**  
By the end of this day, you will:

Understand the basics of how pods and containers can communicate within the same pod and node.  
Explore the critical role of Service objects in Kubernetes networking.  
Gain insights into different Service types, including ClusterIP, NodePort, LoadBalancer, and ExternalName.

Things to know about accessing pods or containers in some scenarios:

*   Multiple containers within the pod access each other through a loopback address(localhost).
*   The cluster provides communication between multiple pods.
*   To access your application from outside of the cluster, you need a Services object in Kubernetes.
*   You can also use the Services object to publish services only for access within the cluster.

**Access containers within the same pod**

apiVersion: v1  
kind: Pod  
metadata:  
  name: day10  
  labels:  
    env: testing  
    department: DevOps  
spec:  
  containers:  
    \- name: container1  
      image: nginx  
    \- name: container2  
      image: ubuntu  
      command: \["/bin/bash", "-c", "while true; do echo This is Day07 of 30DaysOfKubernetes; sleep 5 ; done"\]  
      ports:  
        \- containerPort: 80

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658929036/4dbebc23-b279-4227-8f19-b443c4eedb33.png)

> kubectl exec day07 -it -c container2 — /bin/bash

Update the packages and install the curl

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658930567/f668d48d-f5cf-4a3b-b1ef-efe66b01e303.png)

Run the below command.

> curl localhost:80

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658932018/53717ed4-41c9-49cc-ae55-29b1aa30c517.png)

### **Access containers within the same Node.**

apiVersion: v1  
kind: Pod  
metadata:  
  name: day10-pod1  
  labels:  
    env: testing  
    department: DevOps  
spec:  
  containers:  
    \- name: container1  
      image: nginx  
      ports:  
        \- containerPort: 80

apiVersion: v1  
kind: Pod  
metadata:  
  name: day10-pod2  
  labels:  
    env: testing  
    department: DevOps  
spec:  
  containers:  
    \- name: container2  
      image: httpd  
      ports:  
        \- containerPort: 80

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658933478/d1b6b606-d3ca-4370-845d-904726eb96c2.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658935132/0ac9434f-cfe9-451d-9edc-96fcae27f955.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658936691/c488db61-aa6b-41fc-bc04-518c53f81e9d.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658938240/30af284f-c332-4aa0-b244-39068ce8043c.png)

### **Service Object in Kubernetes**

To configure Networking for deployed applications on pods and containers, we use Service objects.

There are four main types of Kubernetes Services:

1.  **ClusterIP**: As the name suggests, With the help of this Service. Kubernetes expose the service within the cluster which means the service or application won’t be accessible outside of the cluster. You can view the Service YML file and see how to use this service.

apiVersion: v1  
kind: Service                               
metadata:  
  name: httpd-service  
spec:  
  ports:  
    \- port: 80                                 
      targetPort: 80                       
  selector:  
    name: DevOps                      
  type: ClusterIP 

**2\. NodePort**: This is the next stage of the ClusterIP where you want to deploy your application or service that should be accessible to the world without any interruption. In this Service, the node port exposes the service or application through the static port on each node’s IP. You can view the Service YML file and see how to use this service.

apiVersion: v1  
kind: Service                               
metadata:  
  name: httpd-service  
spec:  
  ports:  
    \- port: 80                                 
      targetPort: 80                       
  selector:  
    name: DevOps                      
  type: NodePort 

**3\. LoadBalancer**: Load balancers are used to distribute the traffic between the multiple pods. With the help of this service object, the services will be exposed via the cloud’s load balancer. You can view the Service YML file and see how to use this service.

apiVersion: v1  
kind: Service  
metadata:  
  name: svc-lb  
spec:  
  type: LoadBalancer  
  selector:  
    tag: DevOps  
  ports:  
  \- name: port-lb  
    protocol: TCP  
    port: 80  
    targetPort: 80

**4\. ExternalName**: This is a similar object service to ClusterIP but it does have DNS CName instead of Selectors and labels. In other words, services will be mapped to a DNS name. You can view the Service YML file and see how to use this service.

apiVersion: v1  
kind: Service  
metadata:  
  name: k8-service  
  namespace: dev  
spec:  
  type: ExternalName  
  externalName: k8.learning.com

### Conclusion

Understanding Kubernetes networking is crucial for managing containers effectively. With knowledge of how to access containers within the same pod and the versatility of Service objects, you’re well on your way to becoming a Kubernetes networking expert.

Stay tuned for more exciting Kubernetes insights in our #30DaysOfKubernetes journey!

### Want to Know About Challenge?

If you’re eager to learn more and join our challenge through the GitHub Repository, stay tuned for the upcoming posts. Follow for more exciting insights into the world of Kubernetes!

**GitHub Repository**: [https://github.com/AmanPathak-DevOps/30DaysOfKubernetes](https://github.com/AmanPathak-DevOps/30DaysOfKubernetes)

#Kubernetes #ReplicationController #ReplicaSet #ContainerOrchestration #DevOps #K8sLearning

See you on Day **11** as we unravel more Kubernetes mysteries!

Stay connected on **LinkedIn**: [LinkedIn Profile](https://www.linkedin.com/in/aman-devops/)

Stay up-to-date with **GitHub**: [GitHub Profile](https://github.com/AmanPathak-DevOps)

Feel free to reach out to me, if you have any other queries.

Happy Learning

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658939439/8e5d1367-a4dd-4569-8a37-4d5c94473b53.jpeg)