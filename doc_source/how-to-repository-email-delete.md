--------

 The procedures in this guide support the new console design\. If you choose to use the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\.

--------

# Delete Notification Settings for a Repository<a name="how-to-repository-email-delete"></a>

If you no longer want notifications for your repository, you can delete the settings\. Deleting the settings also deletes the CloudWatch Events rule created for repository notifications\. It does not delete any subscriptions or the Amazon SNS topic used for notifications\. 

**Note**  
If you change the name of a repository from the console, notifications continue to work without modification\. However, if you change the name of your repository from the command line or by using the API, notifications no longer work\. The easiest way to restore notifications is to delete the notification settings and then configure them again\.<a name="how-to-repository-email-delete-console"></a>

**To delete notification settings**

1. Open the AWS CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In **Repositories**, choose the name of the repository where notifications are disabled\. 

1. In the navigation pane, choose **Settings**, and then choose **Notifications**\.  
![\[Notifications configured in an AWS CodeCommit repository\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-notifications-configured.png)

1. \(Optional\) To change or delete the Amazon SNS topic used for notifications after you delete notification settings, go to the Amazon SNS console at [https://console\.aws\.amazon\.com/sns/v2/home](https://console.aws.amazon.com/sns/v2/home)\. For more information, see [Clean Up](https://docs.aws.amazon.com/sns/latest/dg/CleanUp.html) in [Amazon Simple Notification Service Developer Guide](https://docs.aws.amazon.com/sns/latest/dg/)\.