---
title: "Day07- Mastering Kubernetes Labels, Selectors, and Node Selectors"
datePublished: Mon Oct 09 2023 17:01:53 GMT+0000 (Coordinated Universal Time)
cuid: clqnemfzn000808i882q52umn
slug: day07-mastering-kubernetes-labels-selectors-and-node-selectors-3df0293b7336
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703659067599/4b0d92b8-c699-4d9c-8c7b-d103ee8f5044.png

---

Welcome to Day 07 of our #30DaysOfKubernetes journey! Today, we dive deep into Kubernetes’ powerful features: Labels, Selectors, and Node Selectors. These tools are essential for organizing and managing your Kubernetes objects efficiently. Let’s explore how they work and why they matter.

### **Labels**

*   Labels are used to organize Kubernetes Objects such as Pods, nodes, etc.
*   You can add multiple labels over the Kubernetes Objects.
*   Labels are defined in key-value pairs.
*   Labels are similar to tags in AWS or Azure where you give a name to filter the resources quickly.
*   You can add labels like environment, department or anything else according to you.

### **Labels-Selectors**

Once the labels are attached to the Kubernetes objects those objects will be filtered out with the help of labels-Selectors known as Selectors.

The API currently supports two types of label-selectors equality-based and set-based. Label selectors can be made of multiple selectors that are comma-separated.

### **Node Selector**

Node selector means selecting the nodes. If you are not aware of what is nodes. There are two types of nodes Master Nodes and Worker Nodes.

Master Nodes is responsible for the entire Kubernetes Cluster that communicates with the Worker Node and runs the applications on containers smoothly. Master nodes can have multiple Worker Nodes.

Worker Nodes work as a mediator where the nodes communicate with Master nodes and run the applications on the containers smoothly.

So, the use of node selector is choosing the nodes which means on which worker node the command should be applied. This is done by Labels where in the manifest file, we mentioned the node label name. While running the manifest file, master nodes find the node that has the same label and create the pod on that container. Make sure that the node must have the label. If the node doesn’t have any label then, the manifest file will jump to the next node.

### Label and Label-Selector HandsOn

If you don’t understand properly, don’t worry. We will do hands-on which will make it easy to understand the concepts of labels, labels-selectors, and node selectors.

YAML file

apiVersion: v1  
kind: Pod  
metadata:  
  name: day07  
  labels:  
    env: testing  
    department: DevOps  
spec:  
  containers:  
    \- name: containers1  
      image: ubuntu  
      command: \["/bin/bash", "-c", "while true; do echo This is Day07 of 30DaysOfKubernetes; sleep 5 ; done"\]

After creating the pods by the following command.

List the pods to see the labels that are attached to the pod by the given command

> kubectl get pods — show-labels

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659043360/8dd475fa-d17e-49db-894c-b03bad499289.png)

I have created one more manifest file that doesn’t have any label as you can see in the below screenshot.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659044798/f63ca6e1-2a5d-493c-91fc-0b7ca9b3f201.png)

Now, I want to list those pods that have the label env=testing.

As we have discussed earlier, there are two types of label-selectors equality and set-based.

This is the example of equality based where we used equalsTo(=).

> kubectl get pods -l env=testing

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659046360/cdd61be0-cdb8-4a22-b99b-8d0db136ed89.png)

Now, here I want to list those pods that don’t have the label department=DevOps.

> kubectl get pods -l department!=DevOps

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659047925/5931c687-162e-4f5f-a79d-b53a202363a0.png)

Now, Suppose I forgot to add the label through the declarative(via manifest) method. So, I can add labels after creating the pods as well which is known as the imperative(via command) method.

In the below screenshot, I have added the new label location and given it the value India.

> kubectl label pods day07 Location=India

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659049484/e4f5a78c-2f26-4d18-b102-ec09a0603f49.png)

As we discussed the type of label-selector, let’s see the example of a set-based label-selector where we use **in, notin,** and **exists.**

In the below screenshot, we are trying to list all those pods that have an env label with value for either testing or development.

> kubectl get pods -l ‘env in (testing, development)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659050916/ab131631-ae2e-4168-8e62-38ee346a711b.png)

Here, we are trying to list all those pods that don’t have the India or US value for the Location key in the Label.

> kubectl get pods -l ‘Location not in (India, US)’

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659052292/4ba0f8da-8f65-4b06-ac52-afb512539d09.png)

We can also delete the pods as per the label.

Here, We have deleted all those pods that don’t have the Chinda value for the location key in the Label.

> kubectl delete pod -l Location!=China

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659054002/f4451ce8-b562-4ce7-bc95-0eb453bd6491.png)

Here, We have completed the HandsOn for the label and label-selector. You can explore more yourself.

Let’s move it to **node-selector**

### nodeSelector HandsOn

As you remember, we have set up minikube on our machine. So, our master and worker node is on the same machine.

To list the nodes on the master node, use the command

> kubectl get nodes

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659055853/315d1f17-b897-49ac-9d8d-f56856f2ed32.png)

apiVersion: v1  
kind: Pod  
metadata:  
  name: nodelabels  
  labels:  
    env: testing  
spec:  
  containers:  
    \- name: container1  
      image: ubuntu  
      command: \["/bin/bash", "-c", "while true; do echo Node-Selector Example; sleep 5 ; done"\]  
  nodeSelector:                                           
   hardware: t2-medium

Here, I have created the pod but when I list the pods the status is pending for the created pod.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659057202/6eec9116-27be-4ef4-9a7d-bd1de3ea6b87.png)

If you observe the manifest file, the master node is looking for that worker node which has the label hardware=t2-medium.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659058616/a792d93e-cc12-49c7-83ac-529c9f3c6577.png)

As you can see there is no label added like hardware=t2-medium

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659059861/18dac0c3-33e0-47ea-865a-9685a647f38d.png)

Here we have added the label to the worker node

> kubectl label nodes minikube hardware=t2-medium

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659061309/79078223-a634-48e0-95b3-145fb8f064fe.png)

As sooner the label is added, the sooner the pod will be in a running state.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659063066/f6e76a84-e949-4be8-92de-8b51f5cfcdac.png)

### **Conclusion**

By mastering Kubernetes Labels, Selectors, and Node Selectors, you gain granular control over resource organization and deployment. These features are invaluable for managing complex Kubernetes clusters effectively.

### Want to Know About Challenge?

If you’re eager to learn more and join our challenge through the GitHub Repository, stay tuned for the upcoming posts. Follow for more exciting insights into the world of Kubernetes!

**GitHub Repository**: [https://github.com/AmanPathak-DevOps/30DaysOfKubernetes](https://github.com/AmanPathak-DevOps/30DaysOfKubernetes)

#30DaysOfKubernetes #Kubernetes #MasterNode #LabelSelectors #K8s

See you on Day **08** as we unravel more Kubernetes mysteries!

Stay connected on **LinkedIn**: [LinkedIn Profile](https://www.linkedin.com/in/aman-devops/)

Stay up-to-date with **GitHub**: [GitHub Profile](https://github.com/AmanPathak-DevOps)

Feel free to reach out to me, if you have any other queries.

Happy Learning

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659064806/8194deb0-ac0d-4e5c-a43f-45ad5cbbada3.gif)