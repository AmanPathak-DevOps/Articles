---
title: "Day 25- Kubernetes Operators"
datePublished: Fri Nov 24 2023 04:10:04 GMT+0000 (Coordinated Universal Time)
cuid: clqneg7km000b08l6aam24ve2
slug: day-25-kubernetes-operators-d67c8c2ad644
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703658777122/0a7d7dfa-6313-4c9d-903d-a3f87d5a7eaf.png

---

### Introduction

Welcome to Day 25 of our #30DaysOfKubernetes journey! 🚀 Today, we’re delving into the fascinating world of Kubernetes Operators.

### What are Operators?

Kubernetes Operator is a method for packaging or bundling, deploying, and managing the application by extending the functionality of the Kubernetes API.

The above definition is One and a half line but if you deep dive into that line you will see there are a lot of things which is followed to write this definition. Let’s try to understand it.

There are two types of applications Stateless and Stateful Applications.

**Stateless applications** are those applications where the data or persisting data is not the priority. So, if the pods get restarted then it will lose the data but it won’t bother us because we already knew that our application is Stateless.

But **Stateful applications** are those applications where the data is very important and we have to keep our data persistent whether its in Persistent Volume or somewhere else. But whenever the pod is replaced or restarted then the pod will lose the data which is not what we want. To get rid of the data loss, we will use Stateful Applications.

First of all, this is only one usage of the Operators. There will be more usages of the Kubernetes Operators which, we will discuss later in this blog.

Kubernetes Operators have rich features that help to deploy stateless, stateful, complex, or custom applications.

### Features of Operators:

1.  **Custom Resource Definitions(CRD)**: With the help of Operators, you can define your CRD to extend the capability of your Kubernetes for a particular application. We will discuss CRD in a detailed way later.

**Example**: Prometheus is one of the finest Operators, in which there is a Custom Resource known as Prometheus which allows users to define the monitor configuration declaratively. You can specify the details like alerting rules, service monitors, etc using the custom resource.

**2\. Custom Controllers**: There is no use of Custom resource if the Custom Controller is not present. Operators implement custom controllers to watch and reconcile(correct) the state of the custom resource and ensure that the desired state matches the actual state.

**Example**: In Native Kubernetes, etcd is one of the components that take care of the current and desired state thing and other things like stability of the Kubernetes Cluster and node failures, etc.

**3\. Automated Operations**: Operators automate routine tasks which makes it easier to manage the complex applications over their lifecycle.

**Example**: In the MongoDB operator, if the users increase the number of replicas in the custom resource then, the Operator automatically adjusts the replicas to seamless scaling.

**4\. Operational Policies**: Operators consistently enforce the operational policies to keep the environment secure across instances of application.

**Example**: The Vault operators enforce the security policies over HashiCorp Vault which ensures that the sensitive data will be secured.

**5\. Rolling Updates and Upgrades**: Operators manage the rolling updates and upgrades of the application. So that, there will be no downtime.

Example: CockroachDB operator handles the rolling updates on the CockroachDB cluster one node at a time to follow the no downtime protocol during the upgrading hours..

**6\. Integrate with Ecosystem tools**: Operators integrate with other Kubernetes tools to provide better functionality for the application.

**Example**: Prometheus Operator can be integrated with Grafana to provide complete monitoring.

**7\. Stateful Applications**: Operators help to manage complex or stateful applications by handling tasks like data persistent, disaster recovery, scaling, etc.

**Example**: Apache Kafka Operator manages Kafka clusters which automates tasks such as topic creation, partition reassignment, etc.

Kubernetes is one of the best container orchestration tools because of the Operator feature. If you observe, to leverage the rich feature you have to install operators like to get the benefit of the HPA(Horizontal Pod Autoscaler) feature you need to install an operator for it, and the same for Network Policy, etc.

So, without Operators, the life of a Kubernetes DevOps guy is not very easy.

Now this is enough to get a basic understanding of Kubernetes Operators. But there are some more things that you need to know.

1.  **Custom Resource Definition**

CRD is indeed one of the powerful features of Kubernetes, acting like a superpower that lets you define and use custom resources tailored to your specific needs. Let’s put it in a scenario:

Imagine you have a fantastic application ready to roll, but Kubernetes, as amazing as it is, might not have all the necessary tools and features to handle the uniqueness of your application. Here’s where CRD steps in as your superhero sidekick.

In straightforward terms, CRD allows you to create your very own resource types in Kubernetes. It’s like getting a custom tool for your specific job. But, of course, there’s a twist — your CRD needs the Kubernetes seal of approval. Think of it as a passport check; once your CRD gets the nod from Kubernetes, it becomes a certified member of the Kubernetes family.

Now, here’s the exciting part. Once you’ve crafted your CRD to match your application’s needs, you can share it with the world! Just like posting your creation on a hub for others to benefit. In the Kubernetes world, this hub is known as operatorshub.io. It’s a place where your CRD can shine, offering its capabilities to others who might have similar challenges.

So, in a nutshell, CRD empowers you to extend Kubernetes by creating your custom resources, and once validated, you can share your creations on operatorshub.io, contributing to the Kubernetes ecosystem and helping others tackle their unique challenges. It’s like giving your application its very own set of superpowers within the Kubernetes universe!

Custom Resource Definition can be created in YAML

**2\. Custom Controller**

