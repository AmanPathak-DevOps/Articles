---
title: "Terraform Packer"
datePublished: Tue Jun 20 2023 15:46:49 GMT+0000 (Coordinated Universal Time)
cuid: clqne34r2000008jo4b9lhrla
slug: terraform-packer-5f56ec9bf303
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703658166305/e3e880da-7717-4a4b-b96e-a113b57adc7d.png

---

**The scenario of the Terraform Packer**

The prerequisites to learn **Terraform Packer** are:

*   Basic understanding of infrastructure and cloud concepts.
*   Knowledge of the target cloud platform’s services (e.g., AWS, Azure, Google Cloud).
*   Familiarity with writing infrastructure as code using HCL (HashiCorp Configuration Language).
*   Installed Terraform on your local machine.

**What is Packer**

Packer is an open-source tool developed by Hashicorp that helps to build or create images(AMI) of machines(VMs) or images(Docker).

Example-

If you want to create an Image on a cloud provider such as AWS, GCP, or Microsoft Azure then you can do that with the help of Packer.

**Where to use Packer**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658152793/2a1fe4dd-2588-440c-8ad8-258522b8f84a.png)

***Brief use:***

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658154841/39f61a89-5d8e-4eec-b5ce-a7b9e4c82925.png)

**Core components of Packer**

**Template**

In order to create a Virtual Machine image on the cloud, a code with a template must be written. There are two languages that can be used for this purpose: JSON and HCL. The choice between these two languages can be based on personal proficiency and preference.

**Variables**

Variables are essential to coding and are required at every point in time. Depending on the specific situation, variables are utilized in the code as needed.

**Builders**

One of the main components of the packer is the **builder** which is responsible for creating machine images or artifacts for specific platforms or infrastructure targets, such as Amazon Web Services (AWS), Microsoft Azure, or VirtualBox.

**Provisioner:**

To configure a virtual machine that has been created on the cloud, a provisioner is used. This provisioner is responsible for configuring various aspects of the virtual machine after the creation process.

*Example- Installing nginx or apache2.*

**Post-processors:**

Post-processors are used to perform actions on the machine image or artifact after it has been built. These actions can include compressing the image, uploading it to a remote server, or converting it to a different format.

**Why do we use Packer instead of Terraform resources for the creation of Images?**

*   Packer helps create identical machine images or artifacts that can be used across different environments, ensuring consistency in the application infrastructure.
*   Packer automates the process of creating machine images or artifacts, making it easier and more efficient to manage infrastructure as code.
*   Packer allows developers to create reusable machine images or artifacts that can be used across different applications or projects, reducing the need for duplicating efforts.
*   Packer can be used to create machine images or artifacts that are pre-configured with security best practices, such as patching, user management, and network security.
*   Packer can create machine images or artifacts in a matter of minutes, enabling faster deployments and reducing downtime.

**How to Install Packer in the Machine?**

You can refer to the below link to install the packer on your machine for any Operating System(Linux, macOS, etc).

[Packer Installation Official Documentation](https://developer.hashicorp.com/packer/tutorials/docker-get-started/get-started-install-cli)

**Let’s do Hands On**

*Things that need to be kept in mind:*

I am using HCL to create the Image on the AWS console.

Whenever you use HCL then the extension will be **.pkr.hcl**

For example: apache.pkr.hcl

Default VPC must be created because EC2 needs a VPC to create itself.

The Code in the HCL:

[**GitHub - AmanPathak-DevOps/Terraform-Packer: Code to create Images using Terraform Packer**  
*Code to create Images using Terraform Packer. Contribute to AmanPathak-DevOps/Terraform-Packer development by creating…*github.com](https://github.com/AmanPathak-dev/Terraform-Packer "https://github.com/AmanPathak-dev/Terraform-Packer")[](https://github.com/AmanPathak-dev/Terraform-Packer)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658156616/d2ad13d2-f7ec-4456-b61e-0883dc0d24e9.png)

Terraform Code

When you run the command,

**packer build <file\_name>**

The Important logs after running the command:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658158602/1dd67584-804e-4b1d-8b6b-3e0256243537.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658160247/a495ccfe-2c84-472a-bd5e-5c3d3e7a3488.png)

After Running **packer build main.tf**

You can see the logs in the terminal, and when you right away check your EC2 dashboard correctly. You will see this, Packer starts to create EC2.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658161829/66006730-90ab-47e2-b65f-fb72f8cd921a.png)

Server Created

After creating the EC2 Machine. Packer immediately starts to create the AMI of the created EC2 Machine. You can see below.

In the end, Packer deleted the created EC2 VM.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658163385/5eb20ca6-85f0-4677-abe5-c22d7ac5e83a.png)

Image Created

The Server is up and running.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658164607/7cc1890d-fee0-4674-98a0-bdd6604ae107.png)

Final Output of Created Image

That’s all for now.

I hope you have understood the core concept of **Packer** and why it should be used. You can also explore the topic of multi-cloud if you are interested.

Remember to keep practicing and exploring, and don’t hesitate to reach out if you have any questions

LinkedIn: [https://www.linkedin.com/in/aman-devops/](https://www.linkedin.com/in/aman-devops/)

Happy Learning