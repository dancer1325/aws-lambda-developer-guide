# Configuring functions in the AWS Lambda console<a name="configuration-console"></a>

* AWS Lambda console
  * allows
    * configuring function settings,
    * add
      * triggers
      * destinations,
    * update
    * test your code

![\[The function designer in the AWS Lambda console.\]](http://docs.aws.amazon.com/lambda/latest/dg/images/console-designer.png)

* designer
  * see [geeting started](getting-started-create-function.md#use-the-designera-nameget-started-designera)
  * select the function node -> you can modify (== _Function settings_)
    + **Code & dependencies**
      + if you use scripting languages -> you can edit your function code | embedded [editor](code-editor.md)
      + if you add libraries OR use languages / NOT supported by the editor -> upload a [deployment package](gettingstarted-features.md#gettingstarted-features-package)
        + if your deployment package > 50 MB -> choose **Upload a file from Amazon S3**
    + **Runtime**
      + – The [Lambda runtime](lambda-runtimes.md) that executes your function\.
    + **Handler**
      + – The method that the runtime executes when your function is invoked, such as `index.handler`\. The first value is the name of the file or module\. The second value is the name of the method\.
    + **Environment variables**
      + – Key\-value pairs that Lambda sets in the execution environment\. [ Use environment variables](configuration-envvars.md) to extend your function's configuration outside of code\.
    + **Tags**
      + – Key\-value pairs that Lambda attaches to your function resource\. [Use tags](configuration-tags.md) to organize Lambda functions into groups for cost reporting and filtering in the Lambda console\.

        TODO: Tags apply to the entire function, including all versions and aliases\.
+ **Execution role** – The [IAM role](lambda-intro-execution-role.md) that AWS Lambda assumes when it executes your function\.
+ **Description** – A description of the function\.
+ **Memory**– The amount of memory available to the function during execution\. Choose an amount [between 128 MB and 3,008 MB](gettingstarted-limits.md) in 64\-MB increments\.

  Lambda allocates CPU power linearly in proportion to the amount of memory configured\. At 1,792 MB, a function has the equivalent of one full vCPU \(one vCPU\-second of credits per second\)\.
+ **Timeout** – The amount of time that Lambda allows a function to run before stopping it\. The default is 3 seconds\. The maximum allowed value is 900 seconds\.
+ **Virtual private cloud \(VPC\)** – If your function needs network access to resources that are not available over the internet, [configure it to connect to a VPC](configuration-vpc.md)\.
+ **Database proxies** – [Create a database proxy](configuration-database.md) for functions that use an Amazon RDS DB instance or cluster\.
+ **Active tracing** – Sample incoming requests and [trace sampled requests with AWS X\-Ray](services-xray.md)\.
+ **Concurrency** – [Reserve concurrency for a function](configuration-concurrency.md) to set the maximum number of simultaneous executions for a function\. Provision concurrency to ensure that a function can scale without fluctuations in latency\. 

  Reserved concurrency applies to the entire function, including all versions and aliases\.
+ **Asynchronous invocation** – [Configure error handling behavior](invocation-async.md) to reduce the number of retries that Lambda attempts, or the amount of time that unprocessed events stay queued before Lambda discards them\. [Configure a dead\-letter queue](invocation-async.md#dlq) to retain discarded events\.

  You can configure error handling settings on a function, version, or alias\.

Except as noted in the preceding list, you can only change function settings on the unpublished version of a function\. When you publish a version, code and most settings are locked to ensure a consistent experience for users of that version\. Use [aliases](configuration-aliases.md) to propagate configuration changes in a controlled manner\.

To configure functions with the Lambda API, use the following actions:
+ [UpdateFunctionCode](API_UpdateFunctionCode.md) – Update the function's code\.
+ [UpdateFunctionConfiguration](API_UpdateFunctionConfiguration.md) – Update version\-specific settings\.
+ [TagResource](API_TagResource.md) – Tag a function\.
+ [AddPermission](API_AddPermission.md) – Modify the [resource\-based policy](access-control-resource-based.md) of a function, version, or alias\.
+ [PutFunctionConcurrency](API_PutFunctionConcurrency.md) – Configure a function's reserved concurrency\.
+ [PublishVersion](API_PublishVersion.md) – Create an immutable version with the current code and configuration\.
+ [CreateAlias](API_CreateAlias.md) – Create aliases for function versions\.
+ [PutFunctionEventInvokeConfig](https://docs.aws.amazon.com/lambda/latest/dg/API_PutFunctionEventInvokeConfig.html) – Configure error handling for asynchronous invocation\.

For example, to update a function's memory setting with the AWS CLI, use the `update-function-configuration` command\.

```
$ aws lambda update-function-configuration --function-name my-function --memory-size 256
```

For function configuration best practices, see [Function configuration](best-practices.md#function-configuration)\.