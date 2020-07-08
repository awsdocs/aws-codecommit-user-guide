# Manage triggers for an AWS CodeCommit repository<a name="how-to-notify"></a>

You can configure a CodeCommit repository so that code pushes or other events trigger actions, such as sending a notification from Amazon Simple Notification Service \(Amazon SNS\) or invoking a function in AWS Lambda\. You can create up to 10 triggers for each CodeCommit repository\.

Triggers are commonly configured to:
+ Send emails to subscribed users every time someone pushes to the repository\.
+ Notify an external build system to start a build after someone pushes to the main branch of the repository\.

Scenarios like notifying an external build system require writing a Lambda function to interact with other applications\. The email scenario simply requires creating an Amazon SNS topic\. 

This topic shows you how to set permissions that allow CodeCommit to trigger actions in Amazon SNS and Lambda\. It also includes links to examples for creating, editing, testing, and deleting triggers\.

**Topics**
+ [Create the resource and add permissions for CodeCommit](#how-to-notify-permissions)
+ [Example: Create an AWS CodeCommit trigger for an Amazon SNS topic](how-to-notify-sns.md)
+ [Example: Create an AWS CodeCommit trigger for an AWS Lambda function](how-to-notify-lambda.md)
+ [Example: Create a trigger in AWS CodeCommit for an existing AWS Lambda function](how-to-notify-lambda-cc.md)
+ [Edit triggers for an AWS CodeCommit repository](how-to-notify-edit.md)
+ [Test triggers for an AWS CodeCommit repository](how-to-notify-test.md)
+ [Delete triggers from an AWS CodeCommit repository](how-to-notify-delete.md)

## Create the resource and add permissions for CodeCommit<a name="how-to-notify-permissions"></a>

You can integrate Amazon SNS topics and Lambda functions with triggers in CodeCommit, but you must first create and then configure resources with a policy that grants CodeCommit the permissions to interact with those resources\. You must create the resource in the same AWS Region as the CodeCommit repository\. For example, if the repository is in US East \(Ohio\) \(us\-east\-2\), the Amazon SNS topic or Lambda function must be in US East \(Ohio\)\.
+ For Amazon SNS topics, you do not need to configure additional IAM policies or permissions if the Amazon SNS topic is created using the same account as the CodeCommit repository\. You can create the CodeCommit trigger as soon as you have created and subscribed to the Amazon SNS topic\. 
  + For more information about creating topics in Amazon SNS, see the [Amazon SNS documentation](https://docs.aws.amazon.com/sns/latest/dg/GettingStarted.html)\.
  + For information about using Amazon SNS to send messages to Amazon SQS queues, see [Sending Messages to Amazon SQS Queues](https://docs.aws.amazon.com/sns/latest/dg/SendMessageToSQS.html) in the *Amazon SNS Developer Guide*\.
  + For information about using Amazon SNS to invoke a Lambda function, see [Invoking Lambda Functions](https://docs.aws.amazon.com/sns/latest/dg/sns-lambda.html) in the *Amazon SNS Developer Guide*\.
+ If you want to configure your trigger to use an Amazon SNS topic in another AWS account, you must first configure that topic with a policy that allows CodeCommit to publish to that topic\. For more information, see [Example 1: Create a policy that enables cross\-account access to an Amazon SNS topic](auth-and-access-control-iam-identity-based-access-control.md#access-permissions-sns-int)\.
+ You can configure Lambda functions by creating the trigger in the Lambda console as part of the function\. This is the simplest method, because triggers created in the Lambda console automatically include the permissions required for CodeCommit to invoke the Lambda function\. If you create the trigger in CodeCommit, you must include a policy to allow CodeCommit to invoke the function\. For more information, see [Create a trigger for an existing Lambda function](how-to-notify-lambda-cc.md) and [Example 3: Create a policy for AWS Lambda integration with a CodeCommit trigger](auth-and-access-control-iam-identity-based-access-control.md#access-permissions-lambda-int)\.