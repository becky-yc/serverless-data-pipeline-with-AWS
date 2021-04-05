[![CircleCI](https://circleci.com/gh/noahgift/functional_intro_to_python.svg?style=svg&circle-token=d3ccec4d9ec6d4f1052ec528e22dc26554502cde)](https://circleci.com/gh/noahgift/awslambda)

## Hello World Build and Deploy

1.  `sam init`
2.  `sam build`
3.  `sam deploy --guided`

## Deploy and Testing

Test two ways:  

`sam local invoke`
`sam local start-api`

Then curl `curl http://127.0.0.1:3000/hello`

* API Gateway
* CloudWatch Logs
* IAM Security settings

## Build a Serverless Data Engineering Pipeline Walkthrough

- the CloudWatchEvents will call the producer function every minute. The producer function is going to extract the data from the DynamoDB. This project will extract the names of companies.
- The producer will send the data to AWS SQS.
- The SQS will invoke the consumer function. The consumer function will get the data from SQS and send the AWS Comprehend service. This service will identify the critical information for the data and get the sentiment of the data.
- The consumer function will save the results from the Comprehend service to S3.

## Diagram

![Overview](https://camo.githubusercontent.com/bb29cd924f9eb66730bbf7b0ed069a6ae03d2f1a/68747470733a2f2f757365722d696d616765732e67697468756275736572636f6e74656e742e636f6d2f35383739322f35353335343438332d62616537616638302d353437612d313165392d393930392d6135363231323531303635622e706e67)


## Takeaways

- Before creating the lambda function, it is neccessary to create an IAM role.
- I failed to build `producer` several times. It later came to me that the environment requirements in the venv should be downloaded before deploying the lambda function.
- The AWS lambda page helps to verify whether the function is successfully excuted. 
- It might be a better practice to create the static resources beforehand, including the Dynamo DB table, S3 bucket, SQS queue, and trigger settings.
