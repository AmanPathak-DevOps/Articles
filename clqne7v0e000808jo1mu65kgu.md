---
title: "Ansible Server and Nodes Setup"
datePublished: Wed Sep 14 2022 09:18:28 GMT+0000 (Coordinated Universal Time)
cuid: clqne7v0e000808jo1mu65kgu
slug: ansible-server-and-nodes-setup-1aee7918d7c7
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703658387319/f78ed0d4-a7f1-45c2-86a9-2d7682c2e698.png

---

**Ansible Server**

*   apt update (root user)
*   apt install software-properties-common
*   add-apt-repository — yes — update ppa:ansible/ansible
*   apt install ansible

After installing, when you will check the /etc/ansible/ansible.cfg file you will only get this like:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658353269/992d5b44-ad3f-4aa0-90d8-b22f4d04b085.png)

So, run the following command:

*   ansible-config init — disabled -t all > ansible.cfg
*   vim /etc/ansible/hosts

Add the nodes IP which looks like this:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658354590/16cd79da-fc06-4b31-97fa-dc4a7e4d2161.png)

\[Note: demo is a group (necessary but name could be anything)\]

*   vim /etc/ansible/ansible.cfg

Inside the ‘ansible.cfg’ file, you have to find one thing and add one thing:

find-> inventory and remove the ‘;’ which was in the start of inventory ( because ‘;’ used as # to uncomment).

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658356075/b3ef2979-e862-48b8-9222-2300564729c8.png)

add-> sudo\_user = root, you can add anywhere around the previous inventory command.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658357675/2765fa9b-637b-49e3-a0ac-d985f6cea446.png)

*   adduser ansible
*   sudo passwd ansible
*   visudo

Give the permissions to ansible user to perform any commands by adding the command:

Write the ansible line command

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658359364/c88711d6-99e3-476d-b805-0e379e342dd3.png)

*   ssh 172.31.84.64 (ansible user)

Then, you get this

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658361146/066beb1b-27e0-4b7c-976c-d1339ec0b2ec.png)

So, it is showing you this error because you have configure the file /etc/ssh/sshd\_configure, like in the below image:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658363129/b998eeab-362c-466e-825f-07e3f07398b3.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658364499/3050f802-979c-43d6-b2e7-67dbd529fb4c.png)

*   service start ssh/sshd
*   ssh 172.31.84.64 (ansible user)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658365679/0396edfd-638e-46ae-88d6-a98578ff2a60.png)

As you run this command, it will ask you the password of Node1. But when you enter in the

Node using SSH then, you can do anything such as creating a file or installing any package,

etc.

You can refer the below images:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658366987/68a2b4e4-d6bb-4759-8066-503c3601926f.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658368126/ec22018c-620a-4210-a016-c8f9a68c84fd.png)

But as you know, we need to enter the Node without asking for any password. So, to

do that follow the commands below:

*   ssh-keygen (ansible user)

The file is present here,

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658369832/884e10a3-81f6-4b4c-b70d-29435badd7f1.png)

Now, you have to copy this ssh key into both Nodes by the following commands:

*   ssh-copy-id ansible@172.31.84.64 (ansible user)
*   ssh-copy-id ansible@172.31.95.203 (ansible user)

So, the public key has been copied in the both nodes

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658371526/c6c7dacf-fd68-476d-b5b7-b8ab99fe5da4.png)

*   ssh 172.31.84.64 (ansible user)
*   ssh 172.31.95.203 (ansible user)

This time, it won’t ask you for any password.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658373265/7331d58f-3b3e-47a7-98c2-21d0ae86fd06.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658374761/f259d0a3-2b7c-42f6-a6b7-b79b4fc14ba2.png)

Another Demo for Node2

\-> Ansible Server

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658376395/f11ab41b-8f6e-422a-8150-3f5587364548.png)

\-> Node 2

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658377748/cbef49b1-ff9b-4cad-adec-d53acbc0d8ba.png)

**Node1**

*   apt update
*   sudo su
*   adduser ansible
*   sudo passwd ansible
*   service start ssh/sshd
*   visudo (as root user)

Give the permissions to ansible user to perform any commands by adding the command:

Write the ansible line command

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658379401/a1446633-e9bf-4694-9e42-6cec5479c442.png)

*   configure the file /etc/ssh/sshd\_configure, like in the below image:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658380575/c7a56d30-476e-41f8-9941-31ea137558ee.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658381733/3a9e4a58-130d-4a82-a45f-2662c0b75030.png)

*   service start ssh/sshd

**Node2**

*   apt update
*   sudo su
*   adduser ansible
*   sudo passwd ansible
*   service start ssh/sshd
*   visudo

Give the permissions to ansible user to perform any commands by adding the command:

Write the ansible line command

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658383065/4f40e295-ff21-4e78-bb61-e4d81eb3cfdb.png)

*   configure the file /etc/ssh/sshd\_configure, like in the below image:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658384717/0a9ea23b-0de6-4596-8a9d-bfcbcb5d75e7.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703658386075/3a210c75-3b3e-4059-8a35-6f15dce27cf5.png)

*   service start ssh/sshd