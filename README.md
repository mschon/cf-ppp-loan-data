# Querying Paycheck Protection Program (PPP) Loan data with Amazon Athena

## Overview

This AWS CloudFormation stack provides an easy way to perform SQL queries against Paycheck Protection Program (PPP) Loan data in CSV format using Amazon Athena. 

## Costs

Before using this stack, be certain that you understand how Amazon Web Services (AWS) pricing works, especially for the services used in this stack. 

- [AWS CloudFormation](https://aws.amazon.com/cloudformation/faqs/)
- [Amazon S3](https://aws.amazon.com/s3/pricing/)
- [AWS Glue](https://aws.amazon.com/glue/pricing/)
- [Amazon Athena](https://aws.amazon.com/athena/pricing/)

## Prerequisites

The following instructions assume that you already have an Amazon Web Services account and have basic knowledge of the platform and how to write simple SQL queries. 

The estimated time to the complete stack installation is less than 10 minutes. (This does not include data transfer time -- that is, waiting for the dataset to upload.)

First, do the following:

1. Download the CloudFormation template (YAML) from this repository. 
2. Download the [PPP FOIA data](https://data.sba.gov/dataset/ppp-foia) files in CSV format from the U.S. Small Business Administration. 

## Create the stack

Choose between Option 1 or Option 2. 

### Option 1: AWS CLI

This section assumes that you have already installed and configured the [AWS CLI](https://aws.amazon.com/cli/). If you don't want to use the CLI, skip this section and follow the instructions for Option 2 to install it using the management console instead. 

1. Open a terminal of your choice and change to the directory where you downloaded the template file. 

2. Copy the following CLI command into a text editor. At a minimum, you must change the value of the `S3BucketName` parameter because S3 bucket names must be unique across all accounts. For example, replace the word `unique` below with your name or some random digits. 

```
aws cloudformation create-stack \
--template-body file://cf-ppp-loan-data.yml \
--stack-name ppp-loan-data \
--parameters \
ParameterKey=S3BucketName,ParameterValue=ppp-loan-data-unique \
ParameterKey=GlueDBName,ParameterValue=ppp-loan-data \
ParameterKey=GlueDBTableName,ParameterValue=ppp-loan-data \
--region us-east-1
```

3. If desired, change the `region` value. 

4. If desired, you may change the stack name, database name, and table name. 

5. After making the necessary modifications, launch a terminal and change to the directory where the template is located. (The `.yml` file must be in the same directory in which you execute the CLI command.)

6. Copy the modified CLI command from your text editor and paste it into the terminal. Hit enter. 

7. If the command was successful, the CLI should return an object containing a `StackId`. Skip ahead to the section titled **Add data to S3 bucket**. 

### Option 2: AWS Management Console

1. Log in to the management console at https://console.aws.amazon.com
2. Open [AWS CloudFormation](https://console.aws.amazon.com/cloudformation/home).
3. Click **Create stack**. If prompted, select "With new resources (standard)".
4. Under **Prepare template**, select "Template is ready".
5. Under **Specify template**, select "Upload a template file". Click **Choose file** and select the template you downloaded from this repository. 
6. Click **Next**.
7. Enter a stack name.
8. For parameters, specify the names that you want to use for the following resources: 

- **S3 Bucket** - This must be unique across all AWS accounts and comply with [S3 bucket naming rules](https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucketnamingrules.html). If you're not sure what to use, set the S3 Bucket name to `ppp-loan-data-` followed by your name or some random digits to make it unique. 
- **Glue Database** - If you're not sure what to use, set this to `ppp-loan-data`, which complies with the [naming rules for a Glue database](https://docs.aws.amazon.com/athena/latest/ug/tables-databases-columns-names.html). 
- **Glue Database Table** - If you're not sure what to use, set this to `ppp-loan-data`, which complies with the [naming rules for a Glue table](https://docs.aws.amazon.com/athena/latest/ug/tables-databases-columns-names.html). 

9. Click **Next**.
10. If desired, configure the remainder of the stack options. 
11. Click **Next**.
12. Review the stack information, and when ready, click **Submit**.
13. Wait for CloudFormation to create the stack. If it was successful, the status of the stack should be `CREATE_COMPLETE`. 

## Add data to S3 bucket

Once the stack is created, you must take the CSVs downloaded from the SBA website and upload them to the root of the S3 bucket. 

If you're not sure how to do this, the easiest way is to log in to the AWS management console, open [Amazon S3](https://console.aws.amazon.com/s3), and search for the bucket name that you defined in a previous step. Open the bucket and click **Upload** to open the upload dialog, where you may select the CSV files to upload. At the time of this release, the dataset is nearly 5 GB in size, so this upload may take a while. 

Once all of the data has been successfully uploaded to the bucket, you can proceed to perform queries. 

## Query the data

1. Open [Amazon Athena](https://console.aws.amazon.com/athena/home) and go to [Saved Queries](https://console.aws.amazon.com/athena/home#/query-editor/saved-queries).

2. Find the example query called `PPP-loans-ByCityAndState`. Click on its ID to open it in the **Query editor**. You may use this query as a starting point to write your own queries. 

3. You may expand the table to see the available columns that you can use in your query. To understand the values contained within these columns, refer to to the data dictionary that accompanies [the data on the SBA's website](https://data.sba.gov/dataset/ppp-foia).

## Troubleshooting

If after completing the installation steps you don't see the example query in Athena, first make sure that you viewing the same region in which you created the stack. If that doesn't work, open AWS CloudFormation in the management console and look for for the stack you created. Check the **Events** tab to see if something went wrong when creating the stack. 

## Uninstallation

To uninstall the stack, first empty the S3 bucket of all its objects. Then open AWS CloudFormation and delete the stack. If successful, all stack resources will be deleted. 

## Version History

Refer to HISTORY.md

## License

Refer to LICENSE.md

## Contributing to this project

Refer to CONTRIBUTING.md

## Supporting this project

There is no charge to use this template. If you found this project useful, monetary gifts are appreciated. 

Monetary gifts may be sent via [PayPal](https://paypal.me/mattschonert). 

Messages of thanks and encouragement may be directed to me on [LinkedIn](https://www.linkedin.com/in/mattschonert/). 
