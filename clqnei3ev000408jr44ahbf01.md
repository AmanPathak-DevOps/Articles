---
title: "Integrate Circle CI with GitHub Repository"
datePublished: Fri Nov 17 2023 16:14:38 GMT+0000 (Coordinated Universal Time)
cuid: clqnei3ev000408jr44ahbf01
slug: integrate-circle-ci-with-github-repository-b97f90747659
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703658864886/bbeeab2e-3c94-46e6-9c31-935a7fe5a527.png

---

### **Introduction**

Embarking on the journey of continuous integration and continuous deployment (CI/CD) is crucial for modern software development. It streamlines the process of building, testing, and deploying code changes, ensuring a faster and more reliable development lifecycle. CircleCI is a robust CI/CD platform that seamlessly integrates with your GitHub repositories, providing automation and efficiency.

This blog post is your guide to kickstarting your CI/CD journey with CircleCI. We’ll walk through the steps to connect your GitHub repository with CircleCI, enabling automated workflows and streamlined development practices.

### Prerequisites

Before diving into the integration process, ensure you have the following:

1.  **GitHub Account**: Make sure you have an active GitHub account. If not, you can sign up [here](https://github.com/).
2.  **CircleCI Account**: Sign up for a CircleCI account if you haven’t already. You can do this by visiting the official website: [CircleCI Signup](https://circleci.com/vcs-authorize/).
3.  **SSH Key Pair**: We’ll be using SSH keys to establish a secure connection between CircleCI and your GitHub repository.

### Step-by-Step Guide

Go to the CircleCI official website and if you don’t have your account on it then do signup.

[https://circleci.com/vcs-authorize/](https://circleci.com/vcs-authorize/)

Once you complete the signup process by providing the email address and password, you will get an email that you provided on the CircleCI SignUp process.

Kindly verify your email address and go to the CircleCI official website again [https://circleci.com/vcs-authorize/](https://circleci.com/vcs-authorize/).

Click on the login

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658836661/6217c5c8-bc97-41f9-a2e8-df2bd0ee97f2.png)

Click on the **New Project.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658838391/75e583d7-840f-47b8-afec-cd1af4756c0e.png)

You will be landed on the below page

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658840088/7654f36e-cadf-4255-843f-4f5b034723cc.png)

As you observe, in the previous screenshot we got some instructions to connect our Repository of GitHub with CircleCI.

Let’s follow this.

Copy the first step command and paste it into your terminal to generate a new ssh key

ssh-keygen -t ed25519 -f ~/.ssh/project\_key -C email@example.com

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658841410/1cc8a224-4173-4a7b-a7ab-2af5faafc07d.png)

You will see two files that are generated after running the above command.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658842568/19f346a1-3fe9-4d39-9b3d-640809366c4f.png)

Copy the content of the public key

cat project\_key.pub

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658843964/bcf2497b-722f-4ade-a54b-440c59d187e9.png)

Now, we have to paste the public key to the repository that we want to integrate with CircleCI.

Let’s take a demo repository and click on **Settings.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658845594/4fd97542-b0a6-494b-ab7e-e243a18b6183.png)

Click on **Deploy keys**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658847252/7ca0d25c-72fb-4837-b68a-03e91bf2a6db.png)

Click the **Add deploy key**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658848669/224ac1d0-7a6a-4b33-818d-53bb057da3e9.png)

Paste the public key that you have copied in the earlier steps and check the **Allow write access** because CircleCI will create some config files like .circleci and config.yml.  
Click on the **Add key**.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658850284/c5de36ce-1664-46ba-9467-43df539f78f7.png)

Copy the content of the private key.

cat project\_key

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658852074/6074b091-066b-444d-b877-0a74b1f9d156.png)

Go back to the CircleCI page and paste the private key in the dialog box.

Also, I have selected the same repository in which I created the deploy key.   
Click on **Create Project.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658853754/7703dee8-6ae6-43be-a0cc-93d92847a0aa.png)

After clicking on **Create Project,** CircleCI will start to write the configurations and deploy a hello workflow message to show you that your CircleCI integration is successful with GitHub.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658855332/ff144a7f-da3b-4b0d-b289-7fbfe6eafafe.png)

Your integration is successful. Now you can modify the config.yml file according to your requirements.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658856909/9dccc479-0c8a-4d3e-8f36-36e332fc5f4a.png)

If you observe, CirceCI pushed some changes on circleci-project-setup.

Click on Compare & pull request and click Pull Request.

Once you create the Pull Request and click on Merge pull request.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658858613/7bb9707f-452d-4dca-bbb7-e9f98734f733.png)

Once you merge the changes, your repository will have one more folder named .circleci.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658860473/5b483113-0d78-45e2-b671-f74aaf349398.png)

Inside the .circleci folder, you will see the config.yml file which is used to create your customized pipeline like you do for other CI/CD tools.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658862116/3ba5e0cb-e76f-42ed-83b5-6a33b8e9280c.png)

### Conclusion

**Congratulations**! You’ve successfully connected your GitHub repository with CircleCI, laying the foundation for automated CI/CD workflows. With CircleCI, you can now effortlessly manage your development pipeline, ensuring faster and more reliable software delivery. Explore the `.circleci/config.yml` file to tailor your workflows and adapt them to your specific project requirements. Happy coding!

**Feel free to reach out to me if you have any doubts or queries.**

**LinkedIn**\- [https://www.linkedin.com/in/aman-devops/](https://www.linkedin.com/in/aman-devops/)

**Github**\- [https://github.com/AmanPathak-DevOps/](https://github.com/AmanPathak-DevOps/)

Happy Learning!

Aman Pathak