---
title: "Day06- Deploying Your First Node.js Application on Kubernetes Cluster"
datePublished: Sun Oct 08 2023 16:04:37 GMT+0000 (Coordinated Universal Time)
cuid: clqnels8w000o08jofao705ky
slug: day06-deploying-your-first-node-js-application-on-kubernetes-cluster-eaabb19bb9fe
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703659036636/e34fa572-5aed-4ab0-a495-d51aab559033.png

---

Welcome to Day 06 of the #30DaysOfKubernetes challenge! In Today’s blog post, we’ll walk through deploying a Node.js application on a Kubernetes cluster. By following these steps, you’ll have your Node.js app up and running in no time. For reference, you can find the Node.js code and Dockerfile in this [GitHub repository](https://github.com/AmanPathak-DevOps/NodeJS-Dockerfile).

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659015058/627a8ef4-849f-4c46-b0ed-89d402e996fd.png)

To follow this, you need to install minikube on your local/AWS machine. If you don’t know then you can refer to my step-by-step blog which will help you to do it.

[https://medium.com/@aman.pathak\_51134/day-04-setting-up-minikube-on-your-local-machine-or-aws-instance-620a4cb57abc](https://medium.com/@aman.pathak_51134/day-04-setting-up-minikube-on-your-local-machine-or-aws-instance-620a4cb57abc)

### Step 1: Create a Docker Image

Assuming you’re already familiar with Docker, let’s create a Docker image for your Node.js project. Open your terminal and use the following command to build the image:

> docker build — tag avian19/node-app .

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659016424/fd342f09-b18b-4e4a-864b-e7faceecd82d.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659017714/f4c9f13d-af8d-4c1a-8321-c726fa30cec3.png)

### Step 2: Push the Docker Image to Docker Hub

To share your Docker image with your Kubernetes cluster, you can push it to Docker Hub. First, log in to Docker Hub using your terminal:

> docker login

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659019390/f227cdfd-755e-4310-8963-4346b5b95376.png)

Then, push the Docker image:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659021091/2c431e5f-659d-44aa-9773-a89af09cc41a.png)

You can confirm that your image has been successfully pushed to Docker Hub.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659022449/f7dd24fc-0c23-4017-85c3-6ae3ec1b761d.png)

### Step 3: Prepare Kubernetes Deployment and Service Files

Create a dedicated directory for your Node.js application’s deployment. Inside this directory, add the contents of your `deployment.yml` and `service.yml` files.

> deployment.yml file

apiVersion: apps/v1  
kind: Deployment  
metadata:  
  name: node-app-deployment  
  labels:   
    app: node-app  
spec:  
  replicas: 2  
  selector:  
    matchLabels:  
      app: node-app  
  template:   
    metadata:  
      labels:  
        app: node-app  
    spec:  
      containers:  
      \- name: node-container  
        image: avian19/node-app:latest  
        ports:  
        \- containerPort: 3000

> service.yml file

apiVersion: v1  
kind: Service  
metadata:  
  name: node-app-service  
spec:  
  selector:  
    app: node-app  
  type: LoadBalancer  
  ports:  
  \- protocol: TCP  
    port: 5000  
    targetPort: 3000  
    nodePort: 30001

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659024110/d479086b-1dad-4b48-9a4f-fb9c91eb62fd.png)

### Step 4: Deploy Pods

To deploy the pods, use the `deployment.yml` file with the following command:

> kubectl apply -f deployment.yml

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659025589/fcc75a29-de4b-45f6-9022-1f8247b25036.png)

### Step 5: Deploy Services

Next, deploy the services using the `service.yml` file:

> kubectl apply -f service.yml

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659027158/dd8faf59-00c2-4ed2-b773-6fe194057cfd.png)

### Step 6: Validate the Deployment

You can check the status of your deployment by running the following command:

> kubectl get deployment

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659029202/60c8dbd3-190f-4e42-a325-62842af1d23d.png)

### Step 7: Access Your Application

To access your deployed application, use the following command to get the URL:

> minikube service node-app-service

You can now use `curl` to access the content of your Node.js application through the provided URL.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659030805/3b0cebe4-2c44-48eb-9699-68e17bf58d91.png)

In nodejs code, you can see that the content is the same at both places.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659032484/b41466a7-3719-4ac8-9c82-05334854962d.png)

### Conclusion

Congratulations! You’ve successfully deployed your Node.js application on a Kubernetes cluster. This tutorial guides you through the essential steps, from creating a Docker image to deploying your application. Now you can scale and manage your Node.js app with ease on Kubernetes.

Happy coding!

### Want to Know About Challenge?

If you’re eager to learn more and join our challenge through the GitHub Repository, stay tuned for the upcoming posts. Follow for more exciting insights into the world of Kubernetes!

Don’t forget to tag me on LinkedIn.

**GitHub Repository**: [https://github.com/AmanPathak-DevOps/30DaysOfKubernetes](https://github.com/AmanPathak-DevOps/30DaysOfKubernetes)

#30DaysOfKubernetes #Kubernetes #DeploymentFile #ServiceFile #K8s

See you on Day **07** as we unravel more Kubernetes mysteries!

Stay connected on **LinkedIn**: [LinkedIn Profile](https://www.linkedin.com/in/aman-devops/)

Stay up-to-date with **GitHub**: [GitHub Profile](https://github.com/AmanPathak-DevOps)

Feel free to reach out to me, if you have any other queries.

Happy Learning

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659034269/8f1b8a8a-3e0f-4efe-a374-a9638db0e72e.jpeg)