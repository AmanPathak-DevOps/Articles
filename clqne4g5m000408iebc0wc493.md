---
title: "Day15- Kubernetes Jobs"
datePublished: Tue Oct 17 2023 16:23:42 GMT+0000 (Coordinated Universal Time)
cuid: clqne4g5m000408iebc0wc493
slug: day15-kubernetes-jobs-bd18f55cf1be
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703658228117/a7497db8-5cc1-4017-92f4-ce95cfec1bea.png

---

### **Intro**

Kubernetes Jobs: Orchestrating Work in the World of Containers

In the ever-evolving landscape of container orchestration, Kubernetes continues to offer robust solutions to manage containerized applications efficiently. Among its many resources, Kubernetes Jobs stand out as a powerful tool for executing tasks and maintaining workloads, providing a structured and controlled way to achieve specific goals. In this blog, we dive into the world of Kubernetes Jobs, exploring their use cases and key features. Whether it’s database backups, batch processes, scheduled tasks, or log rotation, Kubernetes Jobs offers a flexible and reliable solution for various workloads.

Kubernetes Jobs is a resource that is used to achieve a particular work such as a backup script and, once the work is completed the pod will be deleted.

**Use cases:**

*   Database backup script needs to run
*   Running batch processes
*   Running the task on the scheduled interval
*   Log Rotation

**Key-Features:**

*   **One-time Execution**: If you have a task that needs to be executed one time whether it’s succeed or fail then the job will be finished.
*   **Parallelism**: If you want to run multiple pods at the same time.
*   **Scheduling**: If you want to schedule a specific number of pods after a specific time.
*   **Restart Policy**: You can specify whether the Job should restart if fails.

**Let’s do some hands-on to get a better understanding of Kubernetes Jobs.**

**Work completed and pod deleted**

apiVersion: batch/v1  
kind: Job  
metadata:  
  name: testjob  
spec:  
  template:  
    metadata:  
      name: testjob  
    spec:   
      containers:  
        \- image: ubuntu  
          name: container1  
          command: \["bin/bash", "-c", "sudo apt update; sleep 30"\]  
      restartPolicy: Never

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658222744/df102a4a-0e6f-40e6-846b-0ba3b95eeecb.png)

**Create and run the pods simultaneously and delete once the work is completed.**

apiVersion: batch/v1  
kind: Job  
metadata:  
  name: testjob  
spec:  
  parallelism: 3 \# Create 3 pods and run simultaneously  
  activeDeadlineSeconds: 10 \# Pods will terminate after 40 secs(10+30(command sleep time))  
  template:  
    metadata:  
      name: testjob  
    spec:   
      containers:  
        \- image: ubuntu  
          name: container1  
          command: \["bin/bash", "-c", "sudo apt update; sleep 30"\]  
      restartPolicy: Never

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658224459/a0679410-74d6-4aed-8ca2-5b603d786c00.png)

**Scheduling a pod after each minute**

apiVersion: batch/v1  
kind: CronJob  
metadata:  
  name: testjob  
spec:  
  schedule: "\* \* \* \* \*"  
  jobTemplate:  
    spec:   
      template:  
        spec:  
          containers:  
            \- image: ubuntu  
              name: container1  
              command: \["bin/bash", "-c", "sudo apt update; sleep 30"\]  
          restartPolicy: Never

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658225779/d109aeb1-c344-4fce-9545-deb9e4ad723e.png)

### **Conclusion**

In the fast-paced world of modern application development, Kubernetes Jobs are an invaluable resource, enabling one-time execution, parallelism, scheduling, and restart policies. They empower us to accomplish tasks, ranging from critical database backups to routine batch processes, with confidence and control. As we’ve seen in our hands-on examples, Kubernetes Jobs offers a structured approach to managing workloads, ensuring that once the work is done, the pods are efficiently terminated. With this knowledge in hand, you’re well-equipped to harness the power of Kubernetes Jobs and streamline your containerized workloads, making your Kubernetes journey even more rewarding.

### Want to Know About Challenge?

If you’re eager to learn more and join our challenge through the GitHub Repository, stay tuned for the upcoming posts. Follow for more exciting insights into the world of Kubernetes!

**GitHub Repository**: [https://github.com/AmanPathak-DevOps/30DaysOfKubernetes](https://github.com/AmanPathak-DevOps/30DaysOfKubernetes)

#Kubernetes #LivenessProbe #Volume #ContainerOrchestration #DevOps #K8sLearning

See you on Day **16** as we unravel more Kubernetes mysteries!

Stay connected on **LinkedIn**: [LinkedIn Profile](https://www.linkedin.com/in/aman-devops/)

Stay up-to-date with **GitHub**: [GitHub Profile](https://github.com/AmanPathak-DevOps)

Feel free to reach out to me, if you have any other queries.

Happy Learning