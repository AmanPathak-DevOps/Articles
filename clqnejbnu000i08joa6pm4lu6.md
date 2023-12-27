---
title: "Day18- Kubernetes Resource Quota and Namespace"
datePublished: Sat Oct 21 2023 05:29:44 GMT+0000 (Coordinated Universal Time)
cuid: clqnejbnu000i08joa6pm4lu6
slug: day18-kubernetes-resource-quota-and-namespace-6a21045b0d97
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703658921577/65a5a5b3-f435-4c34-8f63-e87492de7029.png

---

Welcome to Day 18 of our #30DaysOfKubernetes journey! In the ever-evolving world of Kubernetes, managing resources and namespaces is crucial to keep your applications running smoothly. Today, we delve into the powerful tools at your disposal: Resource Quotas and namespaces.

### Namespace

Image taken from [https://stacksimplify.com/azure-aks/azure-kubernetes-service-namespaces-imperative/](https://stacksimplify.com/azure-aks/azure-kubernetes-service-namespaces-imperative/)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658895712/7130a1d5-c517-4aa9-a3e0-79e166b4ac0a.png)

*   A namespace is a logical entity that is used to organize the Kubernetes Cluster into virtual sub-clusters.
*   The namespace is used when an organization shares the same Kubernetes cluster for multiple projects.
*   There can be any number of namespaces can be created inside the Kubernetes cluster.
*   Nodes and Kubernetes Volumes do not come under the namespaces and are visible to every namespace.

**Pre-existed Kubernetes namespaces**

*   default: As the name suggests, whenever we create any Kubernetes object such as pod, replicas, etc it will create in the **default** namespace. If you want to create the particular pod in the different namespace you have to create the namespace and while creating the pod you have to mention the namespace(Will do in handsOn).
*   kube-system: This namespace contains the Kubernetes components such as kube-controller-manager, kube-scheduler, kube-dns or other controllers. This namespace must be avoided to create pods or other objects if you want to keep the Kubernetes cluster stable.
*   kube-public: This namespace is used to share non-sensitive information that can be viewed by any of the members who are part of the Kubernetes cluster.

**When should we consider Kubernetes Namespaces?**

*   Isolation: When there are multiple numbers of projects running then we can consider making namespaces and put the projects accordingly in the namespaces.
*   Organization: If there is only one Kubernetes Cluster which has different environments to keep isolated. If something happens to the particular environment then it won’t affect the other environments.
*   Permission: If some objects are confidential and must need access to the particular persons then, Kubernetes provides RBAC roles as well which we can use in the namespace. It means that only authorized users can access the objects within the namespaces.

### HandsOn

If you observe the first command, while running the command ‘kubectl get pods’ you are getting ‘No resources found in default namespace’ which means that we are trying to list the pods from the default namespace.

If you observe the second command ‘kubectl get namespaces’ which is used to list all the namespaces.

The **default** one is created for pods and others for Kubernetes itself which we don’t use for ourselves.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658897353/bbf45238-47f0-40f0-a357-8b11888d4332.png)

In this snippet, we have created one new namespace named tech-mahindra and you can validate whether the namespace is created or not by running ‘kubectl get namespaces’ command.

apiVersion: v1  
kind: Namespace  
metadata:  
  name: tech-mahindra  
  labels:  
    name: tech-mahindra

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658898985/d960406b-3041-422d-bcd4-98662e890f71.png)

In this snippet, we have created one pod in a namespace that we created in the previous step(tech-mahindra). If you want to list the pods from the other namespace except the default namespace then you have to mention the namespace name by following -n <namespace-name>.

apiVersion: v1  
kind: Pod  
metadata:  
  name: ns-demo-pod  
spec:  
  containers:  
  \- name: container1  
    image: ubuntu  
    command: \["bin/bash", "-c", "while true; do echo We are on 18th Day of 30DaysOfKubernetes; sleep 30; done"\]  
  restartPolicy: Never

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658900809/99f2a128-968c-4a0b-9450-51712c836583.png)

If you want to delete the pod from the different namespace except default then you have to mention the namespace otherwise it will throw the error.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658902509/852f62c5-68cb-42da-b063-13466382901e.png)

If you want to set your namespace as the default namespace, you can use the command ‘*kubectl config set-context $(kubectl config current-context) — namespace <namespace-name>*’’ and if you want to see the default namespace use the command ‘*kubectl config view | grep namespace*’

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658903900/447b6a30-c5eb-4b29-9acf-9fcc7b5f500b.png)

### ResourceQuota

