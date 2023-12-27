---
title: "Unleashing Cloud Savings: Turbo-charge Your AWS Cost Optimization with EBS Snapshot Management!"
datePublished: Wed Jul 26 2023 18:17:18 GMT+0000 (Coordinated Universal Time)
cuid: clqnetc6c000h08l4ewc7bqrg
slug: unleashing-cloud-savings-turbo-charge-your-aws-cost-optimization-with-ebs-snapshot-management-ab4c20bd470d
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703659389264/dbf6cc19-8ee7-49cd-bbc0-a700a9e669b6.gif

---

AWS Infrastructure Diagram

**Introduction:**

Are you ready to uncover hidden savings in your AWS cloud infrastructure? In this engaging blog post, we’ll embark on an exciting journey toward optimizing your AWS costs by efficiently managing EBS snapshots. Say goodbye to unnecessary expenses and hello to a leaner, more cost-effective cloud setup!

**Prerequisites:**

*   Basic understanding of AWS services, specifically EC2 instances and EBS volumes.
*   An AWS account with sufficient permissions to manage EBS snapshots and EC2 instances.
*   Familiarity with Python programming.

**Objective:**

Our mission is simple: to develop a powerful yet user-friendly solution to automate the identification and deletion of unused EBS snapshots. By the end of this blog, you’ll have two stellar approaches in your arsenal — Terraform for those who love shortcuts and a thrilling manual approach for the brave-hearted.

Terraform Approach — Quick & Painless:

Ready to kickstart your cost optimization journey without breaking a sweat? We’ve got you covered! Just follow the link to our GitHub repository containing Terraform magic. With a few clicks, you’ll unleash the potential of a serverless AWS Lambda function. Watch as it dances its way through unnecessary EBS snapshots — leaving you with a lean, cost-optimized environment!

Terraform Code: [https://github.com/AmanPathak-DevOps/Terraform-for-AWS/tree/master/Non-Modularized/EBS-Snapshot-Cost-Optimization](https://github.com/AmanPathak-DevOps/Terraform-for-AWS/tree/master/Non-Modularized/EBS-Snapshot-Cost-Optimization)

Manual Approach — Adventure into the Code:

For the daredevils who seek a deeper understanding, we’ve prepared an action-packed manual approach! Dive into the exhilarating world of Python and the Boto3 library as we create a powerful AWS Lambda function. Be the hero who fetches snapshots, battles unused resources, and emerges victorious by slashing storage costs!

**Step-by-Step Implementation Guide:**

Hold on tight as we embark on an epic step-by-step journey! Witness the magic of Python code, the thrill of EC2 instance status checks, and the triumph of deleting unwanted EBS snapshots. We’ll guide you through each twist and turn, ensuring you grasp the essence of cost optimization.

Click on Launch Instances to create an EC2 instance.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659355215/29734eb3-088f-45c1-9b22-ef971b03a08d.png)

Just give the instance name and the rest of the things keep as it is.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659356506/0662b009-7d64-415d-86df-d28d2f414591.png)

Now, you can see the volumeID that is attached to the instance.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659358094/8e8c9698-70f5-43f6-a9d5-856ad76209b3.png)

Create a Snapshot of the volume which is attached to the instance.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659359903/2ec2895b-f3e8-4b56-9f87-38e59d039ea3.png)

Select VolumeID that is attached to the instance.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659361541/8cd46bc9-485e-43e5-be79-1da319759d3a.png)

You can see here, the snapshot has been created.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659362733/14e8f774-cc8a-4261-b5f9-66d1043cb174.png)

Create a lambda function which will help us to delete the Unused EBS Snapshot.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659363957/8d4632ed-a63f-483f-8d3f-5c5bafc65803.png)

Keep the configuration as it is and click on create.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659365643/22441fc9-6cd9-4bf7-9f90-a8f190821281.png)

Now, Paste the Python code in the lambda function.

Python Code link- [https://github.com/AmanPathak-DevOps/Terraform-for-AWS/blob/master/Non-Modularized/EBS-Snapshot-Cost-Optimization/ebs\_snapshot\_cost\_optimization.py](https://github.com/AmanPathak-DevOps/Terraform-for-AWS/blob/master/Non-Modularized/EBS-Snapshot-Cost-Optimization/ebs_snapshot_cost_optimization.py)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659367410/bb4517c1-59eb-4901-8d15-b83301938b0c.png)

Create a test event to run the lambda function.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659369068/b7f5ca07-4e34-4447-a836-d87c4958279b.png)

Here, we need to increase the lambda timeout.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659370886/4b505d4c-e72f-4837-92ca-4d743fe0e2ec.png)

Increase the lambda timeout from 3 seconds to 10 seconds and click on Save.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659372202/1ece3e18-ce99-452b-a4bb-94ee8fc23479.png)

Now, Here we need to update the roles and permissions for the lambda function.

We are going to work with the EC2 Instances through the lambda function.

Click on Role name. It will take you to the IAM Roles.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659373488/588986ad-6088-4ebc-9c0c-fbadcbf0c8fc.png)

Click on Add permissions and then go to ‘Attach policies’.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659375014/b4423059-2477-4968-be6d-bbabcc53cb19.png)

Now, we have to create our own policy, click on ‘Create policy’.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659376620/3346b6e4-b4c0-4f4e-83e2-98d3c9058c14.png)

Now in the output, the policy should look like this.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659378613/644d2195-702e-474d-9918-3f312871ec17.png)

We have configured everything. Now for the test, we have to terminate our test server. So, the volume will get deleted.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659380379/1ce9cd94-2b6f-44b8-8832-8d891a19a069.png)

Click on ‘Terminate’.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659382073/719f5028-aaa9-48f4-ac84-0d15a99278e0.png)

After clicking on ‘Test’ to run the Lambda function, you will see the output that EBS snapshots were deleted because the associated volume was deleted upon terminating the test server instance. Therefore, as per the code logic, any EBS snapshots whose volumeID was not found will be deleted.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659383742/1f03c6b6-c5bf-4dd8-8f39-b25dd104b1db.png)

In the AWS Console, no snapshots were found.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659385358/cc912186-49e0-48ea-9501-912b7b2a675d.png)

Congratulations, fearless cloud warriors! By implementing our cost optimization solution, you’ve unlocked the gateway to significant AWS savings. But wait, there’s more! Remember to regularly monitor and optimize your cloud resources for a prosperous cloud journey. Our blog is just the beginning — follow us for more AWS adventures and uncharted cost-saving territories!

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659387040/b0e704df-b5b2-4648-a05b-7bf64ffa5a86.gif)

It’s done bruh

If you have any other queries, feel free to reach out to me on

LinkedIn Profile: [https://www.linkedin.com/in/aman-devops/](https://www.linkedin.com/in/aman-devops/)

GitHub Profile: [https://github.com/AmanPathak-DevOps](https://github.com/AmanPathak-DevOps)

Happy Learning

Aman Pathak