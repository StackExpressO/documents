# ECS SETUP DODUMENTATION FOR AWS
# 1. CREATE THE ECS CLUSTER

## 1. Open the Amazon ECS console at https://console.aws.amazon.com/ecs/.
## 2. From the navigation bar, select the Region to use.
## 3. In the navigation pane, choose Clusters.
## 4. On the Clusters page, choose Create Cluster.
## 5. For Select cluster compatibility, choose Networking only, then choose Next Step.
## 6. On the Configure cluster page, enter a Cluster name. Up to 255 letters (uppercase and lowercase), numbers, hyphens, and underscores are allowed.
## 7. In the Networking section, configure the VPC for your cluster. You can keep the default settings, or you can modify these settings with the following steps.

## a)  If you choose to create a new VPC, for CIDR Block, select a CIDR block for your VPC. For more information, see Your VPC and Subnets in the Amazon VPC User Guide. (optional)
## b) For Subnets, select the subnets to use for your VPC. You can keep the default settings or you can modify them to meet your needs.
## 8. In the CloudWatch Container Insights section, choose whether to turn on Container Insights for the cluster. For more information, see Amazon ECS CloudWatch Container Insights.
## 9. Choose Create
_________________________________________________________________
# Creating a private repository ECR

## 1. Open the Amazon ECR console at https://console.aws.amazon.com/ecr/repositories.
## 2. From the navigation bar, choose the Region to create your repository in.
## 3. In the navigation pane, choose Repositories.
## 4. On the Repositories page, choose Create repository.
## 5. For Repository name, enter a unique name for your repository. The repository name can be specified on its own (for example nginx-web-app). Alternatively, it can be prepended with a namespace to group the repository into a category (for example project-a/nginx-web-app).
## 6. For Tag immutability, choose the tag mutability setting for the repository. Repositories configured with immutable tags prevent image tags from being overwritten. For more information, see Image tag mutability.
## 7. For Scan on push, choose the image scanning setting for the repository. Repositories that are configured to scan on push start an image scan whenever an image is pushed. If you want to start an image scan at a different time, you need to manually start the image scans. For more information, see Image scanning.

## 8. For KMS encryption, choose whether to enable encryption of the images in the repository using AWS Key Management Service. By default, when KMS encryption is enabled, Amazon ECR uses an AWS managed key (KMS key) with the alias aws/ecr. This key is created in your account the first time that you create a repository with KMS encryption enabled. For more information, see Encryption at rest.
## 9. When KMS encryption is enabled, select Customer encryption settings (advanced) to choose your own KMS key. The KMS key must be in the same Region as the cluster. Choose Create an AWS KMS key to navigate to the AWS KMS console to create your own key.
## 10. Choose Create Repository
## Your Repository has been Created.
# *Pushing an image to your Repository*
## 1. Select the repository that you created and choose View push commands to view the steps to push an image to your new repository.
## 2. If you have a Dockerfile for the image to push, build the image and tag it for your new repository. Using the docker build command from the console in a terminal window. Make sure that you are in the same directory as your Dockerfile. (optional)
## 3. Tag the image with your Amazon ECR registry URI and your new repository by pasting the docker tag command from the console into a terminal window. The console command assumes that your image was built from a Dockerfile in the previous step. If you did not build your image from a Dockerfile, replace the first instance of repository:latest with the image ID or image name of your local image to push.
## 4. Push the newly tagged image to your repository by using the docker push command in a terminal window.
## Choose Close.

_________________________________________________________________

# Creating a task definition 

## 1. Open the new console at https://console.aws.amazon.com/ecs/v2
## 2. In the navigation pane, choose Task definitions, Create new task definition.
## 3. For Task definition family, specify a unique name for the task definition.
## 4. For each container to define in your task definition, complete the following steps.
##   a) For Name, specify a name for the container.

##   b) For Image URI, specify the image to use to start a container. Images in the Docker Hub registry are may be specified using the Docker Hub registry name only. For example, if amazonlinux:latest is specified, the Amazon Linux container hosted on Docker Hub is used. For all other repositories, specify the repository using either the repository-url/image:tag or repository-url/image@digest formats.
##   c) For Essential container, if your task definition has two or more containers defined, you may specify whether the container should be considered essential. If a container is marked as essential, if that container stops then the task is stopped. Each task definition must contain at least one essential container.
## d) For Container port and Protocol, specify the port mapping to use for the container. A port mapping allows the container to access ports on the host to send or receive traffic. Choose Add more port mappings to specify additional container port mappings.

