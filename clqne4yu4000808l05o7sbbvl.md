---
title: "Day02- Kubernetes Architecture Part-1"
datePublished: Wed Oct 04 2023 18:05:10 GMT+0000 (Coordinated Universal Time)
cuid: clqne4yu4000808l05o7sbbvl
slug: day02-kubernetes-architecture-part-1-c09abee5b1f2
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703658252165/acffd9e2-19d1-4929-96bc-ceeac8256d2a.png

---

### **Introduction**

Welcome to Day 02 of the #30DaysOfKubernetes challenge! On this exciting journey, we are unraveling the mysteries of Kubernetes Architecture, the open-source container orchestration engine that is revolutionizing the way we deploy and manage applications. In our quest to master Kubernetes, we’ll venture deeper into its inner workings, focusing on the critical components that make it all happen. So, buckle up for another day of discovery as we delve into the architecture of Kubernetes! Let’s begin!

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658247777/bb73a9d3-f069-425d-aa53-57deace891e0.png)

Kubernetes Architecture By Kubernetes(Credit)

Kubernetes follows client-server architecture where the Master Node and Worker node exist which constitutes a ‘Kubernetes Cluster’. We can have multiple worker nodes and Master nodes according to the requirement.

### **Control Plane**

The control plane components, including the API server, etcd, scheduler, and controller manager, are typically found on the master node(s) of a Kubernetes cluster. These components are responsible for managing and controlling the cluster as a whole.

### **Master Node**

The master node is responsible for the entire Kubernetes cluster and manages all the activities inside the cluster in which master nodes communicate with the worker node to run the applications on the containers smoothly. Master Node has four primary components which help to manage all the things that we have discussed earlier:

1.  **API Server**: In Simple terms, after installing the kubectl on the master node developers run the commands to create pods. So, the command will go to the API Server, and then, the API Server forwards it to that component which will help to create the pods. **In other words,** the API Server is an entry point for any Kubernetes task where the API Server follows the hierarchical approach to implement the things.
2.  **Etcd**: Etcd is like a database that stores all the pieces of information of the Master node and Worker node(entire cluster) such as Pods IP, Nodes, networking configs, etc. Etcd stored data in key-value pair. The data comes from the API Server to store in etc.
3.  **Controller Manager**: The controller Manager collects the data/information from the API Server of the Kubernetes cluster like the desired state of the cluster and then decides what to do by sending the instructions to the API Server.
4.  **Scheduler**: Once the API Server gathers the information from the Controller Manager, the API Server notifies the **Scheduler** to perform the respective task such as increasing the number of pods, etc. After getting notified, the Scheduler takes action on the provided work.

Let’s understand all four components with a real-time example.

**Master Node — Mall Management:**

*   In a shopping mall, you have a management office that takes care of everything. In Kubernetes, this is the Master Node.
*   The Master Node manages and coordinates all activities in the cluster, just like mall management ensures the mall runs smoothly.

**kube-apiserver — Central Control Desk:**

*   Think of the kube-apiserver as the central control desk of the mall. It’s where all requests (like store openings or customer inquiries) are directed.
*   Just like mall management communicates with stores, kube-apiserver communicates with all Kubernetes components.

**etcd — Master Records:**

*   etcd can be compared to the master records of the mall, containing important information like store locations and hours.
*   It’s a key-value store that stores configuration and cluster state data.

**kube-controller-manager — Task Managers:**

*   Imagine having specialized task managers for different mall departments, like security and maintenance.
*   In Kubernetes, the kube-controller-manager handles various tasks, such as ensuring the desired number of Pods are running.

**kube-scheduler — Scheduler Manager:**

*   Think of the kube-scheduler as a manager who decides which employees (Pods) should work where (on which Worker Node).
*   It ensures even distribution and efficient resource allocation.

### **What’s Next?**

So far, we’ve explored the Master Node and its core components. In our next blog, we’ll delve into the Worker Node and its components. In the meantime, feel free to explore Kubernetes further.

If you’re eager to learn more and join our challenge through the GitHub Repository, stay tuned for the upcoming posts. Follow for more exciting insights into the world of Kubernetes!

**GitHub Repository**: [https://github.com/AmanPathak-DevOps/30DaysOfKubernetes](https://github.com/AmanPathak-DevOps/30DaysOfKubernetes)

#30DaysOfKubernetes #Kubernetes #MasterNode #ControlPlane #K8s

See you on Day **03** as we unravel more Kubernetes mysteries!

Stay connected on **LinkedIn**: [LinkedIn Profile](https://www.linkedin.com/in/aman-devops/)

Stay up-to-date with **GitHub**: [GitHub Profile](https://github.com/AmanPathak-DevOps)

Feel free to reach out to me, if you have any other queries.

Happy Learning

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658249389/5e9fd83a-1a5a-437f-86c3-64c095ed1839.gif)