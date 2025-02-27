# Step 2: Configure a network<a name="service-configure-network"></a>

**Important**  
Amazon ECS has provided a new console experience for creating a service\. For more information, see [Creating a service using the New Amazon ECS console](create-service-console-v2.md)\.

If your service's task definition uses the `awsvpc` network mode, you must configure a VPC, subnet, and security group for your service\.

If your service's task definition uses the `bridge`, `host`, or `none` network modes, you can move on to the next step, [Step 3: Configuring your service to use a load balancer](service-create-loadbalancer.md)\.

For tasks hosted on Amazon EC2 instances, the `awsvpc` network mode doesn't provide task ENIs with public IP addresses\. To access the internet, tasks hosted on Amazon EC2 instances can be launched in a private subnet that is configured to use a NAT gateway\. For more information, see [NAT Gateways](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html) in the *Amazon VPC User Guide*\. Inbound network access must be from within the VPC using the private IP address or DNS hostname, or routed through a load balancer from within the VPC\. Tasks launched within public subnets do not have internet access\.

**To configure VPC and security group settings for your service**

1. If you have not done so already, follow the basic service configuration procedures in [Step 1: Configuring basic service parameters](basic-service-params.md)\.

1. For **Cluster VPC**, if you are hosting tasks on Amazon EC2 instances, choose the VPC in which your instances reside\. If you're hosting tasks on Fargate, select the VPC that the Amazon ECS on Fargate tasks should use\. Ensure that the VPC you choose is not configured to require dedicated hardware tenancy, as that isn't supported by Fargate\.

1. For **Subnets**, choose the available subnets for your service task placement\.

1. For **Security groups**, a security group has been created for your service's tasks, which allows HTTP traffic from the internet \(`0.0.0.0/0`\)\. To edit the name or the rules of this security group, or to choose an existing security group, choose **Edit** and then modify your security group settings\.

1. For **Auto\-assign Public IP**, choose whether to have your tasks receive a public IP address\. For tasks on Fargate, in order for the task to pull the container image it must either use a public subnet and be assigned a public IP address or a private subnet that has a route to the internet or a NAT gateway that can route requests to the internet\.

1. If you are configuring your service to use a load balancer or if you are using the blue/green deployment type, continue to [Step 3: Configuring your service to use a load balancer](service-create-loadbalancer.md)\. If you are not configuring your service to use a load balancer, you can choose **None** as the load balancer type and move on to the next section, [Step 5: Configuring your service to use Service Auto Scaling](service-configure-auto-scaling.md)\.