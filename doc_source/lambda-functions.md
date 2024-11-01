# Managing AWS Lambda functions<a name="lambda-functions"></a>

* settings
  * ways to configure settings | your Lambda functions
    * AWS Lambda API
    * AWS console
  * [Basic function settings](configuration-console.md)
    * description,
    * role,
    * runtime / you specify | you create a function | Lambda console
  * other settings
    * handler name,
    * memory allocation,
    * security groups

* way to keep secrets -- out of -- your function code
  * store them | function's configuration
  * read them -- from the -- execution [Environment variables](configuration-envvars.md) | initialization
    * ALWAYS encrypted | rest,
    * can be encrypted | client-side
    * uses
      * make your function code portable -- by -- removing
        * connection strings,
        * passwords,
        * endpoints | external resources

* [Versions and aliases](configuration-versions.md)
  * == secondary resources / you can create to manage
    * function deployment
    * invocation
  * [versions](configuration-versions.md) of your function
    * store separately, its
      * code
      * configuration
  * [alias](configuration-aliases.md)
    * -- can point to a -- specific version
    * allows
      * configuring your clients easily

* [layers](configuration-layers.md)
  * allows
    * managing your function's dependencies -- independently of the -- functions 
      * -> 👀keep your deployment package small 👀
    * sharing your own libraries
    * using publicly available layers

* if you want to use your Lambda function -- with -- AWS resources | Amazon VPC
  * [create a VPC connection](configuration-vpc.md)
    * configure it with
      * security groups
      * subnets
  * resources | private subnet / Lambda function has got access
    * relational databases
    * caches
  * [create a database proxy](configuration-database.md) -- for -- DB instances
    * MySQL 
    * Aurora

**Topics**
+ [Configuring functions in the AWS Lambda console](configuration-console.md)
+ [Using AWS Lambda environment variables](configuration-envvars.md)
+ [Managing concurrency for a Lambda function](configuration-concurrency.md)
+ [AWS Lambda function versions](configuration-versions.md)
+ [AWS Lambda function aliases](configuration-aliases.md)
+ [AWS Lambda layers](configuration-layers.md)
+ [Configuring a Lambda function to access resources in a VPC](configuration-vpc.md)
+ [Configuring database access for a Lambda function](configuration-database.md)
+ [Configuring file system access for Lambda functions](configuration-filesystem.md)
+ [Tagging Lambda Functions](configuration-tags.md)