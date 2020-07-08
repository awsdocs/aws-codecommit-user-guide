# Example: Create an AWS CodeCommit trigger for an Amazon SNS topic<a name="how-to-notify-sns"></a>

You can create a trigger for a CodeCommit repository so that events in that repository trigger notifications from an Amazon Simple Notification Service \(Amazon SNS\) topic\. You might want to create a trigger to an Amazon SNS topic to enable users to subscribe to notifications about repository events, such as the deletion of branches\. You can also take advantage of the integration of Amazon SNS topics with other services, such as Amazon Simple Queue Service \(Amazon SQS\) and AWS Lambda\. 

**Note**  
You must point the trigger to an existing Amazon SNS topic that is the action taken in response to repository events\. For more information about creating and subscribing to Amazon SNS topics, see [Getting Started with Amazon Simple Notification Service](https://docs.aws.amazon.com/sns/latest/dg/GettingStarted.html)\. 

**Topics**
+ [Create a trigger to an Amazon SNS topic for a CodeCommit repository \(console\)](#how-to-notify-sns-console)
+ [Create a trigger to an Amazon SNS topic for a CodeCommit repository \(AWS CLI\)](#how-to-notify-sns-cli)

## Create a trigger to an Amazon SNS topic for a CodeCommit repository \(console\)<a name="how-to-notify-sns-console"></a>

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In **Repositories**, choose the repository where you want to create triggers for repository events\.

1. In the navigation pane for the repository, choose **Settings**, and then choose **Triggers**\.

1. Choose **Create trigger**, and then do the following:\.
   + In **Trigger name**, enter a name for the trigger \(for example, *MyFirstTrigger*\)\.
   + In **Events**, choose the repository events that trigger the Amazon SNS topic to send notifications\. 

     If you choose **All repository events**, you cannot choose any other events\. To choose a subset of events, remove **All repository events**, and then choose one or more events from the list\. For example, if you want the trigger to run only when a user creates a branch or tag in the CodeCommit repository, remove **All repository events**, and then choose **Create branch or tag**\.
   + If you want the trigger to apply to all branches of the repository, in **Branches**, leave the selection blank, as this default option applies the trigger to all branches automatically\. If you want this trigger to apply to specific branches only, choose up to 10 branch names from the list of repository branches\.
   + In **Choose the service to use**, choose **Amazon SNS**\.
   + In **Amazon SNS**, choose a topic name from the list or enter the ARN for the topic\.
   + In **Custom data**, provide any optional information you want included in the notification sent by the Amazon SNS topic \(for example, an IRC channel name developers use when discussing development in this repository\)\. This field is a string\. It cannot be used to pass any dynamic parameters\. 

1. \(Optional\) Choose **Test trigger**\. This step helps you confirm have correctly configured access between CodeCommit and the Amazon SNS topic\. It uses the Amazon SNS topic to send a test notification using data from your repository, if available\. If no real data is available, the test notification contains sample data\. 

1. Choose **Create trigger** to finish creating the trigger\.

## Create a trigger to an Amazon SNS topic for a CodeCommit repository \(AWS CLI\)<a name="how-to-notify-sns-cli"></a>

You can also use the command line to create a trigger for an Amazon SNS topic in response to CodeCommit repository events, such as when someone pushes a commit to your repository\. 

**To create a trigger for an Amazon SNS topic**

1. Open a plain\-text editor and create a JSON file that specifies:
   + The Amazon SNS topic name\.
   + The repository and branches you want to monitor with this trigger\. \(If you do not specify any branches, the trigger applies to all branches in the repository\.\)
   + The events that activate this trigger\.

    Save the file\. 

   For example, to create a trigger for a repository named *MyDemoRepo* that publishes all repository events to an Amazon SNS topic named *MySNSTopic* for two branches, *master* and *preprod*:

   ```
   {
       "repositoryName": "MyDemoRepo",
       "triggers": [
           {
               "name": "MyFirstTrigger",
               "destinationArn": "arn:aws:sns:us-east-2:80398EXAMPLE:MySNSTopic",
               "customData": "",
               "branches": [
                   "master", "preprod"
               ],
               "events": [
                   "all"
               ]
           }
       ]
   }
   ```

   There must be a trigger block in the JSON for each trigger for a repository\. To create more than one trigger for the repository, include more than one trigger block in the JSON\. Remember that all triggers created in this file are for the specified repository\. You cannot create triggers for multiple repositories in a single JSON file\. For example, if you wanted to create two triggers for a repository, you can create a JSON file with two trigger blocks\. In the following example, no branches are specified for the second trigger, so that trigger applies to all branches:

   ```
   {
       "repositoryName": "MyDemoRepo",
       "triggers": [
           {
               "name": "MyFirstTrigger",
               "destinationArn": "arn:aws:sns:us-east-2:80398EXAMPLE:MySNSTopic",
               "customData": "",
               "branches": [
                   "master", "preprod"
               ],
               "events": [
                   "all"
               ]
           },
           {
               "name": "MySecondTrigger",
               "destinationArn": "arn:aws:sns:us-east-2:80398EXAMPLE:MySNSTopic2",
               "customData": "",
               "branches": [],
               "events": [
                   "updateReference", "deleteReference"
               ]
           }
       ]
   }
   ```

   You can create triggers for events you specify, such as when a commit is pushed to a repository\. Event types include:
   + `all` for all events in the specified repository and branches\.
   + `updateReference` for when commits are pushed to the specified repository and branches\.
   + `createReference` for when a new branch or tag is created in the specified repository\.
   + `deleteReference` for when a branch or tag is deleted in the specified repository\.
**Note**  
You can use more than one event type in a trigger\. However, if you specify `all`, you cannot specify other events\. 

   To see the full list of valid event types, at the terminal or command prompt, enter aws codecommit put\-repository\-triggers help\. 

   In addition, you can include a string in `customData` \(for example, an IRC channel name developers use when discussing development in this repository\)\. This field is a string\. It cannot be used to pass any dynamic parameters\. This string is appended as an attribute to the CodeCommit JSON returned in response to the trigger\.

1. \(Optional\) At a terminal or command prompt, run the test\-repository\-triggers command\. This test uses sample data from the repository \(or generates sample data if no data is available\) to send a notification to the subscribers of the Amazon SNS topic\. For example, the following is used to test that the JSON in the trigger file named *trigger\.json* is valid and that CodeCommit can publish to the Amazon SNS topic: 

   ```
   aws codecommit test-repository-triggers --cli-input-json file://trigger.json
   ```

   If successful, this command returns information similar to the following:

   ```
   {
       "successfulExecutions": [
           "MyFirstTrigger"
       ],
       "failedExecutions": []
   }
   ```

1. At a terminal or command prompt, run the put\-repository\-triggers command to create the trigger in CodeCommit\. For example, to use a JSON file named *trigger\.json* to create the trigger:

   **aws codecommit put\-repository\-triggers \-\-cli\-input\-json file://*trigger\.json***

   This command returns a [configuration ID](https://docs.aws.amazon.com/codecommit/latest/APIReference/API_PutRepositoryTriggers.html#-PutRepositoryTriggers-response-configurationId), similar to the following:

   ```
   {
       "configurationId": "0123456-I-AM-AN-EXAMPLE"
   }
   ```

1. To view the configuration of the trigger, run the get\-repository\-triggers command, specifying the name of the repository:

   **aws codecommit get\-repository\-triggers \-\-repository\-name *MyDemoRepo***

   This command returns the structure of all triggers configured for the repository, similar to the following:

   ```
   {
       "configurationId": "0123456-I-AM-AN-EXAMPLE",
       "triggers": [
           {
               "events": [
                   "all"
               ],
               "destinationArn": "arn:aws:sns:us-east-2:80398EXAMPLE:MySNSTopic",
               "branches": [
                   "master",
                   "preprod"
               ],
               "name": "MyFirstTrigger",
               "customData": "Project ID 12345"
           }
       ]
   }
   ```

1. To test the functionality of the trigger itself, make and push a commit to the repository where you configured the trigger\. You should see a response from the Amazon SNS topic\. For example, if you configured the Amazon SNS topic to send an email, you should see an email from Amazon SNS in the email account subscribed to the topic\.

   The following is example output from an email sent from Amazon SNS in response to a push to a CodeCommit repository:

   ```
   {  
     "Records":[  
        {  
           "awsRegion":"us-east-2",
           "codecommit":{
              "references" : [
                 {
                       "commit":"317f8570EXAMPLE",
                       "created":true,
                       "ref":"refs/heads/NewBranch"
                 },
                 {
                       "commit":"4c925148EXAMPLE",
                       "ref":"refs/heads/preprod",
                 }
               ]
             },
           "eventId":"11111-EXAMPLE-ID",
           "eventName":"ReferenceChange",
           "eventPartNumber":1,
           "eventSource":"aws:codecommit",
           "eventSourceARN":"arn:aws:codecommit:us-east-2:80398EXAMPLE:MyDemoRepo",
           "eventTime":"2016-02-09T00:08:11.743+0000",
           "eventTotalParts":1,
           "eventTriggerConfigId":"0123456-I-AM-AN-EXAMPLE",
           "eventTriggerName":"MyFirstTrigger",
           "eventVersion":"1.0",
           "customData":"Project ID 12345", 
           "userIdentityARN":"arn:aws:iam::80398EXAMPLE:user/JaneDoe-CodeCommit",
        }
     ]
   }
   ```