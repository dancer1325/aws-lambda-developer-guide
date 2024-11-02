# Invoking AWS Lambda functions<a name="lambda-invocation"></a>

* ways to invoke Lambda functions
  * direct or indirect
    * directly -- via --
      * [Lambda console](getting-started-create-function.md#get-started-invoke-manually),
      * Lambda API,
      * AWS SDK,
      * AWS CLI,
      * AWS toolkits
    * indirectly -- via --
      * [other AWS services](lambda-services.md)
      * configure Lambda / 
        * read -- from a -- stream OR queue &
        * afterward, invoke the function
  * sync or async
    * [synchronous invocation](invocation-sync.md)
      * == wait for the function -- to --
        * process the event &
        * return a response
    * [asynchronous](invocation-async.md) invocation,
      * Lambda
        * steps
          * queues the event -- for -- processing
          * returns a response immediately 
        * can handle ALSO
          * retries
          * send invocation records -- to a -- [destination](invocation-async.md#invocation-async-destinations)

* requirements
  * add >=1 triggers

* scaling behavior & the types of errors (-> retry)
  * depends on
    * who invokes your function
    * how it's invoked
      * for async -> can vary
  * see 
    * [AWS Lambda function scaling](invocation-scaling.md)
    * [Error handling and automatic retries in AWS Lambda](invocation-retries.md) 

**Topics**
+ [Synchronous invocation](invocation-sync.md)
+ [Asynchronous invocation](invocation-async.md)
+ [AWS Lambda event source mappings](invocation-eventsourcemapping.md)
+ [Monitoring the state of a function with the Lambda API](functions-states.md)
+ [AWS Lambda function scaling](invocation-scaling.md)
+ [Error handling and automatic retries in AWS Lambda](invocation-retries.md)
+ [Invoking Lambda functions with the AWS Mobile SDK for Android](with-on-demand-custom-android.md)