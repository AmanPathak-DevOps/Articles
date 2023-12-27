---
title: "Day 24: Exploring Kubernetes Network Policies 🌐"
datePublished: Thu Nov 23 2023 04:43:14 GMT+0000 (Coordinated Universal Time)
cuid: clqneg2kn000d08l126szdi52
slug: day-24-exploring-kubernetes-network-policies-0d8e687ff850
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703658770483/62d0f85c-b440-405c-a036-c4f06b040533.png

---

### Introduction

Welcome to Day 24 of our #30DaysOfKubernetes journey! Today, we’ll dive into the world of Network Policies, a powerful feature that adds an extra layer of security and control to your Kubernetes cluster.

### **What is Network Policy?**

By default, a pod can communicate with any other pods whether it’s present in any namespaces. But if you want to secure your pod by providing access to only known pods or authorized pods then Kubernetes has the richest feature known as Network Policy. Network Policy will help you to protect your pod by accessing only authorized pods. So, this way the pod’s security will be enhanced.

Network Policy allows us to define the rules to communicate between the pods. With the help of Networking Policy, Kubernetes provides fine-grained controls over what traffic is allowed or denied which leads to enhancing the security and isolation of your applications.

### Key Features:

1.  **Policy Rules**: Network Policies consist of a set of rules in which you define how the traffic is allowed or denied. You can specify these rules by pod labels, namespaces, or particular IPs.
2.  **Pod Selectors**: If you want to apply the Network Policy to the particular pod then you can use Pod Selector which will select the particular pod and apply the Network Policy on that pod.
3.  **Ingress and Egress**: Network Policies allow you to define the Ingress and Egress rules. Ingress means incoming traffic on the pod from the outside whereas Egress means outgoing traffic to the internet(anywhere) from the pod itself.
4.  **Namespaces**: If you want to apply your Network Policy to the group of pod which is present in the particular namespace then you can namespaceSelector which will help you to invoke the Network Policy on all pods within the particular namespace.
5.  **Priority**: Network Policy also provides the priority feature in which you define the priority of the rules which helps you to get fine-grained control over the traffic rules of your application.

### Use cases of Network Policy:

1.  **Isolation**: You can invoke the Network Policies to isolate different application components inside the Cluster. For example, you have an application with frontend and backend so you will create a namespace and apply the network policy on that namespace to make the frontend and backend application secure.
2.  **Microservices**: Network Policies help to make microservices architecture more secure and prevent unauthorized communications between different microservices.
3.  **Compliance**: For Compliance reasons, you have to follow the protocol in which only authorized pods can communicate with each other. Network policies help you to achieve this.
4.  **Multi-tenancy**: Network Policies make sure that it will not interfere with different tenants. So, the flow of the multi-tenant cluster will be working smoothly if anything fails on the particular tenant.
5.  **Application testing**: Suppose, if you are testing something of an application then, Network Policy helps to control its access to production services, reducing the risks of unintended interactions.

### Hands-On Demo:

To perform the hands-on Network Policy, Kubernetes must have network plugins like Calico or Cilium. By default noop network plugin is installed that does not provide advanced or rich features of Kubernetes Networking.

In the below steps, we will install the Cilum networking plugin to perform our demo. Without any advanced networking plugin, we can’t able to perform a demo.

minikube start — network\-plugin\=cni

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658743427/bc8914a3-4a7c-4329-96e7-f134d989fbe9.png)

curl -LO https://github.com/cilium/cilium-cli/releases/latest/download/cilium-linux-amd64.tar.gz

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658744902/da4d0b7c-1377-4d77-b70a-8bf971b4e2d6.png)

sudo tar xzvfC cilium-linux-amd64.tar.gz /usr/local/bin  
rm cilium-linux-amd64.tar.gz

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658746384/365a1293-b350-4832-81d3-dfe1d663ec1c.png)

cilium install

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658747689/fec5c485-4876-48bc-ace3-a526f7e43aeb.png)

To validate whether your networking pod is running or not, run the below command.

kubectl get pods — namespace\=kube-system -l k8s-app=cilium

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658748882/70026d44-3109-41cc-9205-d124fab9949e.png)

Create three namespaces and deploy the nginx pod on those namespaces with service where we have to expose port 80

kubectl create namespace namespace\-a  
kubectl create deployment nginx - image=nginx - namespace namespace\-a  
kubectl expose deployment nginx - port=80 - namespace namespace\-a  
kubectl create namespace namespace\-b  
kubectl create deployment nginx - image=nginx - namespace namespace\-b  
kubectl expose deployment nginx - port=80 - namespace namespace\-b  
kubectl create namespace namespace\-c  
kubectl create deployment nginx - image=nginx - namespace namespace\-c  
kubectl expose deployment nginx - port=80 - namespace namespace\-c

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658750445/27554cd4-18d6-461b-b1c4-7d64e475cb84.png)

Check whether the pods are running or not of all three namespaces.

kubectl get pods -A

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658752159/5c461028-c411-4060-8c47-29442bf1deaa.png)

List the Private IPs of all three pods running from all three namespaces.

kubectl get pods -A -o wide

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658753634/d5157009-bf5d-4dd0-883c-8ebc43ba2db1.png)

