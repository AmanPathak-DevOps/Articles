---
title: "Day21- Kubernetes Ingress"
datePublished: Wed Nov 01 2023 15:49:10 GMT+0000 (Coordinated Universal Time)
cuid: clqnf9qke000k08l6dfax7kgv
slug: day21-kubernetes-ingress-f5dddf1599bc
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703660153933/47b2eb09-8e85-4229-810f-9086ee121083.png

---

### **Introduction**

Welcome to Day 21 of our #30DaysOfKubernetes journey! In the world of Kubernetes, Ingress is your ticket to managing external traffic to services within the cluster. Before we dive into the details, let’s recap what we’ve learned so far.  
Before Ingress, the Service provides a Load balancer, which is used to distribute the traffic between multiple applications or pods.

Ingress helps to expose the HTTP and HTTPS routes from outside of the cluster.

Ingress enables Path-based and Host-based routing.

Ingress supports Load balancing and SSL termination.

**Simple Definition/Explanation**

Kubernetes Ingress is like a cop for your applications that are running on your Kubernetes cluster. It redirects the incoming requests to the right services based on the Web URL or path in the address.

Ingress provides the encryption feature and helps to balance the load of the applications.

> *In simple words, Ingress is like a receptionist who provides the correct path for the hotel room to the visitor or person.*

**Why do we use Ingress because the load balancer supports the same thing?**

Ingress is used to manage the external traffic to the services within the cluster which provides features like host-based routing, path-based routing, SSL termination, and more. Where a Load balancer is used to manage the traffic but the load balancer does not provide the fine-grained access control like Ingress.

**Example:**

Suppose you have multiple Kubernetes services running on your cluster and each service serves a different application such as example.com/app1 and example.com/app2. With the help of Ingress, you can achieve this. However, the Load Balancer routes the traffic based on the ports and can't handle the URL-based routing.

**There are two types of Routing in Ingress:**

*   **Path-based routing**: Path-based routing directs traffic to the different services based on the path such as example.com**/app1.**
*   **Host-based routing**: Host-based routing directs traffic to the different services based on the Website’s URL such as **demo.**example.com.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660128204/d42a0b5f-ebb3-4a01-ab24-b03d3b6d990e.png)

Path-Based Routing

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660130025/58e7c884-e31b-4d19-851d-dc1db45f27fb.png)

Host-Based Routing

To implement Ingress, we have to deploy Ingress Controllers. We can use any Ingress Controllers according to our requirements.

### Hands-On

Here, we will use the nginx ingress controller.

To install it, use the command.

minikube addons enable ingress

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660131783/b8a24ff7-b4ad-4eb2-a1b4-acb66ca1b36f.png)

Validate whether the controller is deployed or not

kubectl get pods -n ingress-nginx

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660133405/b128eecd-48e2-4a91-8c65-bb73abd5a05d.png)

Now, let’s do some hands-on for Path-based routing.

Deploy home page

**deployment1.yml file**

apiVersion: apps/v1  
kind: Deployment  
metadata:  
  name: nginx-deployment  
spec:  
  replicas: 1  
  selector:  
    matchLabels:  
      app: nginx  
  template:  
    metadata:  
      labels:  
        app: nginx  
    spec:  
      containers:  
      \- name: nginx  
        image: avian19/choco-shop-home  
        ports:  
        \- containerPort: 80

**service1.yml file**

apiVersion: v1  
kind: Service  
metadata:  
  name: nginx-service  
spec:  
  selector:  
    app: nginx  
  ports:  
  \- protocol: TCP  
    port: 80  
  type: NodePort 

kubectl apply -f deployment1.yml  
kubectl apply -f service1.yml

Once you created the deployment and service, now we have to create the ingress for path-based routing. As we want to direct the requests to the default path use the below YAML file.

**ingress.yml**

kind: Ingress  
metadata:  
  name: ingress-deployment  
  annotations:  
    nginx.ingress.kubernetes.io/rewrite-target: /$1  
spec:  
  rules:  
    \- host: example.devops.in  
      http:  
        paths:  
          \- path: /  
            pathType: Prefix    
            backend:  
              service:  
                name: nginx-service  
                port:  
                  number: 80

kubectl apply -f ingress.yml

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660135303/320fa907-4ff1-4b28-9f61-e8d3febcd43b.png)

