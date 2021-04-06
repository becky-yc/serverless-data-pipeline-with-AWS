This repository aims to build a Serverless Data Engineering Pipeline. It consists of setting up AWS Lambda functions (1. Producer, 2. Consumer) using Cloud9. 

## Build a Serverless Data Engineering Pipeline Walkthrough

- the CloudWatchEvents will call the producer function every minute. The producer function is going to extract the data from the DynamoDB. This project will extract the names of companies.
- The producer will send the data to AWS SQS.
- The SQS will invoke the consumer function. The consumer function will get the data from SQS and send the AWS Comprehend service. This service will identify the critical information for the data and get the sentiment of the data.
- The consumer function will save the results from the Comprehend service to S3.

## Diagram

![Overview](https://camo.githubusercontent.com/bb29cd924f9eb66730bbf7b0ed069a6ae03d2f1a/68747470733a2f2f757365722d696d616765732e67697468756275736572636f6e74656e742e636f6d2f35383739322f35353335343438332d62616537616638302d353437612d313165392d393930392d6135363231323531303635622e706e67)

## Producer function
1. Proceed to the Cloud9 service, on the left side of the screen you should see AWS. Click on it, next to AWS Explorer, there should be a window-looking button click on it, a dropdown box will appear, select "Create Lambda Sam Application".
2. A few prompts will come out for the first one, select Python 3.7.
3. For the second prompt, choose "AWS Sam Hello World".
4. Fill in the rest of the prompts by choosing appropriate names for the project.
5. A named folder will appear in your environment, cd into the folder. There will a folder called hello_world. In this folder, there is an app.py file. Replace the content of this .py file with either producer.py.
6. In the command line enter the following commands.
```
sam build --use-container
sam deploy --guided
Fill up all the relevant naming conventions
```
7. Set up IAM access by navigating to IAM services -> Roles -> Check Allow Lambda Functions -> Check Administrator Access (for permissions). Note this is important for the lambda function to have the right access to call other AWS services.
8. Navigate to AWS Lambda, the function created through cloud9 will be deployed there. Select it -> Configuration -> Permissions -> Edit Execution Role -> Select the role created in Step 7.
9. Now we will add a trigger. Create a CloudWatch Trigger (Event Bridge) with a new rule, name it appropriately, under "Schedule Expression" type rate(1 minute). Deploy this trigger.

## Consumer Function
10. Create DynamoDB, SQS and S3 Bucket before proceeding with this step.
11. Repeat Step 1-8
12. Add a trigger, this time instead of CloudWatch, the trigger will be SQS, select the 'producer' queue.

- DynamoDB
  - Create a table named "fang". The table needs to be named this way unless you change the variables in the consumer.py file.
  - Primary Key has to be named "name" for a similar reason as before.
  - Add some company names as entries into the database.
- SQS
  - Create a queue called "producer" with all the default settings.
- S3 Bucket
  - Create a bucket called "fangsentiment-jaryl" with all the default settings. You probably should change the code in consumer.py by giving the bucket a new name as bucket names have to be unique in the region.

## Deploy and Testing

Test two ways:  

`sam local invoke`
`sam local start-api`

Then curl `curl http://127.0.0.1:3000/hello`

* API Gateway
* CloudWatch Logs
* IAM Security settings


## Takeaways

- Before creating the lambda function, it is neccessary to create an IAM role.
- I failed to build `producer` several times. It later came to me that the environment requirements in the venv should be downloaded before deploying the lambda function.
- The AWS lambda page helps to verify whether the function is successfully excuted. 
- It might be a better practice to create the static resources beforehand, including the Dynamo DB table, S3 bucket, SQS queue, and trigger settings.
