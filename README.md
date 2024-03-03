# AWS-EC2-Target-Group-Load-Balancer-Auto-Scaling-Group-Setup-using-CloudFormation
This tutorial outlines the steps to create an EC2 instance, a target group, a load balancer, and an auto scaling group on AWS.

## Prerequisites

- AWS account with the necessary permissions.
- AWS Command Line Interface (CLI) installed and configured or Access to the AWS Management Console.

## CloudFormation Stack Setup

### Step 1: EC2 Instances

#### Edit `ec2-instances.yaml`

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Resources:
  EC2Instance1:
    Type: 'AWS::EC2::Instance'
    Properties:
      ImageId: ami-xxxxxxxxxxxxxxxxx  # Specify your AMI ID
      InstanceType: t2.micro
  EC2Instance2:
    Type: 'AWS::EC2::Instance'
    Properties:
      ImageId: ami-xxxxxxxxxxxxxxxxx  # Specify your AMI ID
      InstanceType: t2.micro
```

#### Deploy EC2 Instances

- Using the CLI
  
```bash
aws cloudformation create-stack --stack-name EC2InstancesStack --template-body file://ec2-instances.yaml
```

- Using the Management Console
  
1. Open the AWS Management Console.

2. Navigate to the **CloudFormation** service.

3. Choose **Create Stack**.

4. In the **Select Template** section, choose **Upload a template file** and upload the `ec2-instances.yaml` file.

5. Choose **Next**.

6. Enter a stack name (e.g., `EC2InstancesStack`) and provide any required parameters.

7. Choose **Next** and follow the on-screen instructions to create the stack.

### Step 2: Target Group

#### Edit `target-group.yaml`

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Resources:
  MyTargetGroup:
    Type: 'AWS::ElasticLoadBalancingV2::TargetGroup'
    Properties:
      Port: 80
      Protocol: HTTP
      VpcId: vpc-xxxxxxxxxxxxxxxxx  # Specify your VPC ID
```

#### Deploy Target Group

- Using the CLI

```bash
aws cloudformation create-stack --stack-name TargetGroupStack --template-body file://target-group.yaml
```

- Using the Management Console

1. Open the AWS Management Console.

2. Navigate to the **CloudFormation** service.

3. Choose **Create Stack**.

4. In the **Select Template** section, choose **Upload a template file** and upload the `target-group.yaml` file.

5. Choose **Next**.

6. Enter a stack name (e.g., `TargetGroupStack`) and provide any required parameters.

7. Choose **Next** and follow the on-screen instructions to create the stack.

### Step 3: Load Balancer

#### Edit `load-balancer.yaml`

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Resources:
  MyLoadBalancer:
    Type: 'AWS::ElasticLoadBalancingV2::LoadBalancer'
    Properties:
      Subnets:
        - subnet-xxxxxxxxxxxxxxxxx  # Specify your subnet ID
        - subnet-yyyyyyyyyyyyyyyyy  # Specify your subnet ID
      SecurityGroups:
        - sg-xxxxxxxxxxxxxxxxx  # Specify your security group ID
      LoadBalancerAttributes:
        - Key: idle_timeout.timeout_seconds
          Value: '60'
      Name: MyLoadBalancer
```

#### Deploy Load Balancer

- Using the CLI

```bash
aws cloudformation create-stack --stack-name LoadBalancerStack --template-body file://load-balancer.yaml
```

- Using the Management Console

1. Open the AWS Management Console.

2. Navigate to the **CloudFormation** service.

3. Choose **Create Stack**.

4. In the **Select Template** section, choose **Upload a template file** and upload the `load-balancer.yaml` file.

5. Choose **Next**.

6. Enter a stack name (e.g., `LoadBalancerStack`) and provide any required parameters.

7. Choose **Next** and follow the on-screen instructions to create the stack.

### Step 4: Auto Scaling Group

#### Edit `auto-scaling-group.yaml`

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Resources:
  MyAutoScalingGroup:
    Type: 'AWS::AutoScaling::AutoScalingGroup'
    Properties:
      LaunchConfigurationName: MyLaunchConfig
      MinSize: 2
      MaxSize: 5
      DesiredCapacity: 2
      VPCZoneIdentifier:
        - subnet-xxxxxxxxxxxxxxxxx  # Specify your subnet ID
        - subnet-yyyyyyyyyyyyyyyyy  # Specify your subnet ID
```

#### Deploy Auto Scaling Group

- Using the CLI

```bash
aws cloudformation create-stack --stack-name AutoScalingGroupStack --template-body file://auto-scaling-group.yaml
```

- Using the Management Console

1. Open the AWS Management Console.

2. Navigate to the **CloudFormation** service.

3. Choose **Create Stack**.

4. In the **Select Template** section, choose **Upload a template file** and upload the `auto-scaling-group.yaml` file.

5. Choose **Next**.

6. Enter a stack name (e.g., `AutoScalingGroupStack`) and provide any required parameters.

7. Choose **Next** and follow the on-screen instructions to create the stack.


### Step 5: Clean Up

#### Delete CloudFormation Stacks

- Using the CLI

```bash
aws cloudformation delete-stack --stack-name EC2InstancesStack
aws cloudformation delete-stack --stack-name TargetGroupStack
aws cloudformation delete-stack --stack-name LoadBalancerStack
aws cloudformation delete-stack --stack-name AutoScalingGroupStack
```

- Using the Management Console

1. Open the AWS Management Console.

2. Navigate to the **CloudFormation** service.

3. Select the stacks created (`EC2InstancesStack`, `TargetGroupStack`, `LoadBalancerStack`, `AutoScalingGroupStack`).

4. Choose **Delete** and confirm the deletion.
