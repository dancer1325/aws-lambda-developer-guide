# AWS Lambda applications<a name="deploying-lambda-apps"></a>

* AWS Lambda application
  * 👀== Lambda functions + event sources + other resources / 👀 
    * work together -- to perform -- tasks
    * allows
      * making your Lambda projects portable
      * -- integrating with -- additional developer tools
        * _Example:_ AWS CodePipeline, AWS CodeBuild, AWS SAM CLI 
* AWS CloudFormation & other tools
  * can collect your application's components | 1! package / 
    * can be deployed and managed -- as -- 1 resource

* [AWS Serverless Application Repository](https://docs.aws.amazon.com/serverlessrepo/latest/devguide/)
  * == collection of Lambda applications /
    * you can deploy | your account
    * includes
      * sample ones
      * your own projects

* [AWS CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-whatis-concepts.html)
  * enables you to
    * create a template / defines your application's resources
    * manage the application -- as a -- *stack* 
      * more safely add or modify resources | your application stack 
  * create multiple environments / your application | SAME template
    * -- via -- AWS CloudFormation parameters 
  * if any part of an update fails -> AWS CloudFormation automatically rolls back to the previous configuration

* [AWS SAM](gettingstarted-tools.md#gettingstarted-tools-awssam)
  * 👀extends AWS CloudFormation 👀/
    * simplified syntax
    * focused on Lambda application development

* [AWS CLI](gettingstarted-tools.md#gettingstarted-tools-awscli) & [SAM CLI](gettingstarted-tools.md#gettingstarted-tools-samcli)
  * allows
    * managing Lambda application stacks 
  * AWS CLI -- supports -- higher-level commands
    * -> simplify tasks
      * _Example:_ uploading deployment packages & updating templates
  * AWS SAM CLI -- provides -- additional functionality
    * validating templates
    * testing locally

**Topics**
+ [Managing applications in the AWS Lambda console](applications-console.md)
+ [Creating an application with continuous delivery in the Lambda console](applications-tutorial.md)
+ [Rolling deployments for Lambda functions](lambda-rolling-deployments.md)
+ [Common Lambda application types and use cases](applications-usecases.md)
+ [Best practices for working with AWS Lambda functions](best-practices.md)