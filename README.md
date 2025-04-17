# Q-Business-Blog

### CloudFormation Templates

This repository contains two CloudFormation templates:

1. [template1.yaml](template1.yaml) - Deploys the initial AWS infrastructure including:
   - Q Business application setup
   - IAM roles and permissions
   - S3 bucket configurations
   - SES email verification resources

2. [template2.yml](template2.yml) - Deploys the custom plugin

### OpenAPI Schema

The [email-plugin.yaml](email-plugin.yaml) file defines the structure and capabilities of your custom email plugin. This schema is used by Q Business to understand how to interact with your email sending functionality.

## Prerequisites

- AWS account with administrative access
- AWS IAM Identity Center is enabled

## Deployment Steps

### Step 1: Configure the customer service email address on Amazon Simple Email Service (SES)
1. Navigate to Amazon SES
2. Go to Verified Identities
3. Create a new identity
4. Choose identity type: Select "Email address" as the identity type
5. Enter the email address you wish to use
6. Click "Create identity" to submit the request
7. Confirm the email address by following the link on the email SES sends you and then your identity will be confirmed (Should take about 2 minutes to receive the email)

### Step 2: Create an S3 Bucket with your training materials in it
1. Navigate to S3
2. Click "Create bucket"
3. Choose a unique bucket name (e.g., "company-training-materials-2025")
4. Upload your training materials into this bucket
   - Mock training data can be found here: [Training-Materials.docx](Training-Materials.docx)

### Step 3: Deploy the First CloudFormation Stack
1. Clone this repository: `https://github.com/nehabhup/Q-Business-Blog/`
2. Navigate to AWS CloudFormation
3. Click "Create stack" and choose "With new resources (standard)"
4. Upload [template1.yaml](https://github.com/nehabhup/Q-Business-Blog/blob/main/template1.yaml)
5. Fill in the required parameters:
   - S3BucketName: The bucket containing your training materials
   - SESSourceEmail: The email that serves as the customer service email address (The same one you verified in Step 0)
   - IdcInstanceArn: Your IAM Identity Center instance ARN
     - To find your IdcInstanceArn:
       - Navigate to IAM Identity Center
       - Click "Go to Settings" on the left hand side
       - Example: arn:aws:sso:::instance/ssoins-722339a1b72acd7b
6. Click Next, review the settings, and create the stack
7. Wait for CREATE_COMPLETE status
8. Note the outputs from the stack for use in the next step

### Step 4: Deploy the Second CloudFormation Stack
1. Open a new CloudFormation tab
2. Click "Create stack" and choose "With new resources (standard)"
3. Upload [template2.yml](https://github.com/nehabhup/Q-Business-Blog/blob/main/template2.yml)
4. Fill in the required parameters:
   - ApiSchemaKey: "email-plugin.yaml" 
   - ApplicationId: Copy from the first stack's outputs
   - S3BucketName: Same bucket containing your training materials
6. Click Next, review, and create the stack

### Step 5: Sync Data Source
1. Navigate to the Amazon Q Business console
2. Locate your application (named 'qbusiness-example-app')
3. Go to "Data sources" section
4. Select your data source and click "Sync now" to index the documents

### Step 6: Set up User Access
1. In the Q Business Console, select your application
2. Click on "Manage User Access"
3. Click "Add Groups or Users"
4. Add desired users
5. Click Add → Assign → Confirm
   - Users will receive invitation emails
   - They will need to set up their passwords upon accepting the invitation

### Step 7: Using the Q Business Bot
1. Navigate to your application in the Q Business Console
2. Click the Deployed URL
3. Log in using credentials set up in Step 6
4. You can now query the bot about your training materials
   - Example: "Tell me about my dental insurance"
5. Use the Custom Plugin to be able to send an email asking for further information to the email you verified with SES in Step 1

## Troubleshooting

Common issues and solutions:
- If the data sync fails, ensure your S3 bucket permissions are correctly configured
- If users can't access the bot, verify they have accepted their invitations and set up passwords
- If the bot doesn't answer questions about your content, try re-syncing the data source

## Resources

- [Amazon Q Business Documentation](https://docs.aws.amazon.com/amazonq/latest/business-use-guide/what-is.html)
- [AWS CloudFormation Documentation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)
- [AWS IAM Identity Center Documentation](https://docs.aws.amazon.com/singlesignon/latest/userguide/what-is.html)