Without a Custom Controller, there is no benefit to using Custom Resource Definition. Custom Controller is a dedicated created for the CRD. The main work of the Custom Controller is to meet the desired state of the custom resources with the actual state such as replicates. Also, Custom Controllers are like a watcher who watches and keep an eye on the custom resource. So, if there is any misshappening occurs then Custom Controllers fix it or take the necessary steps as part of the auto healing.

Custom Controller can be created in many languages like Go, Python, etc and there is one component client-go that is dedicated to Golang which has all the necessary tools for working with Kubernetes.

**3\. Custom Resource**

Once the Custom Controller is deployed, you have to think or you have already prepared the roadmap for how many namespaces you have to deploy your Custom Resource. It can be deployed on multiple worker nodes as per the requirement. Custom Resource is the last step in the process of creating and deploying the Custom Resources where Custom Controller is the second and CRD is the first step.

Let’s go through a sample **Custom Resource Definition**

A scenario could be managing a custom application called “AwesomeApp” that needs to maintain a specific number of replicas based on a defined metric. Here’s how the CRD might look:

\# File: awesomeapp-crd.yaml  
  
apiVersion: apiextensions.k8s.io/v1  
kind: CustomResourceDefinition  
metadata:  
  name: awesomeapps.app.example.com  
spec:  
  group: app.example.com  
  names:  
    kind: AwesomeApp  
    listKind: AwesomeAppList  
    plural: awesomeapps  
    singular: awesomeapp  
  scope: Namespaced  
  versions:  
    \- name: v1  
      served: true  
      storage: true  
  additionalPrinterColumns:  
    \- name: Replicas  
      type: integer  
      JSONPath: .spec.replicas

In this CRD, we define a custom resource named “AwesomeApp” belonging to the group “app.example.com.” It has a field for the number of replicas.

Let’s go through a sample **Custom Controller**:

Now, let’s create a simple Custom Controller in Go that watches for changes to our custom resource and takes action accordingly.

// File: main.go  
  
package main  
  
import (  
 "context"  
 "flag"  
 "fmt"  
 "time"  
  
 "k8s.io/client-go/kubernetes"  
 "k8s.io/client-go/tools/cache"  
 "k8s.io/client-go/tools/clientcmd"  
 "k8s.io/client-go/util/homedir"  
 "k8s.io/client-go/util/wait"  
 "k8s.io/client-go/util/workqueue"  
)  
  
func main() {  
 kubeconfig, \_ := getKubeconfig()  
  
 config, err := clientcmd.BuildConfigFromFlags("", kubeconfig)  
 if err != nil {  
  panic(err.Error())  
 }  
  
 clientset, err := kubernetes.NewForConfig(config)  
 if err != nil {  
  panic(err.Error())  
 }  
  
 informer := cache.NewSharedInformer(  
  cache.NewListWatchFromClient(  
   clientset.AppsV1().RESTClient(),  
   "awesomeapps",  
   "namespace-name",  
   cache.ResourceEventHandlerFuncs{  
    AddFunc: func(obj interface{}) {  
     fmt.Println("AwesomeApp created:", obj)  
     // Logic to handle the creation of AwesomeApp  
    },  
    UpdateFunc: func(oldObj, newObj interface{}) {  
     fmt.Println("AwesomeApp updated:", newObj)  
     // Logic to handle the update of AwesomeApp  
    },  
    DeleteFunc: func(obj interface{}) {  
     fmt.Println("AwesomeApp deleted:", obj)  
     // Logic to handle the deletion of AwesomeApp  
    },  
   },  
  ),  
  &v1.AwesomeApp{},  
  0, // no resync  
  cache.ResourceEventHandlerFuncs{},  
 )  
  
 stopCh := make(chan struct{})  
 defer close(stopCh)  
  
 go informer.Run(stopCh)  
  
 // Run forever  
 select {}  
}  
  
func getKubeconfig() (\*string, error) {  
 home := homedir.HomeDir()  
 kubeconfig := flag.String("kubeconfig", home+"/.kube/config", "absolute path to the kubeconfig file")  
 flag.Parse()  
 return kubeconfig, nil  
}

In this simplified example:

*   The controller watches for changes in AwesomeApp resources.
*   When an AwesomeApp resource is created, updated, or deleted, the corresponding handler functions are invoked.
*   You would extend this controller with your custom logic to manage the AwesomeApp replicas based on the specified metric.

### Conclusion

As we unravel the world of Kubernetes Operators, we recognize their pivotal role in orchestrating the diverse needs of stateless and stateful applications. With their ability to automate, enforce policies, and seamlessly integrate with the Kubernetes ecosystem, Operators elevate the capabilities of Kubernetes, making it a powerhouse for container orchestration.

Stay tuned for more exciting Kubernetes learnings on Day 26 of #30DaysOfKubernetes! 🚀🌐

### Want to Know About Challenge?

If you’re eager to learn more and join our challenge through the GitHub Repository, stay tuned for the upcoming posts. Follow for more exciting insights into the world of Kubernetes!

**GitHub Repository**: [https://github.com/AmanPathak-DevOps/30DaysOfKubernetes](https://github.com/AmanPathak-DevOps/30DaysOfKubernetes)

#Kubernetes #DaemonSet #StatefulSets #NetworkPolicy #Operators #ContainerOrchestration #DevOps #K8sLearning

See you on Day **26** as we unravel more Kubernetes mysteries!

Stay connected on **LinkedIn**: [LinkedIn Profile](https://www.linkedin.com/in/aman-devops/)

Stay up-to-date with **GitHub**: [GitHub Profile](https://github.com/AmanPathak-DevOps)

Feel free to reach out to me, if you have any other queries.

Happy Learning