## e) Expand the Environment variables section to specify environment variables to inject into the container. You can specify environment variables either individually using key-value pairs or in bulk by specifying an environment variable file hosted in an Amazon S3 bucket. For information on how to format an environment variable file, see Specifying environment variables.
## f) Choose Add more containers to add additional containers to the task definition. Choose Next once all containers have been defined. (optional)
## 5. For App environment, choose AWS Fargate (serverless), Amazon EC2 instances, or both. Amazon ECS performs validation using this value to ensure the task definition parameters are valid for the infrastructure type.
## 6. For Operating system/Architecture, choose the operating system and CPU architectire for the task.

## To run your task on a 64-bit ARM architecture, select Linux/ARM64. For more information, see Runtime platform.

## To run your AWS Fargate (serverless) tasks on Windows containers, choose a supported Windows operating system. For more information, see Windows containers on AWS Fargate considerations.
## 7. For Task size, specify the CPU and memory values to reserve for the task. The CPU value is specified as vCPUs and memory is specified as GB.

## 8. Expand the Task roles, network mode section to specify an IAM role to assign to the task. A task IAM role provides permissions for the containers in a task to call AWS APIs. (optional)
## 9. The Storage section is used to expand the amount of ephemeral storage for tasks hosted on Fargate as well as add a data volume configuration for the task. (optional)
### a) For Amount, to expand the available ephemeral storage beyond the default value of 20 GiB for your Fargate tasks, specify a value up to 200 GiB.
## 10. Choose Add volume to add a data volume configuration for the task. For each data volume, complete the following steps.(optional)
### a) For Volume type, choose Bind mount.

### b) For Volume name, specify a name for the data volume. The data volume name is used when creating a container mount point in a later step.
### c) Expand the Container mount points section and choose Add.
### d) For Container, choose the container for the mount point
### e) For Source volume, choose the data volume to mount to the container.
### f) For Container path, specify the path on the container to mount the volume.
### g) For Read only, specify whether to make the volume read only.
### h)Choose Add to add additional mount points until each data volume defined in the task definition has a mount point defined.
## 11. Select the Use log collection option to specify a log configuration. For each available log driver, there are log driver options to specify. The default option sends container logs to CloudWatch Logs. The other log driver options are configured using AWS FireLens. 
## The following describes each container log destination in more detail.
### > Amazon CloudWatch — Configure the task to send container logs to CloudWatch Logs. The default log driver options are provided which creates a CloudWatch log group on your behalf. To specify a different log group name, change the driver option values.

### > Amazon Kinesis Data Firehose — Configure the task to send container logs to Kinesis Data Firehose. The default log driver options are provided which sends logs to an Kinesis Data Firehose delivery stream. To specify a different delivery stream name, change the driver option values.
### > Amazon Kinesis Data Streams — Configure the task to send container logs to Kinesis Data Streams. The default log driver options are provided which sends logs to an Kinesis Data Streams stream. To specify a different stream name, change the driver option values.
### > Amazon OpenSearch Service — Configure the task to send container logs to an OpenSearch Service domain. The log driver options must be provided. 
### > mazon S3 — Configure the task to send container logs to an Amazon S3 bucket. The default log driver options are provided but you must specify a valid Amazon S3 bucket name.
## 12. Select the Use trace collection option to configure your tasks to route trace data from your application to AWS X-Ray. When this option is selected, Amazon ECS creates an AWS Distro for OpenTelemetry container sidecar which is preconfigured to send the trace data.(optional)
## 13. Select the Use metric collection option to collect and send metrics for your tasks to either Amazon CloudWatch or Amazon Managed Service for Prometheus. When this option is selected, Amazon ECS creates an AWS Distro for OpenTelemetry container sidecar which is preconfigured to send the application metrics. (optional)
### a) When Amazon CloudWatch is selected, your custom application metrics are routed to CloudWatch as custom metrics.
### b) When Amazon Managed Service for Prometheus (Prometheus libraries instrumentation is selected, your task-level CPU, memory, network, and storage metrics and your custom application metrics are routed to Amazon Managed Service for Prometheus. For Workspace remote write endpoint, specify the remote write endpoint URL for your Prometheus workspace. For Scraping target, specify the host and port the AWS Distro for OpenTelemetry collector can use to scrape for metrics data.
### c) When Amazon Managed Service for Prometheus (OpenTelemetry instrumentation is selected, your task-level CPU, memory, network, and storage metrics and your custom application metrics are routed to Amazon Managed Service for Prometheus. For Workspace remote write endpoint, specify the remote write endpoint URL for your Prometheus workspace.
## 14. Expand the Tags section to add tags, as key-value pairs, to the task definition. (optional)
## 15.Choose Next to review the task definition. 
## 16. On the Review and create page, review each task definition section. Choose Edit to make changes. Once the task definition is complete, choose Create to register the task definition.
__________________________________________________

