---
title: "Mastering DevSecOps: Elevate Your Tetris Application on AWS EKS using Jenkins and ArgoCD"
seoTitle: "Mastering DevSecOps: Elevate Your Tetris Application on AWS EKS using"
seoDescription: "End-to-End Kubernetes Project using Kubernetes, AWS EKS, Terraform, Jenkins, ArgoCD"
datePublished: Sat Jan 13 2024 07:23:42 GMT+0000 (Coordinated Universal Time)
cuid: clrbqqwoq000408lf8odnbyh3
slug: mastering-devsecops-elevate-your-tetris-application-on-aws-eks-using-jenkins-and-argocd
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1705130297648/389a6053-8c6e-4eb7-b658-f772b03bae55.gif
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1705130513963/870d836b-955f-4c74-b837-c4efa50b0d6d.gif
tags: ec2, docker, aws, sonarqube, kubernetes, devops, terraform, jenkins, devsecops, eks, argocd

---

### Project Introduction

Welcome to the End-to-End DevSecOps Kubernetes Project! This comprehensive guide will walk you through setting up a robust DevSecOps pipeline on AWS using Kubernetes. The project is designed to deploy a Tetris game application on an EKS cluster, incorporating best practices for security and automation.

### Prerequisites

Before diving into the project, make sure you have the following prerequisites in place:

1. **Local Environment Setup**:
    

* **Terraform and AWS CLI**: Install and configure Terraform and AWS CLI on your local machine. Basic knowledge of these tools is necessary.
    
* **Basic Knowledge**: Ensure basic knowledge of Terraform, AWS CLI, and familiarity with cloud concepts.
    

1. **Jenkins Server Deployment:**
    

* **Git**: Basic knowledge of Git commands is required.
    
* **AWS EC2**: Understanding of AWS EC2 instances and security groups.
    

**3\. Jenkins Configuration:**

* **Jenkins**: Familiarity with Jenkins and basic Jenkins pipeline concepts.
    
* **Docker, Sonarqube, Terraform, Kubectl, AWS CLI, Trivy**: Basic knowledge of these tools is necessary.
    

**4\. ArgoCD Setup:**

* **Kubernetes**: Basic knowledge of Kubernetes concepts.
    
* **ArgoCD**: Familiarity with ArgoCD and understanding of continuous deployment concepts.
    

**5\. Pipeline Configuration:**

* **Jenkins Plugins:** Understanding of Jenkins plugins, especially AWS Credentials, and Pipeline: AWS Steps.
    

**Tools Configuration:** Basic knowledge of configuring tools like Docker, NodeJS, OWASP Dependency-Check, and SonarQube in Jenkins.

---

