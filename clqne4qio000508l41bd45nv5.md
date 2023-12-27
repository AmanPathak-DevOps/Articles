---
title: "Day12- Kubernetes Advanced Networking: CNI and Calico"
datePublished: Sat Oct 14 2023 18:55:36 GMT+0000 (Coordinated Universal Time)
cuid: clqne4qio000508l41bd45nv5
slug: day12-kubernetes-advanced-networking-cni-and-calico-ee96734c17bb
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703658241572/e35918db-6bf8-4cda-b596-8efe886a6bb4.png

---

Welcome to Day 12 of our #30DaysOfKubernetes journey!  
Kubernetes is a powerful container orchestration platform, and one of the key factors contributing to its success is its networking capabilities. Container Network Interfaces (CNI) play a pivotal role in shaping how containers and pods connect to the network within a Kubernetes cluster. In this article, we’ll delve into the world of Kubernetes networking and explore the capabilities of Calico CNI.

### What is CNI?

CNI stands for Container Network Interface. As the name suggests, CNI works on the networking level where CNI takes care of the pods and other things in Kubernetes. CNI ensures how the containers and pods should connect to the network. There are many CNIs available in the market but today we will discuss Calico CNI.

### What is Calico CNI?

Calico is an open-source network and network security solution designed for Kubernetes.

Calico is one of the most popular CNIs which used to manage the networking between containers, pods, nodes, or multiple clusters. Calico works only on networking to provide fine-grained control over the containers, pods, nodes, or multiple clusters where Services and

### **Alternative of Calico CNI**

*   Flannel: Flannel is a straightforward CNI plugin that uses Layer 3 networking. It is good for small to medium size clusters but the networking policies feature is not good where Calico provides good control over networking policies.
*   Weave: Weave is quite a good CNI plugin as compared to Flannel as it provides secure, scalable networking and networking policies as well. But it does not have rich features as Calico does.
*   Cilium: Cilium is one of the best CNI plugins which competes with Calico CNI where Cilium provides the best security and observability. Cilium is a good choice for large complex clusters where security is a top concern.
*   kube-router: kube-router is a lightweight CNI plugin where that provides good features like serving load balancing and network policies. It is good for small to medium size clusters.

### **Why you should use Calico CNI over other CNIs(Features)**

*   **Advanced Networking Policies:** With Calico CNI, we can define fine-grained networking policies over the containers or pods such as which pod can communicate to which other pod and apply rules based on labels, ports, and more. This level of control is not possible through Kubernetes Native Networking.
*   **Scalability:** Calico is known for its Scalability where it can handle large clusters with ease and efficiently manage network traffic which makes it suitable for enterprise-level applications with multiple pods.
*   **Cross-Cluster Networking:** Calico can be used to connect multiple Kubernetes clusters together, which can be beneficial in hybrid or multi-cluster scenarios.
*   **Border Gateway Protocol(BGP) routing:** Calico supports BGP for routing which is quite good if you want to integrate with on-premises data centers or public cloud environments.
*   **Security:** Calico supports a very good level of security over the network traffic where Calico encrypts the network traffic so only authorized pods can communicate with the respective pods.

### **Key Concepts and Real-time Example**

*   **IP Address Management:** Calico supports managing the IP address for each pod where each pod is assigned to a unique IP address from the cluster’s IP address range.
*   **Routing and Network Policy:** Calico enables routing for the network traffic between pods. The Network policies can be applied to control traffic between pods. So, you can allow or deny communications between specific pods.
*   **Load Balancing:** Calico handles load balancing in which it distributes the traffic between multiple pods.
*   **Security and Encryption:** Calico provides security features to protect your Kubernetes clusters. It encrypts the network traffic so that you can ensure only authorized pods can communicate.

> Think of Calico as a traffic control system in a city, where every vehicle(**pod**) gets a unique plate number and license and follows the traffic rules. The Traffic lights(**network policies**) ensure safe and fully controlled movement. Police officers(**security**) check for unauthorized actions and keep the movement controlled.

### HandsOn

If you want to do HandsOn where you want to install the Calico Network. So you can refer to the [Day10 of #30DaysOfKubernetes](https://medium.com/devops-dev/setting-up-a-kubernetes-cluster-master-worker-node-using-kubeadm-on-aws-ec2-instances-ubuntu-22-04-3432859b943b) where we have set up the Master and Worker Node on the AWS EC2 Instance in which I have installed the Calico CNI Plugin.

For now, this is a link to install the Calico and list all the networks, and validate the Calico network

kubectl get pods \-n kube\-system

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658234483/f627257c-434b-4b1f-ae74-3213bf8cab95.png)

kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.25.0/manifests/calico.yaml

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658235913/f72de3a1-5b0f-4915-bea1-2534db17b648.png)

kubectl get pods \-n kube\-system

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658237427/67e69dad-8312-43e4-8e2e-55eaf50b2ab1.png)

### Conclusion

🚀 Explored advanced Kubernetes networking with Calico CNI on Day 12! Fine-grained control, scalability, cross-cluster capabilities, and enhanced security make Calico shine. Tomorrow, we dive into more Kubernetes awesomeness! 🌐💡 #Kubernetes #CalicoCNI #Networking #30DaysChallenge #TechLearning

### Want to Know About Challenge?

If you’re eager to learn more and join our challenge through the GitHub Repository, stay tuned for the upcoming posts. Follow for more exciting insights into the world of Kubernetes!

**GitHub Repository**: [https://github.com/AmanPathak-DevOps/30DaysOfKubernetes](https://github.com/AmanPathak-DevOps/30DaysOfKubernetes)

#Kubernetes #ReplicationController #ReplicaSet #ContainerOrchestration #DevOps #K8sLearning

See you on Day **13** as we unravel more Kubernetes mysteries!

Stay connected on **LinkedIn**: [LinkedIn Profile](https://www.linkedin.com/in/aman-devops/)

Stay up-to-date with **GitHub**: [GitHub Profile](https://github.com/AmanPathak-DevOps)

Feel free to reach out to me, if you have any other queries.

Happy Learning

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658238914/e85284e4-c258-44cc-b399-7d1fa216962b.jpeg)