Add the IP ADDRESS that you got in the above snippet from ingress-deployment(192.168.49.2).

vim /etc/hosts

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660136937/1df16ea8-dfd6-41be-854b-5ccb70432127.png)

If you do curl from the terminal then you can able to see the content of your application.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660138183/150f1ab0-c611-40f8-a60b-d2001e15c3f7.png)

If you map the DNS name with ingress Private IP, then you can able to see the content of your application from the browser.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660139460/c6849389-d533-4ab6-9d13-78ee2935b769.png)

Now I have one more module whose name is Menu that I want to deploy on the other services.

To do that, Create a deployment file and a service file.

**deploy2.yml file**

apiVersion: apps/v1  
kind: Deployment  
metadata:  
  name: nginx-deployment2  
spec:  
  replicas: 1  
  selector:  
    matchLabels:  
      app: nginx2  
  template:  
    metadata:  
      labels:  
        app: nginx2  
    spec:  
      containers:  
      \- name: nginx2  
        image: avian19/choco-shop-menu  
        ports:  
        \- containerPort: 80

**service2.yml file**

apiVersion: v1  
kind: Service  
metadata:  
  name: nginx-service2  
spec:  
  selector:  
    app: nginx2  
  ports:  
  \- protocol: TCP  
    port: 80  
  type: NodePort 

kubectl apply -f deploy2.yml  
kubectl apply -f service2.yml

Now, Here we have to modify our ingress file as we have added a new service which has a new application. To avoid confusion, just remove the previous content from the **ingress.yml** file copy and paste the entire content in the **ingress.yml** file, and apply the updated configurations.

**Updated ingress.yml file**

apiVersion: networking.k8s.io/v1  
kind: Ingress  
metadata:  
  name: ingress-deployment  
  annotations:  
    nginx.ingress.kubernetes.io/rewrite-target: /$1  
spec:  
  rules:  
    \- host: example.devops.in  
      http:  
        paths:  
          \- path: /  
            pathType: Prefix    
            backend:  
              service:  
                name: nginx-service  
                port:  
                  number: 80  
          \- path: /menu  
            pathType: Prefix  
            backend:  
              service:  
                name: nginx-service2  
                port:  
                  number: 80

kubectl apply -f ingress.yml

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660141125/01637d99-07f4-4a14-b2a9-f924463eae53.png)

Now, we can access our application on the /menu path.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660142615/65b233d1-cbd1-4249-aa1c-f2ad051641ab.png)

Now I have one more module which name is Reviews that I want to deploy on other services.

To do that, Create a deployment file and a service file.

**deploy3.yml file**

apiVersion: apps/v1  
kind: Deployment  
metadata:  
  name: nginx-deployment3  
spec:  
  replicas: 1  
  selector:  
    matchLabels:  
      app: nginx3  
  template:  
    metadata:  
      labels:  
        app: nginx3  
    spec:  
      containers:  
      \- name: nginx3  
        image: avian19/choco-shop-reviews  
        ports:  
        \- containerPort: 80

**service3.yml file**

apiVersion: v1  
kind: Service  
metadata:  
  name: nginx-service3  
spec:  
  selector:  
    app: nginx3  
  ports:  
  \- protocol: TCP  
    port: 80  
  type: NodePort 

kubectl apply -f deploy3.yml  
kubectl apply -f service3.yml

Now, Here we have to modify our ingress file as we have added a new service which has a new application. To avoid confusion, just remove the previous content from the **ingress.yml** file copy and paste the entire content in the **ingress.yml** file, and apply the updated configurations.

**Updated ingress.yml file**

apiVersion: networking.k8s.io/v1  
kind: Ingress  
metadata:  
  name: ingress-deployment  
  annotations:  
    nginx.ingress.kubernetes.io/rewrite-target: /$1  
spec:  
  rules:  
    \- host: example.devops.in  
      http:  
        paths:  
          \- path: /  
            pathType: Prefix    
            backend:  
              service:  
                name: nginx-service  
                port:  
                  number: 80  
          \- path: /menu  
            pathType: Prefix  
            backend:  
              service:  
                name: nginx-service2  
                port:  
                  number: 80  
          \- path: /reviews  
            pathType: Prefix  
            backend:  
              service:  
                name: nginx-service3  
                port:  
                  number: 80

