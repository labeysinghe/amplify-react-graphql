# Introduction

This project is created using AWS Cloudformation. Scripts are written using YAML. 

## Approach

To achieve the technical challenge, I have taken a three-steps approach. 
1. S3 bucket is created initially. This bucket at first will include a folder called 'data'. The purpose is this folder is to include some dummy data (foo.csv), to be imported into DynamoDB table, which will be created in the next step.

2. DynamoDB table (Movies) is created and data in imported to the table. (DynamoDB DescribeTable API uses an eventually consistent query, and the metadata for your table might not be available at that moment. Wait for a few seconds)

3. Data Pipeline is created to backup data from DynamoDB to S3 bucket.

## Prerequisites
### aws-cli
aws cli should be installed and configured

### AWS region
As AWS Data Pipeline is not available in every region yet, make sure to select a suitable region. I've used Ireland (eu-west-1)

## Launch
### Step 01 - Using AWS CloudFront Console
1. Login to the console to select CloudFront
2. `Create Stack`--> `Template is ready` --> `Upload a template file` --> `Choose file` --> `Next`
3. Provide a `Stack Name` --> `Next` --> `Next`
4. CloudFront will create the resources accordingly. However, make sure to check on `I acknowledge that AWS CloudFormation might create IAM resources.`  when you deploy 3rd template (backupdynamodb.yml) 

### Step 02 -  Using AWS CLI
templates can be deployed using below command.
Example: 
`aws cloudformation create-stack \
--stack-name "importstack"
--template-body file://path/creates3bucket.yml

# Disclaimer
Please note datapipeline template is not working giving an error `DataPipeline cannot assume role: 'DataPipeLineRole' to validate EMR cluster object`. This role is created as `DataPipelineDefaultRole` is deprecated since October 3, 2022. https://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/dp-iam-roles.html