Image taken from [https://www.linkedin.com/pulse/kubernetes-resource-quota-densify/](https://www.linkedin.com/pulse/kubernetes-resource-quota-densify/)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658905558/60a87379-b96b-4e06-93a1-414d549a01b5.png)

ResourceQuota is one of the rich features of Kubernetes that helps to manage and distribute resources according to the requirements.

Suppose in an Organization, two teams share the same Kubernetes Cluster where the first team needs more CPUs and Memory because of heavy workload tasks. Now, in this situation, if the CPUs and other resources go to another team then it might increase the chance of failure. To resolve this issue, we can allocate particular CPU cores and Memory to every project.

*   A pod in Kubernetes will run with no limits on CPU and memory by default.
*   You can specify the RAM, Memory, or CPUs for each container and pod.
*   The scheduler decides which node will create pods, if the node has enough CPU resources available then, the node will place the pods.
*   CPU is specified in units of cores and memory is specified in units of bytes.
*   As you know, the Kubernetes cluster can be divided into namespaces and if a container is created in a namespace that has a default CPU limit and container does not specify the CPU limit then the container will have the default CPI limit.
*   A namespace can be assigned to ResourceQuota objects, this will help to limit the amount of usage to the objects within the namespaces. You can limit the computer (CPU), Memory, and Storage.
*   Restrictions that a resource-quotas imposes on namespaces
*   Every container that is running on the namespace must have its own CPU limit.
*   The total amount of CPU used by all the containers in the namespace should not exceed a specified limit.

**There are two types of constraints that need to be mentioned while using ResourceQuota:**

*   **Limit**: Limit specifies that the container, pod, or namespace will have the limit resources where if the objects will exceed the limit then, the object won’t create.
*   **Request**: The request specifies that the container, pod, or namespace needs a particular amount of resources such as CPU and memory. But if the request is greater than the limit then, Kubernetes won’t allow the creation of pods or containers.

Now, there are some conditions or principles for requests and limit which needs to be understood.

**Let’s understand with hands-on and theoretical.**

1.  If the requests and limits are given in the manifest file, it works accordingly.
2.  If the requests are given but the limit is not provided then, the default limit will be used.
3.  If the requests are not provided but the limit is provided then, the requests will be equal to the limit.

**When booth requests and limits have been mentioned for a pod then it will create the pod according to the provided resources**

apiVersion: v1  
kind: Pod  
metadata:  
  name: resources  
spec:   
  containers:  
  \- image: ubuntu  
    name: res-pod  
    command: \["bin/bash", "-c", "while true; do echo We are on 18th Day of 30DaysOfKubernetes; sleep 30; done"\]  
    resources:  
      requests:   
        memory: "32Mi"  
        cpu: "200m"  
      limits:  
        memory: "64Mi"  
        cpu: "400m"

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658907652/e55aa6e8-1a5e-47e0-bfc7-d408320a91c7.png)

**Creating ResourceQuota**

apiVersion: v1  
kind: ResourceQuota  
metadata:  
  name: res-quota  
spec:  
  hard:   
    limits.cpu: "200m"  
    requests.cpu: "150m"  
    limits.memory: "38Mi"  
    requests.memory: "12Mi"

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658909158/314c675f-1c0e-428d-b0f6-79723171ebd3.png)

**Creating deployment but the pod is not creating due to out of limit**

apiVersion: apps/v1  
kind: Deployment  
metadata:  
  name: rq-deployments  
spec:   
  replicas: 4  
  selector:  
    matchLabels:  
      objtype: rq-deployments  
  template:  
    metadata:  
      name: rq-pod  
      labels:  
        objtype: rq-deployments  
    spec:  
      containers:  
      \- name: rq-cont  
        image: ubuntu  
        command: \["bin/bash", "-c", "while true; do echo We are on 18th Day of 30DaysOfKubernetes; sleep 30; done"\]  
        resources:  
          requests:   
            cpu: "50m"

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658910620/541cb8ef-7678-40f9-b6b2-683c8bb10c71.png)

**Error logs for the above work**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658912464/bdcafe57-d76e-4c82-a535-0f6320ee2a44.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658914443/d24148c7-ff44-479c-90db-4e64fb0cb65d.png)

**When I did not provide requests**

In this snippet, we have created a limit range where we have set the default value for CPU as 1 and from that 1 cpu we are requesting 0.5 CPU only to create a container, if the container needs more than 0.5 CPU then due to the limit range it won’t happen

apiVersion: v1  
kind: LimitRange  
metadata:  
  name: cpulimitrange  
spec:   
  limits:  
  \- default:  
      cpu: 1  
    defaultRequest:  
      cpu: 0.5  
    type: Container

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658915810/4dabe11f-df71-4e88-8bc9-e19c3b8623c6.png)

**When I did not provide the limit**

apiVersion: v1  
kind: Pod  
metadata:  
  name: no\-request-demo  
spec:   
  containers:  
  \- name: container1  
    image: ubuntu  
    resources:  
      limits:   
        cpu: "2"

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658917629/1d1afe60-7012-4638-b80f-6042e1103dd9.png)

**When I did not provide limits**

apiVersion: v1  
kind: Pod  
metadata:  
  name: default-cpu-demo-3  
spec:  
  containers:  
  \- name: default-cpu-demo-3-ctr  
    image: nginx  
    resources:  
      requests:  
        cpu: "0.75"

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658919179/203cd71f-f1fc-4688-9eb2-a1080f89965c.png)

### Conclusion

Resource Quotas and namespaces are your allies in maintaining a well-organized and well-behaved Kubernetes cluster. They provide the structure and resource control necessary to keep your projects running smoothly. Embrace them, and watch your Kubernetes journey flourish! 🚀💡🔒

### Want to Know About Challenge?

If you’re eager to learn more and join our challenge through the GitHub Repository, stay tuned for the upcoming posts. Follow for more exciting insights into the world of Kubernetes!

**GitHub Repository**: [https://github.com/AmanPathak-DevOps/30DaysOfKubernetes](https://github.com/AmanPathak-DevOps/30DaysOfKubernetes)

#Kubernetes #ReplicationController #ReplicaSet #ContainerOrchestration #DevOps #K8sLearning

See you on Day **19** as we unravel more Kubernetes mysteries!

Stay connected on **LinkedIn**: [LinkedIn Profile](https://www.linkedin.com/in/aman-devops/)

Stay up-to-date with **GitHub**: [GitHub Profile](https://github.com/AmanPathak-DevOps)

Feel free to reach out to me, if you have any other queries.

Happy Learning