kubectl apply -f ingress.yml

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660144184/b0cf2469-880d-44de-8540-86c1e2268ee7.png)

Now, we can access our application on the /reviews path.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660145518/c877e285-a36f-4511-a8e9-2e9671d0b087.png)

### **Host-based Routing**

Now, we have completed our hands-on for Path-based Routing.

I want to create one more application to order anything from different hosts. Let’s do that.

Deploy the applications and services.

**deploy4.yml file**

apiVersion: apps/v1  
kind: Deployment  
metadata:  
  name: nginx-deployment4  
spec:  
  replicas: 1  
  selector:  
    matchLabels:  
      app: nginx4  
  template:  
    metadata:  
      labels:  
        app: nginx4  
    spec:  
      containers:  
      \- name: nginx4  
        image: avian19/choco-shop-order  
        ports:  
        \- containerPort: 80

**service4.yml file**

apiVersion: v1  
kind: Service  
metadata:  
  name: nginx-service4  
spec:  
  selector:  
    app: nginx4  
  ports:  
  \- protocol: TCP  
    port: 80  
  type: NodePort

kubectl apply -f deploy4.yml  
kubectl apply -f service4.yml

Now, Here we have to modify our ingress file as we have added a new service which has a new application. To avoid confusion, just remove the previous content from the **ingress.yml** file copy and paste the entire content into the **ingress.yml** file, and apply the updated configurations.

**Updated ingress.yml file**

apiVersion: networking.k8s.io/v1  
kind: Ingress  
metadata:  
  name: ingress-deployment  
  annotations:  
    nginx.ingress.kubernetes.io/rewrite-target: /$1  
spec:  
  rules:  
    \- host: example.devops.in  
      http:  
        paths:  
          \- path: /  
            pathType: Prefix    
            backend:  
              service:  
                name: nginx-service  
                port:  
                  number: 80  
          \- path: /menu  
            pathType: Prefix  
            backend:  
              service:  
                name: nginx-service2  
                port:  
                  number: 80  
          \- path: /reviews  
            pathType: Prefix  
            backend:  
              service:  
                name: nginx-service3  
                port:  
                  number: 80  
\# Host Based Routing   
    \- host: example2.devops.in  
      http:  
        paths:  
          \- path: /  
            pathType: Prefix  
            backend:   
              service:  
                name: nginx-service4  
                port:  
                  number: 80

kubectl apply -f ingress.yml

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660147184/dbeedba4-efc8-4de5-8ded-28942e23285f.png)

Now, if you try to curl from the terminal then you will be able to get the content. But if you try from the browser then you won’t be able to get the content.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660148552/7240431c-746f-482b-ad03-bc81af281a9c.png)

Now, we have added a new host in the ingress file. To get the content on our local host, we need to add this host in the /etc/hosts file.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660150164/707d85c0-9e1b-43c5-a65e-47bd2c3cb7bb.png)

Now check on the browser by hitting the new hostname.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660151313/a71737d9-fd21-47ed-aff4-cc684fb1732f.png)

### **Conclusion**

By mastering Ingress, you can efficiently route and manage external traffic to your services. It’s like having a versatile receptionist for your applications, ensuring visitors get to the right place. With Path-Based and Host-Based routing, encryption, and load balancing, Ingress empowers you to provide a seamless experience for your users. So, why Ingress? Because it offers the fine-grained control and flexibility that a load balancer alone can’t match.

### Want to Know About Challenge?

If you’re eager to learn more and join our challenge through the GitHub Repository, stay tuned for the upcoming posts. Follow for more exciting insights into the world of Kubernetes!

**GitHub Repository**: [https://github.com/AmanPathak-DevOps/30DaysOfKubernetes](https://github.com/AmanPathak-DevOps/30DaysOfKubernetes)

#Kubernetes #IngressController #Ingress #ContainerOrchestration #DevOps #K8sLearning

See you on Day **22** as we unravel more Kubernetes mysteries!

Stay connected on **LinkedIn**: [LinkedIn Profile](https://www.linkedin.com/in/aman-devops/)

Stay up-to-date with **GitHub**: [GitHub Profile](https://github.com/AmanPathak-DevOps)

Feel free to reach out to me, if you have any other queries.

Happy Learning