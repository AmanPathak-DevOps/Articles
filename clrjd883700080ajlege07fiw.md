---
title: "Advanced End-to-End DevSecOps Kubernetes Three-Tier Project using AWS EKS, ArgoCD, Prometheus, Grafana, and Jenkins"
seoTitle: "Kubernetes Three Tier Project"
seoDescription: "End to End Kubernetes DevSecOps Three Tier Project using Jenkins, ArgoCD , Trivy, Sonarqube, Docker, Terraform"
datePublished: Thu Jan 18 2024 15:27:25 GMT+0000 (Coordinated Universal Time)
cuid: clrjd883700080ajlege07fiw
slug: advanced-end-to-end-devsecops-kubernetes-three-tier-project-using-aws-eks-argocd-prometheus-grafana-and-jenkins
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1705590007394/f10496b2-ae05-47e4-bf3a-6fab502c4179.gif
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1705591564682/f43deec6-f0d2-4bb7-9c48-eea7bbf200f8.gif
tags: owasp, docker, sonarqube, kubernetes, devops, jenkins, devsecops, jenkins-devops, argocd, trivy, 90daysofdevops, aws-three-tier-architecture

---

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1705590036578/5d2eb77c-f9fb-4bcd-9b81-1520b478d6bc.gif align="center")

# **Project Introduction:**

Welcome to the **End-to-End DevSecOps Kubernetes Project** guide! In this comprehensive project, we will walk through the process of setting up a robust Three-Tier architecture on AWS using Kubernetes, DevOps best practices, and security measures. This project aims to provide hands-on experience in deploying, securing, and monitoring a scalable application environment.

## **Project Overview:**

In this project, we will cover the following key aspects:

1. **IAM User Setup:** Create an IAM user on AWS with the necessary permissions to facilitate deployment and management activities.
    
2. **Infrastructure as Code (IaC):** Use Terraform and AWS CLI to set up the Jenkins server (EC2 instance) on AWS.
    
3. **Jenkins Server Configuration:** Install and configure essential tools on the Jenkins server, including Jenkins itself, Docker, Sonarqube, Terraform, Kubectl, AWS CLI, and Trivy.
    
4. **EKS Cluster Deployment:** Utilize eksctl commands to create an Amazon EKS cluster, a managed Kubernetes service on AWS.
    
5. **Load Balancer Configuration:** Configure AWS Application Load Balancer (ALB) for the EKS cluster.
    
6. **Amazon ECR Repositories:** Create private repositories for both frontend and backend Docker images on Amazon Elastic Container Registry (ECR).
    
7. **ArgoCD Installation:** Install and set up ArgoCD for continuous delivery and GitOps.
    
8. **Sonarqube Integration:** Integrate Sonarqube for code quality analysis in the DevSecOps pipeline.
    
9. **Jenkins Pipelines:** Create Jenkins pipelines for deploying backend and frontend code to the EKS cluster.
    
10. **Monitoring Setup:** Implement monitoring for the EKS cluster using Helm, Prometheus, and Grafana.
    
11. **ArgoCD Application Deployment:** Use ArgoCD to deploy the Three-Tier application, including database, backend, frontend, and ingress components.
    
12. **DNS Configuration:** Configure DNS settings to make the application accessible via custom subdomains.
    
13. **Data Persistence:** Implement persistent volume and persistent volume claims for database pods to ensure data persistence.
    
14. **Conclusion and Monitoring:** Conclude the project by summarizing key achievements and monitoring the EKS cluster's performance using Grafana.
    

# **Prerequisites:**

Before starting the project, ensure you have the following prerequisites:

* An AWS account with the necessary permissions to create resources.
    
* Terraform and AWS CLI installed on your local machine.
    
* Basic familiarity with Kubernetes, Docker, Jenkins, and DevOps principles.
    

---

## **Step 1: We need to create an IAM user and generate the AWS Access key**  

