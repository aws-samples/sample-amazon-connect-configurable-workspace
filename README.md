# Configurable UI CloudFormation Template

This repository contains CloudFormation templates for deploying a configurable UI solution for Amazon Connect, including data tables, Lambda functions, contact flows, views, and workspaces.

## Prerequisites

- AWS Account with appropriate IAM permissions for CloudFormation, Amazon Connect, Lambda, and S3 services
- Existing Amazon Connect instance configured and operational
- S3 bucket in the same AWS region as your Amazon Connect instance

## Architecture Overview

The solution deploys the following components:
- **Data Table**: Operational Patient Queue Table with attributes and records
- **Lambda Functions**: Four functions for data processing and transformation
- **Contact Flow Module**: Reusable module for reading operational data
- **Views**: Two Connect views for data display and error handling
- **Contact Flow**: Main flow for operational patient data management
- **Workspace**: Connect workspace with integrated views

## Deployment Steps

### Step 1: Prepare S3 Bucket

1. Create an S3 bucket in the same region as your Amazon Connect instance
2. Upload all CloudFormation YAML files to the S3 bucket:
   - `parent-stack.yaml`
   - `datatable-cft.yaml`
   - `lambda.yaml`
   - `contact-flow-module.yaml`
   - `connect-views-from-json.yaml`
   - `contact-flow.yaml`
   - `connect-workspace-view.yaml`
   - `connect-workspace.yaml`

### Step 2: Deploy CloudFormation Stack

1. Navigate to the AWS CloudFormation console
2. Create a new stack
3. Select "Template is ready" and "Amazon S3 URL"
4. Enter the S3 URL for `parent-stack.yaml`
5. Configure the following parameters:
   - **Stack Name**: Provide a descriptive name for the stack
   - **ConnectInstanceArn**: Your Amazon Connect instance ARN
   - **S3BucketName**: Name of the S3 bucket created in Step 1
   - **S3Region**: AWS region where your S3 bucket is located
   - **S3KeyPrefix**: (Optional) Prefix for template files in S3

6. Review the configuration and create the stack
7. Monitor the deployment until completion (Status: CREATE_COMPLETE)

### Step 3: Configure Contact Flow Module

1. Log in to your Amazon Connect instance
2. Navigate to **Flows** → **Modules**
3. Open the contact flow module created by the stack (name will include stack ID suffix)
4. Verify Lambda function and data table references are correct in the contact flow blocks
5. Select the references again
6. Click **Publish**
7. Select **Create new version** and enter a description
8. Click **Publish** to confirm

### Step 4: Update Security Profile

1. Navigate to **Security profiles**
2. Select the security profile used for testing
3. Under **Flow module tools**, add the newly created flow module
4. Click **Save**

### Step 5: Configure Views

#### For Each View (Repeat for both views created):

1. Navigate to **Views**
2. Select the view created by the stack
3. Go to the **Integrations** tab
4. Select the flow module as the tool
5. Set version to **Latest**
6. Enable refresh by toggling the button
7. Set refresh interval to **5 seconds**
8. Click **Publish**

### Step 6: Configure Contact Flow

1. Navigate to **Flows**
2. Open the contact flow created by the stack
3. Verify Lambda function and data table references
4. Select the references again
5. Click **Publish** to activate the flow

### Step 7: Configure Workspace View

1. Navigate to **Views**
2. Open the workspace view created by the stack
3. Verify the contact flow is associated with the view in the Connect application component
4. Click **Publish**

### Step 8: Configure Workspace

1. Navigate to **Workspaces**
2. Open the workspace created by the stack
3. Verify the associated view is configured in the pages
4. Click **Save**

## Verification

After completing all configuration steps, verify the deployment by:

1. Navigating to the home page to access the newly created workspace
2. Confirming that the views display data correctly
3. Testing the contact flow functionality with sample data

### Stack Outputs

The CloudFormation stack provides several outputs that can be used for reference:
- Data Table ARN and Name
- Lambda Function ARNs and Names
- Contact Flow Module ARN
- View ARNs and Names
- Contact Flow ARN and Name
- Workspace ARN

## Resource Cleanup

To remove all deployed resources:

1. Navigate to the AWS CloudFormation console
2. Select the deployed stack
3. Click **Delete**
4. Confirm the deletion

**Important**: This action will permanently remove all created resources including data tables, Lambda functions, contact flows, views, and workspaces.

## Support

For issues or questions, please reach out to your designated AWS point of contact.