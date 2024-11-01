# Create a Lambda function with the console<a name="getting-started-create-function"></a>

* steps
  * create a Lambda function -- via -- AWS Lambda console
  * manually invoke the Lambda function -- via -- sample event data
    * AWS Lambda 
      * executes the Lambda function
      * returns results
  * verify execution results / checking
    * logs / created by your Lambda function
    * CloudWatch metrics 

**To create a Lambda function**

1. Open the [AWS Lambda console](https://console.aws.amazon.com/lambda/home)
2. Choose **Create a function**\.
3. For **Function name**, enter **my-function**\.
4. Choose **Create function**\.
   1. by default,
      1. Node\.js function
      2. execution role / grants the function permission -- to -- upload logs
         1. Lambda -- assumes the -- execution role | you invoke your function
         2. uses
            1. create credentials -- for --
               1. the AWS SDK
               2. reading data from event sources

## Use the designer<a name="get-started-designer"></a>

* shows an overview of your function & its upstream and downstream resources
* uses
  * configure
    * triggers,
      * == services & resources /
        * can be configured
        * invoke your function 
      * see 
        * [Lambda event source mapping](invocation-eventsourcemapping.md)
        * [Using AWS Lambda with other services](lambda-services.md)
    * [layers](configuration-layers.md),
    * destinations
      * uses
        * send details about invocation results -- to -- ANOTHER service
        * if your function is invoked [asynchronously](invocation-async.md), or by an [event source mapping](invocation-eventsourcemapping.md) / reads from a stream -> send invocation records 

![\[A Lambda function with an Amazon S3 trigger and Amazon EventBridge destination.\]](http://docs.aws.amazon.com/lambda/latest/dg/images/console-designer.png)

* [AWS Cloud9](https://docs.aws.amazon.com/cloud9/latest/user-guide/)
  * == embedded editor
  * allows
    * editing your function code / source code <= 3 MB

## Invoke the Lambda function<a name="get-started-invoke-manually"></a>

* invoke your Lambda function -- via the -- sample event data / provided | console
  * choose **Test**
  * choose **Create new test event** | **Configure test event** page
    * leave the default **Hello World** option | **Event template**

        ```
        {
          "key3": "value3",
          "key2": "value2",
          "key1": "value1"
        }
        ```

* **Test**
  * each user can create <= 10 test events / function
    * test events are NOT available | other users
  * `handler` | your Lambda function
    * receives & processes the sample event 
* **Monitoring**
  * -- via -- CloudWatch
  * see [Monitoring functions in the AWS Lambda console](monitoring-functions-access-metrics.md) 
  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/lambda/latest/dg/images/metrics-functions-list.png)

## Clean up<a name="gettingstarted-cleanup"></a>

* entities to delete
  * lambda function
  * execution role
    * open the IAM
      * TODO: still active ❓
  * log group
    * open the [CloudWatch console | log groups panel](https://console.aws.amazon.com/cloudwatch/home#logs:)
* if you want to automate the creation & cleanup of functions, roles, and log groups -> use
  * AWS CloudFormation
  * AWS CLI
* see [Lambda sample applications](lambda-samples.md)