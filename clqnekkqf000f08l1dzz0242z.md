---
title: "Day05: Kubeconfig, Services, and Deployments Files Explained"
datePublished: Sat Oct 07 2023 17:57:36 GMT+0000 (Coordinated Universal Time)
cuid: clqnekkqf000f08l1dzz0242z
slug: day05-kubeconfig-services-and-deployments-files-explained-8733c0cd8b61
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703658979887/d3ba73aa-64f9-4640-9979-1bf7951833a5.png

---

Welcome to Day 05 of the #30DaysOfKubernetes challenge! In Today’s blog post, we’re going to dive into three fundamental aspects of Kubernetes: Kubeconfig Files, Service Files, and Deployment Files. By the end of this journey, you’ll have a solid understanding of these essential components.

### **Kubeconfig Files**

*   Purpose: Kubeconfig files are used for cluster access and authentication. Kubeconfig defines how ‘kubectl’ or any other Kubernetes clients interact with the Kubernetes cluster.
*   Contents: The Kubeconfig file contains information about the cluster, user credentials, certificates, and context.
*   Usage: Kubeconfig files are used by Administrators, developers, or CI/CD systems to authenticate the Kubernetes cluster. They decide who can access and how to access the cluster.

***Kubeconfig files can be stored in the user’s home directory (~/.kube/config) or specified using the KUBECONFIG environment variable.***

### **Service File**

*   **Purpose**: Service files contain all information about networking. The service file defines how networking will be handled on the cluster. Also, the Service file enabled the load balancing option for the applications which is a premium feature of Kubernetes.
*   **Contents**: The service file specifies the service’s name, type(ClusterIP, NodePort, LoadBalancer, etc\[Discuss in Upcoming Blogs\]), and selectors to route traffic to pods.
*   **Usage**: Service files are used by developers and administrators to expose and connect applications within the Kubernetes cluster.

Note: Services can also be used for internal communication between Pods within the cluster, not just for exposing applications externally.

### **Deployment files**

*   **Purpose**: Deployment files contain all information about the application and define how the application or microservices will be deployed on the Kubernetes cluster. In deployment files, we can define the desired state, pod replicas, update strategies, and pod templates.
*   **Contents**: Deployment files define the desired state of a deployment, pod replicas, container images, and resource limits.
*   **Usage**: Deployment files are mainly used by developers and administrators to manage the application lifecycle within Kubernetes. They enable declarative application management, scaling, and rolling updates.

### Practical Examples

To make things even clearer, let’s dive into some practical examples:

### **Kubeconfig file explained**

apiVersion: v1  
kind: Config  
clusters:  
\- name: my-cluster  
  cluster:  
    server: https://api.example.com  
    certificate-authority-data: <ca-data>  
users:  
\- name: my-user  
  user:  
    client-certificate-data: <client-cert-data>  
    client-key-data: <client-key-data>  
contexts:  
\- name: my-context  
  context:  
    cluster: my-cluster  
    user: my-user  
    namespace: my-namespace  
current-context: my-context

In this example,

*   **apiVersion** and **kind** define the resource type.
*   **clusters** specifies the clusters with its server and URL and Certificate Authority(CA) data. Here we have to define the server link or Kubernetes API Server of the Kubernetes cluster. So, when we run any command using kubectl then kubectl interacts with the given link or Kubernetes API Server of the Kubernetes cluster.
*   **users** specify the users with their client certificate and client key name. So, only authorized users can access the Kubernetes cluster.
*   **contexts** specify the cluster, user, and namespace information that has been defined above. You can create multiple contexts and switch between any different clusters at any time.
*   **current-context** specifies that on which cluster the command should run. If you set the current-context one time then you won’t have to specify again and again while running the commands.

### **Service file explained**

apiVersion: v1  
kind: Service  
metadata:  
  name: my-app-service  
spec:  
  selector:  
    app: my-app  
  ports:  
  \- protocol: TCP  
    port: 80  
    targetPort: 8080

In this example,

*   **apiVersion** and **kind** specify the resource type
*   **metadata** specify the name of the service
*   **spec** specify the desired state of the Service
*   **selector** specifies on which pod the configurations will be invoked. If the pod label matches by app value then it will apply the configuration on that pod
*   In the **ports** section, **protocol** specifies the network protocol such as TCP, UDP, etc.
*   **ports** specifies on which port the service listens for the incoming traffic from the external sources.
*   **targetports** specify on which port the pod is listening.

***Example for port and targetports:***

Suppose you have a React.js application running inside a Kubernetes Pod, and it’s configured to listen on port 3000 within the Pod. However, you want external traffic to reach your application on port 80 because that’s the standard HTTP port. To achieve this, you create a Kubernetes Service with a targetPort set to 3000 and a port set to 80. The Service acts as a bridge, directing incoming traffic from port 80 to the application running on port 3000 inside the Pod. This redirection allows users to access your React.js app seamlessly over the standard HTTP port.

### **Deployment file explained**

apiVersion: apps/v1  
kind: Deployment  
metadata:  
  name: my-app  
spec:  
  replicas: 3  
  selector:  
    matchLabels:  
      app: my-app  
  template:  
    metadata:  
      labels:  
        app: my-app  
    spec:  
      containers:  
      \- name: my-app-container  
        image: my-app-image:latest

In this example,

*   **apiVersion** and **kind** define the resource type
*   **metadata** specify the details of deployment such as the name of the deployment, and labels.
*   **spec** defines the desired state of the Deployment.
*   **replicas** specify the desired number of pods to maintain.
*   The **selector** specifies on which pod the replica configuration should be applied with the help of the label of the pod.
*   **template** describes the pod template and how deployment will use it to create new pods.
*   **containers** will list the containers to run within the pod.
*   **name** specifies the name of the container
*   **image** specifies the name that will be used to run the container. The image will be a Docker Image.
*   **containerport** specifies the port at which the container will listen for incoming traffic.

### Conclusion

Understanding Kubeconfig, Service, and Deployment files is essential for anyone working with Kubernetes. These files are the building blocks that allow you to interact with the cluster, manage networking, and deploy applications seamlessly.

As you embark on your Kubernetes journey, keep these files in mind — they’re the keys to unlocking the full potential of this powerful container orchestration platform.

Happy Kuberneting!

### Want to Know About Challenge?

If you’re eager to learn more and join our challenge through the GitHub Repository, stay tuned for the upcoming posts. Follow for more exciting insights into the world of Kubernetes!

Don’t forget to tag me on LinkedIn.

**GitHub Repository**: [https://github.com/AmanPathak-DevOps/30DaysOfKubernetes](https://github.com/AmanPathak-DevOps/30DaysOfKubernetes)

#30DaysOfKubernetes #Kubernetes #DeploymentFile #ServiceFile #K8s

See you on Day **06** as we unravel more Kubernetes mysteries!

Stay connected on **LinkedIn**: [LinkedIn Profile](https://www.linkedin.com/in/aman-devops/)

Stay up-to-date with **GitHub**: [GitHub Profile](https://github.com/AmanPathak-DevOps)

Feel free to reach out to me, if you have any other queries.

Happy Learning

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658976919/e21d13f9-fc82-43fd-9365-36b223901013.gif)