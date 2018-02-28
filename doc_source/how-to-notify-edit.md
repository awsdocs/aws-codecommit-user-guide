# Edit Triggers for an AWS CodeCommit Repository<a name="how-to-notify-edit"></a>

You can edit the triggers that have been created for an AWS CodeCommit repository\. You can change the events and branches for the trigger, the action taken in response to the event, and other settings\. 


+ [Edit a Trigger for a Repository \(Console\)](#how-to-notify-edit-console)
+ [Edit a Trigger for a Repository \(AWS CLI\)](#how-to-notify-edit-cli)

## Edit a Trigger for a Repository \(Console\)<a name="how-to-notify-edit-console"></a>

1. Open the AWS CodeCommit console at [https://console\.aws\.amazon\.com/codecommit](https://console.aws.amazon.com/codecommit)\.

1. From the list of repositories, choose the repository where you want to edit a trigger for repository events\.

1. In the navigation pane for the repository, choose **Settings**\. In **Settings**, choose **Triggers**\.

1. From the list of triggers for the repository, select the trigger you want to edit, and then choose **Edit**\.  

1. Make the changes you want to the trigger, and then choose **Update** to save your changes\.

## Edit a Trigger for a Repository \(AWS CLI\)<a name="how-to-notify-edit-cli"></a>

1. At a terminal \(Linux, macOS, or Unix\) or command prompt \(Windows\), run the get\-repository\-triggers command to create a JSON file with the structure of all of the triggers configured for your repository\. For example, to create a JSON file named *MyTriggers\.json* with the structure of all of the triggers configured for a repository named *MyDemoRepo*:

   ```
   aws codecommit get-repository-triggers --repository-name MyDemoRepo >MyTriggers.json
   ```

   This command returns nothing, but a file named *MyTriggers\.json* is created in the directory where you ran the command\.

1. Edit the JSON file in a plain\-text editor and make changes to the trigger block of the trigger you want to edit\. Replace the `configurationId` pair with a `repositoryName` pair\. Save the file\.

   For example, if you want to edit a trigger named *MyFirstTrigger* in the repository named *MyDemoRepo* so that it applies to all branches, you would replace `configurationId` with `repositoryName`, and remove the specified `master` and `preprod` branches in *red italic text*\. By default, if no branches are specified, the trigger will apply to all branches in the repository:

   ```
   {
       "repositoryName": "MyDemoRepo", 
       "triggers": [
           {
               "destinationArn": "arn:aws:sns:us-east-2:80398EXAMPLE:MyCodeCommitTopic", 
               "branches": [
                   "master", 
                   "preprod"
               ], 
               "name": "MyFirstTrigger", 
               "customData": "", 
               "events": [
                   "all"
               ]
           }  
       ]
   }
   ```

1. At the terminal or command line, run the put\-repository\-triggers command\. This will update all triggers for the repository, including the changes you made to the *MyFirstTrigger* trigger:

   ```
   aws codecommit put-repository-triggers --repository-name MyDemoRepo file://MyTriggers.json
   ```

   This command returns a configuration ID, similar to the following:

   ```
   {
       "configurationId": "0123456-I-AM-AN-EXAMPLE"
   }
   ```