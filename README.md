# The Ultimate AWS ECS Handbook

### Preface

This handbook is nothing but the my own notes and information I've been gathering since last two years from the offical documentation and other oper forums. 

This might not be helpful for first time Amazon ECS learners. I refer this information and links for my go-to tool while working, optimzing and troubleshooting ECS Clusters. 

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

## ECS Components

### Cluster:
Logical grouping of resources namely services and tasks. With Fargete, cluster can exist without instances. 

In addition to tasks and services, a cluster consists of the following resources:
 
The infrastructure capacity
* Amazon EC2 instances in the AWS cloud
* Serverless (AWS Fargate (Fargate)) in the AWS cloud
* On-premises virtual machines (VM) or servers

The network (VPC and subnet) where your tasks and services run. When you use Amazon EC2 instances for the capacity, the subnet can be in Availability Zones, Local Zones, Wavelength Zones or AWS Outposts.

An optional namespace. The namespace is used for service-to-service communication with Service Connect.

A monitoring option. CloudWatch Container Insights comes at an additional cost and is a fully managed service. It automatically collects, aggregates, and summarizes Amazon ECS metrics and logs.


The following are general concepts about Amazon ECS clusters.
* Amazon ECS creates a default cluster. You can create additional clusters to separate your resources.
* Clusters are AWS Region specific.
* Clusters can be in any of the following states.

    - **ACTIVE**
    
        The cluster is ready to accept tasks and, if applicable, you can register container instances with the cluster.

    - **PROVISIONING**

        The cluster has capacity providers associated with it and the resources needed for the capacity provider are being created.

    - **DEPROVISIONING**
    
        The cluster has capacity providers associated with it and the resources needed for the capacity provider are being deleted.
    
    - **FAILED**

        The cluster has capacity providers associated with it and the resources needed for the capacity provider have failed to create.
    
    - **INACTIVE**
    
        The cluster has been deleted. Clusters with an INACTIVE status may remain discoverable in your account for a period of time. This behavior is subject to change in the future, so make sure you do not rely on INACTIVE clusters persisting.

* A cluster can contain a mix of tasks that are hosted on AWS Fargate, Amazon EC2 instances, or external instances. Tasks can run on Fargate or EC2 infrastructure as a launch type or a capacity provider strategy. If you use EC2 as a launch type, Amazon ECS doesn't track and scale the capacity of Amazon EC2 Auto Scaling groups. For more information about launch types, see Amazon ECS launch types.
* A cluster can contain a mix of both Auto Scaling group capacity providers and Fargate capacity providers. A capacity provider strategy can only contain Auto Scaling group capacity providers or Fargate capacity providers.
* You can use different instance types for the EC2 launch type or Auto Scaling group capacity providers. An instance can only be registered to one cluster at a time.
* You can restrict access to clusters by creating custom IAM policies. For information, see Amazon ECS cluster examples section in Identity-based policy examples for Amazon Elastic Container Service.
* You can use Service Auto Scaling to scale Fargate tasks. For more information, see Automatically scale your Amazon ECS service.
* You can configure a default Service Connect namespace for a cluster. After you set a default Service Connect namespace, any new services created in the cluster can be added as client services in the namespace by turning on Service Connect. No additional configuration is required. For more information, see Use Service Connect to connect Amazon ECS services with short names.

[Reference.](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/clusters.html)



### Updating... 