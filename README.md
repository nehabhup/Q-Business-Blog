# Q-Business-Blog

### CloudFormation Template

The [custplugin.yaml](custplugin.yaml) file is used to deploy your entire AWS infrastructure, including the Q Business application, Lambda functions, and other resources.

### OpenAPI Schema

The [email-plugin.yaml](email-plugin.yaml) file defines the structure and capabilities of your custom email plugin. This schema is used by Q Business to understand how to interact with your email sending functionality.

## Prerequisites

- AWS account with administrative access
- AWS IAM Identity Center is enabled

## Deployment Steps

1. Create an S3 bucket for the Custom Plugin schema
    1. Add the schema to the location
    2. Update the cloudformation template to refer to the location and key of the schema
2. Deploy the cloud formation stack
    1. Click "Create stack" and choose "With new resources (standard)".
    2. Fill in the required parameters (IdcInstanceArn, S3BucketName, SESSourceEmail, etc.).
        1. SESSourceEmail should be the email acting as the customer service or the email users can query for further assistance
        2. S3BucketName should be the bucket that has the training / data source materials in it
        3. How to find your IdcInstanceArn
            1. Navigate to IAM Identity Center
            2. Click “Go to Settings” on the left hand side
3. Add training materials to s3 bucket that cloudformation created (change this step based on if the stack creates a bucket, otherwise just exclude)
4. Sync data source on the Q Business console
    1. Navigate to the Q Business console
    2. Find your application and go to the "Data sources" section
    3. Initiate a sync to index the uploaded documents
5. Get your customer service email verified on SES
    1. Navigate to Amazon SES
    2. Go to Verified Identities
    3. Create a new identity
    4. Choose identity type: Select "Email address" as the identity type.
    5. Enter the email address (this should be the same one that you entered in the parameter of the stack)
    6. Click "Create identity" to submit the request.
    7. Confirm the email address by following the link on the email SES sends you
6. Update Server URL in Custom Plugin schema
    1. Navigate to Amazon API Gateway
    2. Go to Stages in the side panel
    3. Choose the relevant API
    4. Copy the URL
    5. Open the Custom Plugin schema and replace the server url and save the document
    6. Navigate to S3
    7. Find the bucket that holds the Custom Plugin schema
    8. Replace the existing document with the updated one - make sure the key is the same as whats being referred to in the Custom Plugin          resource in the cloudformation template
    9. Navigate to Amazon Q Business
    10. Find the newly created App 
    11. Go to Plugins in the side panel
    12. Click on the relevant plugin
    13. Make sure the s3 link is referring to the correct schema document
    14. Click on Update
7. Query the bot and make use of the custom plugin
    1. You can now use natural language to request the bot to send emails. For example: "Can you send an email to john@example.com with the subject 'Additional Information about Dental Insurance' and the body 'Hi, I was wondering if braces is completely covered with my current dental insurance?“
