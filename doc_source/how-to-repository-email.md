# Configuring Notifications for Events in an AWS CodeCommit Repository<a name="how-to-repository-email"></a>

You can set up notifications for a repository so that repository users receive emails about the repository event types you specify\. When you configure notifications, AWS CodeCommit creates an Amazon CloudWatch Events rule for your repository\. This rule responds to the event types you select from the preconfigured options in the AWS CodeCommit console\. Notifications are sent when events match the rule settings\. You can create an Amazon SNS topic to use for notifications, or use an existing one in your AWS account\. 

 You use the AWS CodeCommit console to configure notifications\.

![\[Notifications configured in AWS CodeCommit repository\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-notifications-configured.png)


+ [Using Repository Notifications](#how-to-repository-email-using)
+ [Configure Repository Notifications](how-to-repository-email-create.md)
+ [Change, Disable, or Enable Notifications](how-to-repository-email-console-edit.md)
+ [Delete Notification Settings for a Repository](how-to-repository-email-delete.md)

## Using Repository Notifications<a name="how-to-repository-email-using"></a>

Configuring notifications helps your repository users by sending emails to users when someone takes an action that affects another user\. For example, you can configure a repository to send notifications when comments are made on commits\. In this configuration, when a repository user comments on a line of code in a commit, other repository users receive an email\. They can sign in and view the comment\. Responses to comments also generate emails, so repository users stay informed\.

Notification event types are grouped into the following categories:

+ **Pull request update events**: If you select this option, users receive emails when:

  + A pull request is created or closed\.

  + A pull request is updated with code changes\.

  + The title or description of the pull request changes\.

+ **Pull request comment events**: If you select this option, users receive emails when someone comments or replies to a comment in a pull request\.

+ **Commit comment events**: If you select this option, users receive emails when someone comments on a commit outside of a pull request\. This includes comments on:

  + Lines of code in a commit\.

  + Files in a commit\.

  + The commit itself\.

  For more information, see [Comment on a Commit](how-to-commit-comment.md)\.

Repository notifications are different from repository triggers\. Although you can configure a trigger to use Amazon SNS to send emails about some repository events, those events are limited to operational events such as creating branches and pushing code to a branch\. Triggers do not use CloudWatch Events rules to evaluate repository events\. They are more limited in scope\. For more information about using triggers, see [Manage Triggers for a Repository](how-to-notify.md)\.