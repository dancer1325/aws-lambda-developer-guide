# Configuring a Lambda function -- to access -- resources | VPC<a name="configuration-vpc"></a>

* Connect a Lambda function -- to -- private subnets | VPC
  * Reason of use Amazon VPC: 🧠 create a private network -- for -- resources (_Example:_ databases, cache instances, or internal services) 🧠
  * uses
    * 👀lambda function -- can access to -- private resources | during execution 👀
  * -> 👀Lambda creates an [elastic network interface](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_ElasticNetworkInterfaces.html) / EACH combination of security group + subnet | your function's VPC configuration 👀
    * can take about a minute / during this time
      *  👀 NOT possible to perform additional operations / target the function 👀
        * _Example: [creating versions](configuration-versions.md) or updating the function's code
  * steps
    1. Open the [Lambda console](https://console.aws.amazon.com/lambda)
    2. Choose a function
    3. Under **VPC**, choose **Edit**
    4. Choose **Custom VPC**
    5. Choose a VPC, subnets, and security groups\.
       1. if your function needs internet access -> use [NAT](#vpc-internet)
       2. if you connect a function -- to a -- public subnet -> does NOT give
          1. internet access or
          2. public IP address
    6. Choose **Save**\

* Lambda function states
  * if new functions, NOT possible to invoke the function, until its state transitions from `Pending` -- to -- `Active` 
  * if existing functions, you can STILL invoke the old version
    * while the update is in progress 
  * see [Monitoring the state of a function with the Lambda API](functions-states.md)\.

* TODO:
Multiple functions connected to the same subnets share network interfaces, so connecting additional functions to a subnet that already has a Lambda\-managed network interface is much quicker\. However, Lambda might create additional network interfaces if you have many functions or very busy functions\.

If your functions are not active for a long period of time, Lambda reclaims its network interfaces, and the function becomes `Idle`\. Invoke an idle function to reactivate it\. The first invocation fails and the function enters a pending state again until the network interface is available\.

Lambda functions cannot connect directly to a VPC with [dedicated instance tenancy](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/dedicated-instance.html)\. To connect to resources in a dedicated VPC, [peer it to a second VPC with default tenancy](https://aws.amazon.com/premiumsupport/knowledge-center/lambda-dedicated-vpc/)\.

**VPC tutorials**
+ [Tutorial: Configuring a Lambda function to access Amazon RDS in an Amazon VPC](services-rds-tutorial.md)
+ [Tutorial: Configuring a Lambda function to access Amazon ElastiCache in an Amazon VPC](services-elasticache-tutorial.md)

**Topics**
+ [Execution role and user permissions](#vpc-permissions)
+ [Configuring Amazon VPC access with the Lambda API](#vpc-configuring)
+ [Internet and service access for VPC\-connected functions](#vpc-internet)
+ [Sample VPC configurations](#vpc-samples)

## Execution role and user permissions<a name="vpc-permissions"></a>

Lambda uses your function's permissions to create and manage network interfaces\. To connect to a VPC, your function's execution role must have the following permissions\.

**Execution role permissions**
+ **ec2:CreateNetworkInterface**
+ **ec2:DescribeNetworkInterfaces**
+ **ec2:DeleteNetworkInterface**

These permissions are included in the **AWSLambdaVPCAccessExecutionRole** managed policy\.

When you configure VPC connectivity, Lambda uses your permissions to verify network resources\. To configure a function to connect to a VPC, your IAM user need the following permissions\.

**User permissions**
+ **ec2:DescribeSecurityGroups**
+ **ec2:DescribeSubnets**
+ **ec2:DescribeVpcs**

## Configuring Amazon VPC access with the Lambda API<a name="vpc-configuring"></a>

You can connect a function to a VPC with the following APIs\.
+ [CreateFunction](API_CreateFunction.md)
+ [UpdateFunctionConfiguration](API_UpdateFunctionConfiguration.md)

To connect your function to a VPC during creation with the AWS CLI, use the `vpc-config` option with a list of private subnet IDs and security groups\. The following example creates a function with a connection to a VPC with two subnets and one security group\.

```
$ aws lambda create-function --function-name my-function \
--runtime nodejs12.x --handler index.js --zip-file fileb://function.zip \
--role arn:aws:iam::123456789012:role/lambda-role \
--vpc-config SubnetIds=subnet-071f712345678e7c8,subnet-07fd123456788a036,SecurityGroupIds=sg-085912345678492fb
```

To connect an existing function, use the `vpc-config` option with the `update-function-configuration` command\.

```
$ aws lambda update-function-configuration --function-name my-function \
--vpc-config SubnetIds=subnet-071f712345678e7c8,subnet-07fd123456788a036,SecurityGroupIds=sg-085912345678492fb
```

To disconnect your function from a VPC, update the function configuration with an empty list of subnets and security groups\.

```
$ aws lambda update-function-configuration --function-name my-function \
--vpc-config SubnetIds=[],SecurityGroupIds=[]
```

## Internet and service access for VPC\-connected functions<a name="vpc-internet"></a>

By default, Lambda runs your functions in a secure VPC with access to AWS services and the internet\. The VPC is owned by Lambda and does not connect to your account's default VPC\. When you connect a function to a VPC in your account, it does not have access to the internet unless your VPC provides access\.

**Note**  
Several services offer [VPC endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints.html)\. You can use VPC endpoints to connect to AWS services from within a VPC without internet access\.

Internet access from a private subnet requires network address translation \(NAT\)\. To give your function access to the internet, route outbound traffic to a NAT gateway in a public subnet\. The NAT gateway has a public IP address and can connect to the internet through the VPC's internet gateway\. For more information, see [NAT gateways](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html) in the *Amazon VPC User Guide*\.

## Sample VPC configurations<a name="vpc-samples"></a>

Sample AWS CloudFormation templates for VPC configurations that you can use with Lambda functions are available in this guide's GitHub repository\. There are two templates:
+ [vpc\-private\.yaml](https://github.com/awsdocs/aws-lambda-developer-guide/blob/master/templates/vpc-private.yaml) – A VPC with two private subnets and VPC endpoints for Amazon Simple Storage Service and Amazon DynamoDB\. You can use this template to create a VPC for functions that do not need internet access\. This configuration supports use of Amazon S3 and DynamoDB with the AWS SDK, and access to database resources in the same VPC over a local network connection\.
+ [vpc\-privatepublic\.yaml](https://github.com/awsdocs/aws-lambda-developer-guide/blob/master/templates/vpc-privatepublic.yaml) – A VPC with two private subnets, VPC endpoints, a public subnet with a NAT gateway, and an internet gateway\. Internet\-bound traffic from functions in the private subnets is routed to the NAT gateway by a route table\.

To use a template to create a VPC, choose **Create stack** in the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation) and follow the instructions\.