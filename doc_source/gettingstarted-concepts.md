# AWS Lambda concepts<a name="gettingstarted-concepts"></a>

* AWS Lambda's main goal
  * run functions -- to process -- events
* invoke a Lambda function
  * == send events
  * ways to invoke
    * Lambda API,
    * AWS service

**Topics**
+ [Function](#gettingstarted-concepts-function)
+ [Qualifier](#gettingstarted-concepts-qualifier)
+ [Runtime](#gettingstarted-concepts-runtimes)
+ [Event](#gettingstarted-concepts-event)
+ [Concurrency](#gettingstarted-concepts-concurrency)
+ [Trigger](#gettingstarted-concepts-trigger)

## Function<a name="gettingstarted-concepts-function"></a>

* AWS function
  * := AWS resource / you can invoke -- to run -- your code
  * 's code -- processes -- events 
  * see [Managing AWS Lambda functions](lambda-functions.md)

## Qualifier<a name="gettingstarted-concepts-qualifier"></a>

* use cases
  * invoke a function
  * view a function
* allows
  * specifying a
    * version
      * := immutable snapshot of a function's code & configuration / has a numerical qualifier
      * _Example:_ `my-function:1`
    * alias
      * := pointer to a version / 
        * can be updated
        * uses
          * split traffic between two versions
      * _Example:_ `my-function:BLUE`
* uses 
  * versions + aliases
    * provide a stable interface
      * -- for -- clients
      * -- to invoke -- your function
* see [AWS Lambda function versions](configuration-versions.md)

## Runtime<a name="gettingstarted-concepts-runtimes"></a>

* allow
  * functions | different languages -- can -- run | SAME base execution environment
  * passing requests & responses between Lambda service < -- & --> function code
* how to configure?
  * configure a runtime / matches your programming language
* available one
  * built-in by Lambda
  * build by your own
* see [AWS Lambda runtimes](lambda-runtimes.md)

## Event<a name="gettingstarted-concepts-event"></a>

* 👀== JSON formatted document / contains data -- for being processed by a -- function 👀
  * Lambda runtime
    * converts the event -- to an -- object
    * passes it -- to your -- function code
  * you determine the structure & contents of the event | invoking the function 
* _Examples:_
  * _Example1:_ custom event  

        ```
        {
          "TemperatureK": 281,
          "WindKmh": -3,
          "HumidityPct": 0.55,
          "PressureHPa": 1020
        }
        ```
  * _Example2:_ AWS SNS notification  

    ```
    {
      "Records": [
        {
          "Sns": {
            "Timestamp": "2019-01-02T12:45:07.000Z",
            "Signature": "tcc6faL2yUC6dgZdmrwh1Y4cGa/ebXEkAi6RibDsvpi+tE/1+82j...65r==",
            "MessageId": "95df01b4-ee98-5cb9-9903-4c221d41eb5e",
            "Message": "Hello from SNS!",
            ...
    ```

* see [Using AWS Lambda with other services](lambda-services.md)

## Concurrency<a name="gettingstarted-concepts-concurrency"></a>

* := number of requests / your function is serving | ANY given time
  * When your function is invoked 
    * -> Lambda provisions an instance of it
      * -- to process the -- event
    * & a request is STILL being processed -> ANOTHER instance is provisioned 
      * 👀-> function's concurrency is increased 👀 
  * When the function code finishes running -> Lambda -- can handle -- ANOTHER request
* -- depends on -- limits | region level
* available configurations
  * limit the concurrency 
* see [Managing concurrency for a Lambda function](configuration-concurrency.md)

## Trigger<a name="gettingstarted-concepts-trigger"></a>

* == AWS resource (event source mapping, application, ...) OR configuration (AWS service, ...) / invokes a Lambda function
* see
  * [Invoking AWS Lambda functions](lambda-invocation.md)
  * [Using AWS Lambda with other services](lambda-services.md)