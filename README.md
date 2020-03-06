# Steps
1. Launch the CloudFormation template: [:arrow_right: Here](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=EIECSDemo&templateURL=https://s3.amazonaws.com/amazonei/CF-templates/ECSAmazonElasticInference.yaml)

    The Cloudformation template creates the following resources: 
    ![CFN](./img/eiecs-architecture.png)
    
    1. VPC and a Public Subnet (includes Public Route Table and IGW)
    2. Elastic Inference (EI) Endpoint and EI Endpoint Security Group. 
    3. IAM Policy Roles `ecs-ei-task-role` with `elastic-inference:Connect` 
        1. In this demo, `ecs-ei-task-role` includes additional IAM and EC2 list permissions for debugging.
    5. IAM Roles [bastion] `ClientInstanceRole`, [ec2 instance] `ECSContainerInstanceRole`, [ecs task] `ecsTaskExecutionRole`, [ecs task to EI] `ecs-ei-task-role`
    6. EC2 Instance `ECSContainerInstance` with sec group `ECSContainerInstanceSecurityGroup`
    7. EC2 Instance `ClientInstanceRole` with sec group `ClientInstanceSecGroup`
    8. IAM Instance Role for client instance `ClientInstanceProfile`
    9. SecGroups 
        1. allow 22 to [bastion]
        2. allow 443 in/out of EI Endpoint from within VPC
        3. allow 9000, 8500 to EC2 (Container Instance) from within VPC
    
2. Navigate to the ECS console. The Cloudformation template has also created a Task Definition named `ei-ecs-ubuntu-tfs-bridge`. 
        1. Inspect the Task Definition for 2 containers: TF ModelServer and EI-enabled TF ModelServer from Tensor Flow Serving. 
        2. Both containers are running on ubuntu Deep Learning optimized Container Images. The EI TF MS contianer downloads the additional EI-enabled MS binary and forces EI metadata lookup from the container instance.
        3. Inspect the entry point of the containers



# Resources
1. https://awsfeed.com/whats-new/machine-learning/running-amazon-elastic-inference-workloads-on-amazon-ecs/
