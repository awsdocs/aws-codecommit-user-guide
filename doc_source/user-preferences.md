# Working with User Preferences<a name="user-preferences"></a>

Some default settings in the AWS CodeCommit console can be configured\. For example, you can change the number of repositories displayed on the dashboard\. If you are signed in to the console as an IAM user, you can store information about how you prefer to use the AWS CodeCommit console, also known as your user preferences\. This information is stored and applied every time you use the console\. These preferences are applied to all repositories in all regions for your IAM user any time you access the AWS CodeCommit console\. They are not repository\-specific or region\-specific\. They do not have any effect on your interactions with the AWS CLI, AWS CodeCommit API, or other services that interact with AWS CodeCommit\.

You can still change individual settings on console pages without having saved user preferences\. Those choices persist until you close the console window\. When you return to the console, any saved user preferences are applied\.

**Note**  
User preferences are only available for IAM users\. You cannot set them if you use federated access, temporary access, or a root account to access the console\.

User preferences include:
+ When viewing a list of repositories in your AWS account, the number of repositories displayed on the dashboard\.
+ When viewing changes in code, whether to use **Unified** or **Split** view, and whether to show or hide whitespace changes\.
+ When viewing a graph of commits, whether to display commits on branches to the left or to the right of the default branch\.

## View and Save User Preferences<a name="user-preferences-how-to"></a>

You can view and change your user preferences for the AWS CodeCommit console\. These settings apply only to your IAM user in the AWS CodeCommit console\. They do not affect other IAM users in your AWS account\. 

**To view and save preferences in the AWS CodeCommit console for your IAM user**

1. Open the AWS CodeCommit console at [https://console\.aws\.amazon\.com/codecommit](https://console.aws.amazon.com/codecommit)\.

   You must sign in as an IAM user\. You cannot configure user preferences for other user types\.

1. In the title bar navigation, choose **User preferences**\.

1. In **User preferences**, make your changes to configure your preferences\.  
![\[A view of configurable user preferences in AWS CodeCommit.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-userprefs.png)

   Do one of the following:
   + To save and apply your changes, choose **Save**\.
   + To view the AWS CodeCommit console defaults, choose **Restore**\. These defaults are applied if you choose **Save**\.
   + To return to the console where you left off, choose **Back**\. Alternatively, choose **Dashboard** to go to the AWS CodeCommit console dashboard\.