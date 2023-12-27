---
title: "Day22- Kubernetes StatefulSets"
datePublished: Thu Nov 02 2023 15:05:38 GMT+0000 (Coordinated Universal Time)
cuid: clqnfavgz000n08l60hr9g63m
slug: day21-kubernetes-statefulsets-2ecf9ca2c5fc
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703660207585/3229cbe8-c289-474c-bf13-49ef64e8e773.png

---

### Introduction

Welcome to Day 22 of our Kubernetes journey! Today, we delve into a crucial concept: “Stateful Applications.” These are the backbone of data-intensive and mission-critical services. Let’s explore what they are and how they differ from their stateless counterparts.

### **What are the Stateful applications?**

Stateful applications are those applications that contain the previous states or data of the applications. If the applications, restart or move to other environments then, the data and the state of the applications still exist.

Examples of Stateful applications includes PostgreSQL, MySQL, messages queues. These applications ensures that the data, application reliability, and state of the applications will be taken care of by the StatefulSets Object in Kubernetes.

### **Difference between Stateful and Stateless applications.**

Let’s understand the Stateful and Stateless applications with real-time examples.

**Stateful applications**

*   “Remember when we used to play GTA Vice City during our teen years? Completing that game required hours because we were in school at the time.”
*   “Once I completed a mission, I saved the game. So, whenever I wanted to continue, I just went to the save game section and resumed from where I left off.”
*   “At that moment, we didn’t think about how it worked, but now we understand that there were sessions that helped us save the game data, which was stored in our root folder.”

**Stateless applications**

*   “If you used old keypad mobiles, you might have used a calculator application.”
*   “Your father asked you to perform operations like addition and subtraction.”
*   “By mistake, you tapped the red button, which took you back to the home screen.”
*   “Now, you can’t retrieve the numbers you were entering. This means there is no session involved.”

### **StatefulSets VS Deployment**

Kubernetes has rich features like StatefulSets and deployment. But Statefulsets eliminates the previous states and data stored problems.

Let’s understand both.

**StatefulSets**

*   **Identity and Stable Network hostnames**: StatefulSets are used for those applications that require stable network identity and hostnames. Whenever the pod is created, it gets a unique name, an ordinal index appended to its name. For example, the pod name looks like web-1, web-2, web-3, and so on.
*   **Order deployment and Scaling**: StatefulSets deploy the pods in sequential order. If you observe in deployment, the multiple pods in replicas are created at one time. But In StatefulSets, each pod will be created once the previous pod is created. If pods are deleted, the newest created pod will be deleted first which means, that to delete the pod StatefulSets follows the reverse order.
*   **Data Persistence**: StatefulSets are used for those applications that require data persistence such as databases. StatefulSets allow to attachment and mount of the permanent volumes to the disk. So, if any pod is rescheduled or restarted it will have the all data.
*   **Headless Services**: StatefulSets have one more rich feature which is Headless Services. StatefulSets can be associated with the headless services that provide the DNS for each pod’s hostnames. This will help to communicate with the specific pods.

**Deployment**

*   **Scalability and Rolling updates**: Deployments are often used for stateless applications. Deployments provide the replicas and no downtime feature.
*   **No Stable Hostnames**: Deployments do not provide the feature of stable hostnames which means, that if the pod is created then it will have a randomly generated name.
*   **No Data Persistence**: As deployment objects are often used for stateless applications So, pods are stateless too which means it does not provide data persistence. Whenever a pod is replaced, the data will be lost of the previous pod.
*   **Load Balancing**: Deployment works with Services objects which provides Load Balancing. It distributes the traffic between multiple pods to make the application highly available.

### **Key Features and Facts of StatefulSets:**

1.  StatefulSets provides the Order pod creation which provides the unique name to each pod in a predictable order such as web-1, web-2, and so on.
2.  StatefulSets provides the Stable Network Identity which makes it easy to connect with the pods.
3.  StatefulSets provides the Data Persistence which stores the data in the databases and whenever the pod is restarted or rescheduled, the pods get the same persistent data.
4.  StatefulSets provides the PV and PVC to provide the persistent storage to StatefulSets pods.
5.  StatefulSets provides the Backup and Recovery features which are very crucial for maintaining data integrity with StatefulSets.

### Hands-On

Open the two terminals

We have to see how the pods are created. To do that, run the below command on terminal1

kubectl get pods -w -l app=nginx

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660160677/5d6fb75e-4047-4deb-b664-5f711dd7275d.png)

