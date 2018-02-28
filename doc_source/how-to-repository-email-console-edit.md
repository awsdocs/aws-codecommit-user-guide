# Change, Disable, or Enable Notifications<a name="how-to-repository-email-console-edit"></a>

You can use the AWS CodeCommit console to change how notifications are configured, including the event types that send emails to users and the Amazon SNS topic used to send emails about the repository, or to manage the list of email addresses and endpoints subscribed to the topic\. You can also use the console to temporarily disable notifications\. <a name="how-to-repository-email-change-console"></a>

**To change notification settings \(console\)**

1. Open the AWS CodeCommit console at [https://console\.aws\.amazon\.com/codecommit](https://console.aws.amazon.com/codecommit)\.

1. In the list of repositories, choose the name of the repository where you want to configure notifications\. 

1. In the navigation pane, choose **Settings**, and then choose **Notifications**\.

1. Choose **Edit**\.

1. Make your changes, and then choose **Save**\.

Disabling notifications is an easy way to temporarily prevent users from receiving emails about repository events\. For example, you might want to disable notifications while you are performing repository maintenance\. Because the configuration is preserved, you can quickly enable notifications when you are ready\.

To permanently delete the notification settings, follow the steps in [Delete Notification Settings for a Repository](how-to-repository-email-delete.md)\. <a name="how-to-repository-email-disable-console"></a>

**To disable notifications \(console\)**

1. Open the AWS CodeCommit console at [https://console\.aws\.amazon\.com/codecommit](https://console.aws.amazon.com/codecommit)\.

1. In the list of repositories, choose the name of the repository where you want to disable notifications\. 

1. In the navigation pane, choose **Settings**, and then choose **Notifications**\.

1. Choose **Disable**\. 

1. The notification state changes to **Disabled**\. No emails about events are sent\. When you disable notifications, the CloudWatch Events rule for the repository is disabled automatically\. Do not manually change its status in the CloudWatch Events console\.<a name="how-to-repository-email-enable-console"></a>

**To enable notifications \(console\)**

1. Open the AWS CodeCommit console at [https://console\.aws\.amazon\.com/codecommit](https://console.aws.amazon.com/codecommit)\.

1. In the list of repositories, choose the name of the repository where notifications are disabled\. 

1. In the navigation pane, choose **Settings**, and then choose **Notifications**\.

1. Choose **Enable**\. 

1. The notification state changes to **Enabled**\. Emails about events are sent\. The CloudWatch Events rule for the repository is enabled automatically\.