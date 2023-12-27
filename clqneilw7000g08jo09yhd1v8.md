---
title: "Day19- Kubernetes AutoScaling"
datePublished: Sun Oct 22 2023 15:21:49 GMT+0000 (Coordinated Universal Time)
cuid: clqneilw7000g08jo09yhd1v8
slug: day19-kubernetes-autoscaling-da9da2c1d983
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703658888801/8daa5ec0-bafe-4aa5-aa80-9534fb00a456.png

---

### Introduction

Welcome to Day 18 of our #30DaysOfKubernetes journey! Kubernetes, the powerhouse of container orchestration, comes with a dynamic feature known as autoscaling. Imagine a scenario where the traffic to your application surges unexpectedly, and your servers are struggling to cope with the increased load. Downtime is not an option, and maintaining an excess of idle servers is not cost-effective. This is where Kubernetes autoscaling comes to the rescue. In simple terms, it dynamically adjusts the number of servers or their resources based on the current demand. In this blog, we’ll explore this crucial capability and its two primary types: Horizontal Pod auto-scaling and Vertical Pod auto-scaling.

Kubernetes supports autoscaling. If you don’t know about autoscaling, let me explain you in a simple way. As you know, to run the application we need CPU and memory. Sometimes, there will be a chance where the CPU gets loaded, and this might fail the server or affect the application. Now, we can’t afford the downtime of the applications. To get rid of these, we need to increase the number of servers or increase the capacity of servers.

**Let’s understand with a real-time example**

There are some OTT platforms such as Netflix or Hotstar. If any web show or movie is coming on the platform the audience is eagerly waiting for that. Then, the OTT platform can’t handle the lot of users that might crash the application. This will lead to a loss of business and the OTT platform can’t afford this business loss. Now, they have two options to solve this.

*   First, The Platform knows that they need a particular amount of servers such as 100. So, they can buy those servers forever but in this situation, when the load decreases then the other servers will become unused. Now, if the server is unused, still they have paid for those servers which is not a cost-effective method.
*   Second, The Platform doesn’t know when the load will increase. So, they have one option which is autoscaling in which when the CPU utilization crosses a particular number, it creates new servers. So, the platform can handle loads easily which is very cost effective as well.

**Types of Autoscaling**

*   Horizontal Pod AutoScaling: In this type of scaling, the number of servers will increase according to CPU utilization. In this, you define the minimum number of servers, maximum number of servers, and CPU utilization. If the CPU utilization crosses more than 50% then, it will add the one server automatically.
*   Vertical Pod AutoScaling: In this type of scaling, the server will remain the same in numbers but the server’s configuration will increase such as from 4GB RAM to 8GM RAM, and the same with the other configurations. But this is not cost-effective and business-effective. So, we use Horizontal Pod AutoScaling.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658871210/d5623770-c589-47ce-bfde-6af224e08a44.png)

**Key Features of Horizontal Pod AutoScaler**

*   By default, Kubernetes does not provide AutoScaling. If you need AutoScaling, then you have to create hpa(Horizontal Pod AutoScaling) and vpa(Vertical Pod AutoScaling) objects.
*   Kubernetes supports the autoscaling of the pods based on Observed CPU Utilization.
*   Scaling only supports Scalable objects like Controller, deployment, or ReplicaSet.
*   HPA is implemented as a Kubernetes API resource and a controller.
*   The Controller periodically adjusts the number of replicas in a replication controller or deployment to match the observed average CPU utilization to the target specified in the manifest file or command.

We will not cover Vertical Scaling, but you must be aware of it. To get a better understanding of VPA. Refer this:

[**The Guide To Kubernetes VPA by Example**  
*Learn Kubernetes VPA’s functionality and limitations with demo examples and understand how to use it with other…*www.kubecost.com](https://www.kubecost.com/kubernetes-autoscaling/kubernetes-vpa/ "https://www.kubecost.com/kubernetes-autoscaling/kubernetes-vpa/")[](https://www.kubecost.com/kubernetes-autoscaling/kubernetes-vpa/)

**Let’s do some hands-on for Horizontal Pod AutoScaler.**

We can perform HPA through two types:

*   **Imperative** way: In this way, you will create hpa object by command only.
*   **Declarative** way: In this way, you will create a proper manifest file and then create an object by applying the manifest file.

**HandsOn**

To perform HPA, we need one Kubernetes component which is a metrics server. To download this, use the below command.

curl -LO https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658872564/2b3d1400-93c1-4001-b381-ac1690c915a6.png)

Now, edit the components.yml file

add

hostNetwork: true

under spec: line and,

add

\- \--kubelet-insecure-tls

under the metric-resolutions line, then save the file.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658873941/29acfd17-e7dc-4469-b100-5267c5ee6a5c.png)

Now, you have to apply this by the below command

kubectl apply -f components.yml

To validate, run the command below

kubectl get pods -n kube-system

If the metrics-server pod is running then, you are ready to scale.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658875440/383fe9ca-18c3-46af-a0e6-1cef41e07c5c.png)

**Through Imperative way**

Create the deployment by the below file

