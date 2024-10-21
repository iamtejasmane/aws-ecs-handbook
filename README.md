# The Ultimate AWS ECS Handbook

### Preface

This handbook is nothing but my own notes and the information I've been gathering for the last two years from the official documentation and other open forums.

This might not be helpful for first-time Amazon ECS learners as this includes mostly anecdotes but in an easy-to-understand fashion. I refer to this information and links for my go-to tool while working, optimising and troubleshooting ECS Clusters.

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

#### Cluster:
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

[Reference Read](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/clusters.html)


#### Task Definition 

A JSON formatted template describing what the associated task will run. It contains one or more container specifications to be launched and associated “docker run” parameters. It also defines the networking mode, task placement constraints and volume mounts associated with the task. 

#### Container Instance
An EC2 instance running the ECS agent which has registered itself to a cluster. 

#### Task
An instantiation of a task definition, which launch container or containers listed within the task definition.

#### Container agent: 
An application running on ECS container instances that performs the “bridge” between the local docker daemon API and the ECS endpoint.

#### Service:
The service component allows you to run and maintain a specified number of instances of a task definition. This is capable of integrating with Elastic Load Balancer (ALB) and of being scaled up or down based on specific CloudWatch Metrics by leveraging Application Auto Scaling. 

We recommend that you use the service scheduler for long running stateless services and applications. The service scheduler ensures that the scheduling strategy that you specify is followed and reschedules tasks when a task fails. For example, if the underlying infrastructure fails, the service scheduler reschedules a task. You can use task placement strategies and constraints to customize how the scheduler places and terminates tasks. If a task in a service stops, the scheduler launches a new task to replace it. This process continues until your service reaches your desired number of tasks based on the scheduling strategy that the service uses. The scheduling strategy of the service is also referred to as the service type.

[Reference Read](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs_services.html)


#### Scheduler:
An ECS mechanism that determines the task placement on specific container instances, based on a set algorithm that takes into account several variables. 

#### Container: 
A Docker container that was launched as part of a task. 
____________

### ECS-optimized Linux AMIs


ECS-optimized Amazon Machine Image (AMI) - Preconfigured image to run your container workload on ECS Amazon Linux machines and tested by AWS Engineers.

Components: 
* Preconfigured Storage 
* ECS init
* Docker 
* ECS Agent
* Minimalised Amazon Linux AMI

