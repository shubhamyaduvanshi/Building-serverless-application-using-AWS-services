# Building-serverless-application-using-AWS-services
Serverless application
<!--  -->
# Introduction
In this AWS hands-on lab, we will create a fully working serverless reminder application using S3, Lambda, API Gateway, Step Functions, Simple Email Service, and Simple Notification Service.
# Solution
Log in to the live AWS environment using the credentials provided. Make sure you're in the N. Virginia (us-east-1) region throughout the lab.
# Lambda fn Creation
In the AWS Management Console, navigate to Lambda.
1. Click Create function.
    With Author from scratch selected, set the following values:
2.Function name: email
Runtime: Python 3.8
3.Click Change default execution role.
4.Select Use an existing role, and pick the LambdaRuntimeRole from the dropdown.
5.Click Create function.
6.Scroll down to Code source and double-click lambda_function.py to display the function code.
7.Delete the provided code.
8.In a new browser tab, open the GitHub repo for this lab.
9.Click to open the email_reminder.py file.
10.Click Raw above the code to display the raw function code.
11.Copy the code.
12. Return to the AWS Lambda console, and paste the copied code in to lambda_function.py. Keep this tab open for later