**GitHub Repository for the Project-** [https://github.com/AmanPathak-DevOps/End-to-End-Kubernetes-DevSecOps-Tetris-Project](https://github.com/AmanPathak-DevOps/End-to-End-Kubernetes-DevSecOps-Tetris-Project)

## **Step 1: We need to create an IAM user and generate the AWS Access key**

Create a new IAM User on AWS and give it to the AdministratorAccess for testing purposes (not recommended for your Organization's Projects)

Go to the AWS IAM Service and click on **Users.**

![](https://lh7-us.googleusercontent.com/0YqtZdjDNt7Tu90yifZ5qgyOkCaG-ffjweElhVULl62OuH3DruBzykBMP_LqaaZuE11-paeDy29j1Jjt0TsdO9RlKhK01a-UnCkxxvTgmTqrylPXXmNo_t_AjxfgwX3PnRyYPWhGFkeZVm9nE1_Cj3c align="left")

Click on **Create user**

![](https://lh7-us.googleusercontent.com/i24sbFe-hAuvqhP4wvqS05MTczBMsLyb8Ovy3qb0r62FZ3efAv5-0rnY6HP7ywwepT1ID2kg5_k4bOZiTJf17SBIF_xqOuWoLNoPfxY0SPPRV1h1tzUsIaTSaXKEK0hSjzBTeoSiEROyAr4dOy_iaCI align="left")

Provide the name to your user and click on **Next.**

![](https://lh7-us.googleusercontent.com/ZPqccvsfD91s-VDiHzlqvofPbQHdNgHb6p_dPM6VrVvd3kQfPmm1Q8g0Xl5HR93_v0S0xjVxUAL7Uqg1XZKlCl1rrPWSaMLhSAsHPrhsnhHN57uibMP6uIXZzk3Q42whHFx_mRmDpLDbZ5Yjh1Q7r0E align="left")

Select the **Attach policies directly** option and search for **AdministratorAccess** then select it\*\*.\*\*

Click on the **Next.**

![](https://lh7-us.googleusercontent.com/GYH9q3bCoqVY5eXIzZu6GiriI8aa4I_84znEmPtLB3uznEN_5on-248eyyWkffYovKt9AiP0zh7eCDsMK46DpRZEj0yAfpl4gmEFPeVJXFMzqreInMWtxCn6FZOGbuSILuKW_5FYmxtHlscGpUIoqv0 align="left")

Click on **Create user**

![](https://lh7-us.googleusercontent.com/PzyTelYrdjhzKhz32JO9G49sxN3YqyLkgojTJAQ4uGCuakQMwA51jyHQ1Qc1T5N2jyIkRxextcm9Iobr7k-GJHGtiUMqiTKui3IQdl14GmjIhW3prxH9885FEJxinecJwj3REqCczBJiW25xtpdkzvg align="left")

Now, Select your created user then click on **Security credentials** and generate access key by clicking on **Create access key.**

![](https://lh7-us.googleusercontent.com/O3hOb_6SEACWZ0OjPeblR83ivJo2Hb3P6NTPFp1VFdkdW2GL97diXPoFkgeUjaZQyLhgqmvmxlsqzXofNsWRPWkGhyCd0uiMcQxxX3LYSLWyfIyrsNFtHpiJ8h_F-fNgHi6UXQZVM6VcfCeZ0lkEVOc align="left")

Select the **Command Line Interface (CLI)** then select the checkmark for the confirmation and click on **Next.**

![](https://lh7-us.googleusercontent.com/h9xbu9ZYlKbaYpa_uqyS6kOoem-4AKSuqpVAQ5IzUM5497xcLPBpwQtd9xc9nRoaZtYEl473fNc4ZkXawCGDNzmPhRWuxAmpYQdfOD199qLrWKMd5UJ3FQQ_6cQFxlFlJ5rs4LkpVMsG7SLV9T1grVo align="left")

Provide the **Description** and click on the **Create access key.**

![](https://lh7-us.googleusercontent.com/x5nORlzufQR1XunJN7qEaQmsmsjaCn3N2ZGXux4Fqcr_qd9fdX3N6Wj33N6ctktfyU4s0cuXV0ho8UxJKb-hE1QH3ASuyz1p1GBteaGzQDuQIDnBKYcYxmKgxVtvxihG8TMOzl8Met1Fk0CE7GbYdZA align="left")

Here, you will see that you got the credentials and also you can download the CSV file for the future.

![](https://lh7-us.googleusercontent.com/Eu9uY6sAb2ZGiZtnRXCdg7HiHrSqz2c-GdEzS6uQQkzpESOnvEXxUJDya09krCmb8N4kmealYTalOEXQ-RCD5Mt5Mm_HlNTRjV1jqnfnu5kmxfnMce5AIsKlK40sSQCU5PZBSnOrw6c6Pje2tFZXuEQ align="left")

## **Step 2: We will install Terraform & AWS CLI to deploy our Jenkins Server(EC2) on AWS.**

Install & Configure Terraform and AWS CLI on your local machine to create Jenkins Server on AWS Cloud

**Terraform Installation Script**

```abap
wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update
sudo apt install terraform -y
```

**AWSCLI Installation Script**

```abap
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
sudo apt install unzip -y
unzip awscliv2.zip
sudo ./aws/install
```

Now, Configure both the tools

**Configure Terraform**

Edit the file /etc/environment using the below command add the highlighted lines and add your keys at the blur space.

```abap
sudo vim /etc/environment
```

![](https://lh7-us.googleusercontent.com/gP1JHKqLv-AMURPgf7UIMQIzlkQvId6oYKEwNG1sCQYR7Afu6UuASr-tk94QiCOB3NxeZ8GJAIv2_jXSu7KBqp-Z4fqvc1UqeP0PF7a11r4pWwci2QvYYchHPWgzjXMfPOVBTWJR-39wICaqmHY6TVA align="left")

After doing the changes, restart your machine to reflect the changes in your environment variables.

**Configure AWS CLI**

Run the below command, and add your keys

```abap
aws configure
```

![](https://lh7-us.googleusercontent.com/5ylJwpov96KbCd_7iHNiZH0kgAhdVDcujd8T49-WPR98u25kud-7T25o6jSf0jGhLS6PqjpObB-ji7Aus4NgF6KfRpXRf6IyecDs1MgTnj2cvBq-rglJX9okji2aNuZ8lht6gZy5HUTecjHZJnR35r8 align="left")

## **Step 3: Deploy the Jenkins Server(EC2) using Terraform**

Clone the Git repository- [https://github.com/AmanPathak-DevOps/End-to-End-Kubernetes-DevSecOps-Tetris-Project](https://github.com/AmanPathak-DevOps/End-to-End-Kubernetes-DevSecOps-Tetris-Project)

Navigate to the **Jenkins-Server-TF**

Do some modifications to the [backend.tf](https://github.com/AmanPathak-DevOps/End-to-End-Kubernetes-DevSecOps-Tetris-Project) file such as changing the **bucket** name and **dynamodb** table(make sure you have created both manually on AWS Cloud).

![](https://lh7-us.googleusercontent.com/0hvX6EZPU4fmtcEfZORHPI0jRDMUxEfhEe1LRSN2P_m9HED9FKwaXxRZxy4ovb3M825SzR8tcu81hSsTmvZ22YtwI2duFpN2XcnxSXyOUC-3Gv7U74fp_AyejwJ4v6Al2ZVmAZyd90cZRVDftfqtsPk align="left")

Now, you have to replace the Pem File name as you have some other name for your Pem file. To provide the Pem file name that is already created on AWS

![](https://lh7-us.googleusercontent.com/qT6rod8Hjqjt2XJrCpDdMx8wmccQ2cB295TzKPvsSseQBYFDEUzDjVaMz9L0tRVdKVeSatTGV_XMna2-s8wObNQ1-KLnIjxXUQpUUm4jAd65C_txCDxmuDhE9cb7b2Df2gqR28dFbPWJ8qOrLuvxhvY align="left")

Initialize the backend by running the below command

```abap
terraform init
```

![](https://lh7-us.googleusercontent.com/fnzA673-O9CiDCHzkNrN7FDRN2zj4cLDC0tPnyUzDsBynfAVIXQSshwkWvZ_xOuvufdiSrbkZ2bP_Ja-EiMXxOQrBquuQeixWAvYsTzUhTDiZLjYIe-tqdd2QTY2qZPdMXSZdS0dfqNXORLiUlPTCdc align="left")

Run the below command to check the syntax error

```abap
terraform validate
```

![](https://lh7-us.googleusercontent.com/YUByuhtK6gSLgRpOglrO6x7S9PFZVxk61M3Hcgh7vcp01EmEuyU5Jbo4p0EQKHdu9ydi2LU5eZMwoBt_pRqmP1Fzn5rf6IZJ9XQEbqW6E3aey7dDTiNgyKnlsA9HXynJtaKYSNyLODZ7IEdngn5C9xA align="left")

Run the below command to get the blueprint of what kind of AWS services will be created.

```abap
terraform plan -var-file=variables.tfvars
```

![](https://lh7-us.googleusercontent.com/hGRtZfSXeVN_hg06Vn2Lowi08n7AD9usLFAnofcOAaI1wte_tk5LBANVyhWAonl40hpWinvNn2D6KQnQZynZVBtDqHZzrw8iPQa7pZTfszOrakmNgzOtRnMjkdS63Ii7nNGQY6JL-cQqXBM1HYaxVkg align="left")

Now, run the below command to create the infrastructure on AWS Cloud which will take 3 to 4 minutes maximum

```abap
terraform apply -var-file=variables.tfvars --auto-approve
```

![](https://lh7-us.googleusercontent.com/sEIxDXq2pD0jis6S-MYADKHEw_ou1Gm5O14fBOjx00KlbV-EfxZbdHQUhMMOEUDT5w78za8BNsl9PdPiwemHrRRodkMQkRAd3o5P5oiwBstaeUyXgPzr1eUC4v06gTaUpnq0amYYcNwIVZ4b9nR9Ecc align="left")

Now, connect to your Jenkins-Server by clicking on Connect.

![](https://lh7-us.googleusercontent.com/uQ1uYI-w6Hp_ZvHeaAkfdUhByd0WlnLFuEh_1IWCmXYV2dKzm6f1sYE4_O11EjaBDjyMiMUQhAUwCnuGdXwVK8MBGElwnKqlqzFaNyJStXp-wf1BugE9vSMtub5liTjceCTG_bfH48j0o_NJBNoPggE align="left")

Copy the **ssh** command and paste it on your local machine.

![](https://lh7-us.googleusercontent.com/3VYwwi6CCu4wqSi551qS_oMVRskSD0tkiYN-S0S7TxRqUDq2LZGuqwDWFXcXDwqXQ1tlGE4u0xgGFxNiuXYekqxbKqE_9Rqe6q2Qhzm4pEOONQMHSiLaUZuphAriDndsluj18AnrASNB_4EjMb6QbRQ align="left")

## **Step 4: Configure the Jenkins**

Now, we logged into our **Jenkins server.**

![](https://lh7-us.googleusercontent.com/tI1xLBkexXDmuYfgiqhb-1svhFo4o0oX15kYZS-tBroE9uUCDwnBdmVmIGzHzc0EL30aB0hc3ke9zdbc5v35rXQWhBJAkNAYCQkPlq7Zbbawcjrv7yKy-MhSM16fYEfnHgXVaCBnOBvW214iXm68PQw align="left")

We have installed some services such as Jenkins, Docker, Sonarqube, Terraform, Kubectl, AWS CLI, and Trivy.

Let’s validate whether all our installed or not.

```abap
jenkins --version
docker --version
docker ps
terraform --version
kubectl version
aws --version
trivy --version
```

![](https://lh7-us.googleusercontent.com/7hyNidya2C37mKjfRDEqyibXBB2FMpwJ9dPzQfKnBX_dJCZyProgeBpULOwZu42rhMy7tjGgFmlSoXNEiPQ4te1Pq6VunXmFfTDJ8q0sd2zB_iXVqCnm-1YVcqbvedYlEdm7QeQq_5zqadID7IssZsQ align="left")

Now, we have to configure Jenkins. So, copy the public IP of your Jenkins Server and paste it on your favorite browser with an 8080 port.

![](https://lh7-us.googleusercontent.com/_Cv4Or9iJqsvL-g1K6Sjnd5x6I8P2t2nRGPfqlWjOYQEj0DmEUfndF-Gi65iLeidP5Od9PVdn83xqEkS70YemxA1CQgTOZp9z61LLBALwYjh2Pk2ckhTDr_JnrGDEx2aHK3VmHj9FRnPnVWspoEAzjs align="left")

Now, run the below command to get the administrator password and paste it on your Jenkins.

```abap
systemctl status jenkins.service
```

OR

```abap
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

![](https://lh7-us.googleusercontent.com/fTHnKJwvv5oL1R-BrLPs-BdYhJkXQkuQ5hyMOSMIMDPeJHV9vxItSqWdVbsLgLbL1_Xv0ikVwe54UaSkJ7QRd9GVctklPo6LP82t1k7PFu0RkZvg6mQGVP4_7muI-wvck9Zr-STKFUvEay1Y5FVdhTs align="left")

Click on **Install suggested plugins**

![](https://lh7-us.googleusercontent.com/4MzJkX5gi6ETac7BntU6_Nd-8PSH9ohlCbTSu0o1YpLrnsm-wxSe3htlkYtceMciHx7y7gBOrheSDc5H4wiCAnRDXgbpTgz3vbEQ-VqNi20LOj57MJ4XECiUFJcMcl7ZTfeLI--kf2knztNx45kzgEU align="left")

After installing the plugins, continue as admin

![](https://lh7-us.googleusercontent.com/GTEGefQixOPA9hay1ux0ygqm5Yl9uvQh4IKaCo_Zwnr2MoXT-QiUHxDfjngK5HzzHaiwCckIlBoBz2pf0dODjl6zCdiPIkZ1m_TUKrUl5EXh3QYeAOTNDY3qIjGIYF_1kwcb5FzbBOYEa3o3gFYbA_0 align="left")

Click on **Save and Finish**

![](https://lh7-us.googleusercontent.com/SUnxCliJ3xH6ZI2Yff_m-pnl_keF3dyYxSe2ZQGtNcnVrXo-tJD9OuYa7yUqMxlLd3yZeoClV_hLIPdrqUHCYyzgKIdeDpcz6dye9ECCxbat62GjOVW1U5EbysHS7E4YiYuYGsfYv7ktQqvF370OjCo align="left")

Click on **Start using Jenkins**

![](https://lh7-us.googleusercontent.com/Xl4-olrZX6VLreIbCQ--Kigc34CeZ8YarHrGj-g1uhwH4omfJw3foedCH6zwDSaY2L8vLLDVzXgVRejXBklAKGrQqsRbquNAbLIwc0DoIfhzXr0OOxRBQacuSiAq84_nhT-6w3bApcz_eTYR65dCLMQ align="left")

## **Step 5: We will deploy the EKS Cluster using Jenkins**

Now, go back to your Jenkins Server **terminal** and configure the AWS.

![](https://lh7-us.googleusercontent.com/s9V15sVsajY4vaclH-894gz0M7WGHlEqgUgpqLNfCC6zZVRCao6Sc-QfU9aTq2TD8kmLKNr5r_9vWgYWLTmyabaCNwJXJRlq75yc_EtNtuaZIuL5-7GDBR7tpEAGBCxfHnwwH0IyXjHTGLuO_-dTOEQ align="left")

Click on **Manage Jenkins**

![](https://lh7-us.googleusercontent.com/wB6Y6g6Uix9h0bvd7d75pmI8XXZQEb5tDEsrssr5tI6pNh4eHQDIuyfYqKJiWTqkhv1XnpsU4kLXOUjS0Zj4gQqNxvERtXicT5BZ3fxijoBQb1IaRL1K2qIHtiYtAZ6lG5LkOqxYbWEZCVS2OsE4pIA align="left")

Click on **Plugins**

![](https://lh7-us.googleusercontent.com/3BCq1PDzg8YxTram0U3q8HSDMXL7qUxtlFhThnsuJ83mENm7u-7_Oqn2B2WAL3Anj222XOmlh1leqldcPW3duyuyanpyG3iDWBok_jKRg1Eggia5Ci-sPUps-WzrRjjUAWOqeViZgfGSSFy3ck5wHXg align="left")

Select the **Available plugins** and install the following plugins and click on **Install**

```abap
AWS Credentials
Pipeline: AWS Steps
```

![](https://lh7-us.googleusercontent.com/M8PYy9SCkGGV0S65mpLUvH8PeOXU5Q_Clpnednvp8VvWDrx5iWai8Qx6qzeToRcbkkdc8ljjMAMdMyqGb56ZkhRZ-dhKRaqRXpJKzl-hLTQbFuMRuAC07hZAc-tD6OyW8yKV3GkDvLyYLo-QnXkVLA8 align="left")

Once, both the plugins are installed, restart your Jenkins service.

Now, we have to set our AWS credentials on Jenkins

Go to **Manage Plugins** and click on **Credentials**

![](https://lh7-us.googleusercontent.com/LZswZJeQivRKwc5HBjKBMACzOwgP76GXrynvjJLU_K4g4W4zJF2AZYhqcIbJbgKSWaUjOAGjy1mkaYNsXUGKiRo95lHKz-mGwBx7mq7azGDoBXQScHwTNAqyJoEA6jG2VWcs3UpYNlFaWxtrlrr-6Fg align="left")

Click on **global.**

![](https://lh7-us.googleusercontent.com/hvdR7HUfG_gPYXaNo01xiuyAw1NFlKmuW40BEznJ2IedwmpJ-c5kKyvdn0Imbzj1vrnNW9zRWpgt72wS01-gQz7cNe2uM1nph7nWIqVvACxOUiQ9nObsgp_6bTmqp23ke21KcnV8ZBO5mbEilUDLVSs align="left")

Click on **Add Credentials**

![](https://lh7-us.googleusercontent.com/voh8nI_p77oRpVKzjP7DsfkGfACX-u34bETXhHIB36xCMGXVYv3rG5nA-poTbbVPhzbEwHhSGus_uJdlvE2Lc2tGy87D80FLw4xyoLfljVwMt8SjRXtmfWAdej_ZRv0rxALXoaNxE7m1Lqfr-37pATg align="left")

Select **AWS Credentials** as **Kind** and add **the ID** same as shown in the below snippet except your AWS Access Key & Secret Access key and click on **Create.**

![](https://lh7-us.googleusercontent.com/5Z3owBrqb3fpq5aOsYBZz6AIWqa8UX4GhdWsNdLVK2BVex_eVQYbd3cwLubMPUnYS2L1kZoKuF8JzZD2e2JAX-PhjxAasa4xywO6yWsXxkkook9FmoTb_FOHAH_Dy-ybduGUSKxyZOdkszJXQJkaZFs align="left")

Now, Go to the **Dashboard** and click **Create a job**

![](https://lh7-us.googleusercontent.com/h91otfFBfs16E_k7fbAPvPj0xUn9-3xybpGQLEvKzNAlMp8tDv_aPdt8kGYkjFGTwwJ0Gg9gtrXwP0F7kE4BLMknY7lTHyphVUPQPzW3JMX_xY85CaU7cl3amMV8_VeE2ugiT_v65CZkbvu0l0xWuiU align="left")

Select the **Pipeline** and provide the name to your **Jenkins Pipeline** then click on **OK.**

![](https://lh7-us.googleusercontent.com/tP21gTLxjCv1nS-9Yf4I5d082uosWJ_KlggOrMAr8l0ptqK4iGx3BKIMK0CO_w2iopdPPsiwIItwRht-9fuVHg199AKwagT8TLSXNldEyjinEF3d5dlMb8RN6rYthd_D3na_M9f8qL04r-E2_SEiXIg align="left")

Now, Go to the GitHub Repository in which the Jenkins Pipeline code is located to deploy the EKS service using Terraform.

[https://github.com/AmanPathak-DevOps/End-to-End-Kubernetes-DevSecOps-Tetris-Project/blob/master/Jenkins-Pipeline-Code/Jenkinsfile-EKS-Terraform](https://github.com/AmanPathak-DevOps/End-to-End-Kubernetes-DevSecOps-Tetris-Project)

Copy the entire code and paste it here

**Note:** There will be some configurations like [backend.tf](https://github.com/AmanPathak-DevOps/End-to-End-Kubernetes-DevSecOps-Tetris-Project) files that need to be updated from your side. Kindly do that to avoid errors.

EKS-Terraform Code- [https://github.com/AmanPathak-DevOps/End-to-End-Kubernetes-DevSecOps-Tetris-Project/tree/master/EKS-TF](https://github.com/AmanPathak-DevOps/End-to-End-Kubernetes-DevSecOps-Tetris-Project)

After pasting the Jenkinsfile code, click on **Save** & **Apply.**

![](https://lh7-us.googleusercontent.com/jM54HUBMK8ZzIg28obwmSNVigEXagRahrW02rr7eMal-Vwu-tvgp6wbQB9J7XU-YCao3MC8ngIvTMNEETBs7yR9EfqZPfqUg6H9oriJVxwTrX2bH7t_bSmUw5pT2HOUb5zW8A0lXsBqYAtBqQ_w6QP4 align="left")

Click on **Build**

![](https://lh7-us.googleusercontent.com/_Bp1uE9wqJbRK3rUAQ8OJu1vjjd-1bTsfXjeyVzw0oMvpWBpwRIe311nCgrVJ2chXJfpoyFqRCgIuX3nVm5A3TlxAmHlThxj-6ZQxH0N-LkAtl5y0XJySi5hWCViZZnpuyyD_X5TL3CA9z2HIyYJBII align="left")

You can see our **Pipeline** was **successful**

![](https://lh7-us.googleusercontent.com/A2FvAotPTuBjdEbbAGdqVXH0xS0YOK48CCIEfRsLBduoSCdZ3PLL9yf9prDQzuFiRtvHzhbhpPhnKnmuqL7UOBzcXZ97ftpR22raD_B_qF9-fbGA8caGqaLn1_eG0FP8VI9vPumB6DNyEHV4XmgIIJk align="left")

You can validate how many resources have been created by going to the **console logs**

![](https://lh7-us.googleusercontent.com/0IIkxpSyfdvHUCMXwyaJHfItObv3SlDePpOMqp5rWXXtVhGG0xmA2bInPpMYa919BeQMWTiNdYYYjJaxDbGcy-hMb05EK9h_ufDRNPBvSl_v6FxRefkRSZ0hOnhyN3SK2BWMWabegMzMvwlaMjcdEW8 align="left")

Now, we will configure the **EKS** **Cluster** on the **Jenkins Server**

Run the below command to configure the EKS Cluster

```abap
aws eks update-kubeconfig --region us-east-1 --name Tetris-EKS-Cluster
```

To validate whether the EKS Cluster was successfully configured or not. Run the below command

```abap
kubectl get nodes
```

![](https://lh7-us.googleusercontent.com/_5FW7UdAPZLF4An7t-C3STZ0vLKJ6phkx3XQWPb4fl3XdJMSR6oV3OcIYiWfJjUax_NNsHln3YjWnJN7FGNegzALONKMbO7AlMFOeYD0nd88YBjmijkLKAGX9xhi7LdAsILCimBc4LnhH0ZTInIF-Xc align="left")

## **Step 6: We will install the ArgoCD Controller on the EKS Cluster and make it publicly available**

Create tetris namespace

```abap
kubectl create namespace tetris
```

![](https://lh7-us.googleusercontent.com/zTh1rBvYUud05cJmCXRM__ZGktFTaiRBuoE2Nukexp4j5jpVgNBig8Zuc33AX58NOCSGG9m3IgLRf_PKrd6K2Te0Sd-sWWFAjALMYERmOA2cAl8-vvpXrroYNfHBLzY3DeTxHyiX7JkTXXOloN3AA7Y align="left")

Now, we will configure the argoCD controller on our EKS cluster.

Create a new namespace named argocd and apply the manifest file on the EKS cluster.

```abap
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/v2.4.7/manifests/install.yaml
```

![](https://lh7-us.googleusercontent.com/KAa_fwfnrmFAUQgIPK0-rAG1JEj23Lfsi_ZM5to2OFXN__CSlJBl0rQXM2NcOBEZKx6e627p0F5qjBE4ufHNLdTJUZneLVswG6nxlSWANG97nLr1i5iZua6SvbAKX1DBQ66MfzVVD_VKo7zJAm-OQdY align="left")

Validate whether the argocd controller is deployed or not using the below command.

```abap
kubectl get pods -n argocd
```

![](https://lh7-us.googleusercontent.com/Kt3pNVH8dFIvmR_pigBy99vc2w7ElW7yNt1HWIRy27uiCD4x04rj4Gz1bNGegVExi8KYhEL2j8hcvh21GyLBbL6j0kRDPIOvMt0XuiqM-AlVcvgKkafWXmXlsGhVPbzgBm84_MfjLuiSYukyuL2JNWQ align="left")

As we have to access our ArgoCD controller through GUI, we need to set up the LoadBalancer for it.

Run the below command to set up the **load balancer** which will expose the argoCD controller publicly.

```abap
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
```

![](https://lh7-us.googleusercontent.com/fBKvuusctMhHKkLXV688-Ed8VlwP911wcCwP9Z8pTcQyruy0L30U3IoaDWDEcLvjxKOhw6RykRhY_bqVhryVN-UtcJoAKQRrjFwyWIwFyEORT7whg1qNzSnE-py7gfLRm0bluKxqRNI3TtpEXHStTOI align="left")

Now, you will see **Load Balancer** on the AWS console.

![](https://lh7-us.googleusercontent.com/FmZp15Jbi3Hf09Qdmf6KHJeWn2Qv-ToXuk2eSegBC4zb6E_aXPgrPaY7R602mTOsBoeetxMpFKfciFNZDx5570JZv5NqecfHFD6j82nt779HmKlO16VUMb4R8HtRXoDDzryTK2CcZl85kRQ1MOtFFFA align="left")

Copy the LoadBalancer DNS and paste it into your favorite browser.

Click on **Advanced.**

![](https://lh7-us.googleusercontent.com/NlyjT66-UunLPly7TCoegdll0vXo5P6N1cs2WZtDX37vqXmO2C0nNzpEdNMF1JeyYPkYK49ZFZHPr8EMXic42WdJLBwDwxXmJbe1wvs5ZN9w0Wc6GfI_hASDU5ipm_50VLPRrFXEG5oVj6OSLOD2q8w align="left")

Click on the highlighted link

![](https://lh7-us.googleusercontent.com/tdNDRFJ4lg9JvDl6gOtorIu88N4tn3A6ukFaAQkioIEo7znNf1DzZNJS2JnveaLDO4bvm_XdJt0MPmOwNnGmFWXTa1fZnW1fBXAvMG45Tu9VbIypNMd263SyPhmcQsI3zlafD2AFs25XT7CxwDmPihA align="left")

Here, you can see the ArgoCD login page. But we can’t do anything because we don’t know the password of the admin user.

Now, we need an admin password to log in to our argoCD.

There is one pre-requisite which is **jq** to get the password by using filtration.

```abap
sudo apt install jq -y
```

![](https://lh7-us.googleusercontent.com/dfJ4eDvPuysmVMBIrLJ2ao35RtVSDJA6utFk1D8FMtlATKMZSY_qNLnJnoYXd5QHk5yFogCimGnFGfT63I1HBtq5_0a8bfYOJNPyub4njg5ArCJEQ_AEcoDpv-9csuYTdjfv0OtdVNwj2QlIYdgluaM align="left")

Store the ArgoCD DNS name to the variable

```abap
export ARGOCD_SERVER=`kubectl get svc argocd-server -n argocd -o json | jq --raw-output '.status.loadBalancer.ingress[0].hostname'`
```

![](https://lh7-us.googleusercontent.com/CHj93eUw3zn1B5fITMMQpX_dc9QQpC6ZHiLaRgwgBnGyCHUzG3gantROcPrH_NDjsX9XbcbQgiB_yTqzRAVsM9A-byNUfauSd-lIjgqxRMR7jmy15ELfu1CyU8UgDnBuM64Ezp5BV_W3cUQw5OCA8gg align="left")

Run the below command to get the password.

```abap
export ARGO_PWD=`kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d`
echo $ARGO_PWD
```

![](https://lh7-us.googleusercontent.com/Ylts7w-XSRWtNcTzqUm5j7CEC7e_yVa6ONz2to5a1fPKNBDAv0exh9sx_-o4hr2aMSbqJUl1b9IIRHcc7Lb3H_U5R6sviB4PCeFKhBa0DOthA7HYCC-F7_e8jDFy3tXZb0kVE8v9maSQFFjOepC-95w align="left")

Enter the username and password in **argoCD** and click on **SIGN IN.**

![](https://lh7-us.googleusercontent.com/F4kiukeKCL0fuKT5kBivy53Wa_9D75tYimMXuT6qXMtadwfwR6Vp3lvCAv3pc-IHjX0g8eLayNdoaindj1FjrqGqlKipECEWkkBvqMUMzSymJX3k2ZE8YnSPeyope-WEnLSW9Pc_cEsD8pyDBwpbQhs align="left")

Here is our **ArgoCD Dashboard.**

Click on **CREATE APPLICATION.**

![](https://lh7-us.googleusercontent.com/JoVdBP4TSO6RmZt_sJHAiPyWm4u_D9RQ6oyZudgVSqoSYG-fXiQroLWiO-Ne3DsR807YMbpbzjBTxWFdwxmKJzaNa77eHyz7LvoVpwlhUOU1lFNOFy8iV2yWb0axj02uFzdLiixteJ1yavuox1O5iSM align="left")

Provide the name of your **Application name, the Project name** will be **default**, and **SYNC POLICY** will be **Automatic** then scroll down.

![](https://lh7-us.googleusercontent.com/jFTrTNnmVf4VImK6TJu35qj9vb-HoO3rsfrmo22_IiHmiXW1wjk0EKgC9va05YUdjtw7OzxBibCvKdYRPF1vQHZldbvbAHmms0EuhHD0r653rAsQTcoRfKkZRQ0o5sfWOrpKbGjZ3h4XoE7UWKEyFlQ align="left")

Provide the **GitHub Repo,** enter the path where your deployment file is located, then, **Cluster URL** will be [https://kubernetes.default.svc](https://github.com/AmanPathak-DevOps/End-to-End-Kubernetes-DevSecOps-Tetris-Project) and in the end, **namespace** will be **tetris** but you can write another namespace name.

Click on **CREATE.**

![](https://lh7-us.googleusercontent.com/IEi73Zv3GH0tljwbrOY4qVPvTadntLtVfPeIPw6kIehlEw7ctP5R624SR780MuLhxXJFvZP2J_fzem7vKZfwNeAST8ZYU44aApNaXSv6CT62QaTi0hLuXjpF_SJYrE9F3q8_U1fW7hua0JbAZQ3tWtI align="left")

## **Step 7: Install the required plugins and configure the plugins to deploy our Tetris Application Version1**

Install the following plugins by going to **Dashboard -&gt; Manage Jenkins -&gt; Plugins -&gt; Available Plugins**

```abap
Docker
Docker Commons
Docker Pipeline
Docker API
docker-build-step
Eclipse Temurin installer
NodeJS
OWASP Dependency-Check
SonarQube Scanner
```

![](https://lh7-us.googleusercontent.com/4EbwSmIFgeXV-qEx2NRmn6oThcmEE7BS6MUG_JAkywISkI0WDz2K1oEPcg5WiSJM7ZoJHGAC7b2NRXPhBo9r8WmubC9G6-U7ayExgVE5bxto9LRov36NHSesgWiX_WSAN79myQC7E7LsEv2yMO_c1HA align="left")

Now, we have to configure our Sonarqube.

To do that, copy your Jenkins Server public IP and paste it on your favorite browser with 9000 port

The username and password will be **admin**

Click on **Log In.**

![](https://lh7-us.googleusercontent.com/FkTpI_mUbosIip60w53qVwbXQc9-zNLvnHELvN0yqwKJ3MpRZokAAB6smBCLaQ2E1PqLJjyzgcZaYCkXhWMdNDvIeTPZUpITBDIeVPymd2dJgK-EoxulRE6NRHTumrOaO7eEKWqvDZRuwqB5peI4lII align="left")

Update the password

![](https://lh7-us.googleusercontent.com/dbWmH3F_hET-ZewYTEJlOzK66JmfDN_hDYELM48IVyt10ohsg7E6JsGbnlJAm4onPsu06Ie5G5_92iPz0AhoJkPBDc7TaMBLo4AXbCUt1UjXEYwG6L_nuMoHYdH6KjGMEGd8btD8biVIj2opjyZehTc align="left")

Click on **Administration** then **Security,** and select **Users**

![](https://lh7-us.googleusercontent.com/Dkntfr0x31GgkDjjOT7VsZFSAnGy1Sfum3cjnS_-YFUziPI0pQm2pIW8iSTQFpOoxfWnwwfg45iapTOhz0QRMNE6DA6O76FYVaCIU49f_Luxe3gTBvJAT9BxQDrN_40UMTPB9H8eSFNdbOIe9Mxyoog align="left")

Click on **Update tokens**

![](https://lh7-us.googleusercontent.com/O_qMoaU48LxCuD7PNkD1H0XQAogBLZyV3hTX3RDh6uIi2wxesdu0-72Buzq-gkoJAYGncmJ3r4elR7aTsPGbh9yMHLTh9qANdm8-bUabm-UdH-T4ttdN9WztI51zCNWLpvcFi13T2Z3MJG5vGQNyPU0 align="left")

Click on **Generate**

![](https://lh7-us.googleusercontent.com/K-OophY5erb6qW1KC1thaQaLAW0wtV3tM1s9NX0N1cD2dJy8xQzNPIzERk1e2gbrOXW1kOsdDA5aQV2L449jRPN4DCeaDGM4S3jUDgBc6zLdKkoluj4M4BJKKVMnUOqvETusUNKUDjrQ0WM60DJxgmE align="left")

Copy the **token** keep it somewhere safe and click on **Done.**

![](https://lh7-us.googleusercontent.com/TOhA1t8hTxmzU8lvyTyVcKnWu0XVlqWeYigNykZ0a1pUhFI9LEUL8qYpflZvxn8g7DTWkIq3HTrRF8BIXzyiWruGEhIllk0jAoByxCbOGIYCjCA3qqyOHesWZ49ZFldzUXb54La47Lxd04IKpVnNo2A align="left")

Now, we have to configure **webhooks** for quality checks.

Click on **Administration** then, **Configuration** and select **Webhooks**

![](https://lh7-us.googleusercontent.com/Ai3qq_u7Mmgi0GoD-5FijY_DY4RkW2e8ppF-SN3GqIuyheB-DRFsgrTqGoJmZV1wDcEqG2_vBNcWFuSG-VCnqUCt0QCmWy2F41XnKH-qNt1UqHl-oO_lS_2uX80bWd0H7mQ4O3yEq7rReS7GGnU9i6M align="left")

Click on **Create**

![](https://lh7-us.googleusercontent.com/JehbRdUxydkqrfBii6l5wYa9bREzXKZhamhn1fzX-KBxKjEZPaPg6Hbs_WG1DQawEjAT7ahtAJy4uckewldH2qgUBxuiXD1oMsWoiDFvIQ2LywqjBIbbpdOBTa1uAx5LtzP1FHoxrIMohlNGcH48Phc align="left")

Provide the name of your project and in the **URL,** provide the Jenkins server public IP with port 8080 and add sonarqube-webhook in suffix, and click on **Create.**

[http://&lt;jenkins-server-public-ip&gt;:8080/sonarqube-webhook/](http://34.224.69.67:8080/sonarqube-webhook/)

![](https://lh7-us.googleusercontent.com/yMtlei5uxNRlZzYng5smbJBW0gw88YZZV0uLVzv0ADUveurof-W2OyxUpEdddvfotDf9asWDR6GaTdXweIHihv3Vw-ya6NfM0HqWeBusy0jABmCLOWhhGHBPWVn6256eQkDF_O7b8TNtZ_-c3s4lElo align="left")

Here, you can see the **webhook.**

![](https://lh7-us.googleusercontent.com/bhyp5PpOUpSOGJdFsEqVmF5AipD0ErWuINXixvVfyI6iAzGik_km3wzMXSLMG4oyI7ZektcPCe9Ka-SQjeYDLEJk4eyUrZJS7mU77kP-q4N55_iiIelrxvAatdJvBYzm8RkaBOmTCUkhaf1SYHfKjh8 align="left")

Now, we have to configure the installed plugins.

Go to **Dashboard -&gt; Manage Jenkins -&gt; Tools**

We are configuring jdk

Search for jdk and provide the configuration like the below snippet.

![](https://lh7-us.googleusercontent.com/7c4mVD7jXyl3HX_hPECKdnJeerFNL5mhj-pA-d8-HoIDMBKtFVptSCg7x_GBV4zuOmstedXDKJFvq0aThBcKJLL1vyqvSDlT7Zt1IY8rEPHZzjYgUqoEhmgmJTIumYz7OoNWFgSghP-hejX1YUyoXNI align="left")

Now, we will configure nodejs

Search for node and provide the configuration like the below snippet.

![](https://lh7-us.googleusercontent.com/liirG8Vv2Ro2siqTBlZTAXeV-Vb_jdNxOb3vKhcoCKoXVTx_PrBZde9Fvq7dtDrB7_kxtxZdho-X8anCxne_OSmgWQeflYvcUIliJb_1U9o6ABqx7EbmuTJbO6qeZ7NE9UjEdT94gRJezQRYsreOdGA align="left")

Now, we will configure the OWASP Dependency check

Search for Dependency-Check and provide the configuration like the below snippet.

![](https://lh7-us.googleusercontent.com/talPxrPeztELbmfdru5rx-t0XonahUPbozB1rUJqzKsbt6SRLLJjNs6S0lXAv9FF2DYIKSyJ9SS9XFbNzZ3yERymNB6BewRxJq70mBUhobeE7XmecKlPHvWoNTa440QYA08iogezCWu8CIo-iCUGW_U align="left")

Now, we will configure the docker

Search for docker and provide the configuration like the below snippet.

![](https://lh7-us.googleusercontent.com/6wWPv4gKnap4pBxGwsZ5VgtLruSuRpHIBD_O6hbJuWe6sLhddvWWN5DOFQaBQuxz7KeawtMLN9BgzQtDcJOiSu4IAqEP9s_AnWhBeFh9bwlcrivgnbTC1iK6XaKPdqXuvGpvncByBA4vdnxZKSVjg0E align="left")

Now, we will configure sonarqube

Search for sonarqube and provide the configuration like the below snippet.

![](https://lh7-us.googleusercontent.com/mfZr4lLYHz5JciJp8MFyN5Twvin1Lb3tS28U4DHTm3bBI4YYw289Ft1ytmzdkDTFun1zZXnmcHuGBNIGcZLhf5OK7aW0PUQYdeanINpMpAn0kxmKDSWV1ZocZmQBvnBG6e3sgTNdrcmICfbjPAzdSzA align="left")

This is it, now click on **Apply** and **Save.**

Now, we have to store our generated token for Sonarqube in Jenkins

Go to **Dashboard -&gt; Manage Jenkins -&gt; Credentials**

Select the kind as **Secret text** and paste your token in **Secret** and keep other things as it is.

Click on **Create**

![](https://lh7-us.googleusercontent.com/19TVVX4SZbYdJAkFKhMhmtfnk6plHlCPfd0Xqnjp7tNmsZNXA5wJN1Ud9mw7YOmWUEPMwz4oHsjHFF46LLRAgKFk66lnFjsXcXGT5q2rtKGjDunb6JSr1YkGV5faSubRUQeo57Gc6DA8dXkSoFXN9IA align="left")

Token is created

![](https://lh7-us.googleusercontent.com/zmx9XJyJvTZum2nVay3G5XctTyE13PxZqP8OEEOevOmuCAe36T3mZRk7kV-7nVFfHj3VeZJFXrjMDpVGECeOn5EJIw4mMKN8Q58OOQ7sfjrI4U9qHWv4C-2Efd2YqD6aXnZvjI9s7BcFKCOeUQRPLaU align="left")

Now, we have to set the path for Sonarqube in Jenkins

Go to **Dashboard -&gt; Manage Jenkins -&gt; System**

Search for **SonarQube installations**

Provide the **name** as it is, then in the **Server URL** copy the sonarqube public IP (same as jenkins) with port 9000 and select the sonar token that we have added recently, and click on **Apply** & **Save**.

![](https://lh7-us.googleusercontent.com/qePMj23RpX5tiInHomfr9f7oc7B1aU_OYqA0jHsU3h71keLxYUmysD4hzWKm-6x-mVdTm4hQT3Z6M1JcE97uLxXT06jBqP9TNRtAB_Iv_Cui7xWuUAMQDAJzISPuk0KvubeMiMnwqw3Pdl5posL4jIk align="left")

Now, In our Jenkinsfile we are pushing our docker image to Dockerhub and then, we have to update the same image name with the new tag in the deployment file which is present on GitHub.

To do that, we need to store our Docker & GitHub credentials in Jenkins.

Go to **Dashboard -&gt; Manage Jenkins -&gt; Credentials**

Add your docker hub username and password in the respective field with ID **docker.**

Click on **Create**

![](https://lh7-us.googleusercontent.com/3hWH0y01iMsKBEfRj2Pe6lyLslCTpbX4h4Lm7RleD4L-EDu0ymIjYFXe6EgPs2m2NOMq__c1JMiWAhO2SZK9uOrusMnCjXb2OwtY4V95YDres70iG9Co6BS4bTnl3rAO1nf2RRwWKDUOOIs31DmPjkQ align="left")

Add GitHub credentials

Select the kind as **Secret text** and paste your GitHub Personal access token(not password)  in **Secret** and keep other things as it is.

Click on **Create**

**Note:** If you haven’t generated your token then, you have it generated first then paste it into the Jenkins

![](https://lh7-us.googleusercontent.com/QJ8B65wcJVSAhMCpwmYWnWQw0Rx12pTu7GOzpmi5xBbkXDREHKstLcXVZeOPivIixzQm9a_9DornD4XysMJ9hudeDzufN0NJlCNN7DVm-7mtyMnsSPlc9tFkcjmp1B_06tjQfHuoq6TOOeV9c0f6PX4 align="left")

Now, we are ready to create our Jenkins Pipeline to deploy our Tetris Application.

Go to **Jenkins Dashboard**

Click on **New Item**

![](https://lh7-us.googleusercontent.com/GIXOksoIqeQ_iq_ivU4FtR9TsEt9Cdv6eEfKLjk2S1O9XDjFwhCCKpDGBz1e6NZXGxilb1uXBvBkHZmw3S4k-kC_njQ1blaT7FDc5orYNqbiAQnYMxoWa6moI4Qstz9-XeGlYJEC2xA3l5TwpKYDYhM align="left")

Provide the name of your **Pipeline** and click on **OK.**

![](https://lh7-us.googleusercontent.com/pRjKbBa2q7hzgUNwgefm5BZS_fYPPqMQyLrrZUktbEMDXZcbF8KNUoSyM55WyfLYkq6VGp0Qtyj4AbGizteeShOI2Eh01e9nsQUgm3z-QGCwIXO26XkkEj2JbsBj-eZn8yN2QUfxFveZF4BNibDlFPk align="left")

This is the Jenkins file to deploy Tetris Application Version 1 on EKS.

Copy and paste it into the Jenkins

[https://github.com/AmanPathak-DevOps/End-to-End-Kubernetes-DevSecOps-Tetris-Project/blob/master/Jenkins-Pipeline-Code/Jenkinsfile](https://github.com/AmanPathak-DevOps/End-to-End-Kubernetes-DevSecOps-Tetris-Project)

Click **Apply & Save.**

![](https://lh7-us.googleusercontent.com/hcwIXdeSmh_Mje9hC57zW6rpQzkg1HL1samd_jsmZjlfWlK5GV_ze6UrrJ8A4pxSs1HKTjQbencEPjz4g18uctI2ZIlktccUdIQUaodVVbLd9CTZ4JvYUwAOiehI_X-hEpz0R6ClN1VR_gFZBHKKFz0 align="left")

Now, click on the build.

Our pipeline was successful.

![](https://lh7-us.googleusercontent.com/_35N_eOf50Q4sMosbnUbtPe_J2uIND44t3xhHMgmMhHTA7L28DMZ0J8D36sOMXJ28oJ8pkjJiIkokPuW1TqD2Z9TGXP0QrK3UruyLAggGDhcI1aIRk4g65nVD8IfSrvSi46UWojlGT_mGRBSBj37h6g align="left")

Now, Go to the **ArgoCD Console.** You will see your application has been deployed or deploying

![](https://lh7-us.googleusercontent.com/yPCDnAYaqxJOVWZYLZ7dRYJOK-qmHigZNtA9uEQGWfFAy4HATaSTJmIX83tMbSjmRrinjq_oGtzbu8HfZy6fWrBmuu3rSLTqeK7uO6Opa0vWmhoFkVsOBUIR8H4l4N-HLWW655eOhNneC4Y1gsrXw30 align="left")

This is our Sonarqube which will show you the Code Smells and vulnerabilities in your source code.

![](https://lh7-us.googleusercontent.com/49Aoqxmn9dMvWmNrykQ7Bsb5Y7tygQYnWM_3sV7eO85gFdU650vZ2DBlppn8LWKSzTpdecFzOr0vwXzpwDvS9lhfXzWImZUW3rtY_-cWgzE8z7F8E33oyc6blhJoSEEvsdKHLjrFZl8Buy_bj_sLQws align="left")

This is our Dependency Check Results

![](https://lh7-us.googleusercontent.com/iTe6LNoq2gXZHGhkxjbjRZXJORvTOCPLrao_-oOQzuHuMdK0uVhXkioLGJgcwt3c8fAVwwnEMina0wupzjPwcIiT3xoicNHkbei0VjDGBoAAxV_u18Z334-yp-TrA_Rax4Jr92EuTQZ8GUD2GRhc96c align="left")

Copy the DNS name of your load balancer from ArgoCD Console or you can go to AWS Console and copy the Load Balancer and hit the DNS on your favorite browser to enjoy the **Tetris Game**.

![](https://lh7-us.googleusercontent.com/Z9XiJgxChVtmQnZBGxxkLfl_oKPlK4rOJjGkHMy5LIf0B3V5Wja1V8rUkpOinsKPfxJpgWGUDIjbrDG9je_NPXp2xUmCTqrB8sdXiqVuCXIO5uv7ohbwzMTa0KVuaGrvyeFfqKAO6kNBR-s7sXup5aU align="left")

## **Step 8: We will deploy our Tetris Application Version 2**

Now, suppose we have done some modifications to our previous version to make it more good in the sense of GUI or anything else. Then, we will have to deploy our **Version** **2** of our same application.

To do that, we will create a new pipeline. We can do it in the existing pipeline as well but this way you will be able to understand clearly.

We have a separate code for our Tetris Version 2. In which Dockerfile is present, so we will build the image and push it on docker and then update the same manifest file instead of v1 we will replace it with v2 manually first.

Hope you get the high overview, what are we going to do next?

Let’s make it and finish our project.

Go to **Jenkins** -&gt; **Dashboard** and click on **New Item**

![](https://lh7-us.googleusercontent.com/Wf9nTbwud0aSUhdvHWuebpxTU1SnEXJMLoFF6YMB168XlqhKp_QqzIALFOokYnYZqT-laj9reUZHUIMr31qOV8OwspcrrAH1qnuUl8KdgqCJfvQJliDwC2K3wYQBT-sG9TkOS2Gm-bJZnPOOGHyqeEg align="left")

Provide the name to your **Pipeline name** and click on **OK.**

![](https://lh7-us.googleusercontent.com/r0JgQqrm4BWrNulC2GV7h9l003tX67pN28t1NW6MZuhPP_m6DfVqH1EXloHNrcd6kzFjdOy7ZF2JWyCNymPYpmfvcU5wdXctsMTQAeCV8EDkEsxiuu7GR5RGyO3cOgrV2PiwK0NTam6-KTT3Rrt4-GI align="left")

This is the Jenkinsfile to deploy Tetris Application Version 2 on EKS.

Copy and paste it into the Jenkins

[https://github.com/AmanPathak-DevOps/End-to-End-Kubernetes-DevSecOps-Tetris-Project/blob/master/Jenkins-Pipeline-Code/Jenkinsfile-TetrisV2](https://github.com/AmanPathak-DevOps/End-to-End-Kubernetes-DevSecOps-Tetris-Project)

Click **Apply & Save.**

![](https://lh7-us.googleusercontent.com/Q0kkCz79NX4h5qS4zb3AEPH98kQ3EphWWNCCRyfpKxFYEzKwczYULDe0nxUIK498qvu5pEx4F3UyZ0ohCxP4_V9BVAWfIAdMBU_GXArjhVasBvXfuUmf27fPF9XWDfR_sY7AlXzQ4V1cH-HZWE2ej_M align="left")

Before going to **build** the pipeline, update the manifest file.

Replace the v1 with v2 in the image section.

![](https://lh7-us.googleusercontent.com/aOvII7MTUGYeEs2bh4CjWoMe7gxFRUa7jA3Y811Y4A09Z_okqbQwUgaBe9QB-G9akrM7QVLf-HpBLRKSawb_PIngJg6xomGiOTMgN481WLExa9ZADnEYGLc9AOJjwtuiBiAts2pJbQkEKRQ-ORnxBYM align="left")

Now, Once you click on the build to deploy our Tetris Application Version 2.  
You will see our **pipeline** was **successful**.

![](https://lh7-us.googleusercontent.com/okkeELEy8rN18OctGXjoec0_zA8rgenzboBg7WN_rNT01ZvTXCcR2EoldYcQF7ySvZzx8_qB-RQo98kiRSN8Ompe2Y_GzusrsXoBDa32ex49LlUy9v85wWtwut2aa5Pqa50snDddZkP4dmljENehtYI align="left")

ArgoCD deployed our Application. Now, you can click on the **service** to get the **LoadBalancer** **DNS** name

![](https://lh7-us.googleusercontent.com/k3_EPscnxVnSS1a5sMXXM8peYjO5SdQSatnx9JZFx2SAbeDH7aRLAYAGFv4Xcb68mVVKprl57P3XpNqw52O--Z7zMh_xH2KrvdA_B5gmJD2bVsmx-YskFstxF9YRrQY9UyMSdLb1cn588PLaE3Vv4vQ align="left")

Once you hit the DNS, you will see our Game is live and you need to just click on **Start.**

![](https://lh7-us.googleusercontent.com/RDSypop5fUe7R2JHuBxhLeZ4kgSq4l1drBJTvLA3qP2iSsY_Xrr7jGt8zq89wplvKPhKTD_O8zSz0uCQ9WxX9Oq5lEbfLHWzz4Pperab8tsG3US1JyCJXjxp_W9NpKOTzWqRHyiUwmAmUCvdPI8qBDE align="left")

Now, you can enjoy the game.

![](https://lh7-us.googleusercontent.com/gdAowydUu_K8S1PfZ3rnjp6PiR3-hMxj2DAY8kbWUQDt8QryLTD9h2zsio6CXkGOeWhr4xXrmLUz0c75qfUq5YDvYmcEw6BycboB_cgBM--VFvBeyRMMGSP9FqKZh8iQ0o88JsA7IUeBYiOGnnWoYn4 align="left")

## **Cleanup**

## **Step 9: We will destroy the created AWS Resources**

Delete both the created **LoadBalancer** manually.

![](https://lh7-us.googleusercontent.com/JCag11vnbi2FQ-fpBxxZRjvphYgndIxDEetQSaN5WWnwMUWDbsPSZDy1WQdq6DoLrqctnBA6RHgcS4u_JHVyFTb49HKRNj5tQSE7eMNa66fEmMayPAx9w1w6i1PD0JZtqAgmu6zpOFTUU3Ah57xHhiw align="left")

Select the **EKS-Terraform-Deploy** Pipeline.

Click on **Build with Parameters** select the **destroy** and click on **Build.**

![](https://lh7-us.googleusercontent.com/aEE3F9cFIFBLJkPIZQGWneySLTXrwchi3Q_xl0zn4w8Za_ORks3W32v-Ba5t-ve4bN11ZoWOLdBfASbvXTOkhd3ux-kxK8XXRmEUULdFbWHrNPYKc6oWNMnSDLjofjCFn8G-fRUF3IKpS_Z0hQAL9yA align="left")

The Pipeline ran successfully which means the EKS Cluster has been deleted.

You can validate from the console as well.

![](https://lh7-us.googleusercontent.com/DkirK8kasCqZlHK0HBzl00POUBQo5YIHZ38MRY83QAIwdMoxO2sO3eA2NQ3GV4NBoCWVzoocfzL8SzH0GwBtVoH00k5LJazjapqEWywim9ZT-_oRyU42oMbzoTs0myNjuuXQMpHIYRdT1NdF3p3zH_s align="left")

Now, we have to delete our **Jenkins Server**.

To do that, just run the below command on your local machine from where you create Jenkins Server.

terraform destroy -var-file=variables.tfvars --auto-approve

![](https://lh7-us.googleusercontent.com/WpHYr7yjM_3vJzHhmE-abpTi_Ab4wPY6lNxQKJYi4YVkmVXBP13jVCFrM60hN9I4z9wADlLp67HW5nSgFSNAhZMUTjXiSMQjFyocmNbju5-fySYkEyjKC1ld52_nTMxPIkCn3aSiKEoolPS6iLcK2TM align="left")

---

# **Conclusion**

Congratulations on completing the End-to-End DevSecOps Kubernetes Project! This project provides hands-on experience with DevOps practices, AWS services, Kubernetes, and continuous integration and deployment. Remember to build on this foundation and explore more advanced concepts in your DevOps journey. If you have any questions or want to share your experiences, don't hesitate to reach out. Happy Learning!

Stay connected on **LinkedIn**: [LinkedIn Profile](https://www.linkedin.com/in/aman-devops/)

Stay up-to-date with **GitHub**: [GitHub Profile](https://github.com/AmanPathak-DevOps)

Feel free to reach out to me, if you have any other queries.

Happy Learning