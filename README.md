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

# Verify an Email Address in Simple Email Service (SES)
Open a new browser tab, and navigate to Simple Email Service.
On the left-side menu, under Configuration, go to Verified identities, and click Create identity.
Under Identity details, select Email address.
Enter your email address, and click Create identity.
In a new browser tab or email client, navigate to your email, open the SES verification email, and click the provided link. Check your spam mail if you do not see it in your inbox.
Go back to Lambda.
# updating lambda
Within the Lambda function, delete the YOUR_SES_VERIFIED_EMAIL placeholder, and type in the following replacing YOUR_EMAIL with your verified email address:

VERIFIED_EMAIL = 'YOUR_EMAIL'
Click Deploy.

In Function Overview at the top, copy the Function ARN, and paste it into a text file for later use.

# Create the sms Lambda Function
Click Create function.
Scroll down to Code source, and double-click lambda_function.py to display the function code.
Delete the provided code.
In a new browser tab, open the GitHub repo for this lab.
Click the sms_reminder.py file.
Click Raw to display the raw function code.
Copy the code.
Return to the AWS Lambda console, and paste the copied code into lambda_function.py.
Click Deploy.
In Function Overview at the top, copy the Function ARN, and paste it into a text file for later use.

# Create the api_handler Lambda Function
 follow same steps as abobe one for sns lambda function
# Create a Step Function State Machine

In a new browser tab, navigate to the Step Functions.


Click Get started to run the Hello World example.

In the line under the Review Hello World example title, click here to access more functionality. (Click Leave if prompted with a warning, and then click here again.)

Select Write your workflow in code.

Under Type, select Standard.

Return to the GitHub repo, and click the step-function-template.json file.

Click Raw, and copy the code.

Return to Step Functions, and under Definition, delete the current contents and replace it with your copied code. Ensure all values you add in the next 2 steps remain 

wrapped in double quotes.

On lines 34 and 52, replace the Resource placeholder value with the copied ARN for email.

On lines 40 and 62, replace the Resource placeholder value with the copied ARN for sms.

Click the refresh icon to the right of the code to view the updated function diagram.

Click Next.

Under Permissions, select Choose an existing role.

Under Existing roles, select RoleForStepFunction.

Scroll to the bottom, and click Create state machine.

Under Details, copy the ARN.

Return to the Lambda api_handler function.

Scroll down to Code source and double-click lambda_function.py.

On line 6, replace the SFN_ARN placeholder value with the ARN you just copied.

Click Deploy.

# CReate API gateway

#1. Create the API Gateway
#2.In the AWS Management Console, navigate to API Gateway.
#3Under Choose an API type, go to REST API (not Private), and then select Build.
#4Under Choose the protocol, select REST.
#5Under Create new API, select New API.
#6Under Settings, set the following values:
#7API name: reminders
#8Endpoint Type: Regional
#9Click Create API.
#10Click Actions > Create Resource.
#11In Resource Name, enter reminders.
#12Select Enable API Gateway CORS.
#13Click Create Resource.
#14With /reminders selected, click Actions > Create Method.
#15In the new dropdown under /reminders, select POST and click the adjacent checkmark icon.
#16In /reminders - POST - Setup, set the following values:
#17Integration type: Lambda Function
#18Use Lambda Proxy integration: Selected
#19Lambda Region: us-east-1
#20Lambda Function: api_handler
#21Click Save.
#22Click OK.
#23Click Actions > Deploy API.
#24Set the following values:
Deployment stage: New Stage
Stage name: prod
Click Deploy.
Note: You may ignore any Web Application Firewall (WAF) permissions warning messages received after deployment.

At the top of the page, under prod Stage Editor, copy the Invoke URL for later use.

# create Static S3 websites
Return to the GitHub repo, and download a local copy of the static_website folder with its file contents. You can go to the static_website folder and download a copy of each file, or you can go to the main repository, click Code, and select Download ZIP to get all of the files in the repo.

In your local static_website folder, open the formlogic.js file.

On line 5, delete the API_ENDPOINT placeholder text, and paste in the Invoke URL you previously copied from API Gateway, keeping /reminders on the end of the string:

var API_ENDPOINT = 'UPDATETOYOURINVOKEURLENDPOINT/reminders';
In the AWS Management Console, navigate to S3.

Click Create bucket.

In Bucket name, enter a globally unique bucket name.

Under Object Ownership, select ACLs enabled, and ensure Bucket owner preferred is selected.

Uncheck Block all public access.

Select I acknowledge that the current settings might result in this bucket and the objects within becoming public.

Scroll down, and click Create bucket.

Select the new bucket to open it.

Click Upload > Add files.

Find your local copy of the static_website folder, select all the files and click Open.

Scroll down, and click Permissions to expand access options.

Under Predefined ACLs, select Grant public-read access.

Select I understand the risk of granting public-read access to the specified objects.

Click Upload.

Once uploaded, click Close.

Click the Properties tab.

Scroll down to Static website hosting, and click Edit.

Select Enable.

Set the following values:

Index document: index.html
Error document: error.html
Click Save changes.

Scroll down again to Static website hosting, and click the Bucket website endpoint URL to access the webpage.


# Testing the Static S3 Website

To test the service's functionality, set the following values:
Seconds to wait: 1
Message: Hello

someone@something.com: Your email address
Under Reminder Type, select email. You should see {"Status":"Success"} at the bottom of the page.

Check your email. You should now see a reminder email from the service.

Note: Check your spam folder if you do not see it in your inbox.

Navigate to AWS Step Functions.

Click MyStateMachine to open it.

Under Executions, click on the link for the event that just occurred, and scroll down to Graph inspector to view the event's visual workflow.

