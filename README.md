#  🌈 AWS CLI Commands Cheat Sheet
---
## *COMPUTE SERVICES*

### EC2 (Elastic Compute Cloud)

```css
aws ec2 describe-instances                              # List all instances
aws ec2 start-instances --instance-ids <id>             # Start an instance
aws ec2 stop-instances --instance-ids <id>              # Stop an instance
aws ec2 terminate-instances --instance-ids <id>         # Terminate an instance
aws ec2 create-key-pair --key-name <name>               # Create a key pair
aws ec2 describe-security-groups                        # List security groups
```


### Lambda (Serverless Computing)

```css
aws lambda list-functions                                                              # List all Lambda functions
aws lambda create-function --function-name <name> --runtime <runtime> \
  --role <role> --handler <handler>                                                    # Create a function
aws lambda update-function-code  --function-name <name>  --zip-file fileb://<file>.zip # Update function code
aws lambda delete-function --function-name <name>                                      # Delete a function
aws lambda invoke --function-name <name> output.json                                   # Invoke a function
```

### AWS Elastic Beanstalk

```css 
aws elasticbeanstalk describe-environments                                                       # List all environments
aws elasticbeanstalk create-application --application-name my-app                                # Create an application
aws elasticbeanstalk update-environment --environment-name my-env --version-label new-version    #  Deploy a new version
```
---
## *STORAGE SERVICES*

### S3 (Simple Storage Service)

```css
aws s3 ls                                       # List buckets
aws s3 mb s3://<bucket>                         # Create a bucket
aws s3 cp <file> s3://<bucket>/                 # Upload a file
aws s3 rb s3://<bucket> --force                 # Delete a bucket
aws s3 sync <local-dir> s3://<bucket>/          # Sync local and S3
```

### Amazon ECR (Elastic Container Registry)

```css
aws ecr get-login-password | docker login --username AWS \
  --password-stdin <aws_account_id>.dkr.ecr.<region>.amazonaws.com     # Authenticate Docker with ECR
aws ecr list-repositories                                              # List all ECR repositories
```

---

## CONTAINER SERVICES

### Amazon EKS (Elastic Kubernetes Service)

```css
aws eks list-clusters                                                    # List EKS clusters
aws eks describe-cluster --name my-cluster                               # Describe an EKS cluster
aws eks create-cluster --name my-cluster \
  --role-arn arn:aws:iam::account-id:role/EKSRole \
  --resources-vpc-config subnetIds=subnet-xxxx,securityGroupIds=sg-xxxx  # Create an EKS cluster
aws eks update-kubeconfig --name my-cluster                              # Configure kubectl to use the EKS cluster
kubectl get nodes                                                        # Check worker nodes
kubectl get pods -A                                                      # List running pods
```

### Amazon ECS (Elastic Container Service)

```css
aws ecs list-clusters                                                    # List ECS clusters
aws ecs list-services --cluster my-cluster                               # List ECS services
aws ecs describe-services --cluster my-cluster --services my-service     # Describe an ECS service
```

## *IDENTITY & ACCESS MANAGEMENT*

### IAM (Identity and Access Management)

```css
aws iam list-users                                                                        # List IAM users
aws iam create-user --user-name <name>                                                    # Create a user
aws iam attach-user-policy --user-name <name> --policy-arn <policy>                       # Attach a policy
aws iam list-roles`                                                                       # List IAM roles
aws iam create-role --role-name <name> --assume-role-policy-document file://policy.json   # Create a role
aws iam list-policies                                                                     # List policies
```
---
## *NETWORKING*

### VPC (Virtual Private Cloud)

```css
aws ec2 describe-vpcs                                       # List all VPCs
aws ec2 create-vpc --cidr-block <CIDR>                      # Create a VPC
aws ec2 delete-vpc --vpc-id <id>                            # Delete a VPC
aws ec2 create-subnet --vpc-id <id> --cidr-block <CIDR>     # Create a subnet
aws ec2 describe-subnets                                    # List all subnets
aws ec2 describe-internet-gateways                          # List internet gateways
```

### Elastic Load Balancer (ELB)

```css
aws elbv2 describe-load-balancers                           # List all load balancers
```

### Amazon Route 53

```css
aws route53 list-hosted-zones                               # List hosted zones
```

## *DEPLOYMENT & INFRASTRUCTURE AS CODE*

### AWS CloudFormation

```css
aws cloudformation list-stacks                                        # List all stacks
aws cloudformation create-stack --stack-name my-stack \
  --template-body file://template.yml                                 # Create a stack
aws cloudformation deploy --template-file outpost-config.yml \
  --stack-name my-outpost-stack                                       # Deploy Outpost resources

```

### AWS Auto Scaling

```css
aws autoscaling describe-auto-scaling-groups                                   # List Auto Scaling groups
aws autoscaling update-auto-scaling-group --auto-scaling-group-name my-asg \
  --desired-capacity 3                                                         # Update desired capacity
