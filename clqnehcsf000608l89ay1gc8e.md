---
title: "Day 23- Kubernetes DaemonSet"
datePublished: Mon Nov 06 2023 13:39:25 GMT+0000 (Coordinated Universal Time)
cuid: clqnehcsf000608l89ay1gc8e
slug: day-23-kubernetes-daemonset-cbca1ce3d4c1
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703658830425/ede8b640-6386-4850-8959-29f2bec48c28.png

---

### **Introduction**

Welcome to Day 23 of our #30DaysOfKubernetes journey! Today, we’re diving into the powerful world of DaemonSets in Kubernetes. DaemonSets play a crucial role in ensuring that specific pods run on every node within a cluster. They are perfect for deploying monitoring, logging, security agents, and more. In this blog post, we’ll explore their key features, and use cases, and even provide a hands-on demonstration of how DaemonSets can automatically adapt to changes in your Kubernetes cluster.

*“A DaemonSet ensures that all (or some) Nodes run a copy of a Pod. As nodes are added to the cluster, Pods are added to them. As nodes are removed from the cluster, those Pods are garbage collected. Deleting a DaemonSet will clean up the Pods it created.” —* ***Kubernetes DaemonSet Official Definition***

Suppose you have to deploy the same pod on all nodes for monitoring. DaemonSet ensures that a specific pod runs on all nodes within the cluster.

### **KeyFeatures**

1.  **DaemonSets Ensure Uniformly:** DaemonSets ensures that a designated pod which is used for logs and monitoring will be deployed on every node within the cluster.
2.  **Perfect for Infrastructure Services**: DaemonSets are great for services that need to run on every node like networking, storage, or security agents.
3.  **Scaling Automatically**: If you add more nodes, then the DaemonSets Pods will be added to new nodes automatically.
4.  **Stable Hostnames**: DaemonSets provides stable hostnames which means on each node, the pod's names remain the same if the pods is rescheduled or restarted which makes it easy to reference them across the cluster.

### **UseCases**

1.  **Monitoring and Logging**: With the help of DaemonSets, we can deploy monitoring agents or log collectors to gather the information on the node. Tools like Prometheus, beats, ElasticSearch, etc can be deployed using DaemonSets to ensure complete coverage across the cluster.
2.  **Security Agents**: We can deploy Security Agents like intrusion detection systems (IDS) or anti-malware software on every node to protect the cluster from threats.
3.  **Custom Network Policies**: We can deploy the custom network policies or firewall rules on each node to control communication and security at the node level.
4.  **Operating System Updates**: We can deploy the updates or patches at one time with the help of DaemonSets.
5.  **Storage and Data Management**: Ensuring that each node has access to particular storage resources, such as local storage or network-attached storage(NAS). DaemonSets can manage storage plugins or agents to provide consistent access.

### HandsOn

Initially, I created three machines of which one is a Master Node and the rest of two are Worker Nodes.

If you don’t know how to set up the multi-worker nodes using kubeadm then kindly refer to my blog [https://blog.devops.dev/setting-up-a-kubernetes-cluster-master-worker-node-using-kubeadm-on-aws-ec2-instances-ubuntu-22-04-3432859b943b](https://blog.devops.dev/setting-up-a-kubernetes-cluster-master-worker-node-using-kubeadm-on-aws-ec2-instances-ubuntu-22-04-3432859b943b) which will take maximum 15 minutes

**Let’s see our Object first:**

*   Currently, I have three machines Only(1 Master + 2 Worker Nodes)
*   We will deploy the DaemonSet Pod on two Worker Nodes.
*   Once the Pods will be running on both Worker Nodes. We will create a new machine as Worker Node 3.
*   Worker Node3 will join the Kubernetes cluster.
*   The DaemonSet Pod will be running automatically without any intervention from our side.

This is the Entire Proof of our Hands-On. You just need to take a look. Further Step by Step, we will look into the next steps.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658817638/54e4b638-ed3f-4949-9df1-c97aae2e8ec2.png)

As of now, we have only One Master Node(control plane) and Two Worker Nodes.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658819471/35681119-b7fc-4fad-a318-cc9d7b7dee08.png)

**DaemonSet YAML file**

apiVersion: apps/v1  
kind: DaemonSet  
metadata:  
  name: nginx-daemonset  
spec:  
  selector:  
    matchLabels:  
      name: nginx  
  template:  
    metadata:  
      labels:  
        name: nginx  
    spec:  
      containers:  
      \- name: nginx  
        image: nginx:latest

I have deployed my daemonSet YAML file which is deployed to both Worker Nodes.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658820934/e5f74493-3420-4f1d-a85f-d8a91720b975.png)

Here, I have run the command to join the Kubernetes Cluster for Worker Node3.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658822791/47805d4f-e727-4eba-9f48-d588ea7145a1.png)

After joining, you can see the node is getting ready to be part of the cluster.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658824106/ed0ed4bf-c50f-40a6-9ae7-16002aff2f66.png)

Here, you can see that Worker Node3 is in ready status.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658825630/41eff080-7fac-44ae-8b81-954d703b69a4.png)

If you list all the pods, you will see there is one more started in which the container is creating without running any command.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658826846/7a83e42c-f126-4a5e-bbb1-2a7cd88def41.png)

Here, you can see the Pod is in running status.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658828051/26c1e126-efab-47c7-9176-15fcd097929f.png)

### Conclusion

In conclusion, DaemonSets are a vital tool in the Kubernetes arsenal. They ensure uniform deployment of designated pods across every node in your cluster, making them ideal for services like monitoring, logging, security agents, and more. With DaemonSets, scaling is automatic, hostnames remain stable, and use cases expand to include monitoring, security, custom network policies, OS updates, and data management. As you’ve seen in our hands-on demonstration, DaemonSets simplify the deployment and management of pods across your cluster, adapting seamlessly as nodes are added or removed. Stay tuned for more exciting Kubernetes concepts as we continue our #30DaysOfKubernetes journey!

### Want to Know About Challenge?

If you’re eager to learn more and join our challenge through the GitHub Repository, stay tuned for the upcoming posts. Follow for more exciting insights into the world of Kubernetes!

**GitHub Repository**: [https://github.com/AmanPathak-DevOps/30DaysOfKubernetes](https://github.com/AmanPathak-DevOps/30DaysOfKubernetes)

#Kubernetes #DaemonSet #StatefulSets #ContainerOrchestration #DevOps #K8sLearning

See you on Day **24** as we unravel more Kubernetes mysteries!

Stay connected on **LinkedIn**: [LinkedIn Profile](https://www.linkedin.com/in/aman-devops/)

Stay up-to-date with **GitHub**: [GitHub Profile](https://github.com/AmanPathak-DevOps)

Feel free to reach out to me, if you have any other queries.

Happy Learning