# AWS Lambda

#AWS/SolutionsArchitect

- It is a **serverless** compute service that allows you to run you application code without having to manage EC2 instances
- You only ever pay for compute power when your Lambda Functions are running to the closest millisecond
- In addition to compute power you are also charged based on the number of times your code runs

### Using Lambda

- You can either upload your code to Lambda, or write it within the code editors that Lambda provides
- Configure your Lambda functions to execute upon specific triggers from supported event sources
- Once the specific trigger is initiated, Lambda will run your code (as per your Lambda function) using only the required compute power as defined.
- AWS records the compute time in Milliseconds and the quantity of Lambda functions run to the ascertain the cost of the service
- AWS Lambda can be found within the AWS Management Console under the Compute category

### Components of AWS Lambda

- A **Lambda function** is compiled of your own code that you want Lambda to invoke
- **Events sources** are AWS services that can be used to trigger your Lambda functions
- **Downstream resources** are resources that are required during the execution of your Lambda Function
- **Log streams** help to identify issues and troubleshoot issues with your Lambda Function

---

### _Lambda Functions_

---

#### Language Support

- Node.js
- Java
- C#
- Python
- Go
- Powershell
- Ruby

#### Importing Code

- You can import code into Lambda by creating a deployment package
- Lambda will need global read permissions to your deployment package to perform the import function
- You can upload code using the Management Console, AWS CLI or SDK
- If you created your code from within Lamb da itself, then Lambda would create the deployment package for you
- Options to create a function
  - Author from scratch
  - Blueprints
  - AWS Serverless Application Repository
- Name, Runtime, and IAM Role must be provided to create a function

#### Designer Window

- Configure triggers
  - a trigger is an operation from an event source
- Role execution policies determine what resources the function role has access to when the function is being run
- The function policy defines which AWS resources are allowed to invoke your function

#### Function Code Window

- Allows you to define, write, and import your code
- The **handler** within your function allows Lambda to invoke it when the service executes the function on your behalf, and it’s used as the entry point within your code to execute your function
- **Environment variables** are key value pairs that allow you to incorporate variables into your function without embedding them thoroughly into your code
- Basic settings allows you to determine the compute resource that you want to use to execute your code, and you can only alter the amount of memory used. Lambda then calculates the CPU power itself, based off of this selection

#### Network

- AWS Lambda is only allowed to access resources that are accessible over the internet. To access resources within your VPC requires additional configuration
- The execution role will need permissions to configure ENIs in your VPC

#### Debugging

- A dead-letter queue is used to receive payloads that were not processed due to a failed execution
- Failed **asynchronous** functions would automatically retry the event a further two more times.
- **Synchronous** invocations do not automatically retry failed attempts
- Enable active tracing is used to integrate **AWS X-Ray** to trace event sources that invoked your Lambda function, in addition to tracing other resources that were called upon in response to your Lambda function running

#### Concurrency

- Concurrency measures how many functions can be running at the same time, with a default unreserved concurrency set to 1,000

#### Auditing and Compliance

- **AWS CloudTrail** integrates with AWS Lambda, aiding with auditing and compliance

#### Toolbar Actions

- **Throttling** sets the reserved concurrency limit of your function to zero, and will stop all future invocations of the function until you change the concurrency setting
- Lambda **qualifiers** allow you to change between versions of an alias of your function, and when you create a new version of your function, you're not able to make any further configuration changes, making it immutable
- An alias allows you to create a pointer to a specific version of your function
- By exporting your function, you can redeploy at a later stage, perhaps within a different AWS region
- By creating a test event, you can easily perform different tests against your function

---

### _Event Sources_

---

- An event source is an AWS service that produces the events that your Lambda function responds to by invoking it
- Event sources can either be poll or push-based
  - poll-based
    - Amazon Kinesis
    - Amazon SQS
    - DynamoDB
  - _push-based event sources cover all the remaining supported event sources_

#### Event Source Mappings

- push-based services
  - The mapping is maintained within the Event source
  - By using appropriate API calls for the event source service, you are able to create and configure the relevant mappings
  - This will require specific access to allow your event source to invoke the function
- poll-based services
  - The configuration of the mappings are held within your lambda function
  - With the **CreateEventSourceMapping** API you can set up the relevant event source mapping for your poll-based service
  - The permission is required in the Execution role policy

#### Synchronous vs Asynchronous Invocations

- When you manually invoke a lambda function, you have the ability to use the 'invoke' option which allows you to specify if the function should be invoked synchronously or asynchronously
- **Synchronous** invocation enables you to assess the result of the function before moving onto the next operation required
- **Asynchronous** invocations can be used when there is no need to maintain an order of function execution
- When event sources are used to call and invoke your function the invocation type is dependent on the service:
  - For poll-based event sources, the invocation type is always synchronous
  - For push-based event sources it varies on the service

---

### _Monitoring & Troubleshooting AWS Lambda_

---

- Statistics related to your Lambda function are by default monitored by **Amazon CloudWatch**
- CloudWatch Metrics
  - invocations
  - errors
  - deadLetterErrors
  - duration
  - throttles
  - iteratorAge
  - concurrentExecutions
  - unreservedConcurrentExecutions
- In addition to these metrics, CloudWatch also gathers log data sent by Lambda
  - For each function that you have running, CloudWatch will create a different log group
  - The log group name is defined as '**/aws/lamdba/[functionName]**'
  - It's possible to add custom logging statements into your function code, which are then sent to CloudWatch logs

#### Common Errors Experienced with Lambda

- Some of the common issues as to why your functions might not run relate to permissions
- Check your IAM Role execution policy and function policies to ensure the correct access has been issued to run your function
