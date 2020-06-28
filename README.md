# AWS EKS Demo

This demo will be using CoudFormation (CFN) templates to create the EKS Cluster, Worker Nodes, VPC, Subnets and other network resources needed by the EKS Cluster.

NOTE: EKS Cluster and NAT Gateway are charged per hour after being provisioned. To avoid unexpected charges in your AWS account, remember to delete the stacks you created once you are done.

<img src="images/eks-2az-diagram.png" width="700" height="">


## Create VPC for EKS Cluster

Create VPC using CloudFormation\
https://docs.aws.amazon.com/eks/latest/userguide/create-public-private-vpc.html

CloudFormation template with public and private subnets:\
https://amazon-eks.s3.us-west-2.amazonaws.com/cloudformation/2020-06-10/amazon-eks-vpc-private-subnets.yaml

You may use the CloudFormation template above as a guide and modify according to your requirements.

For this example, I have modified the VPC CIDR and Subnets:

VPC CIDR: 172.29.0.0/16\
Public Subnets: 172.29.1.0/24, 172.29.2.0/24\
Private Subnets: 172.29.3.0/24, 172.29.4.0/24

Other network resources such as Internet GW, NAT GW, Route Tables and SecurityGroup for the control plane will be created as well.

CFN Template: [cfn-vpc-pub-pri.yaml](cfn-vpc-pub-pri.yaml)

1. Go to CloudFormation and Create stack
2. Upload template and continue
3. Enter Stack name: eks-demo-vpc 
4. Configure stack options: leave defaults and continue
5. Review and create stack

Your stack will have a status of "CREATE_COMPLETE" after all resources have been provisioned.

Review the stack by clicking on the Resources and Ouputs tabs.

<img src="images/eks-demo-vpc-resources.png" width="600" height="">

You will need the SecurityGroupId and SubnetIds to create the EKS Cluster.

<img src="images/eks-demo-vpc-outputs.png" width="600" height="">


## Create EKS Cluster

Creating an Amazon EKS cluster (general guide)\
https://docs.aws.amazon.com/eks/latest/userguide/create-cluster.html

Create EKS Cluster using CloudFormation\
https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-eks-cluster.html

CFN Template: [cfn-eks-cluster.yaml](cfn-eks-cluster.yaml)

1. Go to CloudFormation and Create stack
2. Upload template and continue
3. Enter Stack name: eks-cluster
4. Configure stack options: leave defaults and continue
5. Review and create stack

<img src="images/eks-demo-cluster-info.png" width="600" height="">


## Create EKS Worker Nodes

This demo will be using Self-managed nodes. For more information on Worker Nodes, see:
https://docs.aws.amazon.com/eks/latest/userguide/worker.html

Launching Amazon EKS Linux worker nodes (self-managed)\
https://docs.aws.amazon.com/eks/latest/userguide/launch-workers.html#self-managed-nodes

CFN Template: [cfn-eks-worker-nodes.yaml](cfn-eks-worker-nodes.yaml)

1. Go to CloudFormation and Create stack
2. Upload template and continue
3. Enter Stack name: eks-demo-worker-nodes
4. Configure stack options: leave defaults and continue
5. Review and create stack

## Clean Up
