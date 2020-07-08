# Create a notification rule<a name="notification-rule-create"></a>

You can use notification rules to notify users of important changes, such as when a pull request is created in a repository\. Notification rules specify both the events and the Amazon SNS topic that is used to send notifications\. For more information, see [What are notifications?](https://docs.aws.amazon.com/codestar-notifications/latest/userguide/welcome.html)

You can use the console or the AWS CLI to create notification rules for AWS CodeCommit\. <a name="notification-rule-create-console"></a>

# To create a notification rule \(console\)<a name="notification-rule-create-console"></a>

1. Sign in to the AWS Management Console and open the CodeCommit console at [https://console\.aws\.amazon\.com/codecommit/](https://console.aws.amazon.com/codecommit/)\.

1. Choose **Repositories**, and then choose a repository where you want to add notification rules\.

1. On the repository page, choose **Notify**, and then choose **Create notification rule**\. You can also go to the **Settings** page for the repository and choose **Create notification rule**\.

1. In **Notification name**, enter a name for the rule\.

1. In **Detail type**, choose **Basic** if you want only the information provided to Amazon EventBridge included in the notification\. Choose **Full** if you want to include information provided to Amazon EventBridge and information that might be supplied by the CodeCommit or the notification manager\.

   For more information, see [Understanding Notification Contents and Security](https://docs.aws.amazon.com/codestar-notifications/latest/userguide/security.html#security-notifications)\.

1.  In **Events that trigger notifications**, select the events for which you want to send notifications\. For more information, see [ Events for Notification Rules on Repositories](https://docs.aws.amazon.com/codestar-notifications/latest/userguide/concepts.html#events-ref-repositories)\.

1. In **Targets**, do one of the following:
   + If you have already configured a resource to use with notifications, in **Choose target type**, choose either **AWS Chatbot \(Slack\)** or **SNS topic**\. In **Choose target**, choose the name of the client \(for a Slack client configured in AWS Chatbot\) or the Amazon Resource Name \(ARN\) of the Amazon SNS topic \(for Amazon SNS topics already configured with the policy required for notifications\)\.
   + If you have not configured a resource to use with notifications, choose **Create target**, and then choose **SNS topic**\. Provide a name for the topic after **codestar\-notifications\-**, and then choose **Create**\.
**Note**  
If you create the Amazon SNS topic as part of creating the notification rule, the policy that allows the notifications feature to publish events to the topic is applied for you\. Using a topic created for notification rules helps ensure that you subscribe only those users that you want to receive notifications about this resource\.
You cannot create an AWS Chatbot client as part of creating a notification rule\. If you choose AWS Chatbot \(Slack\), you will see a button directing you to configure a client in AWS Chatbot\. Choosing that option opens the AWS Chatbot console\. For more information, see [ Configure Integrations Between Notifications and AWS Chatbot](https://docs.aws.amazon.com/codestar-notifications/latest/userguide/notifications-chatbot.html)\.
If you want to use an existing Amazon SNS topic as a target, you must add the required policy for AWS CodeStar Notifications in addition to any other policies that might exist for that topic\. For more information, see [Configure Amazon SNS Topics for Notifications ](https://docs.aws.amazon.com/codestar-notifications/latest/userguide/set-up-sns.html) and [Understanding Notification Contents and Security](https://docs.aws.amazon.com/codestar-notifications/latest/userguide/security.html#security-notifications)\. 

1. To finish creating the rule, choose **Submit**\.

1. You must subscribe users to the Amazon SNS topic for the rule before they can receive notifications\. For more information, see [Subscribe Users to Amazon SNS Topics That Are Targets](https://docs.aws.amazon.com/codestar-notifications/latest/userguide/subscribe-users-sns.html)\. You can also set up integration between notifications and AWS Chatbot to send notifications to Amazon Chime chatrooms\. For more information, see [Configure Integration Between Notifications and AWS Chatbot](https://docs.aws.amazon.com/codestar-notifications/latest/userguide/notifications-chatbot.html)\.<a name="notification-rule-create-cli"></a>

# To create a notification rule \(AWS CLI\)<a name="notification-rule-create-cli"></a>

1. At a terminal or command prompt, run the create\-notification rule command to generate the JSON skeleton:

   ```
   aws codestar-notifications create-notification-rule --generate-cli-skeleton > rule.json
   ```

   You can name the file anything you want\. In this example, the file is named *rule\.json*\.

1. Open the JSON file in a plain\-text editor and edit it to include the resource, event types, and target you want for the rule\. The following example shows a notification rule named **MyNotificationRule** for a repository named *MyDemoRepo* in an AWS acccount with the ID *123456789012*\. Notifications with the full detail type are sent to an Amazon SNS topic named *MyNotificationTopic* when branches and tags are created:

   ```
   {
       "Name": "MyNotificationRule",
       "EventTypeIds": [
           "codecommit-repository-branches-and-tags-created"
       ],
       "Resource": "arn:aws:codecommit:us-east-1:123456789012:MyDemoRepo",
       "Targets": [
           {
               "TargetType": "SNS",
               "TargetAddress": "arn:aws:sns:us-east-1:123456789012:MyNotificationTopic"
           }
       ],
       "Status": "ENABLED",
       "DetailType": "FULL"
   }
   ```

   Save the file\.

1. Using the file you just edited, at the terminal or command line, run the create\-notification\-rule command again to create the notification rule:

   ```
   aws codestar-notifications create-notification-rule --cli-input-json  file://rule.json
   ```

1. If successful, the command returns the ARN of the notification rule, similar to the following:

   ```
   {
       "Arn": "arn:aws:codestar-notifications:us-east-1:123456789012:notificationrule/dc82df7a-EXAMPLE"
   }
   ```