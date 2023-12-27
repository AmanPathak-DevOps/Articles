---
title: "Day14- ConfigMaps & Secrets"
datePublished: Mon Oct 16 2023 15:56:26 GMT+0000 (Coordinated Universal Time)
cuid: clqnfbqww000n08l155an4coq
slug: day14-configmaps-secrets-1abb910aeb53
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703660248099/3eb038c7-7a03-4079-bc36-fbf691c8da86.png

---

Welcome to Day 14 of our #30DaysOfKubernetes journey! Today, we delve into the world of ConfigMaps and Secrets, two essential components for managing the configuration and sensitive data within your Kubernetes environment. These tools allow you to decouple configuration from your application, ensuring flexibility, security, and smooth operations. Let’s explore ConfigMaps and Secrets and understand how to create and use them effectively.

### ConfigMap

ConfigMap is used to store the configuration data in key-value pairs within Kubernetes. This is one of the ways to decouple the configuration from the application to get rid of hardcoded values. Also, if you observe some important values keep changing according to the environments such as development, testing, production, etc ConfigMap helps to fix this issue to decouple the configurations. ConfigMap stores non-confidential data. ConfigMap can be created through an imperative or declarative way.

*   Creating the configMap is the first process which can be done by commands only or a YAML file.
*   After creating the configMap, we use the data in the pod by injecting the pods.
*   After injecting the pods, if there is any update in the configuration we can modify the configMap, and the changes will be reflected in the injected pod.

### Secrets

There are lot of confidential information that needs to be stored on the server such as database usernames, passwords, or API Keys. To keep all the important data secure, Kubernetes has a Secrets feature that encrypts the data. Secrets can store data up to 1MB which would be enough. Secrets can be created via imperative or declarative ways. Secrets are stored in the /tmps directory and can be accessible to pods only.

*   Creating the Secrets is the first process that can be created by commands or a YAML file.
*   After creating the Secrets, applications need to use the credentials or database credentials which will be done by injecting with the pods.

### ConfigMap HandsOn

Creating ConfigMap **from literal**

In this snippet, we have created the configMap through — from-literal which means you just need to provide the key value instead of providing the file with key-value pair data.

At the bottom, you can see the data that we have created through the Literal.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660214187/ddccc27f-d6b8-48ec-b5c0-cebeae470d06.png)

CM **from file**

In this snippet, We have created one file first.conf which has some data, and created the configMap with the help of that file.

At the bottom, you can see the data that we have created through the file.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660215809/ab5280f5-a755-4c93-a4e8-d1db87272c38.png)

CM **from the env file**

In this snippet, We have created one environment file first.env which has some data in key-value pairs, and created the configMap with the help of the environment file.

At the bottom, you can see the data that we have created through the env file.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660217342/66f58895-debe-4dc8-b636-4a96eab107a4.png)

What if you have to create configMap for tons of files?

In this snippet, We have created multiple files in a directory with different extensions that have different types of data and created the configMap for the entire directory.

At the bottom, you can see the data that we have created for the entire directory.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660219073/f2bc971b-407d-4703-b941-89c74986a698.png)

CM **from the YAML file**

The imperative way is not very good if you have to repeat the same tasks again and again. Now, we will look at how to create configMap through the YAML file.

In this snippet, We have created one file and run the command with — from-file, and in the end, we add -o yaml which generates the YAML file. You can copy that YAML file modify it according to your key-value pairs and apply the file.

At the bottom, you can see the data that we have created through the YAML file.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660220986/a2b41d6f-b974-43bc-bcb9-ca1a5a5d517d.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660222519/a8fd19bf-bd88-4beb-bbb0-f0218edf758d.png)

In the above steps, we have created four types of configMaps but here, we will learn how to use those configMaps by injecting configMaps into the pods.

**Injecting CM into the pod with specific key pairs**

In this snippet, We have created a YAML file in which we have mentioned the configMap name with the key name which will be added to the pod’s environment variable.

apiVersion: v1  
kind: Pod  
metadata:  
  name: firstpod  
spec:   
  containers:  
    \- image: coolgourav147/nginx-custom  
      name: firstcontainer  
      imagePullPolicy: Never  
      env:  
       \- name: valuefromenv  
         valueFrom:  
           configMapKeyRef:  
             key: Subject2  
             name: cm-from-env

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660223929/6ce0f158-61d4-4e1b-a25e-432f43ae9f77.png)

At the bottom, you can see the configuration in the pod’s environment.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660225403/5f1b555a-8ba5-44f8-a6c9-17e7223fffd0.png)

**Injecting multiple CMs with specific and multiple values**

In this snippet, we have added multiple key pairs from different files.

At the bottom, you can see the data that is from different files.

apiVersion: v1  
kind: Pod  
metadata:  
  name: firstpod  
spec:   
  containers:  
    \- image: coolgourav147/nginx-custom  
      name: firstcontainer  
      imagePullPolicy: Never  
      env:  
       \- name: valuefromenv  
         valueFrom:  
           configMapKeyRef:  
             key: Subject2  
             name: cm-from-env  
       \- name: valuefromenv2  
         valueFrom:  
           configMapKeyRef:  
             key: env.sh  
             name: cm2  
       \- name: valuefromenv3  
         valueFrom:  
           configMapKeyRef:  
             key: Subject4  
             name: cm-from-env

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660226988/4daa10a5-5db7-446b-9314-7fb265d9f028.png)

**Injecting the created environment file single cms and getting all the value**

apiVersion: v1  
kind: Pod  
metadata:  
  name: firstpod  
