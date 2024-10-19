# The Ultimate AWS ECS Handbook

### Preface

This is AWS Elastic Container Service (ECS) handbook. This handbook is nothing but the notes and information I've gathered since last two years from the offical documentation and other oper forums. Hence this might not be helpful for first time Amazon ECS learners. I refer this information and links for my go-to tool while working, optimzing and troubleshooting ECS Clusters. 


## Amazon ECS

Elastic Container Service (ECS) is AWS’s managed container orchestrator. And so being an orchestrator, it is going to be put in the same family as a docker swarm or Kubernetes, However, ECS is entirely provided and managed by AWS.

Amazon ECS terminology and components:

There are three layers in Amazon ECS:
* Capacity - The infrastructure where your containers run
* Controller - Deploy and manage your applications that run on the containers
* Provisioning - The tools that you can use to interface with the scheduler to deploy and manage your applications and containers
The following diagram shows the Amazon ECS layers.

![ECS Layers-](/images/ecs-layers.png)

Task definition - Here you define your application and its behaviour.

After you define your task definition, you deploy it as either a <i>**service**</i> or a <i>**task**</i> on your cluster. A cluster is a logical grouping of tasks or services that runs on the capacity infrastructure that is registered to a cluster. 

A task is the instantiation of a task definition within a cluster. You can run a standalone task, or you can run a task as part of a service. - Can relate this concept to Kubernetes <i>Pod</i> for better understanding. 

You can use an Amazon ECS service to run and maintain your desired number of tasks simultaneously in an Amazon ECS cluster. How it works is that, if any of your tasks fail or stop for any reason, the Amazon ECS service scheduler launches another instance based on your task definition. It does this to replace it and thereby maintain your desired number of tasks in the service.  - Can relate this concept to <i>ReplicaSets</i> for better understanding. 

After you deploy the task or service, you can use any of the following tools to monitor your deployment and application:
* CloudWatch
* Runtime Monitoring


ECS Integration with other AWS services:
* IAM - Integrates directly with ECS.
* CloudFormation - Can be used to manage your ECS Cluster and associated resources.
* CloudWatch Through events and logs
* ECR - container images can be stored inside ECR
* Elastic Load Balancing - Integrates directly with ECS
* Application Auto Scaling - Integrates directly as a separate service

Service Quotas - Maximum values for the resources, actions, and items in your AWS account. Each AWS service defines its quotas and establishes default values for those quotas. 