Create a new IAM User on AWS and give it to the AdministratorAccess for the testing purposes (not recommended for your Organization's Projects)

Go to the AWS IAM Service and click on **Users.**

![](https://lh7-us.googleusercontent.com/qWeXrx8eV8z2VkWvQeaDKfw4Xj3zIFUCIXNvikSYNJJMnBOyZhthZnhoGJfi_6mWuFZuqcCV09BXs3UEWAzbuHjfWh0K8AugQzTjgkHJwGi-bFmgqBwihfzqzCeAaDyyl-9kpzWJJDfc2xWWDnd7_9c align="left")

Click on **Create user**

![](https://lh7-us.googleusercontent.com/087hKd6Twa0lmWGuD4qTVawxq7-iLAciRKC1Syt6dG9i0rJbnWmdtZmaCNnR2gMEO3N09e5UNrPRAHN140xf_6agmOu7jfS3dxV64ipv76OrqhlTQSEvTn7AzKnKe4QwlbyASxK_JaCwGPb10odc_f4 align="left")

Provide the name to your user and click on **Next.**

![](https://lh7-us.googleusercontent.com/Cg-K2bNAFSgg7yoLbj9RXEjv2rm0QL3d9BrKis9N-N5fzlV4jMDweFPQPkFKolc07M1o20Wfr25syZ4wGYHdXgpUwjaZvTDOYmQVqIes2PW7y865A0QgnDkuFDqqNQsCb-K7w7LMdlfz0CnDsVN6tkg align="left")

Select the **Attach policies directly** option and search for **AdministratorAccess** then select it**.**

Click on the **Next.**

![](https://lh7-us.googleusercontent.com/ya1LWwasLqjVKNhPodp4E_F70JvMLMIBNxtRpKn1tPO2S0hAERlbiUTNwMNYO_jXCzkDcTv4FJuTqlIFmd7x5e1-qoQEFYnneYcOvAApwz8aA5b-8ENbfjRIcTcU63qN3yZQNogLKlfzOMk2OmR64rg align="left")

Click on **Create user**

![](https://lh7-us.googleusercontent.com/5jwEMkJlzAGyEEGElzMHTrSsOoAgmpVeBWCaeD_XXsvMF2PAfgnLItW9MzMj9yVYWjiAIXZpWIEIQqAC_Z8F8lsKUxgsnGgpiKlXSTY1ArbJpKqmKTK6Nh-Q00k-NkekuWSWFhzf1QdAXpSGQaaH_H8 align="left")

Now, Select your created user then click on **Security credentials** and generate access key by clicking on **Create access key.**

![](https://lh7-us.googleusercontent.com/4EMVcMxwc9u57BBRhnkgw2Q4tLOXfCsli5Ullthjy9zE9NVTb3IVSbKDyDY8QHK64rGKRO4sML40PAtPyPhj_kco6F_TDPt-y6Em1SjE-3vjsTEHXss9ikd__AnZzMPh93oSDVl8nwUnt19Sbjg2Qig align="left")

Select the **Command Line Interface (CLI)** then select the checkmark for the confirmation and click on **Next.**

![](https://lh7-us.googleusercontent.com/lbVrF-tN4LP7UD4NGID1DzfvpeuiKe30TnjOA0iR4-qMVsIM8tWom1-P-HJevwfoNwRyAdg8D1_SR4m0Z78LWqUT93_63VzVjld5yjUnsdz46N38E0TBUB9m0FHnthpBdoPlw9D2k5Yz30s1Sw7yqrY align="left")

Provide the **Description** and click on the **Create access key.**

![](https://lh7-us.googleusercontent.com/dNbdTpA4LV3vOSKWaMfAq7XUDIazNWJfWaAv3ri-yoWguoM_xX8Nknr6EMDDevbiMkWxYEhdC2E2Iq2vd3YuHid7LbD5R-GPrsuCJodGjBQZ2l0Z9-aZlqCLe5mlFhgZz7VLJrKHU-dGL1GXyS4RKXw align="left")

Here, you will see that you got the credentials and also you can download the csv file for the future.

![](https://lh7-us.googleusercontent.com/b2O6Ii0g19DtBm01pAxUcE5WVrWknSQoU1GrQGT16dXV9Fo_9VUzWvyNMVdXHuUV5bkrCWQbrkrX-k8TjMRkt1qdzO8_k75vsdl_W-CVAVg8kh_EzKYLcSuCyunFoWDT-k8fyghQkbOvDUnyhbVznoY align="left")

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

![](https://lh7-us.googleusercontent.com/EbRLaI7uNDKAGI_ihRbTSwg_mwo4wMuDbdpInlqEFz-KfOmRqHJkU1tV-NpOIJUmjMCbyGDnzsHwexMVE5WZpSpERNfz2EnhLKM5ymBFiLrKWU9ULClpClG35gZ5lCLE-KeZssTRatdb0hhDOE31OUY align="left")

After doing the changes, restart your machine to reflect the changes of your environment variables.

**Configure AWS CLI**

Run the below command, and add your keys 

```abap
aws configure
```

![](https://lh7-us.googleusercontent.com/GRrZY0Mj_sLkRCJVVOUyuBB6TJ8BCpldjt6cAHYugvm6Q-81bxjPMT4h6TLcMsDQh1RdkUYY8KGRxjmPHEF1dhJ_XNv8isnyr1oHHJKVYXuzzAeg53v294yE4fcOhK2NE2Caog8ljlpOZ_uCxWHY3DM align="left")

## **Step 3: Deploy the Jenkins Server(EC2) using Terraform**

Clone the Git repository- [https://github.com/AmanPathak-DevOps/End-to-End-Kubernetes-Three-Tier-DevSecOps-Project/tree/master/Jenkins-Server-TF](https://github.com/AmanPathak-DevOps/End-to-End-Kubernetes-Three-Tier-DevSecOps-Project/tree/master/Jenkins-Server-TF)

Navigate to the **Jenkins-Server-TF** 

Do some modifications to the [backend.tf](https://github.com/AmanPathak-DevOps/End-to-End-Kubernetes-DevSecOps-Tetris-Project) file such as changing the **bucket** name and **dynamodb** table(make sure you have created both manually on AWS Cloud).

![](https://lh7-us.googleusercontent.com/UnuPYmSZUMPnugBrMjFbbqo5iaSn4lGN6vGK3qkGvC1tIrJb3S5FXl3ovXp4lQkZ34FVpN2hRyBL6C0FH_ifXRnr4HldYGUGcTIHx07Glg4ykpvtvTUV08cwBSiQxPEQFtvcEXXkYwPqA7H0WCyUdNc align="left")

Now, you have to replace the Pem File name as you have some other name of your Pem file. To provide the Pem file name that is already created on AWS

![](https://lh7-us.googleusercontent.com/tJbavVaBU-V9dwavR6A9WmW6VC87OD2Uxl4iLTt_pyR6D3GQglWbsb5edTUbZrjNFSmfzWrurYmnWBup0HVrblane3Luk1VkJmlIBPJm7KiX1LhZrvUVKa9WomgHWNcA1k4RX3Pn09h-aSiKA-NOJbk align="left")

Initialize the backend by running the below command

```abap
terraform init
```

![](https://lh7-us.googleusercontent.com/Fscfeq9H2eZNZDeOBGSpHu-5Pbg6cyzoyLzoy6ugWosxU3s3Rgq6w8nT_DiLq8EBCzE819GrFDUn68Ia0pyZYJIO86TKZfqfZiHm9mS3Dr2lg85WLlvEzU-yzKA0hg1-W01lw_7gTHOC2GNIoWZjzkc align="left")

Run the below command to check the syntax error

```abap
terraform validate
```

![](https://lh7-us.googleusercontent.com/x9bTUIoJEVJAR1b-8NEPxLiAHDL895XcPecVWKl4lDGsvRQ7RukedESGEq33PTuDqPYcox5DPHQu9Ga8HBV1O0aRbz7Oy9YlP6kwMEC73Cq5ETo76YEicmscLlLndPExKcKOxGNL8gByBrB3vmySe0s align="left")

Run the below command to get the blueprint of what kind of AWS services will be created.

```abap
terraform plan -var-file=variables.tfvars
```

![](https://lh7-us.googleusercontent.com/ktOqHxXT8-Mrnbh0lP4QZ2e8lURAApCP60DDI0kNPZUCbJOVtfA2Bi7Ssm_hQkS_Uyrv_fYwfiojOYJWJe7-hYG8OfhtzTqFJ9bQmyxKsH7pwZTRv9t96OCXg6XSXXb7BVPAMUW5_epU3aHzMoI98UU align="left")

Now, run the below command to create the infrastructure on AWS Cloud which will take 3 to 4 minutes maximum

```abap
terraform apply -var-file=variables.tfvars --auto-approve
```

![](https://lh7-us.googleusercontent.com/-wqJC4hhK4mR6nGzPuwOksf6G3H02ku6wNY_yAKHJufH0tUUhtecDDNMrc1feCqI7HvoIEHFiCSrZdW4GmQy1yPuQ-S79Akh2kvMHN71mEEmoAus_uHeHgQBVS-HJSzDkumkhOuloGhlCLsOuqceilU align="left")

Now, connect to your Jenkins-Server by clicking on Connect.

![](https://lh7-us.googleusercontent.com/1Hb1l3WumQomXT6E1sZouH_b1Muw8Eihu5xYYGzcozhqd7H86dVev-Qaqu_nIxXIR1DiRY-rQb4twZh7LOkMmQrVXSqO4cRqnXzQA6Kypm3tOMev8vtAp-Wi-QFJth6Vr11cNOSjix_UDtOy55Gjzbg align="left")

Copy the **ssh** command and paste it on your local machine.

![](https://lh7-us.googleusercontent.com/H9eXoLH13Y01sfCdnzyKuDM1mcF_yvKPVrN2dFUv1dAs4jhdq1b5ltDkZVc8wIwnmmowmgi9gWkcvX9qNJ9ZSdUbpeLH02eJUItBRXyXMOrqYwmm6Sb1cahe3W088-En1brHlsmhlOzwOJjO46Rq9a4 align="left")

## **Step 4: Configure the Jenkins**

Now, we logged into our **Jenkins server.**

![](https://lh7-us.googleusercontent.com/eCeUUnNqJnERylh0Qrpp-BLzk6LEudzQI2N-fZHWNtEXWAQvr8K_upyayN_YKCvdl7hbLkdsB63gZtcGGzYJ3JHrAVwmW9t_LEfTyOUa1DwVOYVQCNm000gK_0C2LsJSdtcQ1oiSnNe2C3Kl0EfRWTQ align="left")

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
eksctl --version
```

![](https://lh7-us.googleusercontent.com/Ju2CdZFi4GKoHvrXj4ps6Spx57bNQ30E4x1SYICaUGK121uunYgni1E-usbf5kUF4Wj_-FMiagX8TbpeUf4-Z2CW2dU7TkGA5B2HL6ViBALXK7CN3F2VqZGI-yhf7JPeLA7cwse88tySYAbYtECHZts align="left")

![](https://lh7-us.googleusercontent.com/fKM49yS0qvI-_PGY73forV-bZghSeNDUQhcLs0uo0eryavliqDUfB9pqyNyXki-faRt8_PEx9_qmyyc7yV_Hell_rX-fottwj8r30V_kMcEO2tXypwcmRZvBeXW49wwBu34yiqJq3gHwStSt47DVceE align="left")

Now, we have to configure Jenkins. So, copy the public IP of your Jenkins Server and paste it on your favorite browser with an 8080 port.

![](https://lh7-us.googleusercontent.com/z7j6DxVBDKkju2S18CBdLZuOu6PekOlVkiuraOHNhN0PGtxbzVdsSsO5ZBKr8quBnEpR8FVasboN25pZdhlxUVDeFt2PEb9FyZbS_EclLh5FtB4EHXye21BynpYZJXEVm8OW17QdSHTkLhpSEMvgZNY align="left")

Click on **Install suggested plugins**

![](https://lh7-us.googleusercontent.com/XTRK4qgUto7tfy2iWT4uZZkNM216E5_om57wa-dm1PZ4wIlJUotjh8lZZX_O_JEpxWOpdP3GFeTzVx7R4namyqIUB4SwwNCdgM8xvQTqaMtDUvDNEkMAMqo6LlIoI7iTGunJPVxNKu916mj5dqpK3yQ align="left")

The plugins will be installed

![](https://lh7-us.googleusercontent.com/xUQ_FXAYbHo1j46Uv_p5gALmxWOdUDjOqEaXgeAoDeRi3lYgT5uPfLqHts7MJifwjbOFCXC08HvFFhC-5wypeqo1gPdvVMWe8piHYGy14AHMeeCJO1gUXY9nI4s7aaPEvlZ59A5gx_vaBcNtTp7PJlw align="left")

After installing the plugins, continue as admin

![](https://lh7-us.googleusercontent.com/XaWOzYSQXbXO_T7qeQADgIf7boxluWuhBhgmU-RxCrM_iYG_ObuBiFqEViJQMhL2ipm1WaFqjr2wDKVutaiNfEoFuTVjyb3OQxdh9cxt0h8InVSNqOzU051eEPR6vbkq1douzAH968kQcTIJWut-45s align="left")

Click on **Save and Finish**

![](https://lh7-us.googleusercontent.com/66G6gP3Tp5PZsEXufDRLe8DXPh-9lU5Xmc1LEZ_e1UcoUeaPlD-dZs3TalUHWxbOFOfrfsWlVOZaNjbTH33-SFZENcrg3jd6DxGB0t-WXOAF1MffUVx1u9E4W1rz3j2TB_rxUS7XGLNCKZ1SD2ypUAI align="left")

Click on **Start using Jenkins**

![](https://lh7-us.googleusercontent.com/xkHB7y7eRrbyvFz3BVww2kYf2fsaW4Am7BTBomIgx-FZPvodaWJC2LpoHnkdSv-jjIWW1_fMycBmuRdb99la_Kz6QOrqyUMd-uycH0wxoE00azeMtUEkkUs0Vo7bF44CSJpuchk9I2Fp5_icx8M83pE align="left")

The Jenkins Dashboard will look like the below snippet

![](https://lh7-us.googleusercontent.com/DPaCIBwIoiUjbKJ60Nw3nJfybA9Ox1FhRsQV5cJsTTVrLAeh2detfff8wCaCGm2Veq8aMkFvMUv935XgaQuBDbohLxZHZmj6Zd_2F72gERL5JFs7hW9v9iwgWPLyYmBTsxdVGJPPIH_L4mwkobKgkbw align="left")

## **Step 5: We will deploy the EKS Cluster using eksctl commands**

Now, go back to your Jenkins Server **terminal** and configure the AWS.

![](https://lh7-us.googleusercontent.com/k80lTbEw2tLsZof4TuTydGKXbMZohLIWgU-TZEBnpQE4RFhbXGRdjRh-SOsXGTCiUDm_JfaFP_swIMAwZ1e9lN4VZx2OahCnCOMBsuStgRSFSMMfrKdTidYQ__HniApdXHdnqywM389kcZEqMwqoQkw align="left")

Go to **Manage Jenkins**

Click on **Plugins**

![](https://lh7-us.googleusercontent.com/LY6sm5gCYmz64RXJzb-j_lqNtss83kZPu_rNbOMT_lAO_A5YMEoq17QBK9fJ_GDzWlAT43JxkHwu-sgzT6gUU0R43952fizvWBFbT-HGssdRhk-MqiOdmCEAsY-iQtwbOEyWBNlgwMqYCIEswJDSbOo align="left")

Select the **Available plugins,** and install the following plugins and click on **Install**

*AWS Credentials*

*Pipeline: AWS Steps*

![](https://lh7-us.googleusercontent.com/Pq7aYPFiI-BBsrCgOdcWnlUnL2zMqqnr0exYWsTMkV5_I9lTme1nunnfReN5LnRUH8uQ8hgXWl3OHStY-Lt1BXU5ghGtJrflgF8EXHeenIWWIQOqJ9-2cjNwGKzeq1es9MwzUW12oOodNTSJ7u_czjY align="left")

Once, both the plugins are installed, restart your Jenkins service by checking the **Restart Jenkins** option.

![](https://lh7-us.googleusercontent.com/qMtCK5ec6FLj6uatoInPi6Pb6JWynYWrPw3_-VP6dusv2qOFiqsLVYzUQauTh2epYg1EMufhT05k31uC-G5P_fMngd32WIkZ6nv42zpH0aptvmy7AOZjJ9gSy_kdtm8XpHo-CNTLBd3A9MYPF-FlvLk align="left")

Login to your Jenkins Server Again

![](https://lh7-us.googleusercontent.com/RLzasNxnCwIM9kZFLqApYPtlnMU-Gnm8zOxaYFKr6rnbyicmlENiTwr994kWMDlJENh3yeur9UXhJ5AfkHeB3bM-nPQdlResdWm9WCW_InW025ZqDIlCR5vGy_YNGCq8Ki7ZLa6ZGMisT2oFYPgFNrI align="left")

Now, we have to set our AWS credentials on Jenkins

Go to **Manage Plugins** and click on **Credentials**

![](https://lh7-us.googleusercontent.com/LY6sm5gCYmz64RXJzb-j_lqNtss83kZPu_rNbOMT_lAO_A5YMEoq17QBK9fJ_GDzWlAT43JxkHwu-sgzT6gUU0R43952fizvWBFbT-HGssdRhk-MqiOdmCEAsY-iQtwbOEyWBNlgwMqYCIEswJDSbOo align="left")

Click on **global.**

![](https://lh7-us.googleusercontent.com/TNsIchMzknCaKOGia5y36sz9YiNnbX85Vvm-am0aoCA1DavUXz2ViyWBYe8VJLtB1u-w-QhdbSHwJFn618gzh4v9C-Bg4E9Br-aZblwl8oU0VyqiHaHmHmDoBKIK8Zjf61ONy4r73JtI3pv8WSRHzLQ align="left")

Select **AWS Credentials** as **Kind** and add **the ID** same as shown in the below snippet except your AWS Access Key & Secret Access key and click on **Create.**

![](https://lh7-us.googleusercontent.com/J0em6FvpSgie1SUZ-JOM9yvgcNj6QGoKIerroC2LkoBnzTKMatfLxmqPpX49OCdil8vMgVhlRXAwWWZ6XN1rorwN-EJRAJqkSPHpqZ43RwJa70umtWpaZn5zxzaTQinflTAPObXhZeEL5dGEQ1YzTaA align="left")

The Credentials will look like the below snippet.

![](https://lh7-us.googleusercontent.com/DoRjC9lV8JN-xj3zIkfjF2BJYKRd1iv-Q9etMf1kyLW8jLyQeNUp4mdzV36T1qx43DDi7c38o0u8WFEIqff1KJHk8gDdGKtwAQcb01ZEzMvTcKnMth96vmtnTWxkrC4d26ZwIoxn5307G06oq_dhev4 align="left")

Now, We need to add GitHub credentials as well because currently, my repository is Private. 

This thing, I am performing this because in Industry Projects your repository will be private.

So, add the username and personal access token of your GitHub account.

Both credentials will look like this.

![](https://lh7-us.googleusercontent.com/KcFgp3msFaS2HkXTsoBWrfdSym9dfycJ1OuLgHNXXTP2hAT3kpjhUy_n4QxOhVhbM29cwkBWq29e2rsrJeFHm8O9xrNhTLqxAp6qQTzcwzpA3uet8LC1LrVLpvbq65plrYXhWWZbAcq8GVxnsyjWumM align="left")

Create an eks cluster using the below commands.

```abap
eksctl create cluster --name Three-Tier-K8s-EKS-Cluster --region us-east-1 --node-type t2.medium --nodes-min 2 --nodes-max 2
aws eks update-kubeconfig --region us-east-1 --name Three-Tier-K8s-EKS-Cluster
```

![](https://lh7-us.googleusercontent.com/_yVFTlNZts9PYk2o0k9V_VQwK8Hue55Fddg7Dsddikd6lPdxUI-CYqDNVK78EL3K2JeAvx5CP3-c-u-qPzOH1N0l_8VJpsbzYBYxmYGnSSl0ybaBbsaac65dQGMdp-CzZVRPhiO_39SFi3Sx8ZR38m0 align="left")

Once your cluster is created, you can validate whether your nodes are ready or not by the below command

```abap
kubectl get nodes
```

![](https://lh7-us.googleusercontent.com/uJXx7ek1OBt23J_tbwkVop3u_K5evmBlWT7YptAjbYR1Q-kma9-BKAuSmacaUhfzCwiycu7qg59441W-ZN8iCgjerPY8za7mNPSL4QiI4KaaaMIQxuWfFojOJF1NERD5EaRS4gTzdv_T_TKoWlBC5bw align="left")

## **Step 6:** Now, we will configure the Load Balancer on our EKS because our application will have an ingress controller.

Download the policy for LoadBalancer prerequisite.

```abap
curl -O https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.5.4/docs/install/iam_policy.json
```

![](https://lh7-us.googleusercontent.com/K-YkOOpVwGWD7dizVTXCSrwqhyV_4TGc3BA7TKKHr2iCc0QxiC88EBrl3BYRrHYS3osfAR0QjZnlM15ZOVnTSQbhHfeqPr8oRqbG1x5HWSF4VPDBliT6i7W9PjOHc9Osj28L3PyA1wq8UjY78YwuXoU align="left")

Create the IAM policy using the below command

```abap
aws iam create-policy --policy-name AWSLoadBalancerControllerIAMPolicy --policy-document file://iam_policy.json
```

![](https://lh7-us.googleusercontent.com/G0piu7ln_y6UHvQDTQALSSmP_8agnAVBZmbZkJNVTmYNRJURfV1VXG7snj-firVXgBCpL_4nV1Mg0eoRl4pgTfivGn0yz-cqsZpFEHzTUocfeOwpFMolFPWsbAALoe6IjecWNDvJas1f9zz8Wr6zRsk align="left")

Create OIDC Provider

```abap
eksctl utils associate-iam-oidc-provider --region=us-east-1 --cluster=Three-Tier-K8s-EKS-Cluster --approve
```

![](https://lh7-us.googleusercontent.com/phhE8PYjOEIDQAvx-Wyljucgc-z-wjLQqw5OzOjO2Y6DUPcyXH7zX1rSA9SCFelziOs4UJWwtCrEt_rrowwiI6KpeSaNlmgesy0jMxF5hN8I3kMPz0lQWFh6tk0V5pIcByzW8j8sQgsnMDKBfaLF7zU align="left")

Create Service Account 

```abap
eksctl create iamserviceaccount --cluster=Three-Tier-K8s-EKS-Cluster --namespace=kube-system --name=aws-load-balancer-controller --role-name AmazonEKSLoadBalancerControllerRole --attach-policy-arn=arn:aws:iam::407622020962:policy/AWSLoadBalancerControllerIAMPolicy --approve --region=us-east-1
```

![](https://lh7-us.googleusercontent.com/7cyZoz-pWaZ3WDoA7Obdko79OUGjjHw40ZsUpFabwCR98Zmf5d7foaS6ot7DqwcD_kwEtRKhcFRpftycpcY-ST6k3THTUQjiEpCRFjkXheEwVJSbDESvHgUaFN9dhQzlZ4AmZK7QenQ3GTdtTET-X0Q align="left")

After 2 minutes, run the command below to check whether your pods are running or not.

```abap
kubectl get deployment -n kube-system aws-load-balancer
```

![](https://lh7-us.googleusercontent.com/eqBGHebvlJjFKeuVpeNk3CpiRaOz44PDy7xSGd5BolzVFiW1QQ-3eZPU39KzVWjWvGlHsAKVLxa8HALcb2o4PBQcKpYLP6WZpdUJ77-srLLeQRzpEBsAM2WI4Hcaa7XLVfBEj9nAj6WHgRpsSSathaA align="left")

## **Step 7: We need to create Amazon ECR Private Repositories for both Tiers (Frontend & Backend)**

Click on Create repository

![](https://lh7-us.googleusercontent.com/GACkiHJSV6x1-rjDPcGyQltR-j3Ucsgw_2_UMX6ZVfZg2Po3kZ9gkgINCdKyC1FpYrfvX7rljhDWKbrW2WQhUqGGIIlU8j0bkWXGQLByFASozBirGQnmGhiMi4y3CQAqs_rayajqBfanXxlQC1w3kkk align="left")

Select the Private option, provide the repository, and click on **Save**.

![](https://lh7-us.googleusercontent.com/dmVRUycvlmab7yqkZZ11eR5aUNwzIUzBWJdEFDWuKj2Vq7T8svo-lQ7GOOKGvz8v6ZEykZ3i7_JUKUXvWvxoKkwcGrLFsHjK4UCf49wF962luRIjhzVeSKbdiEMrIDlBEVTmhR0_5zMgBWmhmBHzikc align="left")

Do the same for the backend repository and click on **Save**

![](https://lh7-us.googleusercontent.com/SfFak30giL8l5yhYNxfVBF_uQqBbG4bdlxKgvDZbZ_dIFM-C0qqXEeRYUuSomFV_nxDwl9NSj-55TVD8UW4005Codfwvy4c1X4GMMOLZ2cpY0ezQ-3eoxNf9fvZKqAMxQkC2ETrjppfaHP-CgJPmIoE align="left")

Now, we have set up with our both ECR Private Repository and

![](https://lh7-us.googleusercontent.com/ZzNyG1mtMJIFBRXkbkXZ6sTIASXAeOFggVi9akKN_PVamshJDQvZimU5POvbY3gJGlho9UPz0L0CkmlKhJdpK3TexKO9lEfxZ1AhiQ7e8IBSVzU_RweDcavL93Gl9yKEXLswRobPQQBYHwbVHK1M3Zg align="left")

Now, we need to configure ECR locally because we have to upload our images to Amazon ECR.

Copy the **1st** command for login

![](https://lh7-us.googleusercontent.com/P0_8ugXJmaBJ6ADu-lOK2eLJYM3wT_1nLhqxFn6G2gPO_PEOgVzu58skeqZfqvSnEI-ZWv711q3dQBB7xux53Q2Uh_GPFm345zDvIHBYfpQBwbUcym4cGJdi3_el6lqCEU53seYZUsVcX7v4cyoYeMg align="left")

Now, run the copied command on your **Jenkins Server.**

![](https://lh7-us.googleusercontent.com/UhHuq3b7V6m0c2luVluZKO6uGv_MIg5i4Z30PkVeK60RcvfrsHUO8aXHu5RKEvDy0oRdktHjJQro96HjoPPe99aOrzf6Hs79pjJgDB7-FjZzVYfL4Ycou80khvV5dJd-rHcKJHGTMTrGEgsm9SEChdk align="left")

## **Step 8: Install & Configure ArgoCD**

We will be deploying our application on a three-tier namespace. To do that, we will create a three-tier namespace on EKS

```abap
kubectl create namespace three-tier
```

![](https://lh7-us.googleusercontent.com/D0dlJK4ramuKQ_37VmsduO_-19xuzdIBbv91OL0JEwGQ2Ul0hhGank40eeUpShVmv2rZuOGxNrVR4Mvrx4vhpIFzyOBUUR0yZmhkr0NI7CpfhnBQtDhHqAyWYFDMUauIJFwKUs_lgTFlgjbAwVWzOsY align="left")

As you know, Our two ECR repositories are private. So, when we try to push images to the ECR Repos it will give us the error **Imagepullerror.**

To get rid of this error, we will create a secret for our ECR Repo by the below command and then, we will add this secret to the deployment file.

**Note**: The Secrets are coming from the .docker/config.json file which is created while login the ECR in the earlier steps 

```abap
kubectl create secret generic ecr-registry-secret \
  --from-file=.dockerconfigjson=${HOME}/.docker/config.json \
  --type=kubernetes.io/dockerconfigjson --namespace three-tier
kubectl get secrets -n three-tier
```

![](https://lh7-us.googleusercontent.com/dz-1SHT3A1Y6RkU201Kgs9xoFFdDTdHqKRIg2YTJebLUcFy48_uz0_wHYZKNSg5XnYhjVSdYqnmRDAFdh1-MvEgFHi36EZLhbAhSqXDH1Kl2mVY66Ni8fc6HZpe9mp6rCKwVoG1RLWswpQVSrzVfFUc align="left")

Now, we will install argoCD.

To do that, create a separate namespace for it and apply the argocd configuration for installation.

```abap
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/v2.4.7/manifests/install.yaml
```

![](https://lh7-us.googleusercontent.com/zK2QFZo0H5Bj6o1-ahDYYssZLqhjJ5doolOEx30W8vloN2x_T4ui_Z9ER0s3aCcko_jbc6O4yxc1AbbwqevQoxc0XR258sPKFXYd9HVTuXJ18xWgfyXO-VPLiRxpTP_PlIb_DD0loPbYjNzehZncld8 align="left")

All pods must be running, to validate run the below command

```abap
kubectl get pods -n argocd
```

![](https://lh7-us.googleusercontent.com/phhcK5x2HYhoP0Ej9-tW90PPJUbYOgsnGvGSv15a1JcCHTbfTdrMRSq95l38qGnBsfaRipkflY2R92cU9Ued9Lp7XoJM-DktgRMZqmFJJbElL4HqI-hZa23gmLJKDiGikxfNcX6A06UTGPqu1-7aTfg align="left")

Now, expose the argoCD server as LoadBalancer using the below command

```abap
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
```

![](https://lh7-us.googleusercontent.com/vNi9NKh5KTfMF-RIsDzPEaqOncoYkeQBIR4MPIL74zQD3tWcw3r-7RkJTJON8Y9CNCVpTfatd3--MevJ6RtoyONY-0Jupl2EejUIChUxC_CNEjR2U_Xw9lDaTmT3mujtsomOU-dxioJpqpgJSV-6B8U align="left")

You can validate whether the Load Balancer is created or not by going to the AWS Console

![](https://lh7-us.googleusercontent.com/TbeMLjCl2XKRe7fNGSxaugIA4-mEY2OFRvlnGBL8MFI2y3_pj0rfNOmHQaBYNyGbOnquzb_R8zqI_OYWPt2x1L9w4ujU2rzOHGkXriWS__w37UwXJyzMERSxiA2rj7qhn-FLlOFWtMPbNVWoBcGlZDg align="left")

To access the argoCD, copy the LoadBalancer DNS and hit on your favorite browser.

You will get a warning like the below snippet.

Click on **Advanced.**

![](https://lh7-us.googleusercontent.com/N45AvtSDaCznbBTK2EGZA6Y4gEFg7FieXl45VrWWOHsF21hmGGFaWnzQYo9lrzL8LUeTEGnLXFdWfR-51IDF0Tzu9B_hbCwCn2ATMx5S7uJv7pIOqfXxZg5S8oA2FDH3DvR6q2E1DUvoyLSchC7VS48 align="left")

Click on the below link which appears under **Hide advanced**

![](https://lh7-us.googleusercontent.com/7swU_BIo09AVRItH9RJ-M-jERxHO-_bya78qydaIP9kKrdubNDGUEIETcdDQ3cyMXH6yjpviq5KHKNjMR4F4ALb1lKE1oZIhMacPU6GujyMhvMlfDS9S9h3WdLSTcUU-V0Pw8x6QWreYF1kjZl1sgiA align="left")

Now, we need to get the password for our argoCD server to perform the deployment.

To do that, we have a pre-requisite which is **jq.** Install it by the command below.

```abap
sudo apt install jq -y
```

![](https://lh7-us.googleusercontent.com/2z_nyfKSBgik_Lc3Rwj-Xd4btig-kJiIj49fJ9EkhLnwnSfgjdPHMBczoghSbJ--jvrL3ZHgv2Jy7Ee5v5SOSQHWAFUDzDGQPOBbMVH9tyBKCfjRg8qXt6bc_vUZCPzE8Nge4_cxf_uFwWXi-SexSEY align="left")

```abap
export ARGOCD_SERVER=`kubectl get svc argocd-server -n argocd -o json | jq --raw-output '.status.loadBalancer.ingress[0].hostname'`
export ARGO_PWD=`kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d`
echo $ARGO_PWD
```

![](https://lh7-us.googleusercontent.com/sWW_BDbCE95NcETv1tkLQAcPkzRIviU2kklm1HiM_LgTa0AIeZaiXwnZ7-xz44ihLvGNmq8UJcZ5Izn-KlNrUYCxkXdwobpZVbKpwwyAcHN4BC02HHHN5uDiH8ba-H8zCBQi5LgX-Ub4wMvKdxiF_Q4 align="left")

Enter the username and password in argoCD and click on **SIGN IN**.

![](https://lh7-us.googleusercontent.com/05J0JJzaz2Q6VVsA53ptmyhFUcN-mdmeh_BYKXUCKG6t6Ce6raoNYj3Lhewb00Z44mdHSHnkk_pn2Yah0B91k4uROCKES4-3ss5SHpqxGt17oNyQjuCpAFQq0kiW5API1d9rLTCL-fh3X_C7nVLOjBQ align="left")

Here is our ArgoCD **Dashboard**.

![](https://lh7-us.googleusercontent.com/Nmx3DseQ0DG-QNI75SMpdQp7d8f4J9cA_oq1VWbLPOFDiV4C1K0astAwHzLtua1hBXQsUHXPxGLmtsyftnyWQordVx6wyZ1BdxqlzoQxg3yPN8vVX6anaLOIiCRxXtJ9-t95HHpsSf9PYbkAC5GGYJo align="left")

## **Step 9: Now, we have to configure Sonarqube for our DevSecOps Pipeline**

To do that, copy your Jenkins Server public IP and paste it on your favorite browser with a 9000 port

The username and password will be **admin**

Click on **Log In.**

![](https://lh7-us.googleusercontent.com/tZFmXbmGhAJoEw6EU-f8d-hNduWUBLrN8edj2qKpUMsBaC-LsnnqcFlRCxuv9okY87K4CPtaTIcuYVtvb0moLSWLzH_J1anJuD6cTJn2LMCz26wTVJted9QkxF0kVsumcH6QFzu1pvVZwhiNWflARHI align="left")

Update the password

![](https://lh7-us.googleusercontent.com/FcwVmffEzr9L-UaiaSQJc4AzkPty7_ZAbSnVtjmYNOGndlGiASpz6709dNYWBpeTximpJ18PfFAkVHpvkZ3ENnrbYaBXM25D0NYylxr54r0Ae3nKtctaiuxKdkdd33YMfTqFqKHodA4jc3CL-S9Po-g align="left")

Click on **Administration** then **Security**, and select **Users**

![](https://lh7-us.googleusercontent.com/GRDhak5hEj9qsbTrCxsUcXmDZK0jfZ1CDtAJHybRimcv74FvwC1D6ImVhH8jFxrLMLz4kHBXWWxWZwvEENYabLu2Sxh7VbT1ngwogCC-DzNmWoLf2tbCIrpP4jlC6w8nu96mhyjyN1Fr3iIVrrnQOVk align="left")

Click on **Update tokens**

![](https://lh7-us.googleusercontent.com/i_EvUuGM_jQR5x1hkT7jbL9pZkpB5tnI0iMdDrNj3Rlq8gQfR0RNn1kMqFzESYvahEnrW01nv8eh6Vwc8n2u_wie3Qxgp8ncX7AYKkbYVYhZQxAl_hrZmPzxC8aH-l-SswExUn97zfLRugC0PlC3Y1A align="left")

Click on **Generate**

![](https://lh7-us.googleusercontent.com/g40rkAXnfZXczrsJkZGto0nr1lLUwIMVDjv1uVs7Qf4fkhgk44rAKAzjdVOwz8Yy_5568QCYnMFr-6sieaNPhmpZSye0_D1xRXiMDwn16H1pM_D1rtDU4lRksOY9voLRNyMr-mOeDCwi9eHATIb2muE align="left")

Copy the **token** keep it somewhere safe and click on **Done**.

![](https://lh7-us.googleusercontent.com/0TlRbCJJzcvx82IHp-AiRLR9IbJirJGoyuO9tcOxLBPZpkjWP6IbmZsEwkxwVpj3BFY8iu9D6vJM-JbvWofozNTFGk4W-wI6FaNOtC8sUkDa-S1urMj7ZZtMfTATR5i3lVCuCNaWlvd-5m_ow0Y6Jq4 align="left")

Now, we have to configure **webhooks** for quality checks.

Click on **Administration** then, Configuration and select **Webhooks**

![](https://lh7-us.googleusercontent.com/uIii30PJHZBAzESJo4zVoHc1cJC2ZnYYNefe4lQjQlP9PArZ_rya-rEl7HxjMWHcPVR6NY-BpWUKryRUu6JudLCewpkqDV-nVjmuUeMxho8UJvxLg2OHBZrNN8hITZrZ5ZXg4q_hV0kMoZmECQJI0gI align="left")

Click on **Create**

![](https://lh7-us.googleusercontent.com/WFccQMitDBL7PTTLKF_wfzMvcmJSxv-4vKOJK2hvOh2ZoVDS6p8JYdSYPmX5LOom__jSocRFM-iYYsQZkB8QBAAKFtm0HO7wliVlfQjdTmS7j_7FJDrORxfX0GovZ7qqNP3NBlvX1erzdwqgtr1gGkE align="left")

Provide the name of your project and in the URL, provide the Jenkins server public IP with port 8080, add sonarqube-webhook in suffix, and click on Create.

http://&lt;jenkins-server-public-ip&gt;:8080/sonarqube-webhook/

![](https://lh7-us.googleusercontent.com/kQc51aXzUODTqhUxCVt5do9fmFX0GHlx4LwhVKvj2klv-Ao2ivD-nPE_sUwVvKkv2FHWlU0fi9RLdtHgojGmrDRiwWVt3Y2FMoFmaeuNGORkcijkFKFPXTHiv9K9GINNyKDTmfHa8An1KwYD8l2OMw4 align="left")

Here, you can see the **webhook**.

![](https://lh7-us.googleusercontent.com/NQYxo6aoOQF744fOM9oYZ2Ub7zUMvEK38rbxBNockTWfsgE_hlOaGmbPe739K-glvEb-acrfjwl8l6Hzx_7luTe-lAnIz5oaysgqH4S3_nQXILrTPU5ps9lLn4PC-v6JzO1eNvSt5PdoSQHiIjTIFt8 align="left")

Now, we have to create a Project for frontend code**.**

Click on **Manually.**

![](https://lh7-us.googleusercontent.com/5GPnkThqvjrvGOeJRoWRfw7ZN9YYunX769RXapMaTrSHzfsBvyQuKa6VSrROlWi3qaODjEviZ13AiMKQbG4EHhoKCafr3T6AmCiCsHIHbMpyHD8nG7mbchpvu7hDs3ICHCzghPPh-UTStoe9Nl9pzp4 align="left")

Provide the display name to your **Project** and click on **Setup**

![](https://lh7-us.googleusercontent.com/jj2bhhMXCfN0nIYMCWR0jCoJzQLoPbbdqGt-AChXhl7mPHCOu2PNZ_cuQgSKdS2sefBWs-VDnDvRNv5PHHgDu8Ta9OxNInrZ9-S0yJvc8rwNJxQN6R6Jif713xOkIHVp69f9S2QiQlSUJblcXmwTj50 align="left")

Click on **Locally.**

![](https://lh7-us.googleusercontent.com/DQggqi5GOjAgRHHtEfQJz-99Xp2iuJ5ygitIwV6Po29KZ3HUbPnPx3Hx4H5u7F81Gpprhmhkxz_sSZ_rHr8GdJzC8NJccd3eeef_CG2cqDFJ9EScjxx6Zm42ic4weHpSS47jDVDt-I0eNHG6izizn0U align="left")

Select the **Use existing token** and click on **Continue.**

![](https://lh7-us.googleusercontent.com/LnWoYuAf4jYupqdF2JGC7HlQtw3e5mUkJalSUW1e_RaMxwYbSM8wlXrbumfSXkABNj5a0tGlpu_6C3Ublw-tBAWd6t8mCDMCnlvXeZvq9O0r3Y2p1MtqdjmSDmuM7qhMTUJaVg3Ht364XZ2JYjDMMb4 align="left")

Select **Other** and **Linux** as OS.

After performing the above steps, you will get the command which you can see in the below snippet.

Now, use the command in the Jenkins Frontend Pipeline where Code Quality Analysis will be performed.

![](https://lh7-us.googleusercontent.com/S9K6y_dLC3O0QHI_WMM3fDvhLkj6sG79Kq8ghVwAmvvy2QVi_AklA8ruj5A_Owp3MEwMsJBcyxBgwHYA3utbXTHYbRznG_bn9PNNrY_HBVAXbotyYxDPSTfFMR2oZU2oMiDVJVackbe8vLFDn-N6_3o align="left")

Now, we have to create a Project for backend code**.**

Click on **Create Project.**

![](https://lh7-us.googleusercontent.com/knFVRbK_aEdrVTlufJXDh1UUXVYeQO9VQl7KEzY0bZ7A-smp-t2jp1sIq7pGxWt-hAM9DDwN97EL_yyt5mVu8HKxekivk0-I1SXc7JqwhkIKOrh6qI3EBWHTvoZ85RC9QaCQnChqIRFGmr2px0xIQHA align="left")

Provide the name of your project name and click on **Set up.**

![](https://lh7-us.googleusercontent.com/Osc9Nc-RE6S5dWzLGHsTycoL3buO0M14WOARwiWmaRw-hL2ZefMmt3anmrnVg0zxaZULehP6P_PO465VlmK-0GK_oLRjZPfcqtHKJTRL4kE2ACYx1-9JpTEleYFE7y5fSUuylmjuvIKuTWsO4-3ca30 align="left")

Click on **Locally.**

![](https://lh7-us.googleusercontent.com/R_-r5tKw4BRxbxrLLqrukORF29QTJicV7JOTlmkoZrhd5hLjmPfh7bTj-bwGsDOA9Prr_GMDSpXuybMZrhZNm_IArilRgfBGKuO4RbgxQofx6uxld9uSmBWYK3OaT-_2v-HuXzK6sgo1kRsvFce8NJo align="left")

Select the **Use existing token** and click on **Continue.**

![](https://lh7-us.googleusercontent.com/7rxOx3J4dzdi8kwTjGaSaZMe36Y6aQY229A2W4zX6aW2Soh6FImoa7MAT-TY-oyuntCafy-Y90KYBIjQojtS5Y2IMZfYpyGed-zNs6P9rdzTS-SHBhVtQp48s7q-b9hm4-pspODXVV375qragp_YeTc align="left")

Select **Other** and **Linux** as OS.

After performing the above steps, you will get the command which you can see in the below snippet.

Now, use the command in the Jenkins Backend Pipeline where Code Quality Analysis will be performed.

![](https://lh7-us.googleusercontent.com/8XyPxffVw9DxK7Om1zBaAb4tVC7cy81gZ6f6V3YSo_Q1L9AvtBYlckvXZhsi99nU7oxwCu0URGytzRicSPev2T0o41MbyHSgo-d9cd6e-jNWKQIGuoAJbE_V7ymk4r59ZB6x1hwoP3D-1c7g98vKbgA align="left")

Now, we have to store the sonar credentials.

Go to **Dashboard -&gt; Manage Jenkins -&gt; Credentials**

Select the kind as **Secret text,** paste your token in **Secret** and keep other things as it is.

Click on **Create**

![](https://lh7-us.googleusercontent.com/N1-tyxLbqI1c_kD-gaQjAgbau4dH0Q9rZznfMDhmAu9Nf-ZF68yohegam3HICaW2sjzxHbgfSTY1YlLCSpqSAlTZe9eWKs2jw4B31mBzKLYLp3p1wmPsf6n2IezYbFmh8dboDcZZC6c1tEIw7zuVRQg align="left")

Now, we have to store the GitHub Personal access token to push the deployment file which will be modified in the pipeline itself for the ECR image.

**Add GitHub credentials**

Select the kind as **Secret text** and paste your GitHub Personal access token(not password) in Secret and keep other things as it is.

Click on **Create**

**Note**: If you haven’t generated your token then, you have it generated first then paste it into the Jenkins

![](https://lh7-us.googleusercontent.com/iTkDkqvSmccrwDPQXc5y1vAWoXXJKDyc5FbKPZo9ThGyVV1lajFdkKSXOv1O2vFJt0_ZTPopAVEfGsW6f311Fe-cFPpCQEas0nCq5xoNaeZ4pgck1zu_1Oz2yuRBJLAqfQ1rR36M6YIsDX1vFoS7hi8 align="left")

Now, according to our Pipeline, we need to add an Account ID in the Jenkins credentials because of the ECR repo URI.

Select the kind as **Secret text,** paste your AWS Account ID in Secret and keep other things as it is.

Click on **Create**

![](https://lh7-us.googleusercontent.com/KQzlxp9trZlrBTh4HUrrnlrFwb1EoTRJoe1is7aBRq5Py96B5VmzR2wMa6P20upL4t93XE9-EP8h4zGAMrSGEUNhZNWeZSlX__tfIck2HSCKvx6Ot8qQN-n2FhqQvabdyaYNbxTK6EWOs6raK-ukqy0 align="left")

Now, we need to provide our ECR image name for frontend which is **frontend** only.

Select the kind as **Secret text** paste your frontend repo name in Secret and keep other things as it is.

Click on **Create**

![](https://lh7-us.googleusercontent.com/mx-cSBsNNsjCUfUCUFVw1aahvP5gyzf_fKS1OFuuYD9schtIqwLeATntcqHMn2_RJul6_EG2ndGH6VkCSydjR4BkIZ49dT7uKtgl4qqhFKbL_WEMi4_lli6_0jg-N62CHSx1P3cuzE7gbV5MWb-cN74 align="left")

Now, we need to provide our ECR image name for the backend which is the **backend** only.

Select the kind as **Secret text,** paste your backend repo name in Secret and keep other things as it is.

Click on **Create**

![](https://lh7-us.googleusercontent.com/5nKXKOQ7t094KWYPHJjN8xtR8-loHU8PfruAqNojmhQ4ifNdLkJyglJJ50vljFM19k0SlLLMNXKek_j2idzT5TBKWRA7HRJ7jgIvcWRByUbbsWHAXv2NB4vq0ecLzVmJk4pY_yBSECqMsOKd4bYV6sE align="left")

Final Snippet of all Credentials that we needed to implement this project.

![](https://lh7-us.googleusercontent.com/mvwcUM0VVQeAaYERaa6oD_1HfrcVGGrCSiseBgXp-IoIdjm4SWbPqxv4RwPsHp25GS6bpiluIjuKoWbsavhZVKq27-fjeo35dsL9jvX4m-t5aAl12wWcqNV2r3-5jcT7PkdJYIxaQG6tfJybL_Ow79s align="left")

## **Step 10: Install the required plugins and configure the plugins to deploy our Three-Tier Application**

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

![](https://lh7-us.googleusercontent.com/K5BUiZJuLz9vPyktVBJJcVILuEW6o8aXouf6csBGgDfHvUli051FvtjcOpUW0C_59L5ZcwdtbUGWZ7aZOyPmr1S4OPTpEj1fLUaAOkflEJzLi-h4ODaowAVLyJXQb9dQGriIX5hjbnlUDsIjke0B1N8 align="left")

Now, we have to configure the installed plugins.

Go to **Dashboard -&gt; Manage Jenkins -&gt; Tools**

We are configuring jdk

Search for **jdk** and provide the configuration like the below snippet.

![](https://lh7-us.googleusercontent.com/6FZaylSxeUryqKwFhkhaf4ec-ScTT-JKsTZozA45cr7QCM3z_g4XaEUfr_a7GVRV-svz8zPBHoBGHSF8Es3o3gkmxRMRp9ccLaJPLF3UT8VR5wohtwqXmjw1bTwVFnMYG5ouuzR92gAoR1W8bj8otU8 align="left")

Now, we will configure the sonarqube-scanner

Search for the sonarqube scanner and provide the configuration like the below snippet.

![](https://lh7-us.googleusercontent.com/YfgpPSTHcOsDTx60quHqXj7cgx4AZBaREYxg_RYuW-ZgNGx_IuRTtGDOU6mZikcjAH8pj_vGU8On37IYKh1JKWQ6naxHx0pkuY4XQuCFtVQkavMf3TaejttMMtyBlyNdHscFniWFs9MgM_Jwut3KLAk align="left")

Now, we will configure **nodejs**

Search for **node** and provide the configuration like the below snippet.

![](https://lh7-us.googleusercontent.com/P9kEutDHEL3V2iVwR-iU6l26h3RzHTEHPAhRCqAWnBl7bAnrvWPE8Io4vYsL9AS2y2XshvWqa67Mhk3TWsZPgYz4n9xfKDilT80oL7PYp0MFhXHObcdzOuNy1p3HbR82KAdODAQ8QpwXzjivKJa4xwM align="left")

Now, we will configure the OWASP Dependency check

Search for **Dependency-Check** and provide the configuration like the below snippet.

![](https://lh7-us.googleusercontent.com/fZO2EA6HpvNpBtDq13O0HA0suDQxteY3FyHNMKGxBroqq_x42qotErMQp0xKMt-KR3xvxENpCL3-u_k26W6NebdvRq-ADi2R1n9un-IaScgFKhYMFiY7PZeCNtC2LbDcJEd6MrRd8EdeBN2bFJJleyI align="left")

Now, we will configure the docker

Search for **docker** and provide the configuration like the below snippet.

![](https://lh7-us.googleusercontent.com/J--XakHw1I2c1sTzJ740KYD6ykUO2V0Z5XI-Sc_WZ7cvYO0cKWFX1EpCKAhp3CBXf6fqpZ2CcB2VR5TcxgVo4s5mhZ33suWKtjQAr2d85_f-GcTXdZh8Zv2Ie_HoQyNHzFTRa2YMrR8sWFxbZrN5lMg align="left")

Now, we have to set the path for **Sonarqube** in **Jenkins**

Go to **Dashboard -&gt; Manage Jenkins -&gt; System**

Search for **SonarQube** **installations**

Provide the name as it is, then in the Server URL copy the sonarqube public IP (same as Jenkins) with port 9000 and select the sonar token that we have added recently, and click on Apply & Save.

![](https://lh7-us.googleusercontent.com/l36QmW4-dkWTNW3QpySBEP-2cDubvtRRJQ8-4sF4lItWuHhIZEr-rMkTcdjYqVmFdKMWe0se-gh-2PZElge9UnbZL7B_DgCAe4y5H0cICAMrHOQfp6OoHkNB9-JxxMlgZ2VQOfBUPeWcU-RV6aO_Z0M align="left")

Now, we are ready to create our Jenkins Pipeline to deploy our Backend Code.

Go to Jenkins **Dashboard**

Click on **New** **Item**

![](https://lh7-us.googleusercontent.com/pEbHshLHuYJmnG5J45iiKY6j2E1tNAZZIsGQWvFgyvhHHmhkNKS_PeBEMcZas-Td-8SxUUiTQEISJqUvFN4j-bYe6ygmuebGJJaq98KwOOjW-u0hxEwC7uGzbD5tqHNSyutWllNSnQimiXDNoEcdrdk align="left")

Provide the name of your **Pipeline** and click on **OK**.

![](https://lh7-us.googleusercontent.com/a7yciIRwnpbGydsjQfBwsjoiOh9Ln15MnlWMvwiI7Bp7Wzm3R7N7sTZKRXP1a6HI49-biIRfxRDngLAzoKmvhhEnqP9VTkUUP9SQ7_kijc6i3KqbBcw-22S5ChLlBSttn7TndY44LOXvFD3Oa_Aec5Y align="left")

This is the Jenkins file to deploy the Backend Code on **EKS**.

Copy and paste it into the **Jenkins**

[https://github.com/AmanPathak-DevOps/End-to-End-Kubernetes-Three-Tier-DevSecOps-Project/blob/master/Jenkins-Pipeline-Code/Jenkinsfile-Backend](https://github.com/AmanPathak-DevOps/End-to-End-Kubernetes-DevSecOps-Tetris-Project)

Click **Apply** & **Save**.

![](https://lh7-us.googleusercontent.com/jC-GFRyCEQ8LItH7s_Ng3Kw6pbzHC7ONSaWpU3wfYTaesHfLEKaj94JCKpBefJLTydSmEj4Vo0VFtw8E9W1dZhEmOoRS8V9PspYmXzrQZrVb1qMoVOthxX8kZd6Rg3JEt-D8ggTOhKHx1dq3cUHogMg align="left")

Now, click on the **build**.

Our **pipeline** was **successful** after a few common mistakes.

**Note**: Do the changes in the Pipeline according to your project. 

![](https://lh7-us.googleusercontent.com/1SpB5ZUnE4-0nxLdSSedVg-0m5O_dGsXUCeWUflbQyiK23VSUticVQZ0fTLFODdj2m0O1L3pfenIOFY3eS8eV9E3hnOctTEinX199bbX6QAKpNBSvhSu_T5iIMKv8mbWUNUyBk862T0vgjjTrBaeF48 align="left")

Now, we are ready to create our Jenkins Pipeline to deploy our Frontend Code.

Go to Jenkins **Dashboard**

Click on **New** **Item**

Provide the name of your **Pipeline** and click on **OK**.

![](https://lh7-us.googleusercontent.com/1knxpw9gsoKaOz7yuAyC-g3sVkY5Y4Tty-Q6fWSacSkOpnKKK7vDbfMuxvdEu3rHSYh4e9umtrxcRclS3D8ZRWvsQwL1o_kSNwR0WDZsZKmcIe-h5mB-HKNWhpNHhT-g3S2ljn9tUVi7AGXPb3TEyNM align="left")

This is the Jenkins file to deploy the Frontend Code on **EKS**.

Copy and paste it into the **Jenkins**

[https://github.com/AmanPathak-DevOps/End-to-End-Kubernetes-Three-Tier-DevSecOps-Project/blob/master/Jenkins-Pipeline-Code/Jenkinsfile-Frontend](https://github.com/AmanPathak-DevOps/End-to-End-Kubernetes-DevSecOps-Tetris-Project)

Click **Apply** & **Save**.

![](https://lh7-us.googleusercontent.com/QElWxmge1SxeR8AwAxFeGyUuGdD88liGQfBSNb6PnKBCg9nRrMpngI0xlAJq8j6_vmm82fQTinEsFX69pAGt0wRCZx9rI9TEJoKHMZu-pSj570hzvjSHKY2MGVvjgZTplWpFqMbqrRgqzU7_g9pUrJk align="left")

Now, click on the **build**.

Our **pipeline** was **successful** after a few common mistakes.

**Note**: Do the changes in the Pipeline according to your project. 

![](https://lh7-us.googleusercontent.com/bfqpTSR9olunUuK0Z3VKkYmdHaE6ApbksnlnNH7Z2DP0U4zMYl8h7_jmbm8iXGv2KC4GLT-UbbsrAo_J-AN6LDYKuWhjD0e_1-TIqgjp3Nb5uv9uq1TN49X8ZLUYrBBfpX88BfmgbKTuoK9RS5_b-Z4 align="left")

## **Setup 11: We will set up the Monitoring for our EKS Cluster. We can monitor the Cluster Specifications and other necessary things.**

We will achieve the monitoring using Helm  
Add the prometheus repo by using the below command

```abap
helm repo add stable https://charts.helm.sh/stable
```

![](https://lh7-us.googleusercontent.com/6esX8wzIOF9OnN0k8eXswEmD5GFcGy9x1X92wB86r3gMA2XXZ0J4UARnR5hkb8lXFnjldol4gBaljnuPh96SgqV2iOqv4twUWAA86PZiAJOFuPrRcv6SWlnlW_WeU3bxaW93Ysuc_Ta6rvxn0uznRlQ align="left")

Install the prometheus

```abap
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
```

![](https://lh7-us.googleusercontent.com/b9xD07I3MLGCXaHOWEV71v0_Ct6GlsqLYvXPXVZv1MeYX6_FxP98Oda0rafUjpOQgCuUttL1OATulEnEAfF0qhlE2rRie0noKPA3umBYOBiOmVLp5kLCxZfDQQHRxEczatugs_ZOe0j4Hh5dnKhgLpk align="left")

Now, check the service by the below command

```abap
kubectl get svc
```

![](https://lh7-us.googleusercontent.com/rRBX46vuNtsJbRf5qJ45tXAkx4m3IZ7PL1TzucwhmLOGa6gY9wefF4dd-rMcfGlTVJ-T8Lpfp6I6nzONpWUPvItRFLQoYGA70pgH3IQWq3SMd1MAVFCtt2iGHb19uqTJDmOh8n1UvTLTQGPbSm3PzL0 align="left")

Now, we need to access our Prometheus and Grafana consoles from outside of the cluster.

For that, we need to change the Service type from ClusterType to **LoadBalancer**

Edit the **stable-kube-prometheus-sta-prometheus** service

```abap
kubectl edit svc stable-kube-prometheus-sta-prometheus
```

![](https://lh7-us.googleusercontent.com/jZjFIAoglQWukwOaRhNSnAXOMPDZQLF6imAwUAGjBPgeI1toah7wbfFNpCx7vSGo7dBNp95ldONG60wGeVWE8Lytl2gyCTsgZdeKuC5T-aCB7qKolG1rCksUlePxv3vkowpklOKHNkY3PRF-2bND4o0 align="left")

Modification in the 48th line from ClusterType to **LoadBalancer**

![](https://lh7-us.googleusercontent.com/Xhugoqen5yvTKiemaEXYZy-wQ0FfONC37mtjOxgVFVeL6ojYTUrPNT-IjzFq8FwU7PE1EmfptuCIPVOdBLQpnT66BEo0h7eLVOAH6NabJk7etSjNsfN8uPe_CDlW45GWdLU0K2shMT2Kr84UsfJHz_k align="left")

Edit the **stable-grafana** service

```abap
kubectl edit svc stable-grafana
```

![](https://lh7-us.googleusercontent.com/Nh2b0yG51V5t5S0RNL8Q1TiN7F96cnIAip2Y6aEZNqx7jTFY2pxg2zGV870k-hsh9PmWXu7E1t_MJ7GBQ2Ax-Tha5bQDsShtUnljW417nwvG8BgmYyWbgcBhO0nNQcH8v5IHrp3HXqRv5CvvASvqiJI align="left")

Modification in the 39th line from ClusterType to **LoadBalancer**

![](https://lh7-us.googleusercontent.com/muS_IHTF9ln5J_JkOONPIgW4rcMTqtRd1EiBaI3sOo9e8JM5wy_gKh86e4USJpzvVo_b9aD8faU_WD_gcuMwj4j5ZDGFIppY8gZQtl8g8kvA6Yu8d973Ty9WAcwJNCbC9MeIdFIOMnY-O1GGtNgnmBc align="left")

Now, if you list again the service then, you will see the LoadBalancers DNS names

```abap
kubectl get svc
```

![](https://lh7-us.googleusercontent.com/zdV2EOCSUE1K4AhrJq4z12XjlNTIvDA0Wa9-Oj-ybBxxIPR2OH8cr_9W6FhAbSRJa1vhfroioUZx0KTKbwPmoZ9_Nt7p4EBjct1dQac6oJgVBNX9kpF1wPu5MK2LjdlOEDnso0_gG52mjH1MD4D_UKA align="left")

You can also validate from your console.

![](https://lh7-us.googleusercontent.com/XaIRjXrAig32yLUooWdshrwIIvztWLC8MZSU941_FJABdrXl8wd44kFzaJk7iuywhVz8XjdyVGQx0mWC3CUDQ-J_ej1ImLSvJw87WBLf3TIWel3GDQbLGgcHoG8hO0razl4lSTVZeKgNeyETGxqFldE align="left")

Now, access your Prometheus Dashboard

Paste the &lt;Prometheus-LB-DNS&gt;:9090 in your favorite browser and you will see like this

![](https://lh7-us.googleusercontent.com/BzxDjJt3JpXYELzextIUlc0gUgEQ5kjEqPcRZabDoh0ppP9js6prLGQOwJjnZ_PAGwtq3VdtX6avjYF6n2zw45qASHIJPdrKBCIOyVo9MYdmOOH4O_K6-7N055iwfbvavdFODBm29p8WhfrSNMT5Wiw align="left")

Click on **Status** and select **Target.** 

You will see a lot of Targets

![](https://lh7-us.googleusercontent.com/H-sTM8LauMov-FL5KjRMzA1gBwK3bd-yuR2UVZtfkMab1WGKMFn6qswFYGL-J31-vhcYMmZ95aagZmqQQuGHa4F6CiO5Tcz84FxSpkvN0uYZ5TvJyTX7Lk-g8iCs2XM6r2UXNQZud73J0EWwwkt5pEE align="left")

Now, access your **Grafana** **Dashboard**

Copy the ALB DNS of Grafana and paste it into your favorite browser.

The username will be **admin** and the password will be **prom-operator** for your Grafana LogIn.

![](https://lh7-us.googleusercontent.com/ibLVmja-woGrjNSCyVRUlUMH0MmxnOhpDWAyaLoB36E1yAVtwkm2C4NC11dlDBEtZgbr8bq1giE5_bSC18aWkn4QB3eaK1ScyDgGIR64u9txBDppInHPRe0LL49fPQMh99FDVnDdj5r8YCQVhDEmYYA align="left")

Now, click on **Data Source**

![](https://lh7-us.googleusercontent.com/Xig9A500EhOXBF_vdjilDH0HKC_A856gxgL36zKazZ-btimrwDAutEL0e9-haTtfGxdMOxFnR669yiJdCdtZdgs8iNW4I0EyEGMiU7dSlUOFMfI-h5MxBEHgod-m54hLj0KMYSndpLP5QgRSPYXUt6o align="left")

Select **Prometheus**

![](https://lh7-us.googleusercontent.com/A2xEyfmWZxWZx2RKTHejrwzfF6jp2OJKzYRNWU_EWmYlC2qzU1sw4-yhKLtFvCIguzmQVQQmNK_BRqYIb1LHyamezdR2DBQFPxdBKM4Gz1MJP-tctqO6xS6ldIfxQzIoCHM6L5bx0X5eIIq9bIhmntg align="left")

In the **Connection,** paste your &lt;Prometheus-LB-DNS&gt;:9090.

![](https://lh7-us.googleusercontent.com/BVfXwz6zJbnEnS-UU2c7lMPZBU4DMUjcfm6uJazvd-GXvoQHFM_fWQmK9Afn3oORESaqUXvnWaHAcYMAsKa-wSeWA-r0pHWWWHGWBseU2oAOWZ9dqnxrgehGMyh9fAckJIIim7vzfCoM941VsWFDCfc align="left")

If the URL is correct, then you will see a green notification/

Click on **Save** & **test**.

![](https://lh7-us.googleusercontent.com/TZp9R9T9Hx0KQEVFdM2HaX25OBZJ7-WFeAnXufXDCPMUCytsy2WvL2IjjPZPax85PDIONs4IMUgH3wb209zYx3AZJomb1cK9dCXpI-SYzbNSr7dZoPDinU1RxQjDr4nHSSzywLd2xjWgCHiU35hxUwI align="left")

Now, we will create a dashboard to visualize our Kubernetes Cluster Logs.

Click on **Dashboard.**

![](https://lh7-us.googleusercontent.com/O8iJbgpBALLzR1eJiLftNcwtIs1fRyEeiLYW7FWS3v2Mo6hkK0s-40le76aUzYZV31dSH05S4zoIpB34YiLu9kRToCd_q9lozUsGGtw2E2bhk0ykPblDlTJEoOZe93tYT-gsWRMeaYZ1kQ5GAWBCzrw align="left")

Once you click on **Dashboard.** You will see a lot of Kubernetes components monitoring.

![](https://lh7-us.googleusercontent.com/NHXkztyBGdvUfHPlj34fLtK9atYgL7qqvbEnwnp5936GL425jeDcjSQhytfqIk1XNvJ0nbh7yGov_juJdwEGXTdbPt2gc9z0jTGiywwjLaWecHAhHkQ3av2TxfOkGkvRPCidgpDwlvKziTm8yyQfRuE align="left")

Let’s try to import a type of Kubernetes Dashboard.

Click on **New** and select **Import**

![](https://lh7-us.googleusercontent.com/voKgn4_UfMqTx2sOA2-5Z3v5PRZVUCXDl91HtJvYzQVw2ymWw19I6KBj00Ig8maXwO29kIaqbgBFbiBEHL1sSM_xaBrIDKxKHvgRwuFvUBm1WmoGI0vczoT2JxLttSBkci2arOS7ewHAeajinb-dshw align="left")

Provide **6417** ID and click on **Load**

**Note:** 6417 is a unique ID from Grafana which is used to Monitor and visualize Kubernetes Data

![](https://lh7-us.googleusercontent.com/9rJoXrdBmmpmMhTuwZzd5aq-eNoYD8DxqsU0TCarZrl_mWd-0j8Phi8CZAMU72UtpNdAyvuOlTK_XS5m3PDs0HYN0gQY0UbLFN1n4QVrkLgtCSY2spF--YA3FYpAVVU39UmZwK3k0bQogZVsJHyhR2w align="left")

Select the **data source** that you have created earlier and click on **Import.**

![](https://lh7-us.googleusercontent.com/jN7QR0PtPJnJhq7or3oNUN4njdQrrj-yzcdnY7fA-a3egFidRw5aKa-etI2yVA7ZnpOonQekw77UWyJeP2CjzTNtFjxAW7adg9VwdbY5W0Dp4v6fmdgJfVGxL16sL5jLU1vVoVL_HrF_tUdkO-yDpus align="left")

Here, you go.

You can view your Kubernetes Cluster Data.

Feel free to explore the other details of the Kubernetes Cluster.

![](https://lh7-us.googleusercontent.com/SG8pwO2ombKCS5HivbvWT1IpFoR_587l6epn8pG4Csi21l1bwGOB89jLFQh9B12tZsov24sZ9jzMBUNynOdltyS6Wf5JiLNgwRQ0e8rwIeBOuT-5odjHUBeTsCVut_VLCNHZihX-P_2ocqHV1z0bHaI align="left")

## **Step 11: We will deploy our Three-Tier Application using ArgoCD.**

As our repository is private. So, we need to configure the Private Repository in ArgoCD.

Click on **Settings** and select **Repositories**

![](https://lh7-us.googleusercontent.com/MCV5J_pCFc6xieG5kSlwcbkRAT583K0gBmk1oP1GJQzOUNJljhtbkdjuNb5shGVJK6YPDwKuboVJE1EGRjSTbUsdczxeQ8o67BM0BrWI9rE7LQLNepSLlWOyrq9VCwTAO_6NoRV1CVjkz2AJ1F2bWrk align="left")

Click on **CONNECT REPO USING HTTPS**

![](https://lh7-us.googleusercontent.com/Po-1B84Rx1zBJsD9gSCkq095vXKoGfzFNwQT7xlITnOr3M3UHX2LCZV2XZ6gqen-dU5xcziHnfzK7_yqztNWMuPLDHR9DJi0AE9yy7tUHf5kv6U2jgdk6OPufnqKfpQSoV2Y3D4IAXbZwQhF9GE4nEo align="left")

Now, provide the repository name where your Manifests files are present.

Provide the username and GitHub Personal Access token and click on **CONNECT.**

![](https://lh7-us.googleusercontent.com/HfQa4AVvzB5C8M-boZTft2fZ9pqM2B9ACBxY6qvetaIZi1rJAcbHYHYR3vJ9Syp_pUn6bzG3J9yz9_vP059ROwmQltX9XRh5ojITEyBCzU7eDP1dwSHmVWv59W9gG1GwMWX9GSfExX9TPkwD1Plaegg align="left")

If your **Connection Status** is **Successful** it means repository connected successfully.

![](https://lh7-us.googleusercontent.com/WDxEQkJc7qUibxxY6vUWeDZQmJlOMg2SwhQJ5gqCWscbWn3PgliUErgABmrxo7Ao8Q4_dq1YYDY-YQnzCn8olmG-9vjYYT6BazdZJhoKeC5uYuBcJOmtNsikPwDEbp8dv0vSkJ_km9CsPHxNKXYGf9c align="left")

Now, we will create our first application which will be a database.

Click on **CREATE APPLICATION.**

Provide the details as it is provided in the below snippet and scroll down.

![](https://lh7-us.googleusercontent.com/BCQwimV__AfL5viIQCypnsxi9mINXL-zNjRd59BxmPC7I3RAAgkflSKysXc_hfgNdEVhAX3nm9oozmzGV0ncK6S2b552M7u91Ozi2hq4DsVlYkVlOAFx0NMfc5gSCWFMLyVYizpqd2sGSsImVEKTurI align="left")

Select the same repository that you configured in the earlier step.

In the **Path**, provide the location where your Manifest files are presented and provide other things as shown in the below screenshot.

Click on **CREATE.**

![](https://lh7-us.googleusercontent.com/WysasbwGURQhC7cG2AveAnyLPw9oS-auPvXiIs5EwxKoy13H9APDCUmGxelRcM4DIUUB2s6MXrBkVABP7aNouwOBEW138dDLPWTOGhr9xuBSkYLQmYMqqwKCgsZgvEHOjpv5VAuuVT9IC5OaDsI97Yk align="left")

While your database Application is starting to deploy, We will create an application for the backend.

Provide the details as it is provided in the below snippet and scroll down.

![](https://lh7-us.googleusercontent.com/x1WrmSrsJed70_9bq8EXYJAGqBOYI0_r457RQl4brZJveWygzFuRjtwZzEB4Ewx9uTFpDRuckgqIg_1mCV3dG4OvKec9douwIkGaAAm41Lb2nC_sHk5auFnzuugClER1GB9vWnX4C1vN3vQlhszHIX0 align="left")

Select the same repository that you configured in the earlier step.

In the **Path**, provide the location where your Manifest files are presented and provide other things as shown in the below screenshot.

Click on **CREATE.**

![](https://lh7-us.googleusercontent.com/-wYk7d8dat0iPUFTsENIOImiEiyqkYqbO4UNfqf7lAbQtUxGPU-UdydGP3F2yXBpIVEiOPQ1eVENk_TCdVfT740oDLU6WZDZPNonzRESqcqDTfNAA_h96WWmcsPmkzNpzgwnWrr52DH04_B38akWAmQ align="left")

While your backend Application is starting to deploy, We will create an application for the frontend.

Provide the details as it is provided in the below snippet and scroll down.

![](https://lh7-us.googleusercontent.com/qJNnHFeF4TWY_b6LG07dehTuHcwo9YZ6ZNxmgz14WRZ3Eed_06u7TbPG4a_AORfZ_KIbBFsrNZubcwvmLpcW07baHqBL33rBNKY9pQ6Co3HQrEO0Hydek1M2ZF9C6JkDBAjC_xGcGy0VMHXfAFcU1F0 align="left")

Select the same repository that you configured in the earlier step.

In the **Path**, provide the location where your Manifest files are presented and provide other things as shown in the below screenshot.

Click on **CREATE.**

![](https://lh7-us.googleusercontent.com/l5S3e_1etS2oFHjLZFuxDF5wP1ga0JqHLfh4N1usKCT7yBJKl-7z3Z864FwUi7SM5DQc4Rf1epoRGVgY99ga_4m9HZxOYNHkFfHrcZJJ8IzHY-G-wBhZJUG_kd9eV3CZzdoDNUi0yxwBjCi3x13XbHA align="left")

While your frontend Application is starting to deploy, We will create an application for the ingress.

Provide the details as it is provided in the below snippet and scroll down.

![](https://lh7-us.googleusercontent.com/3V5oNJnjkClGBwnipF68zftO_fFW-PX_XUu4Tgzn9vjYvCaJUquK44RDqY6nKLProS4tsgyK08dqVe0eSa64V9rkjxyr4xrZ10DfcA_10AWXbgFCNdoG5gUxhcipBR_wi5-9O6nJkTmIjdbIKDgpg9A align="left")

Select the same repository that you configured in the earlier step.

In the **Path**, provide the location where your Manifest files are presented and provide other things as shown in the below screenshot.

Click on **CREATE.**

Once your Ingress application is deployed. It will create an **Application Load Balancer**

You can check out the load balancer named with k8s-three.

![](https://lh7-us.googleusercontent.com/jklGDYW5beDNaBB8u9EM4QLCcgjxFD8_vfG7lVdPrJvokfkwSBqvwWMr-4mFzuaPsxA0Ct6kSTS-pStPSbdIK4zinh3eKj1hcUzo-Dn5pKtEtZ9DDZu5ev65JYGg2uI5U5Mud8on8aGy5LAJI7YhVt0 align="left")

Now, Copy the ALB-DNS and go to your Domain Provider in my case porkbun is the domain provider.

Go to **DNS** and add a **CNAME** type with hostname **backend** then add your **ALB** in the Answer and click on **Save**

**Note:** I have created a subdomain [backend.amanpathakdevops.study](https://github.com/AmanPathak-DevOps/End-to-End-Kubernetes-DevSecOps-Tetris-Project)

![](https://lh7-us.googleusercontent.com/KQK-CT_fenmoI-hUBTtp9Dz8o5rPC-Qy-7HWEWpyfU2nCVS7f54e77rgoT8HcLLjnvfcAMkGK2B5ttl3ae7d5hoy443HedU40O36hwcZXnCYqgK2uLd-QEQb805n93S68N7JJn57kTx8s1eDM8TcVV8 align="left")

You can see all 4 application deployments in the below snippet.

![](https://lh7-us.googleusercontent.com/0je11eOZwnW0KOxiceT254TtiA_6unr8eIFrLgUjwBGtkhWRV-emzAhbHtxffnKfcTx18XfbP1sq0igWa5t0ChaMhmIIPJiFaeWyrOuTVBic2cC4emBuet-Bg2nwXmVoczFYCGzhqt9ZI5ZetF7nfL0 align="left")

Now, hit your subdomain after 2 to 3 minutes in your browser to see the magic.

![](https://lh7-us.googleusercontent.com/qkIGg_S39EGLCRzWDScgvIkjMSoBcjousyCylyqUOOv5N_ucJDZpImIELZHBImRA2Oy03iy96BNdAbNOfTse2G3o-x-JbLuZG1eytd-uqRhabUExlW2zY4NyRi7ZqfwPbYP62l47K-JygHToUM8SQPY align="left")

You can play with the application by adding the records.

![](https://lh7-us.googleusercontent.com/6dG6gXSPl25B2oZtrZy1PW0SL_A5rldsPZ-1lDspCXztrxQaASPjgiKaMRpFwTa8uNVtCDz2E-W30DQeCGD-2AZVrrNpThUK-M0YT22aiVA7kZ1pCVwpyG_Tehohz5J4FRtCwwmsgbgmvSKjdbJIOCE align="left")

You can play with the application by deleting the records.

![](https://lh7-us.googleusercontent.com/uFNqZaIQZmkMffoXo4BN8t3y9a-pEKM42Rz0-Uael2HRcoOCokBJ2WUPMyXuZWbfg8OylPP1AJMBPzK5FU54mZsDoXTmuyqhPlu6MAsE2SbeVlhfjbyL5qnIg0LzVQMdVcY3P-KGHq_9scNl843LeRw align="left")

Now, you can see your Grafana Dashboard to view the EKS data such as pods, namespace, deployments, etc.

If you want to monitor the three-tier namespace. 

In the namespace, replace three-tier with another namespace.

You will see the deployments that are done by ArgoCD

![](https://lh7-us.googleusercontent.com/W5RzLbE83E05kpKnTnhhEJGkuL-Agpwv0nMshAbpJ4uga_WbfEEbLJE9Er3SHqOtKycMwX5J5b3G2zNXsDCqdTudlIHJWr4GaPtW-fOymOB-jBMrXIn2pf5nRDRSvEHr_xPz34CcioL0II5lQHVy8Ik align="left")

This is the **Ingress** Application Deployment in ArgoCD

![](https://lh7-us.googleusercontent.com/sFR4MkUqu_Gm7V9saCrD8bAMcKotiNXc4DGpi5xzXQj4fOmNlMItCcbp0AKbfVr9BWP2I4VncHKWzNHn9INOuILjDQkU86pyLYZ4jrsx19HTyuZCb0cpuxJU-Qeif_oJ6t28kyU5XZ2loA4E6gUfRu8 align="left")

This is the **Frontend** Application Deployment in ArgoCD

![](https://lh7-us.googleusercontent.com/ofvgL5OM7sfRmUDwYs9e6A1mgkZDNmgbaTp6MzHkcr2rENy1ZtErFh99dT-eZPe4aRUBMZhlxxvg55f2ITvcN1f1I0TircNsnGKZn_zy0rDSzPdcN8Kox_kl4l2vyMdzqlRFFTh0yzIdqV7czwu7RTI align="left")

This is the **Backend** Application Deployment in ArgoCD

![](https://lh7-us.googleusercontent.com/Dgx2GFPW2DY5jVNiYQLoTUp9FYfvH1tA-8jfJKJPBoRe0ZQO3maAFTyASSAjkVwveNMDQjeoexVeQtNAKqh1zBxCI0bIwaqD3-z_5GKERb8cmqo4zsOOsrOwfUvQVfNH5Y2jH2eX7yc3n_bhq0fDeE0 align="left")

This is the **Database** Application Deployment in ArgoCD

![](https://lh7-us.googleusercontent.com/E8V_5FTYeTOCa1b0ULBJAb0Bqj3fzKGwHG_QHJ4yINQOBFxwX6F4AO5cbgi3gGfdmXC3hlW3ay3gBo50XDg3uZR20zBDrlY5vYIvBC9Th3lLHYcL9J8Tqi4cMdeb_ZHcH2g2qMd-8uSEK7vs_VhCECU align="left")

If you observe, we have configured the Persistent Volume & Persistent Volume Claim. So, if the pods get deleted then, the data won’t be lost. The Data will be stored on the host machine.

To validate it, delete both Database pods.

![](https://lh7-us.googleusercontent.com/D75ptjRnfxA-QP5Z5h58Jo4j72UpylKsVXz9ccZ3uUMia8y2TLH5KMZ8WvycSadXWvraf_olh1lQRcLPDTg3jEw_05fdxIKLkc_AW0SiVtIqdvGVEoc0ULF6nxH6zvX4qQ7mOYx4RZds9Qxjq1B4iAk align="left")

Now, the new pods will be started.

![](https://lh7-us.googleusercontent.com/IJZ8Hc7S4WuCm0ShDD4J3J1xHWDfcaGzeVlpJseYVLzW8jBx9nc2mTXhpN2l7PYy_h3-5O4ZpdojS8scLzr4BeaBtYq5KcXKg6dyPpVDsfn-ok6banwBW8YTZ-uEVtQvaZjczx5LwFf6RKrkofYW3xI align="left")

And Your Application won’t lose a single piece of data.

![](https://lh7-us.googleusercontent.com/yyz9ZoU94pYmmlX2mBCcvQ0reW7RoIVzfh63mB2kqEgUY-fDMGYvo9C2UH8Ya8kIjaq9Cx9LhvSi2S6bgP9OpYtoS_oHVNv0to6_tMpKZgDCQWCtMdr32bAYNpVL6iRrnusZ-KXVd_yaN-kXgp6Y6Pw align="left")

---

# **Conclusion:**

In this comprehensive DevSecOps Kubernetes project, we successfully:

* Established IAM user and Terraform for AWS setup.
    
* Deployed Jenkins on AWS, configured tools, and integrated it with Sonarqube.
    
* Set up an EKS cluster, configured a Load Balancer, and established private ECR repositories.
    
* Implemented monitoring with Helm, Prometheus, and Grafana.
    
* Installed and configured ArgoCD for GitOps practices.
    
* Created Jenkins pipelines for CI/CD, deploying a Three-Tier application.
    
* Ensured data persistence with persistent volumes and claims.
    

Stay connected on **LinkedIn**: [LinkedIn Profile](https://www.linkedin.com/in/aman-devops/)

Stay up-to-date with **GitHub**: [GitHub Profile](https://github.com/AmanPathak-DevOps)

Feel free to reach out to me, if you have any other queries.

Happy Learning