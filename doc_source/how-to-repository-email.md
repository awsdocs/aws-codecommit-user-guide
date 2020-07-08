# Configuring notifications for events in an AWS CodeCommit repository<a name="how-to-repository-email"></a>

You can set up notification rules for a repository so that repository users receive emails about the repository event types you specify\. Notifications are sent when events match the notification rule settings\. You can create an Amazon SNS topic to use for notifications or use an existing one in your AWS account\. You can use the CodeCommit console and the AWS CLI to configure notification rules\.

![\[A notification rule configured in an CodeCommit repository\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/create-notification-rule-repository.png)

**Topics**
+ [Using repository notification rules](#how-to-repository-email-using)
+ [Create a notification rule](notification-rule-create.md)
+ [Change or disable notifications](how-to-repository-email-console-edit.md)
+ [Delete notifications](how-to-repository-email-delete.md)

## Using repository notification rules<a name="how-to-repository-email-using"></a>

Configuring notification rules helps your repository users by sending emails when someone takes an action that affects another user\. For example, you can configure a notification rule to send notifications when comments are made on commits\. In this configuration, when a repository user comments on a line of code in a commit, other repository users receive an email\. They can sign in and view the comment\. Responses to comments also generate emails, so repository users stay informed\.

Notification rules are different from repository triggers, and they are also different than the notifications you could configure in the CodeCommit console before November 5, 2019\. 
+ Although you can configure a trigger to use Amazon SNS to send emails about some repository events, those events are limited to operational events, such as creating branches and pushing code to a branch\. Triggers do not use CloudWatch Events rules to evaluate repository events\. They are more limited in scope\. For more information about using triggers, see [Manage triggers for a repository](how-to-notify.md)\.
+ Notifications configured before November 5, 2019 had fewer event types available, and could not be configured for integration with Amazon Chime chatrooms or Slack channels\. You can continue to use notifications configured before November 5, 2019, but you cannot create notifications of this type\. Instead, create and use notification rules\. We recommend using notification rules and disabling or deleting notifications created before November 5, 2019\. For more information, see [Create a notification rule](notification-rule-create.md) and [Delete notifications](how-to-repository-email-delete.md)\. 