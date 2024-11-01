# AWS Lambda event source mappings<a name="invocation-eventsourcemapping"></a>

* event source mapping
  * := AWS Lambda resource /
    * reads -- from an -- event source
      * event source -- can be --
        * stream
        * queue | batches
          * batch size maximum -- depends on the -- AWS service
          * scenarios / #ofItems < batch size
            * NOT enough items available or
            * batch is too large -- to send in -- 1 event -> has to be split up
    * invokes a Lambda function
  * uses
    * process items -- from a -- stream or queue | services / -- NOT invoke directly -- Lambda functions 
  * AWS services / Lambda -- provides -- event source mappings
    + [Amazon Kinesis](with-kinesis.md)
    + [Amazon DynamoDB](with-ddb.md)
    + [Amazon Simple Queue Service](with-sqs.md)
  * how does it work?
    * function's [execution role](lambda-intro-execution-role.md) 's permissions -- are mapped to --
      * read items | event source
      * manage items | event source
  * Permissions, event structure, settings, and polling behavior -- vary by -- event source
  * if you want to manage event source mappings -- via -- AWS CLI or AWS SDK -> use the following API actions
    + [CreateEventSourceMapping](API_CreateEventSourceMapping.md)
    + [ListEventSourceMappings](API_ListEventSourceMappings.md)
    + [GetEventSourceMapping](API_GetEventSourceMapping.md)
    + [UpdateEventSourceMapping](API_UpdateEventSourceMapping.md)
    + [DeleteEventSourceMapping](API_DeleteEventSourceMapping.md)
* _Example1:_ map a Lambda function -- via AWS CLI, to a -- DynamoDB stream / -- specified by -- its ARN
    ```
    $ aws lambda create-event-source-mapping --function-name my-function --batch-size 500 --starting-position LATEST \
    --event-source-arn arn:aws:dynamodb:us-east-2:123456789012:table/my-table/stream/2019-06-10T19:26:16.525
    {
        "UUID": "14e0db71-5d35-4eb5-b481-8945cf9d10c2",
        "BatchSize": 500,
        "MaximumBatchingWindowInSeconds": 0,
        "ParallelizationFactor": 1,
        "EventSourceArn": "arn:aws:dynamodb:us-east-2:123456789012:table/my-table/stream/2019-06-10T19:26:16.525",
        "FunctionArn": "arn:aws:lambda:us-east-2:123456789012:function:my-function",
        "LastModified": 1560209851.963,
        "LastProcessingResult": "No records processed",
        "State": "Creating",
        "StateTransitionReason": "User action",
        "DestinationConfig": {},
        "MaximumRecordAgeInSeconds": 604800,
        "BisectBatchOnFunctionError": false,
        "MaximumRetryAttempts": 10000
    }
    ```
* _Example2:_ event source mapping / 
  * -- reads from a -- Kinesis stream
  * if a batch of events fails ALL processing attempts -> the event source mapping -- sends details about the batch to an -- SQS queue

    ![\[\]](http://docs.aws.amazon.com/lambda/latest/dg/images/features-eventsourcemapping.png)

* TODO:
The event batch is the event that Lambda sends to the function\. It is a batch of records or messages compiled from the items that the event source mapping reads from a stream or queue\. Batch size and other settings only apply to the event batch\.

For streams, an event source mapping creates an iterator for each shard in the stream and processes items in each shard in order\. You can configure the event source mapping to read only new items that appear in the stream, or to start with older items\. Processed items aren't removed from the stream and can be processed by other functions or consumers\.

By default, if your function returns an error, the entire batch is reprocessed until the function succeeds, or the items in the batch expire\. To ensure in\-order processing, processing for the affected shard is paused until the error is resolved\. You can configure the event source mapping to discard old events, restrict the number of retries, or process multiple batches in parallel\. If you process multiple batches in parallel, in\-order processing is still guaranteed for each partition key, but multiple partition keys in the same shard are processed simultaneously\.

You can also configure the event source mapping to send an invocation record to another service when it discards an event batch\. Lambda supports the following [destinations](invocation-async.md#invocation-async-destinations) for event source mappings\.
+ **Amazon SQS** – An SQS queue\.
+ **Amazon SNS** – An SNS topic\.

The invocation record contains details about the failed event batch in JSON format\.

The following example shows an invocation record for a Kinesis stream\.

**Example invocation Record**  

```
{
    "requestContext": {
        "requestId": "c9b8fa9f-5a7f-xmpl-af9c-0c604cde93a5",
        "functionArn": "arn:aws:lambda:us-east-2:123456789012:function:myfunction",
        "condition": "RetryAttemptsExhausted",
        "approximateInvokeCount": 1
    },
    "responseContext": {
        "statusCode": 200,
        "executedVersion": "$LATEST",
        "functionError": "Unhandled"
    },
    "version": "1.0",
    "timestamp": "2019-11-14T00:38:06.021Z",
    "KinesisBatchInfo": {
        "shardId": "shardId-000000000001",
        "startSequenceNumber": "49601189658422359378836298521827638475320189012309704722",
        "endSequenceNumber": "49601189658422359378836298522902373528957594348623495186",
        "approximateArrivalOfFirstRecord": "2019-11-14T00:38:04.835Z",
        "approximateArrivalOfLastRecord": "2019-11-14T00:38:05.580Z",
        "batchSize": 500,
        "streamArn": "arn:aws:kinesis:us-east-2:123456789012:stream/mystream"
    }
}
```

Lambda also supports in\-order processing for [FIFO \(first\-in, first\-out\) queues](with-sqs.md), scaling up to the number of active message groups\. For standard queues, items aren't necessarily processed in order\. Lambda scales up to process a standard queue as quickly as possible\. When an error occurs, batches are returned to the queue as individual items and might be processed in a different grouping than the original batch\. Occasionally, the event source mapping might receive the same item from the queue twice, even if no function error occurred\. Lambda deletes items from the queue after they're processed successfully\. You can configure the source queue to send items to a dead\-letter queue if they can't be processed\.

* services / -- invoke directly -- Lambda functions
  * see [Using AWS Lambda with other services](lambda-services.md)