# What is AWS Lambda?<a name="welcome"></a>

 * AWS Lambda
   * := compute service /
     * your code is 
       * run WITHOUT
         * provisioning servers or
         * managing servers
           * _Example:_ OS maintenance, provision capacity (memory, CPU), network, apply security patches, ...
       * executed ONLY when it's needed
       * scaled automatically
         * [few requests / day, thousands / second]
       * monitored and logged
     * 👀pay for the compute time / you consume 👀
       * NO charge | your code is NOT running
     * highly available service
       * see [AWS Lambda service level agreement](https://aws.amazon.com/lambda/sla/)
   * how to set up?
     * supply your code | one of the [supported languages of AWS Lambda](lambda-runtimes.md) 
   * use cases
     * run your code -- in response to --
       * events (_Example:_ changes to data | Amazon S3 bucket or Amazon DynamoDB table)
       * HTTP requests -- via -- Amazon API Gateway
     * invoke your code -- via -- API calls / made using AWS SDKs
   * uses
     * build data processing triggers -- for -- AWS services (_Example:_ Amazon S3, Amazon DynamoDB)
     * process streaming data / stored | Kinesis
     * create your own back end / operates | AWS scale, performance, and security
     * build serverless applications / == functions /
       * triggered -- by -- events
       * automatically deploy them -- via --
         * CodePipeline
         * AWS CodeBuild
       * see [AWS Lambda applications](deploying-lambda-apps.md)

## When should I use AWS Lambda?<a name="when-to-use-cloud-functions"></a>

* | many application scenarios
* 👀you are responsible ONLY for your code 👀
  * -> you can NOT
    * log | compute instances
    * customize the OS
* 👀if you need to manage your own compute resources -> use other AWS compute services 👀
  + Amazon Elastic Compute Cloud \(Amazon EC2\)
    + lets you
      + flexibility & wide range of EC2 instance types
      + customize
        + OS,
        + network & security settings,
        + entire software stack
    + you are -- responsible for -- 
      + provisioning capacity,
      + monitoring fleet health & performance,
      + using Availability Zones / fault tolerance
  + Elastic Beanstalk
    + easy-to-use service -- for -- deploying & scaling applications | Amazon EC2 / you retain
      + ownership
      + full control | EC2 instances

## Are you a first\-time user of AWS Lambda?<a name="welcome-first-time-user"></a>

1. **Read the product overview & watch the introductory video to understand sample use cases\.**
   1. check [AWS Lambda webpage](https://aws.amazon.com/lambda/)
2. **Try the console-based getting started exercise\.**
   1. instructions to create & test your first Lambda function -- via -- AWS console
   2. programming model & other Lambda concepts 
   3. see [Getting started with AWS Lambda](getting-started.md)
3. **Read the [deploying applications with AWS Lambda](deploying-lambda-apps.md) section of this guide**
   1. AWS Lambda components / you work with
   2. end-to-end experience

* see [Using AWS Lambda with other services](lambda-services.md)