spec:   
  containers:  
    \- image: coolgourav147/nginx-custom  
      name: firstcontainer  
      imagePullPolicy: Never  
      envFrom:  
       \- configMapRef:  
           name: cm-from-env

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660228739/9d5fd90c-74ba-4774-9006-736f266b5d5d.png)

**Injecting cm in the pod with the entire proper file**

apiVersion: v1  
kind: Pod  
metadata:  
  name: firstpod  
spec:   
  containers:  
    \- image: coolgourav147/nginx-custom  
      name: firstcontainer  
      imagePullPolicy: Never  
      volumeMounts:  
      \- name: test  
        mountPath: "/env-values"  
        readOnly: true    
  volumes:  
    \- name: test  
      configMap:  
            name: cm-from-env

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660230214/3751baf4-b252-4cf8-a73e-2b3518681d31.png)

**Injecting cm and creating a file in the pod with the selected key pairs**

apiVersion: v1  
kind: Pod  
metadata:  
  name: firstpod  
spec:   
  containers:  
    \- image: coolgourav147/nginx-custom  
      name: firstcontainer  
      imagePullPolicy: Never  
      volumeMounts:  
      \- name: test  
        mountPath: "/env-values"  
        readOnly: true    
  volumes:  
    \- name: test  
      configMap:  
            name: cm-from-env  
            items:  
             \- key: Subject3  
               path: "topic3"  
             \- key: Subject5  
               path: "topic5"

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660231988/1189b4af-6c95-4f8e-84d4-ac8629c346dc.png)

### Secrets HandsOn

Creating Secrets **from literal**

In this snippet, we have created the Secrets through — from-literal which means you just need to provide the key value instead of providing the file with key-value pair data.

At the bottom, you can see the key and encrypted value because Kubernetes encrypts the secrets.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660233461/d9f6f9fb-6f8c-4155-8957-c8da9ea234ea.png)

Secrets **from file**

In this snippet, We have created one file first.conf which has some data, and created the Secrets with the help of that file.

At the bottom, you can see the encrypted data..

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660235467/1e76ec58-4495-42a4-9074-ee402d2ff662.png)

Secrets from the **env file**

In this snippet, We have created one environment file first.env which has some data in key-value pairs, and created the Secrets with the help of the environment file.

At the bottom, you can see the encrypted data.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660237105/457d2956-1a6a-4ae7-a6b0-cc9db5b5c618.png)

What if you have to create Secrets for tons of files?

In this snippet, We have created multiple files in a directory with a different extension that has different types of data and created the Secrets for the entire directory.

At the bottom, you can see the encrypted data.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660238918/587eb4c2-c51b-4457-af63-82a8f75c493d.png)

CM from the **YAML file**

In this snippet, We have created one file and run the command with — from-file, and in the end, we add -o yaml which generates the YAML file. You can copy that YAML file modify it according to your key-value pairs and apply the file.

At the bottom, you can see the encrypted data.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660240445/0e531a99-d3b8-4cd8-8bc1-f3cbe425349d.png)

In the above steps, we have created four types of Secrets but here, we will learn how to use those Secrets by injecting Secrets to the pods.

**Injecting Secret with a pod for a particular key pairs**

In this snippet, We have created a YAML file in which we have mentioned the Secrets name with the key name which will be added to the pod’s environment variable.

apiVersion: v1  
kind: Pod  
metadata:  
  name: secret-pod  
spec:   
  containers:  
    \- image: coolgourav147/nginx-custom  
      name: firstcontainer  
      imagePullPolicy: Never  
      env:  
        \- name: the-variable  
          valueFrom:  
            secretKeyRef:  
              key: Subject1  
              name: third

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660242031/451ddc20-8779-4ada-96bb-7ec83da16604.png)

**Injecting the created environment file single cms and getting all the value**

apiVersion: v1  
kind: Pod  
metadata:  
  name: secret-pod  
spec:   
  containers:  
    \- image: coolgourav147/nginx-custom  
      name: firstcontainer  
      imagePullPolicy: Never  
      volumeMounts:  
        \- name: test  
          mountPath: "/secrets-values"  
  volumes:  
    \- name: test  
      secret:  
            secretName: seonc

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660243759/c9a52df8-a9db-46ea-9bbe-f44e14547add.png)

**Injecting Secrets and creating a file in the pod with the selected key pairs**

apiVersion: v1  
kind: Pod  
metadata:  
  name: secret-pod  
spec:   
  containers:  
    \- image: coolgourav147/nginx-custom  
      name: firstcontainer  
      imagePullPolicy: Never  
      volumeMounts:  
        \- name: test  
          mountPath: "/secrets-values"  
  volumes:  
    \- name: test  
      secret:  
            secretName: third  
            items:  
             \- key: Subject3  
               path: "topic3"  
             \- key: Subject5  
               path: "topic5"

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660245204/ed5216c0-0979-4b95-872e-7788ce4cb485.png)

**Conclusion**  
In this blog, we’ve learned the significance of ConfigMaps and Secrets in Kubernetes. ConfigMaps helps manage configuration data, while Secrets provides a secure way to handle sensitive information. We’ve explored various methods to create ConfigMaps and Secrets, both imperatively and declaratively. Additionally, we’ve seen how to inject these configurations into pods, making them accessible to your applications. By mastering ConfigMaps and Secrets, you’ll enhance the flexibility and security of your Kubernetes deployments. Stay tuned for more Kubernetes insights on our #30DaysOfKubernetes journey!