kind: Deployment  
apiVersion: apps/v1  
metadata:  
   name: thdeploy  
spec:  
   replicas: 1  
   selector:  
    matchLabels:  
     name: deployment  
   template:  
     metadata:  
       name: testpod8  
       labels:  
         name: deployment  
     spec:  
      containers:  
        \- name: container1  
          image: nginx  
          ports:  
          \- containerPort: 80  
          resources:  
            limits:  
              cpu: 500m  
            requests:  
              cpu: 200m

Apply the deployment file

kubectl apply -f <file\_name.yml\>

Now, I have opened two windows to show you whether the pod is scaling or not.

Run the watch ‘kubectl get all’ command to see the pod creation.

On the other window, we are creating an hpa object through command.

kubectl autoscale deployment <deployment\_name\_from\_yml> — cpu-percent=20 — min\=1 — max\=5

Now on the other window, go into the container using the below command:

kubectl exec <pod\_name> -it — /bin/bash/

Now run the below command inside the container and see the magic.

while true; do apt update; done

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658876919/3bb49be2-ddc9-4c91-92d0-24d2a8fe5f99.png)

In this scenario, you can observe that the pod count has increased, reaching its maximum value of 5. If you intend to downscale, simply halt the command currently executing within the container. Afterward, Kubernetes will automatically delete the pods within approximately 5 minutes. This delay is designed to allow Kubernetes to assess the system load, if the load surges once more, Kubernetes will need to scale up the number of pods. This phase is commonly referred to as the “cooldown period.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658878781/824071a0-ed99-4b1a-bafc-5527769f9e57.png)

Here, you can see the pod number again comes to 1 because there is no load on the container.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658880501/541e4f1f-a1b7-41e1-82db-1e2b56136924.png)

**Through declarative way**

apiVersion: autoscaling/v2  
kind: HorizontalPodAutoscaler  
metadata:  
  name: demo-hpa  
spec:  
  scaleTargetRef:  
    apiVersion: apps/v1  
    kind: Deployment  
    name: thdeploy  
  minReplicas: 1  
  maxReplicas: 5  
  metrics:  
    \- type: Resource  
      resource:  
        name: cpu  
        target:  
          type: Utilization  
          averageUtilization: 20

Create a file for the hpa object and copy the above content in the file.

kubectl apply -f <hpa.yml\>

The hpa object has been created. Now, you can perform the above steps to increase the load on the container to see the pod scaling.

I have opened two windows to show you whether the pod is scaling or not.

Run the watch **‘kubectl get all’** command to see the pod creation.

Now on the other window, go into the container using the below command:

kubectl exec <pod\_name> -it — /bin/bash/

Now run the below command inside the container and see the magic.

while true; do apt update; done

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658882655/ed6f1cd4-058d-42ff-8821-78bec4c19b91.png)

In this scenario, you can observe that the pod count has increased, reaching its maximum value of 5. If you intend to downscale, simply halt the command currently executing within the container. Afterward, Kubernetes will automatically delete the pods within approximately 5 minutes. This delay is designed to allow Kubernetes to assess the system load, if the load surges once more, Kubernetes will need to scale up the number of pods. This phase is commonly referred to as the “cooldown period.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658884387/af6c1b19-0481-44aa-9acb-d49498c60ec8.png)

Here, you can see the pod number again comes to 1 because there is no load on the container.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658886060/475b68d5-49b5-4d9b-aff9-9b78afabc50c.png)

### Conclusion

As we wrap up our journey into Kubernetes autoscaling, it’s essential to grasp the significance of this feature. Autoscaling ensures your applications can handle fluctuations in traffic effortlessly, without unnecessary costs or downtimes. Whether you’re running an OTT platform handling a massive audience or managing a web application, Kubernetes autoscaling can be a game-changer. By understanding and implementing Horizontal Pod AutoScaling, you’re taking a big step toward making your applications resilient and cost-efficient. While we haven’t covered Vertical Pod AutoScaling in detail, it’s worth exploring if your use case demands resource adjustments on a different scale.

Stay tuned for more insights into Kubernetes as we continue our 30DaysOfKubernetes journey. Remember, the power of Kubernetes lies not just in running containers but in managing them intelligently to meet your application’s dynamic needs.

### Want to Know About Challenge?

If you’re eager to learn more and join our challenge through the GitHub Repository, stay tuned for the upcoming posts. Follow for more exciting insights into the world of Kubernetes!

**GitHub Repository**: [https://github.com/AmanPathak-DevOps/30DaysOfKubernetes](https://github.com/AmanPathak-DevOps/30DaysOfKubernetes)

#Kubernetes #ReplicationController #ReplicaSet #ContainerOrchestration #DevOps #K8sLearning

See you on Day **20** as we unravel more Kubernetes mysteries!

Stay connected on **LinkedIn**: [LinkedIn Profile](https://www.linkedin.com/in/aman-devops/)

Stay up-to-date with **GitHub**: [GitHub Profile](https://github.com/AmanPathak-DevOps)

Feel free to reach out to me, if you have any other queries.

Happy Learning