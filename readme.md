# Contact Form

A serverless contact form for a static website

# Quick Rundown 

![Diagram](https://i.imgur.com/sEDlZot.png)

The serverless solution consists of:
 - API Gateway
 - Lambda
 - DynamoDB
 - SES

The user will first submit the contact form from the static website.  API Gateway will then route the request to a lambda function, which is then sent to the provided email account in SES and saved to its DynamoDB table.

The first order of business is to create an IAM role for this serverless design, it will need admin and programmatic access. After the IAM role is established use `sls config credentials` to connect the account with serverless. 

    sls create --template aws-python

The **sls create** command will then create a standard serverless template to base the project on. 



## Serverless.yml - The Bread and Butter

The YAML file will define all of the necessary components of our serverless infrastructure. It is broken down into three main components: IAM, Lambda, and resources.  The IAM portion of the file simply gives SES and DynamoDB permission to execute and allowing functions to use these services. The functions portion is seperated into two different functions, **sendMail** and **list**.  What actually suprised me about serverless was the fact an API Gateway did not have to be configured just assumed, similar to VPC routers, but still, suprising. The resources portion will provision a DynamoDB table for us. 

## The Functions

The **sendMail** will send mail to the provided email via **POST** and simultaneously save the mail and data to DynamoDB. The **list** function will provide the user will the data stored in DynamoDB via **GET**.

## Deployment 

A simple `sls deploy` will execute your code into AWS and provide you with endpoints to test and implement your design. You can then implement your **POST /sendMail** URL endpoint into your site for deployment.
