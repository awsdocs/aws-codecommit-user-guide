# Delete notifications<a name="how-to-repository-email-delete"></a>

If you no longer want to use notifications created for a repository before November 5, 2019, you can delete the Amazon CloudWatch Events rule associated with the notification\. That will automatically delete the notification\. It does not delete any subscriptions or the Amazon SNS topic used for notifications\. 

**Note**  
If you change the name of a repository from the console, notifications created before November 5, 2019 continue to work without modification\. However, if you change the name of your repository from the command line or by using the API, notifications no longer work\. The easiest way to restore notifications is to delete the notification settings and then configure them again\.<a name="how-to-repository-email-delete-console"></a>

**To delete notification settings**

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In **Repositories**, choose the name of the repository where you want to remove notifications created before November 5, 2019\. 

1. In the navigation pane, choose **Settings**, and then choose **Notifications**\. If you see a banner informing you that you have notifications instead of notification rules, choose **Manage existing notifications**\.

1. In **CloudWatch event rule**, copy the name of the rule that was created for the notification\.

1. Sign in to the AWS Management Console and open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In **Events**, choose **Rules**\. In **Name**, paste the name of the rule created for the notification\. Choose the rule, and in **Actions**, choose **Delete**\.

1. \(Optional\) To change or delete the Amazon SNS topic used for notifications after you delete notification settings, go to the Amazon SNS console at [https://console\.aws\.amazon\.com/sns/v3/home](https://console.aws.amazon.com/sns/v3/home)\. For more information, see [Clean Up](https://docs.aws.amazon.com/sns/latest/dg/CleanUp.html) in [Amazon Simple Notification Service Developer Guide](https://docs.aws.amazon.com/sns/latest/dg/)\.