Create a service file and StatefulSet file copy the below content in the respective file and apply it.

service.yml

apiVersion: v1  
kind: Service  
metadata:  
  name: nginx  
  labels:   
    app: nginx  
spec:  
  ports:  
  \- port: 80  
    name: web  
  clusterIP: None  
  selector:   
    app: nginx

StatefulSets.yml

apiVersion: apps/v1  
kind: StatefulSet  
metadata:  
  name: web  
spec:  
  serviceName: "nginx"  
  replicas: 2  
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
        image: registry.k8s.io/nginx-slim:0.8  
        ports:  
        \- containerPort: 80  
          name: web  
        volumeMounts:  
        \- name: www  
          mountPath: /usr/share/nginx/html  
  volumeClaimTemplates:  
  \- metadata:  
      name: www  
    spec:  
      accessModes: \[ "ReadWriteOnce" \]  
      resources:  
        requests:  
          storage: 1Gi

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660162417/2aeeac30-f338-4330-9352-623cb152a100.png)

The Pods are creating sequential, observations in the first command output.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660164160/c5d8778a-1538-4982-9619-d2a5e0b34431.png)

As we have created pods from statefulset, So the pod's names have sticky and unique names.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660165800/910d0f12-d81b-46e5-85b8-8c6ef4b4192b.png)

If you run the below command, you will see that both pods have stable hostnames on their ordinal index.

for i in 0 1; do kubectl exec “web-$i” — sh -c ‘hostname’; done

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660167289/f25324cd-a402-4984-84e9-10c7e10b5d36.png)

Now, I want to check the pod’s dns addresses. Run the below command

kubectl run -i — tty — image busybox:1.28 dns-test — restart=Never — rm

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660168607/c7026c2e-3320-4adc-a75e-1e171ee04dcf.png)

Once you enter the container, run the below command.

nslookup web-0.nginx

Now, you can check the dns address.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660170020/4d0f1edc-3f1a-4f71-b3d0-4bf7925d7177.png)

Open two terminals, run the below command on terminal1

kubectl get pod -w -l app=nginx

As you can see both pods are running.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660171300/6a16f422-8092-4b79-9228-0ac8b978b241.png)

Now, run the below command to delete both pods on terminal 2.

kubectl delete pod -l app=nginx

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660172999/7f0e4539-983a-430d-b71c-20c55cbb4203.png)

As you know, we have set up the replicas=2. So, if we delete any pod it will create a new pod to meet with the desired state of the replicas.

But there is one more thing to notice here, that the pod is deleting sequentially.

If you run the last command, you will see that both pods have been created again.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660174517/4a84cacc-8356-4df0-b41c-3be58d4d57d7.png)

If you run the below command, you will see the same hostnames that were present for previous delete pods.

for i in 0 1; do kubectl exec web-$i — sh -c ‘hostname’; done

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660176118/fbfe21fd-adae-4a59-bfb9-c7d0e3cfeda0.png)

Now, if you log in to the container. You will be able to see the same dns address but the IP might have changed.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660177785/4a66d5ad-2c9c-40c6-8bb1-050d5694e52c.png)

Run the command to get a list of all the persistent volume claims that are attached to the app nginx.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660179573/50c662df-6279-4de0-9d2a-06530478faa1.png)

As we have mounted our persistent volume to the path /usr/share/nginx/html in the Stateful.yml file. So, the path will be backed by the Persistent Volume.

Now, to check the hostnames of both pods run the below command.

for i in 0 1; do kubectl exec "web-$i" - sh -c 'echo "$(hostname)" > /usr/share/nginx/html/index.html'; done  
for i in 0 1; do kubectl exec -i -t "web-$i" - curl http://localhost/; done

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660180838/9ae2f8f2-3e7d-469c-bcdc-946e54e72329.png)

Delete both pods

kubectl delete pod -l app=nginx

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660182265/18b0ca52-259d-48f0-bd83-4a985cbb5039.png)

Validate the pods' deletion and creation on the other window

kubectl get pod -w -l app=nginx

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660183798/5ade3792-7309-44fe-b1d6-1dba1b5de986.png)

Now, validate the web servers whether the hostname is the same or not

for i in 0 1; do kubectl exec -i -t “web-$i” — curl http://localhost/; done

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660185338/c3e9be35-6a6e-4551-97c5-bca2f21bbac3.png)

Now, let’s scale up the StatefulSets. You can use the *kubectl scale* or *kubectl patch* to scale up or scale down the replicas.

