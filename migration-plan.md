
# Migration plan for (Add the application name)
* ## ECS to Kubernetes 1.2.1

##  1.	Overview
* ### We need to migrate complete infrastructure for (your application) from ECS  to kubernetes ,which would enable us to have highly scalable and serverless infrastructure. 
## 2. Goals
 * ### 2.1.	Migrating to kubernetes v 1.2.1 : Migrating from ECS stack  to kubernetes cluster. 
* ### 2.2.	Maintain minimum/no downtime at migration: The app should not be down during the migration process.
## 3. Points to consider.
* ### 3.1. VPC is new and different from the existing one for both environments staging & prod and taken care by terraform eks module.
* ### 3.2. Subnets will be new and different from the existing one for both environments staging & prod 
* ### 3.3. Security groups should be new 
* ### 3.4. 3.4. Use the docker containers as a pods in kubernetes cluster with (your required size) nodes

## 4. Migration phase 1.
* ### 4.1.	Create core -infra including vpc, subnets, routing-tables, security-groups
* ### 4.2.	Bring up the Kubernetes cluster
* ### 4.3.	Setup docker registry on ECR
* ### 4.4.	Build app image and push to ECR
* ### 4.5.	Test build and push pipeline
* ### 4.6.	Migrate the RDS to new VPC, Make sure RDS is accessible from the application pod on the cluster
* ### 4.7.	Setup ElasticSearch with access policy and add NAT gateway IP for kubernetes cluster
* ### 4.8.	Snapshot the RDS database

* * ### 4.9.1.	Namespace
* * ### 4.9.2.	Ingress
* * ### 4.9.3.	Secrets
* * ### 4.9.4.	Deployment
* * ### 4.9.5.	Service
* ### 4.10.	Create deployment configuration as per ECR url for your specific application and deploy.We might use docker hub instead
* ### 4.11.	Make the host entry to test application
* ### 4.12.	Test the application from browser for reachability and login
* ### 4.13.	Verify  SSL for the domain as per environment (SSL would not be valid at this point)
* ### 4.14.	Test deployment pipeline
* ### 4.15.	Snapshot the RDS database
## 5. Migration phase 2.
* ### 5.1.	Testing team makes the host entry in their system to test the application on the new cluster.
* ### 5.2.	Testing team access the application on the browser (Not valid SSL yet).
* ### 5.3.	Reduce the TTL to DNS record.
* ### 5.4.	Testing team confirm the application and related components are working fine.
## 6. Migration phase 3.
* ### 6.1.	Snapshot the RDS database
* ### 6.2.	Change the DNS to cluster
* ### 6.3.	Ensure the valid SSL certificate is issued by the certificate service
* ### 6.4.	Ask the test team to test the application again

> ## You can also add the same using a terraform template including ECS and EKS cluster.
