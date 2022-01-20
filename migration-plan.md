
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

```terraform
provider "aws" {
    region = "ap-south-1"
}

module "ecs-module" {
    source = "./ecs"
    
}
```
 
```
# Configure the AWS Provider

terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 3.0"
    }
  }
}

# # Configure the AWS Provider
# provider "aws" {
#   region = var.aws_region
# }

provider "aws" {
  region = var.region
}

data "aws_caller_identity" "current" {}
```
```
// Registry

resource "aws_ecr_repository" "web" {
  name                     = join("-", [ var.application_name, var.environment, "web" ])
  image_tag_mutability     = "MUTABLE"

  image_scanning_configuration {
    scan_on_push = true
  }

  // tags = {
  //   Name           = join("-", [ var.application_name, var.environment, "web" ])
  //   environment    = var.environment
  //   maintainer     = var.maintainer
  // }

}


// Load Balancer

resource "aws_s3_bucket" "web-logs" {
  bucket = join("-", [ var.application_name, var.environment, "logs" ])
  acl    = "private"

  tags = {
    Name           = join("-", [ var.application_name, var.environment, "web", "logs" ])
    environment    = var.environment
    maintainer     = var.maintainer
  }
}

// https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/enable-access-logs.html#d0e10520.%20variable
resource "aws_s3_bucket_policy" "web-logs" {
  bucket = join("-", [ var.application_name, var.environment, "logs" ])
  policy = jsonencode(
              {
                Id        = "LB-Logs-Allow-Policy"
                Statement = [
                    {
                        Action    = "s3:PutObject"
                        Effect    = "Allow"
                        Principal = {
                                      AWS = join(":", [ "arn:aws:iam:", var.alb_ac_id, "root" ])
                                    }
                        
                        Resource  = join("/", [ 
                                        aws_s3_bucket.web-logs.arn,
                                        join("-", [ var.application_name, var.environment, "web" ]), 
                                        "AWSLogs",
                                        data.aws_caller_identity.current.account_id,
                                        "*"
                                      ])
                        Sid       = "Load Balancer Account Access Web"
                    },
                    {
                        Action    = "s3:PutObject"
                        Condition = {
                                      StringEquals = {
                                          "s3:x-amz-acl" = "bucket-owner-full-control"
                                        }
                                    }
                        Effect    = "Allow"
                        Principal = {
                                      Service = "delivery.logs.amazonaws.com"
                                    }
                        
                        Resource  = join("/", [ 
                                        aws_s3_bucket.web-logs.arn,
                                        join("-", [ var.application_name, var.environment, "web" ]), 
                                        "AWSLogs",
                                        data.aws_caller_identity.current.account_id,
                                        "*"
                                      ])
                        Sid       = "AWSLogDeliveryWrite Web"
                    },
                    {
                        Action    = "s3:GetBucketAcl"
                        Effect    = "Allow"
                        Principal = {
                                      Service = "delivery.logs.amazonaws.com"
                                    }
                        
                        Resource  = aws_s3_bucket.web-logs.arn
                        Sid       = "AWSLogDeliveryAclCheck"
                    },
                  ]
                Version   = "2012-10-17"
              }
          )

  depends_on  = [ aws_s3_bucket.web-logs ]
}



resource "aws_lb" "web" {
  name                = join("-", [ var.application_name, var.environment, "web" ])
  internal            = false
  load_balancer_type  = "application"
  security_groups     = [ data.terraform_remote_state.core.outputs.sg-allow-public-web-id ]
  
  subnets             = [
                          data.terraform_remote_state.core.outputs.subnet-id-public-01,
                          data.terraform_remote_state.core.outputs.subnet-id-public-02,
                          data.terraform_remote_state.core.outputs.subnet-id-public-03,
                        ]

  enable_deletion_protection = true

  access_logs {
    bucket  = aws_s3_bucket.web-logs.bucket
    prefix  = join("-", [ var.application_name, var.environment, "web" ])
    enabled = true
  }


  depends_on          = [ aws_s3_bucket_policy.web-logs ]

  tags = {
    Name           = join("-", [ var.application_name, var.environment, "web" ])
    environment    = var.environment
    maintainer     = var.maintainer
  }
}

resource "aws_lb_target_group" "web" {
  name     = join("-", [ var.application_name, var.environment, "web", "http" ])
  port     = 80
  protocol = var.protocol
  target_type    = "ip"
  vpc_id   = data.terraform_remote_state.core.outputs.vpc-main-id

  depends_on          = [ aws_lb.web ]
}
 
// resource "aws_lb_listener" "https" {
//   load_balancer_arn = aws_lb.web.arn
//   port              = "443"
//   protocol          = "HTTPS"
//   ssl_policy        = "ELBSecurityPolicy-2016-08"
//   certificate_arn   = "arn:aws:acm:eu-west-1:031789349449:certificate/9b0875fa-5e43-476f-889e-2b991a838575"
//   depends_on         = [ aws_lb.web, aws_lb_target_group.web ]
//   default_action {
//     type             = "forward"
//     target_group_arn = aws_lb_target_group.web.arn
//   }
// }

resource "aws_lb_listener" "http"{
  load_balancer_arn = aws_lb.web.arn
  port              = "80"
  protocol          = var.protocol


  // Change it if we are using HTTPS

  // default_action {
  //   type = "redirect"

  //   redirect {
  //     port        = "443"
  //     protocol    = "HTTPS"
  //     status_code = "HTTP_301"
  //   }
  // }

  default_action {
    type             = "forward"
    target_group_arn = aws_lb_target_group.web.arn
  }
}

resource "aws_ecs_cluster" "web" {
  name               = join("-", [ var.application_name, var.environment ])

  capacity_providers = [ "FARGATE", "FARGATE_SPOT" ]

  setting {
      name  = "containerInsights"
      value = "enabled"
  }

  tags = {
    Name           = join("-", [ var.application_name, var.environment ])
    environment    = var.environment
    maintainer     = var.maintainer
  }

}

resource "aws_ecs_task_definition" "web" {
  family                     = join("-", [ var.application_name, var.environment, "web" ])

  // Task definition file might be created dynamically locally
  container_definitions      = file("task-definitions/demo-web.json")

  requires_compatibilities   = [ "FARGATE" ]
  network_mode               = "awsvpc"
  cpu                        = 1024
  memory                     = 2048

  execution_role_arn         = join(":", [ "arn:aws:iam:", var.aws_account_id, "role/ecsTaskExecutionRole" ])

  task_role_arn         = join(":", [ "arn:aws:iam:", var.aws_account_id, "role/ecsTaskExecutionRole" ])
  
  // placement_constraints {
  //   type       = "memberOf"
  //   expression = "attribute:ecs.availability-zone in [eu-west-1a, eu-west-1b]"
  // }

  tags = {
    Name           = join("-", [ var.application_name, var.environment ])
    environment    = var.environment
    maintainer     = var.maintainer
  }
}


resource "aws_ecs_service" "web" {
  name                = join("-", [ var.application_name, var.environment, "web" ])
  cluster             = aws_ecs_cluster.web.id
  task_definition     = aws_ecs_task_definition.web.arn
  desired_count       = 1
  // scheduling_strategy = "DAEMON"

  launch_type         = "FARGATE"

  network_configuration {
    security_groups = [
                        data.terraform_remote_state.core.outputs.sg-testing-mode-id
                      ]
    subnets         = [
                        data.terraform_remote_state.core.outputs.subnet-id-private-01,
                        data.terraform_remote_state.core.outputs.subnet-id-private-02,
                        data.terraform_remote_state.core.outputs.subnet-id-private-03,
                      ]
  }

  load_balancer {
    // target_group_arn = "${aws_lb_target_group.foo.arn}"
    target_group_arn = aws_lb_target_group.web.arn
    container_name   = join("-", [ var.application_name, var.environment, "web" ])
    container_port   = 80
  }


  depends_on          = [ aws_ecs_cluster.web, aws_ecs_task_definition.web, aws_lb_target_group.web, aws_lb_listener.http ]

  // Change it if we are using HTTPS
  // aws_lb_listener.https


  // tags = {
  //   Name           = join("-", [ var.application_name, var.environment ])
  //   environment    = var.environment
  //   maintainer     = var.maintainer
  // }
}


```



