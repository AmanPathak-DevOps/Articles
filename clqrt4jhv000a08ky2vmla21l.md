---
title: "End-to-End DevSecOps Kubernetes Project"
seoDescription: "End-to-End DevSecOps Kubernetes Project using Docker Kubernetes Jenkins OWASP Trivy Sonarqube Prometheus Grafanan Node Exporter and AWS Cloud"
datePublished: Sat Dec 30 2023 08:34:54 GMT+0000 (Coordinated Universal Time)
cuid: clqrt4jhv000a08ky2vmla21l
slug: end-to-end-devsecops-kubernetes-project
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703928337868/ca32bcd5-668c-4964-9ddf-fa7d7f45b1b0.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1703925131504/7d1ed634-b117-4296-a466-cacb22abf823.gif
tags: cloud, docker, aws, sonarqube, kubernetes, cloud-computing, devops, jenkins, prometheus, devsecops, grafana

---

%[https://youtu.be/L4C2yg1j4t4] 

### Introduction:

In today’s rapidly evolving tech landscape, deploying applications using Kubernetes has become a crucial aspect of modern software development. This guide provides a detailed walkthrough for setting up an end-to-end Kubernetes project, covering everything from infrastructure provisioning to application deployment and monitoring.

### Prerequisites:

Before diving into the implementation, ensure you have the following in place:

1. Basic understanding of Kubernetes concepts.
    
2. Access to AWS or any other cloud provider for server instances.
    
3. A TMDB API key for accessing movie databases in your Netflix Clone application.
    
4. DockerHub account for pushing and pulling Docker images.
    
5. Gmail account for email notifications.
    
6. Jenkins, Kubernetes, Docker, and necessary plugins installed.
    

You will get the Jenkinsfile and Kubernetes Manifest files along with the Dockerfile. Feel free to modify it accordingly

**Project GitHub Repo**\- [https://github.com/AmanPathak-DevOps/Netflix-Clone-K8S-End-to-End-Project](https://github.com/AmanPathak-DevOps/Netflix-Clone-K8S-End-to-End-Project)

### High-Level Overview:

1. Infrastructure Setup: Provisioned servers for Jenkins, Monitoring, and Kubernetes nodes.
    
2. Toolchain Integration: Integrated essential tools like Jenkins, SonarQube, Trivy, Prometheus, Grafana, and OWASP Dependency-Check.
    
3. Continuous Integration/Continuous Deployment (CI/CD): Automated workflows with Jenkins pipelines for code analysis, building Docker images, and deploying applications on Kubernetes.
    
4. Security Scanning: Implemented Trivy and OWASP Dependency-Check to scan for vulnerabilities in code and Docker images.
    
5. Monitoring and Visualization: Set up Prometheus and Grafana for real-time monitoring and visualization of both hardware and application metrics.
    
6. Email Notifications: Configured Jenkins for email alerts based on pipeline results.
    

---

We need four servers for our today’s Project

* Jenkins Server- On this Server, Jenkins will be installed with some other tools such as Sonarqube (docker container), trivy, and kubectl.
    
* Monitoring Server- This Server will be used for Monitoring where we will use Prometheus, Node Exporter, and Grafana.
    
* Kubernetes Master Server- This Server will be used as the Kubernetes Master Cluster Node which will deploy the applications on worker nodes.
    

Kubernetes Worker Server- This Server will be used as the Kubernetes Worker Node on which the application will be deployed by the master node.

Let’s create the following instances.

# **Jenkins Server**

Click on **Launch Instances.**

![](https://lh7-us.googleusercontent.com/TG5j2OzUCR3p65XqERnFYRO9YLBrymZpjlPsoWuw9jfTG_BiKIEfjRYkG19kNTj39ZxMt7p74fNXXs61gs2EHNTczr8luW77-6sd_aObWadBb4Hs5JRege_BfrPH5byLhwlI9cQ49UDarVx-5BFJumo align="left")

Provide the name of your Jenkins instance, and select the Ubuntu OS 22.04 version.

![](https://lh7-us.googleusercontent.com/jJjXPwKFMPXdN16Dq1UFTfFWAHnnfq6HTnrRFHYBP722tCPeNCjohVQOA8L-k3NjTm-6sucQicvzI-aL1Ze4l_yskMN9XmCa9xhhr1Yj_ctGMpxppA__Ws4PGSmvDdS7NUm_XeM_pDLIGSdjlSaon0g align="left")

We need to configure multiple things on the Jenkins instance. So, select the t2.large instance type, provide the key or you can create if you want.

Keep the networking things as it is. But make sure to open all inbound and outbound traffic in the selected security groups.

![](https://lh7-us.googleusercontent.com/RLQoIXlVb1fGeGW03ih_cD00zCiOzWKclKIjxwrb0wk7misY5Zn-F3VreD-sknnchpwGSuk-aMNAsoF_L2QO3ApZ_0c0w4XszHVlsDLFKnLuSuTbA7vqMjXw21lL-aLEFSk8XG33rrLVlCqsfMSk-MI align="left")

Increase the storage capacity for Jenkins Instance from 8GB to 35GB and click on **Launch Instance.**

![](https://lh7-us.googleusercontent.com/aYmrxC85Tewouf-GJb_cSSmJOqe33JF6XtLgb8qRTzpECEtEIBhGrYtLJxpo1bTfw2h-ET8jkXiylUEp9_jSJo6iZLxrLpJaYZ-zw4Y2FmQXPQhyvBVbE61wLKmFinJBJTpTkjhu3z3jgkQoXQIdXNo align="left")

## **Monitoring Server**

Provide the name of your Monitoring Instance, and select the Ubuntu 22.04 OS.

![](https://lh7-us.googleusercontent.com/vDJVT44tnzHq9AxdARswQqJu318Q6lkyWnIsC83Hyqk3mWXo1UuQBt95bSHItSLiXMxjPCVy-7MR630waGX0BmBBau_UVW7_ZcfyHTUy8PesO8BLsEyucLZFGUOR7TltZkuzBCETIOXf6aPM8NtKDYI align="left")

We need to configure the monitoring tools on this instance which needs a minimum of 4GB RAM. So, select the t2.medium instance type, provide the key or you can create if you want.

Keep the networking things as it is. But make sure to open all inbound and outbound traffic in the selected security groups.

![](https://lh7-us.googleusercontent.com/ri_t0LrJMuFbDxL7aQe-aic788Bp-p9YjK7RtfuzVeMnMiCVSJomakVL0yv2daa6hS2nb1aRfFy8B5Py9YqC4h6VCzzP7vmrzwpdn_v24x1Yg2v44UVb4Zesrh6vwrjm-LrT69oM80qII4n8tAMIB0Y align="left")

Increase the storage capacity for Jenkins Instance from 8GB to 15GB and click on **Launch Instance.**

![](https://lh7-us.googleusercontent.com/Er6I_gFZLPxCDm5NFnqhDupQbsxjupuBz9xc4tXlWjQROxdnixrkq42e6v9qRAQgNmaJj792hTmBnGDTFKCOi3jCjF4ASgzIKixP3puC4JzqhEz64KBNBoJeNJnQlT2m-lsvQs2NXMS0OPPCzqInaww align="left")

## **Kubernetes Master & Worker Node**

We have to create two Kubernetes Nodes which need at least 2 CPUs.

Provide the name of your Kubernetes Master Instance, and select the Ubuntu 22.04 OS.

In the Number of Instances, replace 1 with 2 because we need two Kubernetes Nodes.

![](https://lh7-us.googleusercontent.com/q257bQ-xbC65fV40210XZdaA7B9RhbMRPCA3do5EI4VDHijM47ChlzPrDVDvBb1FeSzTuUdyRVMwQAaPnjf-Hgt2u05UODKgfsaz911JG2RGYVG_xhMwndf3fepfdsO0agxdrMw_wSOXnv5RUUFx74o align="left")

Select the t2.medium instance type, provide the key or you can create if you want.

Keep the networking things as it is. But make sure to open all inbound and outbound traffic in the selected security groups then keep the rest of the things as it is and click on **Launch Instance.**

![](https://lh7-us.googleusercontent.com/qDb60LoS4zkgUelNMjK5QezDQc4uQVtItjKwuwNTsHw1ATUVtgf-eRkF5BoggNZAAwfyLmKQb95YR41DxgdxmfaLm_5z1o8OaNy6QcNDz7FhWY-jfjqlafOUZXcVUtI55pDtc7rej7mvajgYTnya8qU align="left")

Rename the Kubernetes Servers and all four servers will look like the below snippet.

![](https://lh7-us.googleusercontent.com/eIuoOJaki5pPE252E-Bd0oQLTZu3jRpHuQ1u9adtJuI8Mm1JQZ5jgYz2T2-Bc4yHzu1y7MpOdIk4BkeF_lcq8fYguUAWPmBKjEi9tvXPeNY-Ym4APpeLtsrjH8-3YU-eVmZaLXxUvIAJkmiUZo9yYwI align="left")

Log in to the Jenkins Server

![](https://lh7-us.googleusercontent.com/LAejRjH4HDgO3cObDREBdgqToj_ao1mVmVqOQHi-PwAqe6nLlcLZw7WFD9TlFDHWlDDPxA3lCvl-g-6JGU-loWohdwBYVVI_qYIhMuC-D0mHkgfi_zbBg3MNPMuT9RtGYxvt2OGwa11iyc8i8_WKdYU align="left")

Download Open JDK and Jenkins

```bash
# Intsalling Java
sudo apt update -y
sudo apt install openjdk-11-jre -y
java --version

# Installing Jenkins
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update -y
sudo apt-get install jenkins -y
```

![](https://lh7-us.googleusercontent.com/n_fcuCD1pH1T4_UQ_RaICz4mSoi5pmstXWEmCYsUx_mmQuDWIgJ24ZVp69v1qqGqv3N7TDV86R4NKEBXeOXCUvRuTol-ECkJ7F7vizebjU5m_Ot7TtypCXcyykJVqwGF-ajPJepR4ttshHBezJirRxQ align="left")

Check the status of the Jenkins server

![](https://lh7-us.googleusercontent.com/4Yg1QMbgtp88lUyg2XJf9S874uQ9A01ycwSd-xxALJhHMvoFFoV6OnHotNPtZrk1lDq2vKag4XE7e2_nshZGKHV7jFm5UJyc-Abxr2FW92JhLAuhhPWrk45j_R3sRZouXRJPLi7bsG5-AjFFLPfYSh4 align="left")

Copy your Jenkins Server Public IP and paste it into your favorite browser with port number 8080.

![](https://lh7-us.googleusercontent.com/BDu7Rc4McJm3K6CGTqm4q-9MOfilT-PWlvZXQBifDt94rbvfD9IfvY5AM25-3Un3TEtAYLCJnK_on19T5HQlJZlDZpNdUY5T7GaXq6tyvjoLUi3D44UaubAHm2ggjVSboiSrXMggeOEtee84PWqakFY align="left")

Run the command on your Jenkins server

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Copy the output and paste it into your above snippet text field and click on Continue.

![](https://lh7-us.googleusercontent.com/44ST3Ddz1aTB7krJdhxx6vNMKTFMjAKEePY7yqnKitNAOa9ddRNOBDXgvcy-ynTrzt5MrIhmfGMCWmSYQ9GmE3L9cYhDgJg1DLJbbSe3oPncbdnxgqbBYmshZ_xNwmIjSgrCVyf4X1hgsUnkhdWJn6U align="left")

Click on the **Install suggested plugins**

![](https://lh7-us.googleusercontent.com/LmkWpmVZs--a-0Ze40a-GfJDbEB4V75TFE2_lawzsPcvu5cfVzMSk8Wit3vZRReSyYM81KEhJq5oOm9xtSKeXCe-w4ZeHpzENagqnOyjBorQD77XkTftGd6otx_FIApcASGO9jvUQag2AJQkitEhYYQ align="left")

Click on the **Skip and continue as admin**

![](https://lh7-us.googleusercontent.com/M2_GWwaKfhDv_-qZpcnQD4uaO5kV3NmU5L9HVVBJj3OP7XsNDEtIATMaBlFwgfXA8I0iMnOHgDBbJ1JdAVeooUobH51tkMqqk-IkMjZEX_abLAldsb4nGjIErAe1eYeEb6LxrRzlvI6anPciVwLdPs4 align="left")

Click on **Save and Finish**

![](https://lh7-us.googleusercontent.com/IOnsasRYVzt8iH5YuooxQTa04gw_jtURAz0y8z8wtIjX-5yYjF2_8E5LVv0PaUIUlYyeHMBuop9RGMKERtqUR8yNxk1rU9UaX25qCNW6CCfu8xfvAA4DYgLKF8Ovp9eNi5IaAHEJh-tn4_P6Er4K9wA align="left")

Install Docker and configure on the **Jenkins Server**

```bash
sudo apt update
sudo apt install docker.io -y
sudo usermod -aG docker jenkins
sudo usermod -aG docker ubuntu
sudo systemctl restart docker
sudo chmod 777 /var/run/docker.sock
```

![](https://lh7-us.googleusercontent.com/zGir8C8S1RcgyyzuCesic1o02YPtekU5sh0UaYwzapzyUJHX4NexT3pgdRgIQvp2Bh9zXeU4CjSnjJgFsRMepSlc2mZzB2FvssGf5j3dLLP7YNhLqFKqhLKDG4543zb-Px2nD3swOMn1Pw-EPcFxM44 align="left")

Install Sonarqube on your **Jenkins Server**

We will use a docker container for Sonarqube

```bash
docker run -d --name sonar -p 9000:9000 sonarqube:lts-community
```

![](https://lh7-us.googleusercontent.com/CWY9TOdL6JGEu1vRKwxvuEBEYmgKnXUbzJ68zHoFAwYrK-28eDyKJk1pjYVxdByuLasF37wIVl5970i3ll7rYtRBhMmyufU6sJYbZd-kmmrcbOBx9yO_WLKRUTUkTOKKSpruw7Uvvb73gMNdq5ZAB2o align="left")

Now, copy your Public IP of Jenkins Server and add 9000 Port on your browser.

The username and password will be admin

![](https://lh7-us.googleusercontent.com/1rP0aKYZUyBN3TA2F70r_sGjQ5aamUvyQDeeiKunxiBLRyh8CN6266_ahLdcmiECKxsqWpOglp53B6UFzz7codpxqvg80jv7GWLFbgm1QVlemcpw0zUeMTRZZLuqiiY8_EglWsNqJ_rt35ck4TRw6pw align="left")

Reset the password and click on **Update**

![](https://lh7-us.googleusercontent.com/6KoHJCV01n2K9wtCQlh-FWZrYZ6zlHWBeHJrThKoQqp233SfB6iUjwMMhhhlfhERCfTfgRJMf2vMXOJKgvrVh8vsbPTySmu2VUvqlghrmOqpIqGjI8pX01vghYTKEJLBocBgxLZ1tMfjGVxEnXssEIk align="left")

You will see your Sonarqube Server in the below snippet.

![](https://lh7-us.googleusercontent.com/6mlmRaFSfoMzfCmcjbT3wKm9mdERM223tDr7g7PfCaIdM20ljwOKuvOxiJU0BnbUpeAGKzb8wn_9lsJSCKsVdChW9EdoUMF3wLk7hIyBcHNMzeviTDQDi6aJTL4LSJFLacjeNpblsetYoDOIgX4DeeI align="left")

Install the Trivy tool on the **Jenkins Server**

```bash
sudo apt-get install wget apt-transport-https gnupg lsb-release
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -
echo deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main | sudo tee -a /etc/apt/sources.list.d/trivy.list
sudo apt-get update
sudo apt-get install trivy
```

![](https://lh7-us.googleusercontent.com/1i-jNmhAe74lWz-AUdI7UHpzmwU2chWzjnwIteFUJdlYsu4Woyt3qKAc6-jx0SOy92CWry_A9R4D4PEIKVO1Rd2UQAcQY7BxueZzvjS9mLngrtyzkmhA_K7kieEczy8mU_AxkoSvFv-BiOUv6nBWkNk align="left")

Install and Configure the Prometheus, Node Exporter, and Grafana on the **Monitoring Server**

Login to the **Monitoring Server**

![](https://lh7-us.googleusercontent.com/dtfO3wE0J8pYz0SZTKhtTKib0A0WyUV6rr4fs6AV_qQJf-lqVfZx5BjXLL8onMouF_SeKhYLshBWV0ENDAfUVGd-1LsyhmYao4j0aQEeA6O1x4sZunV0R7ZcUugs0tVRaGuKu-EiyiZLEdVrJYAN9uc align="left")

Create Prometheus user

```bash
sudo useradd \
--system \
--no-create-home \
--shell /bin/false prometheus
```

![](https://lh7-us.googleusercontent.com/bWUohptdOL9txxX6UwrVuLi0oMb3wS1zo5nKWs1CHmKPiK3QZmzCt87cvuujso4MklAxCa_7TBSivuqbgx-hkptmqE8tI6jzR4qhpCc14iKjObgMJXyMWVa23bWHMwjIxoOxHP6viaxOfCKcEHsR2nA align="left")

Download the Prometheus file on the **Monitoring Server**

```bash
wget https://github.com/prometheus/prometheus/releases/download/v2.49.0-rc.1/prometheus-2.49.0-rc.1.linux-amd64.tar.gz
```

![](https://lh7-us.googleusercontent.com/w6Dr-ciB0Rll4e0tPOfA23yWBWUr_mWMt2II15xTDjvrlEfK1J7-mzw8iJzU4n2_6mnz4jv6slOY0n4g_4uQDQtMFRZaOpZZhPZVnqvOT38Kt7VCJsDC_eF0ZidWYZCUhZkbMUczDqsjfKmwj5zIT_k align="left")

Untar the Prometheus download package

```bash
tar -xvf prometheus-2.49.0-rc.1.linux-amd64.tar.gz
```

![](https://lh7-us.googleusercontent.com/ha7mT8gQL4VTYLDFYD3UcmG6j5nPFwag_I_pt8CavLxySZYu5rgH78PGswLoQqh_FoQzSKD_Ko7ve9385W35NfLbjHhPKZJr0luURx7CEf4Dq_PQze1I6DOJYi9blJeDRU4ddgpHE3KeUzD7zuAK2xE align="left")

Create two directories /data and /etc/prometheus to configure the Prometheus

```bash
sudo mkdir -p /data /etc/prometheus
Now, enter into the prometheus package file that you have untar in the earlier step.
cd prometheus-2.49.0-rc.1.linux-amd64/
```

![](https://lh7-us.googleusercontent.com/hfAQ5fIoKDLIq4vL6s98s2ITvSh9jMSd_P4bTOoe83cTBYaEUcnpAwrpxAG3xlT2kcsF_sQOJQvw59qoIcpPeVQ7bI3Q5jKIHqMR6F13J5Aq0ItkmZak0no6Xp9W9E79TMqLaRjfFFt5sf8phXgPyXY align="left")

Move the **prometheus** and **promtool** files package in /usr/local/bin

```bash
sudo mv prometheus promtool /usr/local/bin/
```

![](https://lh7-us.googleusercontent.com/bgseyg130Tx61wl2Fwh6MgJIPXm9g8er1p9ntZtt4_gGhcQPDcA_GsGoS6d6gqQT8MSMBF_GRy7rsdctc2XthNnktxzbNxyEa64IMDFOecV7ArR7On5Olad_jIe2MNMU7Noyg4jIkgQQjJ0GBI7mRKg align="left")

Move the console and console\_libraries and prometheus.yml in the /etc/prometheus

```bash
sudo mv consoles console_libraries/ prometheus.yml /etc/prometheus/
```

![](https://lh7-us.googleusercontent.com/X5GSU5Qnbj5ru80uqc3lFE0jzdQiUYqv04JcQCeUN_CTy9Zzc57tTvBb1lwmQ26c-eJq4jT2hfGo1t0yEMJc6ub8WZq5zMZDR6L_qIyvheW4RlNr-NjPkIFDXuZhXp7PthHLHcgYT5kLrWlORYInkok align="left")

Provide the permissions to prometheus user

```bash
sudo chown -R prometheus:prometheus /etc/prometheus/ /data/
```

![](https://lh7-us.googleusercontent.com/zksLQve5O9vOKA68J11b92-iTIDUOkGyMvYTwWh4zL9k1D2_DNr2Bz_wbG0cWedqWN4bK12RkhAdxVBr7I1eiNi1bn2UurQBVITR3JXesR0nRKk04iOUquAtz3B6HHrTfk0fxYocuB24m6bfc6eBtso align="left")

Check and validate the Prometheus

```bash
prometheus --version
```

![](https://lh7-us.googleusercontent.com/VceuG0TqnPQGLMe6vo4NA--nRa-9vIA3zoBvrtGvh7UpO15ARJe3iWHKFqaiHQIrl_uwdeDmRrbkLTGqyA6b0QjRmka3XPEWem2RNYpH2AHB5Er2r5aLn-uAmxIHM9hFvwhCESY9inaVLB6qoISLJYE align="left")

Create a systemd configuration file for Prometheus

Edit the file /etc/systemd/system/prometheus.service

```bash
sudo vim /etc/systemd/system/prometheus.service
```

and paste the below configurations in your prometheus.service configuration file and save it

```bash
[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target
StartLimitIntervalSec=500
StartLimitBurst=5
[Service]
User=node_exporter
Group=node_exporter
Type=simple
Restart=on-failure
RestartSec=5s
ExecStart=/usr/local/bin/node_exporter \
--collector.logind
[Install]
WantedBy=multi-user.target
```

![](https://lh7-us.googleusercontent.com/aVuoKAUlqvSnpLPqJipXqahKL7m1-bPjo4ELh580ZpwjXinb303FArgu23GW9OJsAnlbq-nJpGoQ-JGkmWCh7uS7gEvAq7sxKxWkEFZIcw89WX1QsLgTk_dYOsRt1cnsc3btX6CldPMpZdxBBJF29eA align="left")

Once you write the system configuration file for Prometheus, then enable it and start the Prometheus service.

```bash
sudo systemctl enable node_exporter
sudo systemctl enable node_exporter
systemctl status node_exporter.service
```

![](https://lh7-us.googleusercontent.com/OkF3Ok4ndP3wrK438FcIchR6AzlwCN--0Xdf57XaMINZCIRvgCGwXQDNohzWHxffHC2R4m7kUUslwxD3JmRbMA3QHnnnBlPzxLqXMeG8UjJ5AtLExwu8wyDQ6xCbpae8N1m7ByEIBtriQ6KdoBZnDJQ align="left")

Once the Prometheus service is up and running then, copy the public IP of your **Monitoring Server** and paste it into your favorite browser with a 9090 port.

![](https://lh7-us.googleusercontent.com/ukmCRjhzEDBwNznJ6yDYpqZR37CXkxTeYDxICqyPcfRubWIK7upNo_u8nKVNFpCs-EA-ODWS9IJVroI8wRVnBzAvNS7ipeCEieVoXG55SXKXBJ6kJnfOuIoUsoUo9Su_7raCFF2nYGxyMxaV_mJzZ7Q align="left")

Now, we have to install a node exporter to visualize the machine or hardware level data such as CPU, RAM, etc on our Grafana dashboard.

To do that, we have to create a user for it.

```bash
sudo useradd \
    --system \
    --no-create-home \
    --shell /bin/false node_exporter
```

![](https://lh7-us.googleusercontent.com/HPUN0lBqwj7JeeoPCFZYRmkzcmyBKj8k-d28qBjWAXWB54aK7vR0S6f5gfR6ds6bc3hziuVXDEKbhVjU4tUdlE2eN6lWatdPtDhrnlnS1nB-5sKKx6eJX3BdhXoKjkOJDcuIoga1KgWAaCS4v7PaWdw align="left")

Download the node exporter package

```bash
wget https://github.com/prometheus/node_exporter/releases/download/v1.7.0/node_exporter-1.7.0.linux-amd64.tar.gz
```

![](https://lh7-us.googleusercontent.com/LojYLs07mJ4K0q8tfJlykUkW_egnn5cnbayzW_MxFbIGDz5iyFzOlxPIVhz8HTKrR-5fvyocx6UUPcsUaTNMQrSzOd1N1cfRKp_h3OO-MsA8X7QsEg4uAlIXXTcKkYoU8c3bd54PWEurJy64GlzK3BQ align="left")

Untar the node exporter package file and move node\_exporter directory in the /usr/local/bin directory

```bash
tar -xvf node_exporter-1.7.0.linux-amd64.tar.gz
sudo mv node_exporter-1.7.0.linux-amd64/node_exporter /usr/local/bin/
```

![](https://lh7-us.googleusercontent.com/5n4b4eXxcvefEA8mdR9dBAstsPJZQdCqWFlhbK4knwblDI9DE3ztpEQ9QNTP3Y-bCR7lE8JE4wjdpZCcEClPjro4b-szjFRWaVKzWM7jMjviGXna8_OoL07we5OwgwbYFfF4XjmmMRvRwAy1TogLHyM align="left")

Validate the version of node exporter

```bash
node_exporter --version
```

![](https://lh7-us.googleusercontent.com/eA2kX0Q7_4llzYYRWxc5_AJkyGeBixT4X5zjuDgH_dJYNS_LoRxUJIBmSr0kwZ7Fr6GLCccs52Pt06p_A8VvFeAZjOefEwyeN5RtBDVWwUrZkxJsVzgqdWZOTHt1fd04izs7ocOui_vRaDwqi9S273I align="left")

Create the systemd configuration file for node exporter.

Edit the file

```bash
sudo vim /etc/systemd/system/node_exporter.service
```

Copy the below configurations and paste in the /etc/systemd/system/node\_exporter.service file.

```bash
[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target
StartLimitIntervalSec=500
StartLimitBurst=5
[Service]
User=node_exporter
Group=node_exporter
Type=simple
Restart=on-failure
RestartSec=5s
ExecStart=/usr/local/bin/node_exporter \
--collector.logind
[Install]
WantedBy=multi-user.target
```

![](https://lh7-us.googleusercontent.com/ax6CTvhlHhHhqy-x4ZKQcfeaFmTRgOEo6lphHiwG2zuHvDn3NZZbp2C8_5dNGytw3eX7Oi7vj5aVRcUdWaKltvbeEZiGbYWsblY_Jjwg5x4oIowTUkJjuBbC-FXXlYp1sAecFxOIfVL5uSpKysOQjH8 align="left")

Enable the node exporter systemd configuration file and start it.

```bash
sudo systemctl enable node_exporter
sudo systemctl enable node_exporter
systemctl status node_exporter.service
```

Now, we have to add a node exporter on our Prometheus target section. So, we will be able to monitor our server.

edit the file

```bash
sudo vim /etc/prometheus/prometheus.yml
```

Copy the content in the file

```bash
  - job_name: "node_exporter"
    static_configs:
       - targets: ["localhost:9100"]
```

![](https://lh7-us.googleusercontent.com/urgW8cMjAi-uAaguWyT7wiq0NhilX33MeSFka8O-k0LdD-anZabqdsyRrlWLmI17pmI62KsLiB9fqLphCtrdzUVRNya0aKXhTsuLFLUpVVHtcl7VKNDWGsXIPEIoXVfdkja_iaz1whRms3W7wHOI_BQ align="left")

After saving the file, validate the changes that you have made using promtool.

```bash
promtool check config /etc/prometheus/prometheus.yml
```

![](https://lh7-us.googleusercontent.com/090U63BHPivtoBOPnp0HHKLBbwOBDNkNG1PEILueZVrAfMuxlNR40JPOH35LikeugFmSJ-Fn7PbNjFNBT0yWO_ga_nhX4IK0O7irk-i6RGEmztb04aJ-ylQLmZO3DFh8_C6D0pabXgbNB3f9LzcWQZg align="left")

If your changes have been validated then, push the changes to the Prometheus server.

```bash
curl -X POST http://localhost:9090/-/reload
```

![](https://lh7-us.googleusercontent.com/JCACZbK-eD1misidFsTu41bfMrmGQGUGqIE62Ure_ztrh5fYMt6jvAyrjCBYQJQNjsn-X_Ux1Mu5I6C_VSHvQ66aoDvuEwwGMTlsMoMrb3wI5U2HenE72INhvec93agDUnJG15CzkQDt5hbDHYoCgvk align="left")

Now, go to your Prometheus server and this time, you will see one more target section as node\_exporter which should be up and running.

![](https://lh7-us.googleusercontent.com/YAjP6xPeQ9G8654UvSi6crHk-wwc9E3apyebT9vh_pMXrHSqrXDWPFYer-RPiXPpXLzaGCst5t7wL1RCag2Pt-E-N8V11ZVo6EkbiibmrCEjGDCwBkWxnesYdCb9kmIaYohoRDNdQXihdpfrCUQORJI align="left")

Now, install the Grafana tool to visualize all the data that is coming with the help of Prometheus.

```bash
sudo apt-get install -y apt-transport-https software-properties-common wget
sudo mkdir -p /etc/apt/keyrings/
wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/grafana.gpg > /dev/null
echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com beta main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
sudo apt-get update
```

![](https://lh7-us.googleusercontent.com/jcpkigQV4roaGwtYuEyrWwBdw7iDETuhZpUb2nr_oXeqz-oVFSzwt-5rJ_LmOAkSA6oLhOnb4MIh2GmUPgKWyfCoFO5VrTSRoMr4WChQ2FEjGwIDpi9-uliW3mNBzGo5KOav0WBHaF0Pxg4uGzvj2qM align="left")

Install the Grafana

```bash
sudo apt-get install grafana
```

![](https://lh7-us.googleusercontent.com/WPAPmUSdgLBdkwTSpQ0SVZeORscnbOxKhYpspr_jruCYc-aZJH21YO5Zwb5A0xWVhZeZpmS0d-6yVX3lUcz7EnZDO2T8PU-uee2NAEljI5exJ6SW9cewJD29aw5gsvkpIFkICr8uyCG5chfviDslC5Q align="left")

Enable and start the Grafana Service

```bash
sudo systemctl enable node_exporter
sudo systemctl enable node_exporter
systemctl status node_exporter.service
```

![](https://lh7-us.googleusercontent.com/JCpqd5-v7GY-NfihtskDyrDokeCIEeOIdKiYa5sjHS18tF1QB5Is2JnFhXEjav7CExmofzCtx2rxmhwGaCgyvkZ-0_XePlxurwNRQII2PMufW-WcM3zdv5L3-NqcVnQyjO5PpNHrcMIexpiqkvssOM0 align="left")

To access the Grafana dashboard, copy the public IP address of the **Monitoring Server** and paste it into your favorite browser with port 3000

username and password will be admin

![](https://lh7-us.googleusercontent.com/tr192RsTg0knb9uQ6HCgROBLRFFFZe5C_aSgUStKORnuvw89eircGRLfVtEzXCyXB2sP7MX72XNow66zNm-wBNFSvntMh1B3xV8R5Cz4AK1pEUZUGmd_sJdhlWljktnejB94ioKAhwOsStZkl0WtVEg align="left")

Reset the password

![](https://lh7-us.googleusercontent.com/0JFlVo0O-pz5mI7z31tS8jYvyrCk1O8071dHpWJpN5yQm_dv7-mfDlcQmHRFYou60DndeaB7r4eUKWFm-pwsaBdX1-0l5HdMlzVXqx0VP_ckustL573S8vj_HO0aAewgaSbQQQRsJrRHQ-lWHJ06w58 align="left")

Click on **Data sources**

![](https://lh7-us.googleusercontent.com/mPjnSkTUvFgZXltIPa1o_YrZWOp_vdBIE-e1qp7YBM2SCuEi0c-sHBIcEQP_nu0nPw82doEX_7_2hSNYtonSZNgsj42Qm5fFf4OpCM0FpwNM90V7w4r99fDz7CQ7eXDzCVExgJMQdcNVIBoZtdHXOgg align="left")

Select the **Prometheus**

![](https://lh7-us.googleusercontent.com/pCscyvX01Orsp_qfCz_Jl18nX9QOsldYKJ4ZBxendgR_6FSpnDsK-ST11v6ZJHmWaewMCl6No-derm7Umex9xmO9YTt2FUGbDV-KwlnPbS_effdiBnHvH0E9EJ6km0sSHeMXawhliP3xjTsdtMtUo0Q align="left")

Provide the **Monitoring Server Public IP** with port 9090 to monitor the **Monitoring Server.**

![](https://lh7-us.googleusercontent.com/wadQT64ItgoHbQrlKnnnhNl_tN4yvtLjiLSHxOxd5fSzaun2vRs1ht6k5Fog0xcTqYradhQ2bIWKN5FEJW2wIwLALQNahTQNdg0yPyFw3u3br89RWGDblp4U8QkFDFOYEXNtD8YwLXZin6BvHNpKeKM align="left")

Click on **Save and test.**

![](https://lh7-us.googleusercontent.com/yhNClmrBKQ_bhKzpddRCPdc8MrvvoW60dMEocxkxUQuBsvHolT5f0GfCAqzrGF6GRtTdo8_8Jl5ZG0OPfdVbq2NTmugfjPrL_52Qw8rdMxOBjIrhYw2Xoryv3X2hQoJI4gALU2Gk2_EFlTsPpXK3__Y align="left")

Go to the dashboard section of Grafana and click on the Import **dashboard.**

![](https://lh7-us.googleusercontent.com/MnWz9GWXG6PWq1vNSyZhRjgK2qJ4APS0_bsCFJoYu12B44AYdrn_m-VscwyBFKga9YJv3XKdONfHo_YOg7syjSKh_0tet8JvTGn230qj51woe8c2JPlDOd-5rresGb1kR9vwpLKQnkQLAkVvPdwZe9s align="left")

Add 1860 for the node exporter dashboard and click on **Load**.

![](https://lh7-us.googleusercontent.com/j_eZpWVEKldtmRAkLc27CLEzhl5ZPILCg0UZG-1GOOfvdDrxwrZHAyiXFU6SQ-FX1_MMyM8tK5zVI3QMYv_pyb-74OAjNv4z7mXO_bUhYAhjAmRrzPTmvZ_9QHctIZ_wJhW7LcjmXFy6MGebJJFVC0U align="left")

Then, select the Prometheus from the drop-down menu and click on **Import**

![](https://lh7-us.googleusercontent.com/0DD433f7R4wQvcWcKjr-brLpN2P0Awqf99vI6tXI4AXauSUjUspACZK9fbvW_G3ZAmXYeb4OE6BrumY2TytkrjSPmoc553WpW0Z91AX6j3Q5JzXq2eyM9ZFd3byPvirrawVh0-hSm4HvKR1rb1VEnDs align="left")

The dashboard will look like this

![](https://lh7-us.googleusercontent.com/nK4_Y7ttJUg-KPi5GXLV_Nb0XpnRHiq_EEe9IjZXscoYTBGVxs1srgZzeoEk8h8zLYDfwNg3gNH7VybXCyt6h1acDhTxuzodekg8WqSsn_deNkoM5hhj-zJG0lslqoGe1lhyWPTM-O_7fnzcDu5u1Hc align="left")

Now, we have to monitor our **Jenkins Server** as well.

For that, we need to install the Prometheus metric plugin on our Jenkins.

Go to **Manage Jenkins** -&gt; Plugin search for Prometheus metrics install it and restart your Jenkins.

![](https://lh7-us.googleusercontent.com/jX8lchle1n27E79eBU-ER1JDgYclGRopC1uGHcamlEBFthfONNQqSPzjVDN5IIDbJEg2nhD3v7GYJlPCGGTyAqE37ATmcr-hx9HZTForoWnxC5w9hJs3yUV9gqirp2EBahGF5zEwH6sLGBXWjqxGKKY align="left")

Edit the /etc/prometheus/prometheus.yml file

```bash
sudo vim /etc/prometheus/prometheus.yml
```

```bash
- job_name: "jenkins"
    static_configs:
      - targets: ["<jenkins-server-public-ip>:8080"]
```

![](https://lh7-us.googleusercontent.com/QOOgJXdIG9d4NobJAiomXeEBJIJzZi7jBM6hC0bcMXpXJ54FJ3qgtPnpQt85SXV2sarf-VBXajXYMQJl9J5MeEXPOMAO-07xbalKxd_U5XHp7GGy6BTzE32S8yv2c9HpHIuG3NsU75soEq4bT4spnRM align="left")

Once you add the Jenkins job, validate the Prometheus config file whether it is correct or not by running the below command.

```bash
promtool check config /etc/prometheus/prometheus.yml
```

Now, push the new changes on the Prometheus server

```bash
curl -X POST http://localhost:9090/-/reload
```

![](https://lh7-us.googleusercontent.com/ey2DlTSzpkwExFusknev38srVZLUeXPOIlDHsp9-9HeNBX62dHjX67iUP_bm-22thkYYDIdQUStpV-TscLG7qY1aTnlOCu8VqEpcsIIWoxiRq_BNEi1II_7fsUkDlzkfiDbtPEwMbkLEMP0KbbrCJx4 align="left")

Copy the public IP of your **Monitoring Server** and paste into your favorite browser with a 9090 port with /target. You will see the targets that you have added in the /etc/prometheus/prometheus.yml file.

![](https://lh7-us.googleusercontent.com/iqbiFc4SifnXGWwlpRm2EwGtZ53aUfUtffo3P7jphSXIxRcgjDTZh1E6YIXTwOOADjbXSSI3zbfW4Q8LPHohf5Z27hflcVh3Vk7mY__4IH5TEhM1LYd6D5N8ltw5XymsP_jCD3geKUmfP8xv0eiquWU align="left")

To add the Jenkins Dashboard on your Grafana server.

Click on **New -&gt; Import.**

![](https://lh7-us.googleusercontent.com/ru5oi_kP8ThCNLWr71RdTOP2akBYL7LjJwQgMgTGfnl6r7Ws8muZ3ggkua6XJomWdM4t3MbV3pSqdfadi0IcKHFOsPLqmt3aXwPyTsLYvFriqbTw2ForK-eD7Ra_dP6C287PlGz36Wo43cSE0MrZ11g align="left")

Provide the 9964 to Load the dashboard.

![](https://lh7-us.googleusercontent.com/kaeYd2M2I4BFmD6ihRucBHQHtpIqVb5C-saOV6t0zj_2spPfgbVTg2FznV3xIFm0UbxGklk_dY2gp4pVs1Ts0M8HGR_xO8ZxMs0h9mEemrMWcle7OWfgkKWStbd7u25JLyT7FWAhdBgU6ZYBMvdAXLw align="left")

Select the default Prometheus from the drop-down menu and click on **Import.**

![](https://lh7-us.googleusercontent.com/wQ3kgjUDcEcZLYucn3TAdiHy90YfxJ8hmZ7-s7S0_-sqV88Esmk9PxMnSOJYI1kkAB3DZCVGpNo98pugWheQm6NpDY-Yq441tG9eSOHy0zxKKb8Z4fDzR4z3vSdtLZAAVzNqm19ZapW2yXEoy_jmxqk align="left")

You will see your Jenkins Monitoring dashboard like the below snippet.

![](https://lh7-us.googleusercontent.com/ZmHVIghNqP3wZqr5CpuZtuJaHGaL4h3oQUOmVJWvNVLO4xrqBMClUH1_gNZMjdRCJl84SWrkn3-shlJM6_gtvMARA2oMztUr6xWrEYIBLZzUQuxGxz3tVgSK-F5OXbi1oAwBLE3qmemLkZflvPdV6kc align="left")

Now, we have to integrate Email Alert. So, if our Jenkins pipeline will succeed or fail we will get a notification alert on our email.

To do that, we need to install the Jenkins Plugin, whose name is **Email Extension Template**.

Manage Jenkins -&gt; Plugins and install the **Email Extension Template** plugin.

![](https://lh7-us.googleusercontent.com/KMRqAqERA4ekhH7xakBRzr9cNIqr02Ov7c_qYicVQFWSyV5Ksn19AczQamncHp0GDSLvFyOPGkSsUcd2VDpHR19zeRWrkcepOm6lxGIt1rsuxxJ_5_9c9j1D5VRZstG8kMNlFIGlCtL0yUwOW6YLPYc align="left")

After installing the plugin, go to your email ID and click on Manage account and you will see what looks like the below snippet.

![](https://lh7-us.googleusercontent.com/Bie0WXBpgIULp2CSh3Hmw5jADGgTjLtWkVS6oEZu79Y2PnJ5Tsl_IXCJvvMbYVM2ZjKBKoDuFU0Vbh7EXByx1lY44Rpr8VdFircJcgWlNASi5QbxOoC4a1ZwD-3o471F37ufHuUOKw1Jc30YUVuQeBE align="left")

In the **Security** section, search for **App passwords** and click on it.

![](https://lh7-us.googleusercontent.com/Rua21pB_6rvKdzNTO1QBcOn3MOyZlAtYxmc8IDbTQyx884y5ZADoKJMZl6TB5qSkSHprQ-FJuyP8V_F5jB-S2gOQCMHkp47BNp4YzymYYkI2uP8IjfxOX9T7uJOU0Fv4H4HZdX5KSJCtt76tx1kj6U4 align="left")

Gmail will prompt you for the password. Provide the password then you have to provide the name of your app where you are integrating email service.

![](https://lh7-us.googleusercontent.com/nxxZgOVqzQBn2xxp-HUch4RsDpMh-rFEsKHjYg9R-fVH-nAbLnhR7oInJviw10phsStyZ54C8PyN93GQF5o4rY_WnFRzaPyWwIuKBWvxXjHQxX-JYMISPSInX11WyB3fo6cUX5UAoaG6IQK7Y8lioOo align="left")

You will get your password below. Copy the password and keep it secure somewhere.

![](https://lh7-us.googleusercontent.com/cBBbC-NW1O1MxUZxE_yq9hGCP3smyW_LxLB5uZqyribvqHwMZBqae14fX8Cf7tAQYB8c2k6fpAk7s7cdU41IC3dtIyHnkZhqJHyGx4enl4u7YDdqbK8xL85orQ1bMtoychRg84IEEqiMDirx9MpRBEM align="left")

Add your Gmail ID and the password that you have generated in the previous step.

Go to **Manage Jenkins -&gt; Credentials.**

Click on **(global)**.

![](https://lh7-us.googleusercontent.com/HW1HEebcj0-17PDRE2ck8EB8HusqIxAo33vyMEyjnuz-NPhbGbo9zjPPBpydooT9cMde0H_l-CUIJWcIlvAfS3y5D7lcafn-xEVTwRGFFO929qcEGkcnSJrSCGC0PfVo70wNtN9U8bFMFyFhoE0MxC8 align="left")

Click on **Add Credentials**

![](https://lh7-us.googleusercontent.com/CjzL5MpXs7wHPGM97GL-__0lyRDj5c5vMQFJfjaoKGQuI9_upaEjU-0Na6PcabqfAyADEfkdZM73qYyhW5XeVHPuikzor9p16NsEgOGrMZFq-19scEJyrko-O8sgJM90un296SMAPaVmNvxwOcI_UZU align="left")

Select the Username with password in Kind.

Provide your email ID and generated password then provide the ID as mail to call both credentials.

![](https://lh7-us.googleusercontent.com/ohdE1xG1DfYnA_sigmTjNPatlokzCyfkhvqv0S_3FOhAotdzyEAxeDX141wBC3wYLBjGiH1WoJcJ5Yltmam3McwzLXIJibQpvV-ISIS6e0iGjq6tNLEIKq165tR5JLGf6kRmKiICHIGqS-qV0LCa-lY align="left")

You can see we have added the credentials for the mail.

![](https://lh7-us.googleusercontent.com/Cb2obDZoP1z4DgygdZndn1izA9Jfkiv7Autx7WbJkDaODqIiFjtKeVlTguvx4EVJHGAb74OhpzatwcoGDNEVmzGDeNIZhvq3Wd8ze5doRLSd-93shTLNu0rTM9JeO7CBWfLhPvoexp-tokT3vbF_5zg align="left")

Now, we have to configure our mail for the alerts.

Go to Jenkins -&gt; **Manage Jenkins** -&gt; **System**

Search for Extend E-mail Notification.

Provide the [smtp.gmail.com](https://github.com/prometheus/prometheus/releases/download/v2.49.0-rc.1/prometheus-2.49.0-rc.1.linux-amd64.tar.gz) in the **SMTP server** and 465 in the **SMTP port.**

![](https://lh7-us.googleusercontent.com/0roaFYOpCbFnvJtLoeqw19CG-GykR3zE8-Y28F_Rd8GqvWA1p4B3HkuJeFYEyywWJ6HkFeZ10WZnLc1SBImpGAvH_hB3WZjbLJdAQp3iRtrRY_iUWDwnxBKR_VkYqWPj4VaFXTaHbVkRwIky2eddIAE align="left")

Then, On the same page Search for Extend E-mail Notification.

Provide the [smtp.gmail.com](https://github.com/prometheus/prometheus/releases/download/v2.49.0-rc.1/prometheus-2.49.0-rc.1.linux-amd64.tar.gz) in the **SMTP server** and 465 in the **SMTP port.**

Select the **Use SMTP Authentication** and provide the Gmail ID and its password in the Username and password.

To validate whether Jenkins can send the emails to you or not, check the **Test configuration by sending a test e-mail.**

![](https://lh7-us.googleusercontent.com/lvO21zKhyrFekxwEc8cE2gGHcEyJxKnpAPbrNlQ_tZwLT1yr30-jpocesc6Gxa53FUQSIjEjCUXw-32un3wRxqREryRYo8WZLTwGcGUidz1QtpHNtLWFWQ7jRphRhKmDshIk4BqEz6BPGkw2tLf0Uvc align="left")

You can see below for the reference.

![](https://lh7-us.googleusercontent.com/Ddvq5dMV2bon_lsEIxmoT7iXpYoLSU06cPKPvSNoLPSDuQ6vUfURPU4c8Vp-EkPuZpDQrtD4YBTYzGngbWY8NC7-22y_-Ibv2WMaz0N0E9ByE1YhApEy9BqhJnZ5KK86pLhrDylWF9Pfee1u-M8Fy7E align="left")

Now, we will set up our Jenkins Pipeline. But there are some plugins required to work with them.

Download the following plugins

```abap
Eclipse Temurin installer
SonarQube Scanner
NodeJS
```

![](https://lh7-us.googleusercontent.com/aF-Wb8AIaPwpHaUfsWn-Hd3ulspfDL49taiaHfgJ76JoACH4soURXFrwFhZPlSAtLnwLN8lZZ_go6eZVfHYsJp1Ht6Ifp3bygOP0YPIW273yKA55ZdDDp6LaIymVDrQg9grFW2wysdmFE1LbKy5K_Qs align="left")

Now, configure the plugins

Go to **Manage Jenkins -&gt; Tools**

Click on Add JDK and provide the following things below

![](https://lh7-us.googleusercontent.com/uLxQpzKqzxR_xoJI6kYF7ORFShbQOy3YDTUvQx2nZuc1amrKNJy0ktxj7gRY0CGxCly06lNvEEnkMQcZxSW2J52lm4R9mbr96gt32zd0GjQdzeTxY_k-qKrr0nUzm3BB3WVQo4TBj-Tq8Fd_wy0jyGk align="left")

Click on Add NodeJS and provide the following things below

![](https://lh7-us.googleusercontent.com/VC_WT3tO0VNPNcQEFpQtnxuVwCExdxreC2wp1hTS5gs3naZyxVPvMR9mKDBEFNof5fqUE0ejR8dEn9Setcju8B4RIiHLCiFLkterXCV0zhSOmwbP5AL4O9TQuOVXe115KaDMkYJk4PSZ9hhqi8d8tOs align="left")

Now, we will configure Sonarqube

To access the sonarqube, copy the Jenkins Server public IP with port number 9000

Then, click **Security** and click on **Users.**

![](https://lh7-us.googleusercontent.com/OhHgDc6083aOfM4hUZV7xz5538BZLUWWVZaa6AzAekr09wnbh5wYeWog8i4DZe5HyHSOf8TFKV_Mdv2_ZF5w5zxGTr4qmVYTQ3pesZRoiiSzJbsjWhfakXWiT663UIotbFlQFUGwLnPqE9w8UblYu60 align="left")

Click on the highlighted blue box on the right to generate the token.

![](https://lh7-us.googleusercontent.com/8j-chkLmAlxxIsGW4yAk4ZHiDtA7npH8La2qjc-j1azcqbLEL2vUsNk6wWcbV_buw8-_yBLq2fN6WdPwAGDANYtNHz68qmE1RYD1Qf744PW0TMPjsg8SC2XgO-dXy9NHVEqgC3eF0NTyrUWEtZYjCyg align="left")

Now provide the name of your token and click on **Generate.**

![](https://lh7-us.googleusercontent.com/qy4-EHXw8XzFcBrHp3i7CiF2xOs_URtf9jSXNmYvAJa0jX8RMjw9weav4T5ttxJId14uc-459ujFUwEvTSi7plsK0s49qP-5UpoJTFCExQuOO0fB93jhHDXi-g99oLxe9F8MXre9DOcht5jT6JLEScM align="left")

Copy the generated token and keep it somewhere.

![](https://lh7-us.googleusercontent.com/D5vPPKMJZrJi9FUL8Mf5CCSZOfwy3JrBKtvdrrsIMpNj4UNnqL0TtF3uKOw-KGd-EeHKfQ9RYxMZKB6GNvGuF-kQ_pq48XjIQ6OistrC2xRt4S3KjmN5es3abndJ9OR7Qk1A-x9pQu1qHIscdFGhymA align="left")

Now, add the token to your Jenkins credentials

Go to **Manage Jenkins -&gt; Credentials.**

Select the Secret text in Kind.

Provide your token then provide the ID as sonar-token to call the credentials.

![](https://lh7-us.googleusercontent.com/Hkdoh5iHMPKUZNfSEQA-1wi6pLngoKAjtHFd68v0gvaT1BUlfElYOKlcOJXauJskudVLXFA-oggb9jOdnZAyuVUotQ3Bqq_d-6rvdtb1WvAkyVY7SeLel5btx95kf1bX_7Oycz5Wai2MuU9ZHL6HkqI align="left")

Go to **Manage Jenkins -&gt; System**

Click on **Add Sonarqube**

![](https://lh7-us.googleusercontent.com/tX1oQOQXtk_x_4WgnP0etRSoDz9xudfZc_RsVp1E6oIT7rwkESmcegNk3xLwZDGJ6O17Qbns8dUNLiZDEwY0EMq-AD3NAc3Y_EVonq44bRSklTliJ1up2HNOQ5f35UYiEtoEYqXCDUFwv6FJuULIIjk align="left")

Provide the name sonar-server with the Server URL and select the credentials that we have added.

![](https://lh7-us.googleusercontent.com/XkVVkOGqcoOti3vzHPlj-EBGAoKN9gOMoiKDEqRtavFudI_vzxvhVVmEJsKnej8Z9JQGMmQ9BwNP9yu8bAtXdZhEblBNS9bgRCdi3eQqeu_LPESiI38Y23ASSfEN-41obNkIGG1QEInpUPiRt6B3oG8 align="left")

Go to **Manage Jenkins -&gt; Tools**

Find Sonarqube Scanner and click on **Add**

![](https://lh7-us.googleusercontent.com/9jg5bAKs0yKmFyfzAUDaWDU7KKd0pjnmld3GChe_E7s79_Xaj_DCcy0cmJ7B1LhLwFKoCYG6_6ubtMmtUQaNAgODvfJGPtBqrvK8kI7aGjBQL3TYU_Mr37QUPiIm1M-uXfCQA0pw_a7AIZF7ojistus align="left")

Provide the name sonar-server and select the latest version of Sonarqube.

![](https://lh7-us.googleusercontent.com/ersxPCk4fxcp2AwoOpXXzPXPHU5nbB5hMDXJba3QNaYPniiczU7LLZzkKuCxDoFry8GeKjpu-SORVAI6kC_fSGxemqW9LyEx5pdHCG6KXfsibamiCpY3hegjyoz1k_Am1d6KQhqNNwTJPjhpR639O0c align="left")

To create a webhook, click on **Configuration** and select **Webhooks.**

![](https://lh7-us.googleusercontent.com/DlzyLKwyTMsdGc1EAvSFdKYB-R5mh38d-uy0LadJcT-lXtXO1eImjBxGejcaSCOIfQRFpulIJLoAaDDodO4eX67wtR8HyEloV_g6jT1xqz_hUvNsijDJEHlUg6hCMc6GMtcxwb9iEOkJzrlYhtfqjtQ align="left")

Click on **Create.**

![](https://lh7-us.googleusercontent.com/amEaFB7rEJZyQ2pBwJTJz_lvtD7WA5P4SGzTPLFzQgHawdhfzh65KtbJoC4OABiyTXkcKO-KUGk9dBt26A3jQcL5lOe52PF2vkFgpj7wl0F_cPzXaGOOenyAFfZwO57wyQzIDgqfX_Eh-N8Y0L47_vg align="left")

Provide the name and Jenkins URL like below and click on **Create.**

![](https://lh7-us.googleusercontent.com/zvchxlTfm8GMjO3hiyX12XdQWVE67q98UYdKiefE_5LIchxSuO42J4_twmIIOiz2iOkZQHIeh8-XqZCKMZzHXvDRNoMxvwYZFqK0YJyGKT9IpLlPSfjlpXou2j-I_g-BAyRkVOM1LEZ-vb55KC_WZDQ align="left")

The Webhook will be showing the below snippet.

![](https://lh7-us.googleusercontent.com/2o9dd16iAuISz2XFkyNCOMHkydeToN7TTL_qjXksMUqw-ZE4glovvWexVQuB_K-vahcGmfamtzc_NnSUFHhYdioh-Si6HKXZcI1AqqZxPYOm50MnPrAzfNAksdWMLw8Lu0NRAGeEE9RMuIWokd_UQs8 align="left")

To create a project, click on **Manually.**

![](https://lh7-us.googleusercontent.com/eDFFReIFxZhe-DscqHZRkX2_TW_rQhiNqICehR_BwW7ghVzV6qLC8pZ11s9q_p9XKFGYqEy_PKC29JDGXGY240f0FMw5B5yFZpO5A_ZCbGsloZXrStQ4WKKUrgM9vv-Mz3cUGr0M6ozcXYCeQcdPnR4 align="left")

Provide the name of your project and click on **Set up.**

![](https://lh7-us.googleusercontent.com/RHYAhKkEFC7tNsAco99Z9EIE9k5Q4XBcnt4C6QHc0oVjFfvcPvqXjsaxBpIu4aeEe13e7lqSVPs0bMYYn7pCJSuJu_imGNSc9zRqPeUAZhx-nHWwJHl2bUzlFpWf2-b1l6lYvWErqW1iTFQlGIodSeg align="left")

Select the existing token and click on **continue.**

![](https://lh7-us.googleusercontent.com/s1_j55ROUJz55cz-zN308d2H3n0ZEOWd5z1-dE8WXGwj_9cfH_9yXrPjjhv2wievhntalCUJE0PStkBB8CBmZNHXf2-mAxxvBjRmQY73ZtRQI-CPYDok0ifGXcx4yw3rU69fZAZx7RCzQRdPiBY-9ok align="left")

Select the Other as your build and Linux as OS.

![](https://lh7-us.googleusercontent.com/oDGyEQU5MXKi11-BJTJCTpspTlv72rLbhKZzH_hcT1UoaTPWSM4k-m9aF_VbmfcLEW3awsOHAGrv_DoBlHwmy21ET1VVFVB4GvbfxCfoG_A8XC8mx4jBUnO-yvxAjbyrZQgWdglxbPI5BIkihoZbosA align="left")

Now, we will create the Jenkins Pipeline

Click on **Create item.**

![](https://lh7-us.googleusercontent.com/yVjyihb0TRRkzhC3GbjT6TIt0Im3C7UPkRX1CtI2pssUGQM18ann8Nx_pyDVyaNrYFkUCXytoTgIrvgZp_aMS03VIije6epljp3EYNW_ZixBkNxva0_JWcsLH9VWq9xDbkxNBpnW4ZGC0Mzt_vazX_k align="left")

Provide the name of your Jenkins Pipeline and select **Pipeline.**

![](https://lh7-us.googleusercontent.com/Ej6ds9XneEXWil9RP3vGeHKlXud_4b3btAMDntj-KdgaahehN07BoFSxN2oWiHJnazJEE5sIbsDE12svUd5TnpA8yCqH4WsNJID_VKjBdsfyatf_OKK3hnkIeFijdtkLZb41mQPEd9RfM0gOnpRZtMs align="left")

Currently, we are just creating a pipeline for Sonarqube analysis of the code, quality gate for Sonarqube, and installing the dependencies.

In the post-build, we have added email alerts for the success or failure of the pipeline.

```bash
pipeline{
    agent any
    tools{
        jdk 'jdk'
        nodejs 'nodejs'
    }
    environment {
        SCANNER_HOME=tool 'sonar-server'
    }
    stages {
        stage('Workspace Cleaning'){
            steps{
                cleanWs()
            }
        }
        stage('Checkout from Git'){
            steps{
                git branch: 'master', url: 'https://github.com/AmanPathak-DevOps/Netflix-Clone-K8S-End-to-End-Project.git'
            }
        }
        stage("Sonarqube Analysis"){
            steps{
                withSonarQubeEnv('sonar-server') {
                    sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Netflix \
                    -Dsonar.projectKey=Netflix \
                    '''
                }
            }
        }
        stage("Quality Gate"){
           steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token' 
                }
            } 
        }
        stage('Install Dependencies') {
            steps {
                sh "npm install"
            }
        }  
    }
    post {
     always {
        emailext attachLog: true,
            subject: "'${currentBuild.result}'",
            body: "Project: ${env.JOB_NAME}<br/>" +
                "Build Number: ${env.BUILD_NUMBER}<br/>" +
                "URL: ${env.BUILD_URL}<br/>",
            to: 'aman07pathak@gmail.com',
            attachmentsPattern: 'trivyfs.txt,trivyimage.txt'
        }
    }
}
```

![](https://lh7-us.googleusercontent.com/s7jOE0S-hTjPa3Z7_2_raNA6oxzkFdIY_Iqo5zpW6Afyaxgg7zXZT1nzrfacQKa9nzmcc4bO_3A8q_yamRZcHqfjOpGfLMt58qJk-dXzR1wiplj3qQYOjdzmVBkMcCo_4x-m2lyh0cW-i8ayJnRX80I align="left")

Click on build pipeline and after getting the success of the pipeline.

You will see the Sonarqube code quality analysis which will look like the below snippet.

![](https://lh7-us.googleusercontent.com/kcnrQxAEc8gMUgQ5lVwJ1_egnB2ZbI8Yxi6P-wkW6Ua5uui6aqP7xNhHvfqK5XwUr1CLdg33V2odlGvJkN7FVsAO6KwpTs_AzpDLws2p8C3xy9TlD6qMPxydNPKP2enaTT7LfDgReUITX6c0iR6g5KA align="left")

Now, we have to add one more tool for our application named OWASP Dependency-check.

Go to **Manage Jenkins -&gt; Plugins**

Search for OWASP Dependency-Check and install it.

![](https://lh7-us.googleusercontent.com/xYPtK_65wB7ThsEY20L2Iaf19V85Wxwhwg0zDaWW-87yeUci3lWN-H-rJ3ovfQwFuL_Kf0DiEDSV4vyIwUidnx6-VZmHvlsCw41srAiB95SUEX8w9Tvz8PBPV46FJXZz6SHmMGOZgfeMKwwPXj2_G0k align="left")

After installing, make sure to configure the OWASP.

Provide the name select the latest version of OWASP and click on **Save.**

![](https://lh7-us.googleusercontent.com/Z-iA7UWStsp93NSZjjb3JOtuZ7umIBr2L_ChMmwjiaKpxVUj0LhS0aT83xW8p-HsUSXrSS2pZpVpAIP5r2AQ3i8RHIukwnTt_qVy5VeRU2FqABCP-ORanQWuMeRsC7RgykfqFWICwomoys4hzQwAJ24 align="left")

Now, add the OWASP dependency check stage in the Jenkins pipeline and click on **Save.**

```bash
stage('OWASP DP SCAN') {
            steps {
                dependencyCheck additionalArguments: '--scan ./ --disableYarnAudit --disableNodeAudit', odcInstallation: 'owasp-dp-check'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
stage('TRIVY FS SCAN') {
            steps {
                sh "trivy fs . > trivyfs.txt"
            }
        }
```

![](https://lh7-us.googleusercontent.com/_XQPNEeL7aEPqJHGfHCVb-ybUdrLOola6qaDzbqk92gi-U_ezAunlqJK_OA-gTKWrZZ1xLuB4Bg2rM5Oer8QlAx2_miFPX3eyjAgCX6ymwVhOc7LefGZXpGyetGo1m085cGdQiOGKOUKUw4mPcJv3ak align="left")

Now, click on **Build Now.**

![](https://lh7-us.googleusercontent.com/YjXrQFMJRTX0ziU7sf3JY2rCUYOeoxdEUTmkR_4m-jh_ReubV0q9TJZLlUV8yK1VDE4NCqwBOWyEUQaPRyok5mbhem3YThEa0k1YH822zWIZiQdaoTp71bLQlNA3-GkbPXSkGMikotTP2JVtZtkxlDo align="left")

Once your pipeline is successful. Then, scroll down and you will see a dependency check.

Click on it and you will see the output like the below snippet.

![](https://lh7-us.googleusercontent.com/TUyLNj9QNewHIyTMKB34MsrAIaCod0d3bFA363rqSwu5BQy4GwY8yCpPSNqzzcEMmOYnaGavtDwvHwPufD8jKubRdsaPlUv95dednTx784h5kvES8qwjkdaFORjc3MLYKoCpzWmq9_ik1AtoluM6SaU align="left")

Now, we have to build our Docker Image and push it to DockerHub

To do that, we need to configure the following things.

Go to **Manage Jenkins -&gt; Credentials**

Add Docker Credentials to your Jenkins

![](https://lh7-us.googleusercontent.com/5Wf-hjQbtu4atox265pA_9Uo5jIVlaeg5EnzZ5bMRaaevgsgcxNriSa551VYiN0PDB7mPfTagFipsoYEeds66MjRBOlJYBJ7r9RQP67FmQWp-LfNEvtjoUiiw7iHGY1jzXjji6QRIMZ59FAfSF5bb48 align="left")

Add your credentials and click on **Create.**

![](https://lh7-us.googleusercontent.com/kuBFYYW76E1gblUxS7b3Rha__71sWZhnZNot9EaY8j_CZEasEiQNZe0NVOoBjGZD84u_vZa2hFQvrWG4UIdsOziRbLYnVFr91vBpOxkyK04W6HvclNkNKEldTxxvsRep6Hs0JOtViRexcMriw2t5xGI align="left")

Install the following Docker plugins on your Jenkins

```yaml
Docker
Docker Commons
Docker Pipeline
Docker API
docker-build-step
```

![](https://lh7-us.googleusercontent.com/hFJgTsggWmg4wRt52LSYA0wLgGxGSkrQzug98S7jRDTfWuyszneHLNJX8mdAvIr8NfJQ5QelesMyVScWDQAlQ4dfCKNY8_WxVZJItRwlKwqz_0Xi_OfUHRWN_y2XHn7M56BadcHnZfZhh1pPtZilEhw align="left")

Restart your Jenkins

![](https://lh7-us.googleusercontent.com/I8hebhents1v39KqIVw1Q25HWpAdlCGMmqA3ZsY0lIgRWRMuSzAqAiA6AWHQxjsCXPJeHcR6J46LTCxPyEPp-7KSaX_q3fzT_Dgg1U3PwgyjFQlRbiyWqjdFrFRtVR8KbNfkMNmW90gkwTUY4PfuavM align="left")

Configure the tool in Jenkins

Go to **Manage Jenkins** -&gt; Tools and provide the below details.

![](https://lh7-us.googleusercontent.com/KcEEMRlaFgglmRDFqFSCY_OEefSJ-B2uJXIe_ETI_YZzOlkpJfZjzsaziihR9dCGj9S-Mh-QCSbNJo-yvUP2sytLpTedcCcZeDASqb18vA4bBZl6-zkGqbLaL6SgVywKW2Nw25uxVPIugWTuLN8MHhE align="left")

Our application is Netflix Clone. So we need some movie databases on our application.

For that, we have one application that will provide the API. So, we can use the API to get the movies on our application.

TMDB is one of them

Go to this link [https://www.themoviedb.org/](https://www.themoviedb.org/)

Click on **Join TMDB**

![](https://lh7-us.googleusercontent.com/LoaUioTucwxFMpiX22GDLPMpRzsVmzD-3qLQcplT0S_oct6QomU0bU2Xw9dxKx_MjtfX1xS3_D9q4BoMSTNfbBH089QreoQwEUCNOB86-cwaxX3f3N7lWHQL56NL6Jm86dVHGXBDPa3oPCkYM4hVJ_o align="left")

Enter the details and click on **SignUp**

![](https://lh7-us.googleusercontent.com/uUpQ9y5ILsacxCFjPpoUHjQ3f_oF1tFHUG8rK_AEw19-emVXQn5_gs9x01UWsR7b-KdxoeSODFFfjP_-5FJaAcMsVd52_I32hWZexZDfWCcISBLSFiHQvE14NMr2-cKSQrvtQttMggA2dVUdPVQeud0 align="left")

Once you sign up, you will get a confirmation email on your account. Confirm it.

Log in to your TMDB account and go to the settings.

![](https://lh7-us.googleusercontent.com/0qfa_e0DJHbo-kxOXUH4RaCWTGJDNxBU4F63AB76ODayyomnV_4Kr9tZVivQzpRyQVfyanr7R35g-8kbtXeF0HUz-V8j1rQYBVIanUrYuFMZdrzTVsYkl2csFC9uKsJW42z2up4tm--OX7r_dZ_gVog align="left")

Go to the **API** section.

![](https://lh7-us.googleusercontent.com/LbnISfKCl1puI4Oq-CAEbXPd-lkyo6F2bVR8A52NRlHE-2DEUoLftXO2utPG1Y9EGZD5YCbJdqHlghrcOQXyJGpwPCxSuvhC_tmHv8gg1QHTnhkv8JmYlQk_qNpp0JLJ_hXbFmCdtVWhqObylIW0KMU align="left")

Click on **Create** to generate API

![](https://lh7-us.googleusercontent.com/QUe3LBjfx88qXWb3g8GN88ZuyqS1XRM4YdMJ9BjIlTC7HS-PwZrOIslwUXNeicFedsSvUlOtkAGlR-rqDkTjxMod4Oyeb3jB-bfhAlEefYdjyQEOfvcUVAXeNJL642qu-V2FB4UyC3to7nuEtYni-Hg align="left")

Select **Developer.**

![](https://lh7-us.googleusercontent.com/YjZ_r72KT_1kobjWZ4l_4wWwWg3QFcGHjC1GxWZSAGvSWnHxLxYc1QFutq7fisKf2O7zUFEuAnPcOqsRNxfsNi1GWLGeTt_k96cpQwwVB5Tl0mDmkUXogBqWKKuO2YemXtyWWvxM0STKrla6mCp_Mac align="left")

**Accept** the Terms & Conditions.

![](https://lh7-us.googleusercontent.com/e42AtJof44g0X18h3xcbbPSazdlbufN4G0K4IvMfTLvk8AULtjoDVtJZtrSDlYfsshtWY4jR4cd5vaDhXunsbb57WZv8EdflXLoJbsgxqAJEbrXL9cpkUI5d1CjwwvEjmJDgMrfpKAa5FvQHf2fvMuU align="left")

Provide the basic details and click on **Submit.**

![](https://lh7-us.googleusercontent.com/2MuGPKyJgNAI_j1MlkT0J5CuJoV7FjW42pcuiEttTAZb0-QBw1gzRpKOtbykEOfTvLV8wb_SPIQyKnZyQ43yRnDovjxLHmz22M1nbbFuV093ZLT4tKZtWPg3jwGqWJCBfo4YxmI-0pBEzIeDFKVi_jY align="left")

After clicking on **Submit**. You will get your **API.** Copy the API and keep it somewhere.

![](https://lh7-us.googleusercontent.com/FJcJns5kUir_2aguz2HigGm4KHS8RGyrVXNgRktXmWoTf6aqxnYchE0pW-PDK2ho3BNmuAYNcfR8Q2OZa9237VuwBGqwMiTJBFy-_VrXwiX3jb7f7VcZ_3J7RRgn9Cb7-U2df_KG1Ct2EJLN1zmjWlE align="left")

Now, we have to configure our Docker images where we will build our image with the help of new code and then, push it to DockerHub.

After pushing the image, we will scan our DockerHub Image to find the vulnerabilities in the image.

Make sure to replace the API with your API and if you are pushing Dockerfile on your Dockerhub account then, replace my username of the Dockerhub with yours.

```bash
stage("Docker Image Build"){
            steps{
                script{
                   withDockerRegistry(credentialsId: 'docker', toolName: 'docker'){   
                       sh "docker system prune -f"
                       sh "docker container prune -f"
                       sh "docker build --build-arg TMDB_V3_API_KEY=8b174e589e2f03f9fd8123907bd7800c -t netflix ."
                    }
                }
            }
        }
        stage("Docker Image Pushing"){
            steps{
                script{
                   withDockerRegistry(credentialsId: 'docker', toolName: 'docker'){   
                       sh "docker tag netflix avian19/netflix:latest "
                       sh "docker push avian19/netflix:latest "
                    }
                }
            }
        }
```

![](https://lh7-us.googleusercontent.com/LCzot_dcZsaXjSVwS2SbSjt8-bVm6mNC0FyfRTylZZ3Ix0LlsH8UCmwMYrlIz3jZNsE8UghfKBF56XlB1TApcsNFwt6ERTlWVQdqiw5BUx19vNc6jI5im7a4YBswyMe4FzfFoUSD0R46b_wMCBX8P6U align="left")

Click on **Build**

![](https://lh7-us.googleusercontent.com/qCZmIK45aTsgv061CQK7Edd0StLbVVQdertc4BOzAb3eiqnhB-Rsg2aEstZUXw-kNp18q7QTijsF97fn_kLNcz6dDaT3AZR613O50nWAjDF3tGD0Jq9aB0I-TUOHJCUyOgr6Qs3hefG4BefQR0vlzMQ align="left")

As you can see Our Pipeline is successful.

![](https://lh7-us.googleusercontent.com/H-J1S04UMqlpsUGBzfqJPNCt7AMkAQ8RyrkQBJIGeFHlM6B2sBf4cHY4rDBJLzIJNeiHjXdDPVP5ppemhHAdKbF_DRApNEk-XK6SclrQTZiaYTYeve9_DdwDK52P6AZEYdP5nnpAYsFa6zHe5IOyHAI align="left")

Now, validate whether the docker image has been pushed to DockerHub or not.

Log in to your Dockerhub account.

As you can see in the below screenshot, Our Docker image is present on Docker Hub.

![](https://lh7-us.googleusercontent.com/cdt0w6DRSy4a5pbpdguH39BgS55EMsj4hyDhjdItAUtoYFoML2yxWqWLHRfYnFY0TVk17q7YMG_NkzQkB5I5Oqjxp4Xfq7GGbrcLN1l7GHzT3mzi5dBmKdOpq9iBrlMksGf-j3dWsjeBYFim9GrVqNs align="left")

Now, we have to deploy our application using Kubernetes.

To do that, we need to install kubectl on the **Jenkins server.**

```bash
sudo apt update
sudo apt install curl
curl -LO https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
kubectl version --client
```

![](https://lh7-us.googleusercontent.com/-FSlOvDti4JcvOcMYcpAzu7So_9ner0VDj8OktmTjHFkLqFaFhcokFW5MRCwaN3sGMrmtlqktAKHHaGuWReg8_Qu2Hvfw7yINUrYTCO16o5UYmsBwzVL4Jhh7abvyctu5WZbyyNPioZa1SUa2Vj7m1w align="left")

As you know, we have two Kubernetes Nodes of which is Master and the other one the Worker Node.

Login to your both Kubernetes Master and Worker Nodes

**Master Node**

![](https://lh7-us.googleusercontent.com/N3ksNkpzSNYUb_z_LyaIOVxYZNyyWGt13oeYMOA9ZHQekW6bncL47xNWusTXYMefEJVcRNiPAPQQ0ahoSOWIE8tge62TLrmcy1t8_blSbtbig3ToXEhUIVG2oS0f5U6BaVRm7N2zfrdys0C7lRqJpAM align="left")

**Worker Node**

![](https://lh7-us.googleusercontent.com/-HIHWGNPvNSGCi3IsgIg_CRiENlxHSiaiDqD9tIReEvw4knTnVLgZGXW9WvqxTIX9EOVf7RjmGhYr0p6sFpplksqidK-GLyQTqPaNNoSq_ml8g-iCeKLq1wNpgClm5FV8GSQZpeDJ_4NqhjJupnH-fA align="left")

Add the hostname to your Kubernetes master node

```bash
sudo hostnamectl set-hostname K8s-Master
```

![](https://lh7-us.googleusercontent.com/9kDt8SeRyq9qWQNRK7b5M3rOlGErJiO4VS40bAMVcfCsUWiNTaPNAJyoOYEUyznGsiXUCmY1eUMVLOXvWoAwU3qv5dWII25Rx2ij7dtgZ73tnRfHN0Efg85_ikvHkSiN7XSqJKnkhsWoB8DR7p9yJGo align="left")

Add the hostname to your Kubernetes worker node

```bash
sudo hostnamectl set-hostname K8s-Worker
```

![](https://lh7-us.googleusercontent.com/MFJ-5Let89QM92gW-_NoAH2c34TC6pkmymLPI3du4VRFlnSaUPbKD5pUWfYYRCmtdXS8_I789rF1YZO6Vy3hJoVTaDVgIXtEKWvASCJmDfvy3bg-wbDiO-YAHbr-Y8ul2wSSboWnCR-mQXKyQA-DInM align="left")

Run the below commands on the both **Master** and **Worker** Nodes.

```bash
sudo su
swapoff -a; sed -i '/swap/d' /etc/fstab
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF
sudo modprobe overlay
sudo modprobe br_netfilter
# sysctl params required by setup, params persist across reboots
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables  = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward                 = 1
EOF
# Apply sysctl params without reboot
sudo sysctl --system
apt update
sudo apt-get install -y apt-transport-https ca-certificates curl
curl -fsSL https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-archive-keyring.gpg
echo "deb [signed-by=/etc/apt/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
apt update
apt-get install -y kubelet kubeadm kubectl kubernetes-cni
apt install docker.io -y
sudo mkdir /etc/containerd
sudo sh -c "containerd config default > /etc/containerd/config.toml"
sudo sed -i 's/ SystemdCgroup = false/ SystemdCgroup = true/' /etc/containerd/config.toml
systemctl restart containerd.service
systemctl restart kubelet.service
systemctl enable kubelet.service
```

Now, run the following commands **Only on the Master Node,** and then you will get the command that is highlighted in the below snippet.

```abap
kubeadm config images pull
kubeadm init
```

![](https://lh7-us.googleusercontent.com/Lb9aWgOoqA68dotwSXW9dxSG8_uTVSzcLdtBX7DDHcsC2uEwR9kL05ZajDEJedFAZeBW_oEAc0musEEZo6pV90pa2lmNZjuJdZlfOVV42q8mneC0YvzwHxBtJ_TmwZb84mTkDYBN10MR-S1TXl5NwjU align="left")

Exit from the root user and run the below commands

```abap
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

![](https://lh7-us.googleusercontent.com/ahLuNAX5x2gqGLSS80efBq5JdVEvLHjXjmrffPTm7wAV2p7hahoXg3KPWT9Rpd394RyaHEKjOAgObhAP6ob72v6E-m3saaJU0ua9ZK35R6Wu4eLZBQIM5iqRgRhf3s4Q0K-4xdPCO9XgOGqxXQnvjas align="left")

Run on the Worker Node

Run the below command as a root user

```abap
kubeadm join 172.31.59.154:6443 --token deq9nl.y34go2ziii0fu8c1 \
--discovery-token-ca-cert-hash sha256:e93c56bd59b175b81845a671a82ffd1839e42272d922f9c43ca8d8f6d145ce02
```

![](https://lh7-us.googleusercontent.com/Y_13tpS8o7SbHVJmUXELAw7RUs4St6cyXOr5UBtjnTMWB7LXSL95WtPv5of0CWXQl69Au4mJFOYMyvR8d2fGXLDSvrSBQmp8UwQCpb8z7FkBYY4gqViTMpR7oYDxrFgiJ9N0f1VMwfHOMH_C1PR1f4s align="left")

Both nodes are not ready because the network plugin is not installed on the master node

![](https://lh7-us.googleusercontent.com/raKK6nT0sJaHeeGw7BJ6MPk6QZsaxx73ngkNylODW5tlXmBP86K-mMs3Oq44eCvA9jx8jeS6xZGGPFv7hGL44_wPcnGMpb2XcQBBvYcYDAMkcSlRfKdFj27wxEHuj9bC2R3DkF1zCXTokuStpmcTRJQ align="left")

## **Only on the Master Node**

Run the below command to install the network plugin on the Master node

```abap
kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.25.0/manifests/calico.yaml
```

![](https://lh7-us.googleusercontent.com/GmCyK8Aw5rqYKLuDW2N8PDJvFaxAh7xzQeApj-NZLWFpBxrnmPmCJ8hOi8cXB19FSzPdVrxozLKedzpMroZOkzbaskabGAzaj4Qv3sHSCRjewpaEFSi1feumcmgxHHVW8uUVI0YiJkPMtXrKVWEcUfg align="left")

Both nodes are ready.

![](https://lh7-us.googleusercontent.com/0BoOC23XqJiEHjvrFMbCDG-_Uj9uzewWFIRflsJ4XsAsHj_HSzUmjCNFgOhBAAf8cuLuwvI01DIaG_E3D3i6LVip-MdQRWa-ovbvTEk__oNgcsBWCBMH1tu_m6tqoVCYguLI1M5naMSQ57jWzsOUI10 align="left")

Install the following Kubernetes Plugins on your Jenkins

```abap
Kubernetes 
Kubernetes Credentials
Kubernetes Client API
Kubernetes CLI
Kubernetes Credential Provider
```

![](https://lh7-us.googleusercontent.com/1P_w0b8W1olL99nlKnXmu5VLVbTs34zoN5Af_-xpM_Tz3vbEcFL4tag8ww39WWxZ-Ifv_G9jirJ_vLTSS14tl_VHfGegHBsLrJ6THFt1x-3naYTZJMTs0ie2u9RKzJ1Gu9Yus4m3JE1jebytuxyLEAA align="left")

![](https://lh7-us.googleusercontent.com/0Kf7ncuT-H1nbKvfptQbNZ-m6D6hi1ZcMQxaDJdH6KkgMmMZrYdny_EadSSUPs2XkzRsyUC9gdvq1_xMmuGTlPCnrw1_fu-FLf-cw82bTvY4BWZrX1iP97odIiDI4SUWhEFoI_xWPiEyZMdlmBghIjU align="left")

Now, we will set up the Monitoring for both Kubernetes Master and worker Nodes

Run the below command on both Kubernetes Nodes

```bash
sudo useradd \
    --system \
    --no-create-home \
    --shell /bin/false node_exporter
```

![](https://lh7-us.googleusercontent.com/LncBuoD8PRkO8mK_zjgQnMsrzUB7VVU7ad74cACYNS7vSXgINWI_VKsnPG9M9MMRYTAhasFdLIAEA-dq8rZnNcet079Po6MQs1mbVEDZr547RDHeboMU6z0l5F8MgNCz8J-V7cDdfcCexGpyIU69-38 align="left")

Download the node exporter package on both Kubernetes Nodes and Untar the node exporter package file and move the node\_exporter directory to the /usr/local/bin directory.

```bash
wget https://github.com/prometheus/node_exporter/releases/download/v1.7.0/node_exporter-1.7.0.linux-amd64.tar.gz 
tar -xvf node_exporter-1.7.0.linux-amd64.tar.gz
sudo mv node_exporter-1.7.0.linux-amd64/node_exporter /usr/local/bin/
```

![](https://lh7-us.googleusercontent.com/iJ83Etb3cRMaEcPfJmtLmT6tToBaHsvwRnpAkzts3Cw3gBHDZluWle93No9E_zUvsHTl8l9LZ0_KIm563NRlWco2PBYxqU-IwN28WjWMwVb_9z2UxPePr-gIpEaUDz8NgPN8DIfd0jIAXd3c2lFJPEg align="left")

Create the systemd configuration file for node exporter.

Edit the file

```bash
sudo vim /etc/systemd/system/node_exporter.service
```

Copy the below configurations and paste them into the /etc/systemd/system/node\_exporter.service file.

```bash
[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target
StartLimitIntervalSec=500
StartLimitBurst=5
[Service]
User=node_exporter
Group=node_exporter
Type=simple
Restart=on-failure
RestartSec=5s
ExecStart=/usr/local/bin/node_exporter \
--collector.logind
[Install]
WantedBy=multi-user.target
```

![](https://lh7-us.googleusercontent.com/JRrU2MoxixMDZChVgZTGkS8MkTmUUUsb7f6d7y2EripQyJjY-NYhJfHPHLBwx6PJ-4gboRr3cWVkFJTVoUvVMQouVRUp74SUZaOlIRabrcetSwAHyFi5WOIXzRneRB1OFz3Q4AXhoQ_c_pD8E_kL1bE align="left")

Enable the node exporter systemd configuration file and start it.

```bash
sudo systemctl enable node_exporter
sudo systemctl enable node_exporter
systemctl status node_exporter.service
```

![](https://lh7-us.googleusercontent.com/XUqV_Mo90Dt_0lKseNabW87fVhSErpjBCdzVSepXkDD4oyswySSGb_WEjQHpnK41Ya4hG9Qy1Ci0WZj2nw018LiXbKWvT3QLguPO0SPQ-hPTKOgGP9lydO4RWKm_2RTc__xve87Hv4syeZ4uJxZ8SCY align="left")

Now, we have to add a node exporter to our Prometheus target section. So, we will be able to monitor both Kubernetes Servers.

edit the file

```bash
sudo vim /etc/prometheus/prometheus.yml
```

Add both job names(Master & Worker nodes) with their respective public.

![](https://lh7-us.googleusercontent.com/7HvPtWpeASirrYDXx7x5fx2o-c9ZWqP4ZqzdwrwtnkgfxDZlgtHkxFB4k2xv_tUdZowBYrVQTqRLvJ7mKbsCxfMVFccr_evE9bsvag32AXooPZilRCo6r_HTn2JAShus29SS1atKwnFSSs0-cdmMTeE align="left")

After saving the file, validate the changes that you have made using promtool.

```bash
promtool check config /etc/prometheus/prometheus.yml
```

If your changes have been validated then, push the changes to the Prometheus server.

```bash
curl -X POST http://localhost:9090/-/reload
```

![](https://lh7-us.googleusercontent.com/z28X1f6hUz3rP4AEiMX8EexRfnAxArIZvXXI3u9OUVWRrwvUIORcoHE12Jp2W9XjlkT1pytagXYMoHPtZhCpEO47VTjXjA7o8L49Wh-JFH2jvVOQ-WV5VNxTnOfhILi6n4MeXDbrpfzQeo-kp9xqQbs align="left")

As you know, Jenkins will deploy our application on the Kubernetes Cluster. To do that, Jenkins must have the access keys or something to connect with the master node.

To do that copy the content inside .kube/config on Kubernetes Master node.

```bash
cat .kube/config
```

![](https://lh7-us.googleusercontent.com/QGvd6XNvWa6VnqcaI8ssD7sfUH2WFw8wu7b9wNEi5CxuqaFoL3-zxvKqeVurHWCv0slOXP_DVUMHdM86UvMkgkwmDGLMzWNPiDCbb71CFGS51Q_m0bilZQ1LKGjCoBPrIr7m8afe9IahMLYgKY9iIhI align="left")

Save the file with the .txt extension.

![](https://lh7-us.googleusercontent.com/Al8siRhxhVh205b_yuiclpnds8VKqe2-vPKCqADbe_CxQo5TMxNvp0WDVgAGOPs-LY6ArRPiMBCbGHHeCPSlURLELvOfWCQxNc22XqCBy0KMun6VZ6Fcsy1hejVv4Jpg1gklp0AaRCjUetqK2mYVm9w align="left")

Now, add the Secret file in Jenkins Credentials.

Click on **Add credentials.**

![](https://lh7-us.googleusercontent.com/PHvtSh5UN614Y6rBiGoCWJGzhfmONxxv7s3nEa78LrStQPeEq6hxoZpuaSuGOu9suhGWxN24_7leYE1btUapah9LgJYh4rKtLMHcygo_qzqefA0XqDq_vDdueE-VmDolIh1iTk9ZkYTHfWpPrOaYXZE align="left")

Select the **Secret file** and provide the Secret file that you have saved earlier enter the ID k8s then click on **Create.**

![](https://lh7-us.googleusercontent.com/kUGxf8KAxa76stZK2_t17tfp9HzOrY9f-_2j6zOP1YLjyPD4fsEgT_KBaHUTPcwJ3JMFLTbeUJHdEmZbQjWHQaXL9MeaeEzKzARCD7P4iM8BPDoRC1oi1_uWm3-_rnt6PRzjHpLIae9ElTGtAQ6Bhi8 align="left")

Now, Add the deploy to the Kubernetes stage in your Jenkins pipeline.

```bash
stage('Deploy to Kubernetes'){
            steps{
                script{
                    dir('Kubernetes') {
                        withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'k8s', namespace: '', restrictKubeConfigAccess: false, serverUrl: '') {
                                sh 'kubectl apply -f deployment.yml'
                                sh 'kubectl apply -f service.yml'
                                sh 'kubectl get svc'
                                sh 'kubectl get all'
                        }   
                    }
                }
            }
        }
```

![](https://lh7-us.googleusercontent.com/GLlZKAsUBAm39yN0gKdK_YdDEOclCS7izY6DG-oDo6yZM7ezUQVoKNmeOoVS-IIi7Eugq5eYdljSkpek7_hsbyeRuL8U9G84_ZBK6LBAnBmxYZQuWc-xMGr_cZNEKQEQWPby12ZYGlcp5fbezEX1efk align="left")

Click on **Build Now**

You will see that our Application has been deployed successfully on Kubernetes.

![](https://lh7-us.googleusercontent.com/CCDZ4h-r-7Cll3RrgIz_t6rPFJ2bA_ksCxR2JKfx-P9bcF__H1bXgyqnNtPfeoigWEOhxl2WKQ5FUPbZx2U6wqPM6NDK6-7IMfd8l0c1H3q8bM83ZdOxK_VTGdMcOE3ER_wbeI9YVzcPDVksbX1vnNA align="left")

You can validate whether your pods are running or not from your Kubernetes master node.

![](https://lh7-us.googleusercontent.com/4yEup7-fuYt_wTIMlB1iCBgKIY06qEgPTjXNIfHvCGn39CcEcRljwxREqGZrBh3M74G5GR6rsRQT2gbmXVssT-PwGmvdDn50M7dKuAQ9QXSe2MmqMEzSyitEkGjbapyE2ZQMoxprmuhK8zdna5f7_Tk align="left")

Also, you can check the Console logs for the earlier results.

![](https://lh7-us.googleusercontent.com/vJwgbvMjBEDLbyeHgzoIhGUTU4L0SeKkYswpqz7TS4D7xJ4faLll0G9-nqHOG_DaHpRx717UPxKk2gGNtLCWxovyBf295icipOiMRTZcI-pDV-7aiBwAPcvE8if27xtL6VTSKdm1OF3tcBLtQdjJrqU align="left")

We got the email that our pipeline was successful.

![](https://lh7-us.googleusercontent.com/SBcQ_nfRIZYprzDkJ1IgSu4fT76kOyvcRImGUveacFS5wR4JmuxrKmLUz0uh4M_QEhOEpSFyZX3klqtUSJG0rVz1hrULgTzZwVzYxvZICKaAnYJzosxvrWboFBr9FPK20wdjza0q1kbMi7nPe4qbpsQ align="left")

We get the trivyfs.txt file which contains the vulnerabilities.

![](https://lh7-us.googleusercontent.com/vVimMPLd2rJcC7pV7XvZhNCvw_dTQPa6tY-UFtYMCSdPwidlX8cJwnYtWv8WyS9zqBmJLGTMm2nrdFy1StBuaiHMVfu9zPuSk_SkwEsyrJUknuV0z6Yrpvutxz9gRZi1vnnYdXUNIPTxMayzT6qR70w align="left")

Also, we got the vulnerabilities for our Docker Image.

![](https://lh7-us.googleusercontent.com/r9opSycYNPKCHIpFLyUkrNN_tutG7KVJ1ZoEdTqPwHADXIO_sOwGE9KDSPRbElnDdJYFAziAitMdkSHhVS1YARJmeINzs5C9FdDEeSrWy36FtWvcGGWYROddeaE4WuBfTQy29Fb8GpLCJSDkJQnpB0g align="left")

Jenkins sent the console logs by email.

If you want to access your Netflix **Clone** Application\*\*.\*\*

Copy the Public IP of Worker Node and paste it on your favorite browser with port 32000 and see the magic.

![](https://lh7-us.googleusercontent.com/dG-A7yFsZdOEH0EjDCvIxYcVzypo6bnvbOyk6xrBIQ2TgKmPEhKZBUmYYjU0_e50h_XAu4TQZFqM2aLCGtixtMCpB-dWKvjXwCPlErVteyBu5rBzjFTj-cpNxKLsq2hIA4O5Gp-XY-X7O5AiFg9JLug align="left")

Another Snippet of our Netflix **Clone** application.

![](https://lh7-us.googleusercontent.com/12ZGjnMsvGHJowiJFjuBhi2dV2s-1LcVf5zd65Y-9exDgZoCYD1yRKDWX2ANGPFG0iEvbXc7KNq9RipKUXqhxitGhR9THmEDAL6EFSr5GQvXHVCixesAirOZ6zlOeVSyJdg4doeJJsQM80B3lBVznf0 align="left")

Go to the Grafana Dashboard and select Node Exporter.

You will see the real-time hardware specs of your Kubernetes master node.

![](https://lh7-us.googleusercontent.com/M1PUelHk5kksVGUvk2FBa8_Lc8nMdWAJFQxxMuYTycKFC05ovvYg7RmmsvZMCb7xw1OzKYeZgbBzbF4MMJZt9VIGKpFt8sjMY2tsC7HiU6iN1NcjDminXfO_kQNUqSqvv4AHIfBE3Gp7vf7bJqFLQ_Y align="left")

You will see the real-time hardware specs of your Kubernetes worker node.

![](https://lh7-us.googleusercontent.com/8LEM4A3YWusNIh83L4_qAxfgPjuDAuxK8howQ2bVFvhAvHf2bIc09H4bXy9b7K4j11JRQD2J7DBYxTRnm7L7pPcmAhbmFiSAW4J28QFHXSVyO4SHx6YPXtNZfubPVKPyZ4F3rzYI4_YQ-D78NlZozIM align="left")

---

### Conclusion:

In conclusion, this guide has equipped you with the knowledge and steps needed to deploy a sophisticated application using Kubernetes. From setting up infrastructure on AWS to integrating monitoring with Grafana and Prometheus, and finally deploying a Netflix Clone application, you’ve covered a wide array of DevSecOps practices.

---

🚀 Culmination of Our Kubernetes Expedition! 🎉

This undertaking marked the grand finale, concluding our enriching #30DaysOfKubernetes Series on its 30th day.

We trust you garnered valuable insights throughout this journey. Whether you're a seasoned explorer or just joining, explore the comprehensive topics covered in our GitHub Repo.

A heartfelt note for the last day: Thank you for embarking on this Kubernetes adventure with us. May your tech odyssey continue with boundless discoveries! 🌐💻✨ #KubernetesFinale #TechJourney #LearningTogether #GitHubRepo

[https://github.com/AmanPathak-DevOps/30DaysOfKubernetes](https://github.com/AmanPathak-DevOps/30DaysOfKubernetes)

---

### **Want to Know About Challenge?**

If you’re eager to learn more and join our challenge through the GitHub Repository, stay tuned for the upcoming posts. Follow for more exciting insights into the world of Kubernetes!

**GitHub Repository**: [https://github.com/AmanPathak-DevOps/30DaysOfKubernetes](https://github.com/AmanPathak-DevOps/30DaysOfKubernetes)

#Kubernetes #DaemonSet #StatefulSets #NetworkPolicy #Operators #ContainerOrchestration #DevOps #AKS #Azure #K8sLearning #EndtoEndProject #KubernetesProject

Stay connected on **LinkedIn**: [LinkedIn Profile](https://www.linkedin.com/in/aman-devops/)

Stay up-to-date with **GitHub**: [GitHub Profile](https://github.com/AmanPathak-DevOps)

Feel free to reach out to me, if you have any other queries.

Happy Learning