Now, try to access the pod of namespace-b from the namespace-a pod

kubectl -n namespace\-c exec <namespace\-c\_pod\_name> - curl <namespace\-a\_pod\_private\_ip>  
kubectl -n namespace\-c exec nginx\-77b4fdf86c-v4qdd - curl 10.244.0.106

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658755530/53eae36a-fe97-4848-850c-6ff6b94f946a.png)

Now, try to access the pod of namespace-b from the namespace-c pod

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658756944/6863419e-ddd8-4868-8682-031864a81c68.png)

As you saw in the above two steps, different namespace pods are able to access the other namespace’s pods which is not good DevOps practice. Let’s first try to implement where the namespace-b pod can’t be accessible to any other pod.

**Deny All**

apiVersion: networking.k8s.io/v1  
kind: NetworkPolicy  
metadata:  
  name: deny-all-traffic  
  namespace: namespace-b  
spec:  
  podSelector: {}  
  policyTypes:  
  \- Ingress  
  \- Egress

Deploy the NetworkPolicy to deny all access for namespace-b

After deploying the network policy, now try to access the namespace-b pod from both namespace-a and namespace-c pod and you will see in the below snippet that you can’t access the namespace-b pod which is expected.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658758533/a9894312-ee49-4dbe-ac8a-03627b6dc5e5.png)

Once you delete the Network Policy, then try to access the namespace-b pod from the other pod and you will see that you can access the namespace-b pod.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658760528/dd8fc6d8-4d1a-4340-8ef9-f5a5ebc24739.png)

Now, let’s try to impose accessibility like the one below snippet where the namespace-c pod can only access the namespace-b pod. If namespace-a tries to access namespace-b pod then our goal is to prevent the access.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658762173/06d79ede-2ca4-4118-b0b0-c825b6ff6110.png)

Add the label to all three K8s namespaces

kubectl label namespaces namespace\-a ns=namespacea  
kubectl label namespaces namespace\-b ns=namespaceb  
kubectl label namespaces namespace\-c ns=namespacec

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658763782/5dcd5018-f74d-41b1-b882-1bf80080da6d.png)

Now, Our motive is that only namespace-a can be able to access the namespace-b pod whereas the namespace-c can’t be able to access the namespace-b pod. Let’s implement the Network Policy.

Add the label environment=QA to the namespace-b pod because there can be multiple pods running inside one namespace. So, if you have to give access to the particular pod instead of all.

kubectl get pods - namespace namespace\-b  
kubectl label - namespace namespace\-b pod nginx-7854ff8877-m69p4 environment=QA  
kubectl get pods - namespace namespace\-b - show-labels

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658765393/33e6a75a-29a5-40b3-9941-34c644f654a8.png)

Now, try to access namespace-b pod from namespace-a where it should be accessible.

apiVersion: networking.k8s.io/v1  
kind: NetworkPolicy  
metadata:  
  name: nginx-ingress  
  namespace: namespace-b  
spec:  
  podSelector:  
    matchLabels:  
      environment: QA  
  policyTypes:  
  \- Ingress  
  ingress:  
  \- from:  
    \- namespaceSelector:  
        matchLabels:  
          ns: namespacea

kubectl apply -f Allow-namsepacea.yml  
kubectl -n namespace-a exec nginx-7854ff8877-rx8b8 - curl 10.0.0.165

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658766769/cc78f1c8-2496-4b9b-9426-6460e646a140.png)

Now, Let’s try to access namespace-b pod from namespace-c.

kubectl -n namespace-c exec nginx-7854ff8877-gmz9k — curl 10.0.0.165

As you can see in the below snippet, namespace-c pod could not able to access namespace-b pod which is expected.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658768038/a8f063e6-c72d-4081-81ed-308efccca2d5.png)

### Conclusion

Kubernetes Network Policies offer a robust solution for securing and controlling communication within your cluster. By leveraging these features, you can implement a proactive approach to network security, ensuring the integrity and isolation of your applications. As you continue your Kubernetes learning journey, mastering Network Policies will undoubtedly enhance your ability to design and manage secure, scalable, and efficient containerized environments.

Stay tuned for more exciting Kubernetes learnings on Day 25 of #30DaysOfKubernetes! 🚀🌐

### Want to Know About Challenge?

If you’re eager to learn more and join our challenge through the GitHub Repository, stay tuned for the upcoming posts. Follow for more exciting insights into the world of Kubernetes!

**GitHub Repository**: [https://github.com/AmanPathak-DevOps/30DaysOfKubernetes](https://github.com/AmanPathak-DevOps/30DaysOfKubernetes)

#Kubernetes #DaemonSet #StatefulSets #NetworkPolicy #ContainerOrchestration #DevOps #K8sLearning

See you on Day **25** as we unravel more Kubernetes mysteries!

Stay connected on **LinkedIn**: [LinkedIn Profile](https://www.linkedin.com/in/aman-devops/)

Stay up-to-date with **GitHub**: [GitHub Profile](https://github.com/AmanPathak-DevOps)

Feel free to reach out to me, if you have any other queries.

Happy Learning