# Using AWS Lambda -- via -- AWS CLI <a name="gettingstarted-awscli"></a>

* goal here
  * manage & invoke Lambda functions -- via -- AWS CLI

* AWS CLI
  * 👀interact -- via, AWS SDK for Python -- with the Lambda API 👀

## Prerequisites<a name="with-userapp-walkthrough-custom-events-deploy"></a>

* syntax structure of the next commands / added here

    ```
    ~/lambda-project$ this is a command
    this is output
    ```

* | Windows 10
  * recommended to [install the Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/install-win10)
* [install the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html)

## Create the execution role<a name="with-userapp-walkthrough-custom-events-create-iam-role"></a>

* `aws iam create-role `
  * create the [execution role](lambda-intro-execution-role.md)
  * [trust policy](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html) -- for the -- role
    * ways to specify
      * `file://pathToTheFile`
        * == -- via -- file
      * inline 
  * _Example1:_ specify trust policy -- via -- `file`

      ```
      $ aws iam create-role --role-name lambda-ex --assume-role-policy-document file://trust-policy.json
      {
          "Role": {
              "Path": "/",
              "RoleName": "lambda-ex",
              "RoleId": "AROAQFOXMPL6TZ6ITKWND",
              "Arn": "arn:aws:iam::123456789012:role/lambda-ex",
              "CreateDate": "2020-01-17T23:19:12Z",
              "AssumeRolePolicyDocument": {
                  "Version": "2012-10-17",
                  "Statement": [
                      {
                          "Effect": "Allow",
                          "Principal": {
                              "Service": "lambda.amazonaws.com"
                          },
                          "Action": "sts:AssumeRole"
                      }
                  ]
              }
          }
      }
      ```

      * `trust-policy.json`
        * _Example:_ 

          ```
          {
            "Version": "2012-10-17",
            "Statement": [
              {
                "Effect": "Allow",
                "Principal": {
                  "Service": "lambda.amazonaws.com"
                },
                "Action": "sts:AssumeRole"
              }
            ]
          }
          ```
  * _Example2:_ specify trust policy -- via -- inline
      ```
       $ aws iam create-role --role-name lambda-ex --assume-role-policy-document '{"Version": "2012-10-17","Statement": [{ "Effect": "Allow", "Principal": {"Service": "lambda.amazonaws.com"}, "Action": "sts:AssumeRole"}]}'
      ```

* `aws iam attach-policy-to-role`
  * add permissions | role
  * _Example:_
    ```
    $ aws iam attach-role-policy --role-name lambda-ex --policy-arn arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole
    ```

## Create the function<a name="with-userapp-walkthrough-custom-events-upload"></a>

* TODO:
The following example logs the values of environment variables and the event object\.

**Example index\.js**  

```
exports.handler = async function(event, context) {
  console.log("ENVIRONMENT VARIABLES\n" + JSON.stringify(process.env, null, 2))
  console.log("EVENT\n" + JSON.stringify(event, null, 2))
  return context.logStreamName
}
```

**To create the function**

1. Copy the sample code into a file named `index.js`\.

1. Create a deployment package\.

   ```
   $ zip function.zip index.js
   ```

1. Create a Lambda function with the `create-function` command\. Replace the highlighted text in the role ARN with your account ID\.

   ```
   $ aws lambda create-function --function-name my-function \
   --zip-file fileb://function.zip --handler index.handler --runtime nodejs12.x \
   --role arn:aws:iam::123456789012:role/lambda-ex
   {
       "FunctionName": "my-function",
       "FunctionArn": "arn:aws:lambda:us-east-2:123456789012:function:my-function",
       "Runtime": "nodejs12.x",
       "Role": "arn:aws:iam::123456789012:role/lambda-ex",
       "Handler": "index.handler",
       "CodeSha256": "FpFMvUhayLkOoVBpNuNiIVML/tuGv2iJQ7t0yWVTU8c=",
       "Version": "$LATEST",
       "TracingConfig": {
           "Mode": "PassThrough"
       },
       "RevisionId": "88ebe1e1-bfdf-4dc3-84de-3017268fa1ff",
       ...
   }
   ```

To get logs for an invocation from the command line, use the `--log-type` option\. The response includes a `LogResult` field that contains up to 4 KB of base64\-encoded logs from the invocation\.

```
$ aws lambda invoke --function-name my-function out --log-type Tail
{
    "StatusCode": 200,
    "LogResult": "U1RBUlQgUmVxdWVzdElkOiA4N2QwNDRiOC1mMTU0LTExZTgtOGNkYS0yOTc0YzVlNGZiMjEgVmVyc2lvb...",
    "ExecutedVersion": "$LATEST"
}
```

You can use the `base64` utility to decode the logs\.

```
$ aws lambda invoke --function-name my-function out --log-type Tail \
--query 'LogResult' --output text |  base64 -d
START RequestId: 57f231fb-1730-4395-85cb-4f71bd2b87b8 Version: $LATEST
  "AWS_SESSION_TOKEN": "AgoJb3JpZ2luX2VjELj...", "_X_AMZN_TRACE_ID": "Root=1-5d02e5ca-f5792818b6fe8368e5b51d50;Parent=191db58857df8395;Sampled=0"",ask/lib:/opt/lib",
END RequestId: 57f231fb-1730-4395-85cb-4f71bd2b87b8
REPORT RequestId: 57f231fb-1730-4395-85cb-4f71bd2b87b8  Duration: 79.67 ms      Billed Duration: 100 ms         Memory Size: 128 MB     Max Memory Used: 73 MB
```