aws autoscaling set-desired-capacity --auto-scaling-group-name my-asg \
  --desired-capacity 2                                                         # Manually scale group

```
---

## CI/CD SERVICES

### AWS CodePipeline

```css
aws codepipeline list-pipelines                                 # List all pipelines
aws codepipeline start-pipeline-execution --name my-pipeline    # Start a pipeline execution
```

### AWS CodeBuild

```css
aws codebuild list-projects                                      # List all CodeBuild projects
aws codebuild start-build --project-name my-project              # Start a build
```

### AWS CodeDeploy

```css
aws deploy list-applications                                      # List all CodeDeploy applications
aws deploy create-deployment --application-name MyApp \
  --deployment-group-name MyDeploymentGroup \
  --s3-location bucket=my-bucket,key=app.zip,bundleType=zip       # Deploy application

```
---

## *SECURITY & COMPLIANCE*

### AWS CloudTrail

```css
aws cloudtrail describe-trails                                       # List CloudTrail logs
```

### AWS Secrets Manager

```css
aws secretsmanager list-secrets                                      # List all secrets
aws secretsmanager get-secret-value --secret-id my-secret            #  Retrieve a secret
```

### AWS GuardDuty

```css
aws guardduty list-findings                                           # Detect security threats
```
---

## *MONITORING & LOGGING*

### CloudWatch

```css
aws cloudwatch list-metrics                                  # List available metrics

aws cloudwatch put-metric-alarm \
  --alarm-name cpu-high \
  --metric-name CPUUtilization \
  --namespace AWS/EC2 \
  --statistic Average \
  --period 300 \
  --threshold 80                                              # Create alarm

aws logs describe-log-groups                                  # List all log groups

aws logs get-log-events --log-group-name my-log-group \
  --log-stream-name my-log-stream                             # Retrieve log events
```
---
## *MESSAGING & WORKFLOW*

### AWS SNS (Simple Notification Service)

```css
aws sns list-topics                                                                           # List all SNS topics
aws sns publish --topic-arn arn:aws:sns:region:account-id:MyTopic --message "Test Message"    # Publish message
```

### AWS SQS (Simple Queue Service)
```css
aws sqs list-queues                                                   # List all SQS queues

aws sqs send-message \
  --queue-url https://sqs.region.amazonaws.com/account-id/my-queue \
  --message-body "Hello World"                                        # Send message
```

### AWS Step Functions

```css
aws stepfunctions list-state-machines                                                # List all state machines
aws stepfunctions start-execution \
  --state-machine-arn arn:aws:states:region:account-id:stateMachine:MyStateMachine   # Start execution
```
---
## *DATA & ANALYTICS*

### AWS Glue (ETL Service)

```css 
aws glue get-databases                                # List all Glue databases
aws glue get-tables --database-name my-database       # List tables in database
aws glue start-job-run --job-name my-glue-job         # Start a Glue job

```
---
## *MANAGEMENT TOOLS*

### AWS Systems Manager (SSM)

```css
aws ssm describe-instances                                      # List managed instances
aws ssm send-command --document-name "AWS-RunShellScript" \
  --targets "Key=instanceIds,Values=i-xxxxxxxxx" \
  --parameters commands="sudo apt update"                       # Run command

```
---
## *AWS OUTPOSTS (HYBRID CLOUD)*

### List and Describe Outposts

```css
aws outposts list-outposts                                          # List all AWS Outposts
aws outposts get-outpost --outpost-id <outpost-id>                  # Get Outpost details
aws outposts get-outpost-instance-types --outpost-id <outpost-id>   # List instance types

```

### Manage Outpost Resources

```css
aws outposts list-sites                                          # List all Outpost sites
aws outposts list-orders                                         # List Outpost orders
aws outposts list-outpost-instances --outpost-id <outpost-id>    # List EC2 instances

```

### Deploy and Configure Outposts

```css
aws outposts create-order \
  --line-items "[{\"catalogItemId\": \"item-id\", \"quantity\": 1}]" \
  --outpost-id <outpost-id>                                                # Order Outpost
aws outposts update-outpost --outpost-id <outpost-id> --name <new-name>    # Update configuration

```

### Networking and Storage on Outposts

```css
aws outposts list-outpost-network-devices --outpost-id <outpost-id>    # List network devices
aws s3 ls --outpost-id <outpost-id>                                    # List S3 buckets on Outpost
aws s3 mb s3://<bucket-name> --outpost-id <outpost-id>                 # Create S3 bucket in Outpost

```

### Deploy EC2 in Outposts

```css
aws ec2 run-instances --image-id <ami-id> --instance-type <type> --subnet-id <outpost-subnet-id>   # Launch EC2 in Outpost
```

### Delete Outposts

```css
aws outposts delete-outpost --outpost-id <outpost-id>            # Delete Outpost (must be empty)
aws outposts cancel-order --order-id <order-id>                  # Cancel Outpost order
```