# Creating a service using the Amazon ECS console

## 1. Open the new console at https://console.aws.amazon.com/ecs/v2
## 2. On the Clusters page, select the cluster to create the service in.
## 3. From the Services tab, choose Deploy.
## 4. The Compute configuration section can be expanded to change the compute option for your service to use. By default, the console will select a compute option for you so in most cases you can go to the next step. The following describes the order that the console uses to select a default:
### > If your cluster has a default capacity provider strategy defined, it will be selected.
### > If your cluster doesn't have a default capacity provider strategy defined but you do have the Fargate capacity providers added to the cluster, a custom capacity provider strategy using the FARGATE capacity provider will be selected.
### > If your cluster doesn't have a default capacity provider strategy defined but you do have one or more Auto Scaling group capacity providers added to the cluster, the Use custom (Advanced) option is selected and you will need to manually define the strategy.
### > If your cluster doesn't have a default capacity provider strategy defined but you do have one or more Auto Scaling group capacity providers added to the cluster, the Use custom (Advanced) option is selected and you will need to manually define the strategy.
## 5. For Application type, select Service.
## 6. For Task definition, choose the task definition family and revision to use.
## 7. For Service name, specify a name for your service
## 8. For Desired tasks, specify the number of tasks to launch and maintain in the service.
## 9. The Deployment options section can be expanded to change the minimum healthy percent and maximum percent of running tasks allowed during a service deployment. The console has default values for the most common use case selected.
## 10. The Load balancing section can be expanded to configure a load balancer for your service. Use the following steps to configure your service to use an Application Load Balancer. (optional)
### >For Load balancer type, select Application Load Balancer.
### >Choose Create a new load balancer to create a new Application Load Balancer or Use an existing load balancer to select an existing Application Load Balancer.
### >When creating a new load balancer, for Load balancer name, specify a unique name for your load balancer. When using an existing load balancer, for Load balancer, select your existing load balancer.

### >For Listener, specify a port and protocol for the Application Load Balancer to listen for connection requests on. By default, the load balancer will be configured to use port 80 and HTTP.
### >For Target group name, specify a name and a protocol for the target group that the Application Load Balancer will route requests to. By default, the target group will route requests to the first container defined in your task definition.
### >For Health check path, specify a path that exists within your container where the Application Load Balancer should periodically send requests to verify the connection health between the Application Load Balancer and the container. By default, a path of / is used which is the root directory.
### >For Health check grace period, specify the amount of time (in seconds) that the service scheduler should ignore unhealthy Elastic Load Balancing target health checks for.
## 11. The Networking section can be expanded to define the network configuration for the service. Task definitions that use the awsvpc network mode or services configured to use a load balancer must have a networking configuration. By default, the console selects the default Amazon VPC along with all subnets and the default security group within the default Amazon VPC. Use the following steps to specify a custom configuration.
### a) For VPC, select the VPC to use.
### b) For Subnets, select one or more subnets in the VPC that the task scheduler should consider when placing your tasks.
### c) For Security group, you can either select an existing security group or create a new one. To use an existing security group, select the security group and move to the next step. To create a new security group, choose Create a new security group. You must specify a security group name, description, and then add one or more inbound rules for the security group.
### d) For Public IP, choose whether to auto-assign a public IP address to the elastic network interface (ENI) of the task. Tasks that are launched on AWS Fargate can be assigned a public IP address when run using a public subnet so they have a route to the internet. 
## 12. The Tags section can be expanded to add tags, in the form of key-value pairs, to the service. (optional)
__________________________________________________