The `base64` utility is available on Linux, macOS, and [Ubuntu on Windows](https://docs.microsoft.com/en-us/windows/wsl/install-win10)\. For macOS, the command is `base64 -D`\.

To get full log events from the command line, you can include the log stream name in the output of your function, as shown in the preceding example\. The following example script invokes a function named `my-function` and downloads the last five log events\.

**Example get\-logs\.sh Script**  
This example requires that `my-function` returns a log stream ID\.  

```
#!/bin/bash
aws lambda invoke --function-name my-function --payload '{"key": "value"}' out
sed -i'' -e 's/"//g' out
sleep 15
aws logs get-log-events --log-group-name /aws/lambda/my-function --log-stream-name $(cat out) --limit 5
```

The script uses `sed` to remove quotes from the output file, and sleeps for 15 seconds to allow time for the logs to be available\. The output includes the response from Lambda and the output from the `get-log-events` command\.

```
$ ./get-logs.sh
{
    "StatusCode": 200,
    "ExecutedVersion": "$LATEST"
}
{
    "events": [
        {
            "timestamp": 1559763003171,
            "message": "START RequestId: 4ce9340a-b765-490f-ad8a-02ab3415e2bf Version: $LATEST\n",
            "ingestionTime": 1559763003309
        },
        {
            "timestamp": 1559763003173,
            "message": "2019-06-05T19:30:03.173Z\t4ce9340a-b765-490f-ad8a-02ab3415e2bf\tINFO\tENVIRONMENT VARIABLES\r{\r  \"AWS_LAMBDA_FUNCTION_VERSION\": \"$LATEST\",\r ...",
            "ingestionTime": 1559763018353
        },
        {
            "timestamp": 1559763003173,
            "message": "2019-06-05T19:30:03.173Z\t4ce9340a-b765-490f-ad8a-02ab3415e2bf\tINFO\tEVENT\r{\r  \"key\": \"value\"\r}\n",
            "ingestionTime": 1559763018353
        },
        {
            "timestamp": 1559763003218,
            "message": "END RequestId: 4ce9340a-b765-490f-ad8a-02ab3415e2bf\n",
            "ingestionTime": 1559763018353
        },
        {
            "timestamp": 1559763003218,
            "message": "REPORT RequestId: 4ce9340a-b765-490f-ad8a-02ab3415e2bf\tDuration: 26.73 ms\tBilled Duration: 100 ms \tMemory Size: 128 MB\tMax Memory Used: 75 MB\t\n",
            "ingestionTime": 1559763018353
        }
    ],
    "nextForwardToken": "f/34783877304859518393868359594929986069206639495374241795",
    "nextBackwardToken": "b/34783877303811383369537420289090800615709599058929582080"
}
```

## List the Lambda functions in your account<a name="with-userapp-walkthrough-custom-events-list-functions"></a>

Execute the following AWS CLI `list-functions` command to retrieve a list of functions that you have created\. 

```
$ aws lambda list-functions --max-items 10
{
    "Functions": [
        {
            "FunctionName": "cli",
            "FunctionArn": "arn:aws:lambda:us-east-2:123456789012:function:my-function",
            "Runtime": "nodejs12.x",
            "Role": "arn:aws:iam::123456789012:role/lambda-ex",
            "Handler": "index.handler",
            ...
        },
        {
            "FunctionName": "random-error",
            "FunctionArn": "arn:aws:lambda:us-east-2:123456789012:function:random-error",
            "Runtime": "nodejs12.x",
            "Role": "arn:aws:iam::123456789012:role/lambda-role",
            "Handler": "index.handler",
            ...
        },
        ...
      ],
      "NextToken": "eyJNYXJrZXIiOiBudWxsLCAiYm90b190cnVuY2F0ZV9hbW91bnQiOiAxMH0="
}
```

In response, Lambda returns a list of up to 10 functions\. If there are more functions you can retrieve, `NextToken` provides a marker you can use in the next `list-functions` request\. The following `list-functions` AWS CLI command is an example that shows the `--starting-token` parameter\.

```
$ aws lambda list-functions --max-items 10 --starting-token eyJNYXJrZXIiOiBudWxsLCAiYm90b190cnVuY2F0ZV9hbW91bnQiOiAxMH0=
```

## Retrieve a Lambda function<a name="with-userapp-walkthrough-custom-events-get-configuration"></a>

The Lambda CLI `get-function` command returns Lambda function metadata and a presigned URL that you can use to download the function's deployment package\.

```
$ aws lambda get-function --function-name my-function
{
    "Configuration": {
        "FunctionName": "my-function",
        "FunctionArn": "arn:aws:lambda:us-east-2:123456789012:function:my-function",
        "Runtime": "nodejs12.x",
        "Role": "arn:aws:iam::123456789012:role/lambda-ex",
        "CodeSha256": "FpFMvUhayLkOoVBpNuNiIVML/tuGv2iJQ7t0yWVTU8c=",
        "Version": "$LATEST",
        "TracingConfig": {
            "Mode": "PassThrough"
        },
        "RevisionId": "88ebe1e1-bfdf-4dc3-84de-3017268fa1ff",
        ...
    },
    "Code": {
        "RepositoryType": "S3",
        "Location": "https://awslambda-us-east-2-tasks.s3.us-east-2.amazonaws.com/snapshots/123456789012/my-function-4203078a-b7c9-4f35-..."
    }
}
```

For more information, see [GetFunction](API_GetFunction.md)\.

## Clean up<a name="with-userapp-walkthrough-custom-events-delete-function"></a>

Execute the following `delete-function` command to delete the `my-function` function\.

```
$ aws lambda delete-function --function-name my-function
```

Delete the IAM role you created in the IAM console\. For information about deleting a role, see [Deleting roles or instance profiles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_manage_delete.html) in the *IAM User Guide*\. 