[Reference Read](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs-optimized_AMI.html#ecs-optimized-ami-linux-releasenotes)

ECS Container Instances 
* Is an EC2 instance where tasks are run
* Include a running ECS agent
* Registered to a Cluster
* Need access to communicate with the amazon ECS service endpoint (IGW, NAT, HTTP Proxy, VPC endpoint)
* Instance types can’t be changed; the instance must be replaced.
* You can’t deregister an instance from one cluster and move to another.
* ECS-optimized AMI is provided and is the recommended way to run ECS EC2 task

“The above doesn’t apply to Fargete, because Fargate abstracts the container instances outside the customer control.” 

[Reference Read](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/create-capacity.html)
____________

### Container Instance LifeCycle

**Active**:
* A container instance can accept run task requests. 
* The one that will be used to place tasks scheduled by a Scheduler or the user.

**Draining**:
* No new Tasks are placed on the instance. 
* Active Services are ended and relocated to other instances. 
* Doc: https://docs.aws.amazon.com/AmazonECS/latest/developerguide/container-instance-draining.html

**Inactive**:
* If you deregister or end a container instance, the container instance status changes to INACTIVE immediately

**ECS-Agent Connected**:
* ECS-Agent CONNECTED status indicates the agent is able to talk with the ECS Scheduler on AWS.

**ECS-Agent Disconnected**: 
* ECS-Agent DISCONNECTED status indicates there is a problem with the agent. It can be networking or permissions.
* [Reference Read](https://repost.aws/knowledge-center/ecs-agent-disconnected)

____________
### Container Instance Bootstrap components

#### ECS Agent

When the ECS agent starts on an instance, it will search for configuration options from the file ecs.config in location /etc/ecs. The attribute in the ECS configuration file, which indicates to which cluster the instance should be registered, is ECS_CLUSTER. 

To register an EC2 instance with ECS cluster, add the attribute ECS_CLUSTER to file /etc/ecs/ecs.conf. There are several options that can be added to the ECS agent, such as the log level, that the agent should log the message to, proxy server, from which the ECS agent should communicate, and others. All those attributes can be added to the ECS.conf file before the agent starts.

[Reference Read](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs-agent-config.html)

#### Docker Daemon

Similar to the ECS agent, the Docker daemon will load the configuration attributes from one of the four configuration
files shown here. 
* /etc/sysconfig/docker
* /etc/sysconfig/docker-storage
* /etc/sysconfig/docker-storage-setup
* /etc/docker/daemon.json

You should add the required options to one of these files before the Docker daemon starts, for the options to  take effect. These options include 
* The base device size
* Debug level logging,
* Storage options, 
* Networking options, and others.

Select the link below for all of the options available for the Docker daemon.
[Reference Read](https://docs.docker.com/reference/cli/dockerd/)


Cloud-init
Cloud-init is a package available to configure cloud instances on boot. There are several different formats supported by Cloud-init. 

We pass a MIME file as part of the instance user data, and depending on the format of the content of the user data file, the commands or scripts in the file, will be started at different points in time during the boot. Coming up, you will look at the shellscript format (to configure ECS agent) and cloud-boothook format (to configure Docker daemon).
[Reference Read](https://cloudinit.readthedocs.io/en/latest/index.html)


User Data Format Used to Configure ECS Agent: `text/x-shellscript`<br>
User Data format used to configure Docker Daemon: `text/cloud-boothook` <br>

[Reference Read](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/bootstrap_container_instance.html)


Bootstrapping an ECS instance to configure the ECS Agent. For example: -
```
#!/bin/bash
echo “ECS_CLUSTER=MyCluster” >> /etc/ecs/ecs.config
echo “ECS_LOGLEVEL=debug” >> /etc/ecs/ecs.config
```
____________
### ECS Container Agent
ECS Container Agent
* ECS container agent Open-source, programmed In Go. 
* Process ECS commands and turns them into Docker commands.
* Instruct EC2 instances to start/stop containers, monitor resources. 
* Repo Link: https://github.com/aws/amazon-ecs-agent/

Linux: Amazon/Linux AMI: On Amazon or other Linux AMIs, ECS container agent will be run in a Docker container on an EC2 instance. 

Windows: On Windows AMI, ECS container agent runs as a service on the host. “\\. \pipe\docker_engine”

API: ECS container agent provides an API for gathering details about the container instance. 
	Curl command: 
	$ curl http://localhost:51678/v1/metadata | python - mjson.tool

For ECS-Optimized AMI, ECS agent is preinstalled. For manual installation on refer to the below link:Doc: https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs-agent-install.html

Understanding “Agent Disconnected”

A container instance to register to the cluster and function with ECS, the ECS agent must be in connected status. It is important to note that the ECS agent will disconnect and reconnect several times per hour to ensure that the connectivity can be established, and that there are no connectivity issues.

If an agent stays in a disconnected state for a long period, this indicates a problem with: 
* The agent, 
* Instance, 
* Permissions, 
* Connectivity, or 
* The service. 

Problems can occur when an agent has been disconnected for an extended period of time, because the instance will not be able to receive commands from the ECS Scheduler. This can affect cluster operations because tasks may be left running, even after an update to the service, and tasks that fail on that particular instance will not be restarted.

If the agent is unable to reconnect to the service for any reason, you will need to either troubleshoot the issue or just end the instance.

#### ECS Anywhere
 
It is a feature of ECS for running and managing container workloads on your infrastructure to help you meet compliance requirements and scale your business without sacrificing on-premises investments. 

[Additional Read](https://aws.amazon.com/ecs/anywhere/)

________

### Task Definitions

### Progressing... 