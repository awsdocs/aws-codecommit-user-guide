--------

 The procedures in this guide support the new console design\. If you choose to use the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\.

--------

# Manage Triggers for an AWS CodeCommit Repository<a name="how-to-notify"></a>

You can configure an AWS CodeCommit repository so that code pushes or other events trigger actions, such as sending a notification from Amazon Simple Notification Service \(Amazon SNS\) or invoking a function in AWS Lambda\. You can create up to 10 triggers for each AWS CodeCommit repository\.

Triggers are commonly configured to:
+ Send emails to subscribed users every time someone pushes to the repository\.
+ Notify an external build system to start a build after someone pushes to the main branch of the repository\.

Scenarios like notifying an external build system require writing a Lambda function to interact with other applications\. The email scenario simply requires creating an Amazon SNS topic\. 

This topic shows you how to set permissions that allow AWS CodeCommit to trigger actions in Amazon SNS and Lambda\. It also includes links to examples for creating, editing, testing, and deleting triggers\.

**Topics**
+ [Create the Resource and Add Permissions for AWS CodeCommit](#how-to-notify-permissions)
+ [Example: Create an AWS CodeCommit Trigger for an Amazon SNS Topic](how-to-notify-sns.md)
+ [Example: Create an AWS CodeCommit Trigger for an AWS Lambda Function](how-to-notify-lambda.md)
+ [Example: Create a Trigger in AWS CodeCommit for an Existing AWS Lambda Function](how-to-notify-lambda-cc.md)
+ [Edit Triggers for an AWS CodeCommit Repository](how-to-notify-edit.md)
+ [Test Triggers for an AWS CodeCommit Repository](how-to-notify-test.md)
+ [Delete Triggers from an AWS CodeCommit Repository](how-to-notify-delete.md)

## Create the Resource and Add Permissions for AWS CodeCommit<a name="how-to-notify-permissions"></a>

You can integrate Amazon SNS topics and Lambda functions with triggers in AWS CodeCommit, but you must first create and then configure resources with a policy that allows AWS CodeCommit the permissions to interact with those resources\. You must create the resource in the same region as the AWS CodeCommit repository\. For example, if the repository is in US East \(Ohio\) \(us\-east\-2\), the Amazon SNS topic or Lambda function must be in US East \(Ohio\)\.
+ For Amazon SNS topics, you do not need to configure additional IAM policies or permissions if the Amazon SNS topic is created using the same account as the AWS CodeCommit repository\. You can create the AWS CodeCommit trigger as soon as you have created and subscribed to the Amazon SNS topic\. 
  + For more information about creating topics in Amazon SNS, see the [Amazon SNS documentation](https://docs.aws.amazon.com/sns/latest/dg/GettingStarted.html)\.
  + For information about using Amazon SNS to send messages to Amazon SQS queues, see [Sending Messages to Amazon SQS Queues](https://docs.aws.amazon.com/sns/latest/dg/SendMessageToSQS.html) in the *Amazon SNS Developer Guide*\.
  + For information about using Amazon SNS to invoke a Lambda function, see [Invoking Lambda Functions](https://docs.aws.amazon.com/sns/latest/dg/sns-lambda.html) in the *Amazon SNS Developer Guide*\.
+ If you want to configure your trigger to use an Amazon SNS topic in another AWS account, you must first configure that topic with a policy that allows AWS CodeCommit to publish to that topic\. For more information, see [Example 1: Create a Policy That Enables Cross\-Account Access to an Amazon SNS Topic](auth-and-access-control-iam-identity-based-access-control.md#access-permissions-sns-int)\.
+ You can configure Lambda functions by creating the trigger in the Lambda console as part of the function\. This is the simplest method, because triggers created in the Lambda console automatically include the permissions required for AWS CodeCommit to invoke the Lambda function\. If you create the trigger in AWS CodeCommit, you must include a policy to allow AWS CodeCommit to invoke the function\. For more information, see [Create a Trigger for an Existing Lambda Function](how-to-notify-lambda-cc.md) and [Example 3: Create a Policy for AWS Lambda Integration with an AWS CodeCommit Trigger](auth-and-access-control-iam-identity-based-access-control.md#access-permissions-lambda-int)\.