kubectl scale sts web — replicas=5

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660186554/e1a84053-4111-4025-9918-8e1851f7a41c.png)

If you see the persistent volume claim, it will be increased as pods are created.

kubectl get pvc -l app=nginx

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660188254/1a4a7f3e-7e18-413a-bad7-75d17c98595f.png)

Now, scale down the number of replicas through the kubectl patch

kubectl patch sts web -p ‘{“spec”:{“replicas”:3}}’

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660189493/e227c540-4173-4705-a3a9-29acaea98003.png)

If you list the Persistent volume claims, you will see all 5 PVCs present. This is because StatefulSets assumes that you deleted the pods by mistake.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660190696/b45aca42-dbc8-4630-b9a9-aeeaad58e38a.png)

Now, to rollback use the below command

kubectl patch statefulset web -p ‘{“spec”:{“updateStrategy”:{“type”:”RollingUpdate”}}}’

The below command will change the image of the container.

kubectl patch statefulset web — type\=’json’ -p=’\[{“op”: “replace”, “path”: “/spec/template/spec/containers/0/image”, “value”:”gcr.io/google\_containers/nginx-slim:0.8"}\]’

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660192137/b24b8954-badc-4d30-ac61-fbfe2c5414a6.png)

Use the below command to check the container image

for p in 0 1 2; do kubectl get pod “web-$p” — template ‘{{range $i, $c := .spec.containers}}{{$c.image}}{{end}}’; echo; done

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660193490/11aa69d9-a327-421f-8447-9508754964af.png)

If you want to delete StatefulSets then you have two options Non-Cascading deletion and Cascading deletion.

In Non Cascading deletion, the StatefulSet’s Pods are not deleted when StatefulSets is deleted.

In Cascading deletion, both the StatefulSet’s Pod and StatefulSets are deleted.

**Non-Cascading deleted**

Open the two terminals and run the below command on the first terminal.

kubectl get pods -w -l app=nginx

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660195108/fb8d8670-513a-403b-b7fd-df8a5a188a9a.png)

On the second terminal, this command will delete only StatefulSets only, not the StatefulSets pods.

kubectl delete statefulset web — cascade=orphan

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660196784/62de07bc-bd20-4c35-b264-054cc55611ab.png)

If you go to the first terminal, you will see the pods still exist.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660197938/f3714b87-7c53-41be-831b-387a34865593.png)

If you try to delete any pod, you will observe that the deleted pods are not relaunching.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660199448/49c3dd9f-2549-438a-a5ab-34cf423707e6.png)

Now, we have to perform the StatefulSets deletion through the Cascading method.

To do that, apply the both service and StatefulSets files.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660201042/4f424f56-9f16-475d-b1a6-a78dbbcf7a0c.png)

If you see both pods are running

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660202376/14bf5703-29bc-4004-b672-2c49ede4be99.png)

Check the hostname of both pods by running the below command

for i in 0 1; do kubectl exec -i -t “web-$i” — curl http://localhost/; done

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660203719/f7b0608c-087c-4d76-9d64-e33e30f17801.png)

**Cascading deleted**

In this method, the StatefulSets will be deleted with StatefulSet’s Pod.

kubectl delete statefulset web

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660205276/f9588cdf-a9a4-418e-a24e-12be0db88be9.png)

### **Conclusion**

Today, we’ve uncovered the essence of stateful applications and their counterparts, stateless applications. We’ve explored the critical differences and how Kubernetes leverages StatefulSets to manage these stateful applications with care and precision.

As we continue our Kubernetes adventure, we’ll keep delving deeper into essential concepts and hands-on experiences. Stay tuned for more exciting learnings ahead!

### Want to Know About Challenge?

If you’re eager to learn more and join our challenge through the GitHub Repository, stay tuned for the upcoming posts. Follow for more exciting insights into the world of Kubernetes!

**GitHub Repository**: [https://github.com/AmanPathak-DevOps/30DaysOfKubernetes](https://github.com/AmanPathak-DevOps/30DaysOfKubernetes)

#Kubernetes #IngressController #Ingress #ContainerOrchestration #DevOps #K8sLearning

See you on Day **22** as we unravel more Kubernetes mysteries!

Stay connected on **LinkedIn**: [LinkedIn Profile](https://www.linkedin.com/in/aman-devops/)

Stay up-to-date with **GitHub**: [GitHub Profile](https://github.com/AmanPathak-DevOps)

Feel free to reach out to me, if you have any other queries.

Happy Learning