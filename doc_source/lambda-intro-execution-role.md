# AWS Lambda execution role<a name="lambda-intro-execution-role"></a>

* AWS Lambda function's execution role
  * := IAM role / assumed by Lambda | invoke a function  
    * grants it permission / -- access --
      * AWS services
      * AWS resources
  * how is it used?
    * when you create a function -> role is provided
      * 👀by default, Lambda creates an execution role / minimal permissions 👀
    * when your function is invoked -> Lambda assumes the role 
    * add or remove permissions from a function's execution role | ANY time
  * use cases
    * execution role / has permission to
      * send logs to Amazon CloudWatch
      * upload trace data | AWS X-Ray
  * how to view a function's execution role?
    1. Open the Lambda console [Functions page](https://console.aws.amazon.com/lambda/home#/functions)\.
    2. Choose a function\.
    3. Choose **Permissions**\.
    4. resource summary of the services & resources / function has access to

## Creating an execution role | IAM console<a name="permissions-executionrole-console"></a>

* steps
  1. Open the [roles page | IAM console](https://console.aws.amazon.com/iam/home#/roles)
  2. Choose **Create role**\.
  3. Under **Common use cases**, choose **Lambda**\.
  4. Choose **Next: Permissions**\.
  5. Under **Attach permissions policies**, choose the managed policies
     1. **AWSLambdaBasicExecutionRole** &
     2. **AWSXRayDaemonWriteAccess**
  6. Choose consecutive **Next:** to end up creating it

* see [Creating a role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html#roles-creatingrole-service-console)

## Managing roles with the IAM API<a name="permissions-executionrole-api"></a>

* see [getting started CLI](gettingstarted-awscli.md#create-the-execution-rolea-namewith-userapp-walkthrough-custom-events-create-iam-rolea)

## Managed policies -- for -- Lambda features<a name="permissions-executionrole-features"></a>

* managed policies / 
  * provide permissions -- to use -- Lambda features
    + **AWSLambdaBasicExecutionRole**
      + == Permission -- to upload -- logs to CloudWatch\.
    + **AWSLambdaKinesisExecutionRole**
      + == Permission -- to read -- events / from Amazon Kinesis
        + data stream or
        + consumer
    + **AWSLambdaDynamoDBExecutionRole**
      + == Permission -- to read -- records / from an Amazon DynamoDB stream\.
    + **AWSLambdaSQSQueueExecutionRole** 
      + == Permission -- to read a -- message / from an Amazon Simple Queue Service queue
    + **AWSLambdaVPCAccessExecutionRole**
      + == Permission -- to manage -- elastic network interfaces
        + -> connect your function -- to a -- VPC
    + **AWSXRayDaemonWriteAccess**
      + == Permission -- to upload -- trace data to X-Ray
  * add to your execution role | BEFORE enabling features

* if you use an [event source mapping](invocation-eventsourcemapping.md) / invoke your function -> event data -- can be read, via -- execution role

* templates
  * -- provided by -- AWS Lambda console
  * allows
    * creating a custom policy / has the permissions -- related to -- additional use cases
  * uses
    * applied automatically when you
      * create a function -- from a -- blueprint, or
      * configure options / -- require access to -- other services
  * check ["iam policies/" | this GitHub repository](https://github.com/awsdocs/aws-lambda-developer-guide/tree/master/iam-policies)