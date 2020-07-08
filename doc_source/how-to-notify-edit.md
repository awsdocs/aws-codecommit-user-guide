# Edit triggers for an AWS CodeCommit repository<a name="how-to-notify-edit"></a>

You can edit the triggers that have been created for a CodeCommit repository\. You can change the events and branches for the trigger, the action taken in response to the event, and other settings\. 

**Topics**
+ [Edit a trigger for a repository \(console\)](#how-to-notify-edit-console)
+ [Edit a trigger for a repository \(AWS CLI\)](#how-to-notify-edit-cli)

## Edit a trigger for a repository \(console\)<a name="how-to-notify-edit-console"></a>

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In **Repositories**, choose the repository where you want to edit a trigger for repository events\.

1. In the navigation pane for the repository, choose **Settings**, and then choose **Triggers**\.

1. From the list of triggers for the repository, choose the trigger you want to edit, and then choose **Edit**\. 

1. Make the changes you want to the trigger, and then choose **Save**\.

## Edit a trigger for a repository \(AWS CLI\)<a name="how-to-notify-edit-cli"></a>

1. At a terminal \(Linux, macOS, or Unix\) or command prompt \(Windows\), run the get\-repository\-triggers command to create a JSON file with the structure of all of the triggers configured for your repository\. For example, to create a JSON file named *MyTriggers\.json* with the structure of all of the triggers configured for a repository named *MyDemoRepo*:

   ```
   aws codecommit get-repository-triggers --repository-name MyDemoRepo >MyTriggers.json
   ```

   This command returns nothing, but a file named *MyTriggers\.json* is created in the directory where you ran the command\.

1. Edit the JSON file in a plain\-text editor and make changes to the trigger block of the trigger you want to edit\. Replace the `configurationId` pair with a `repositoryName` pair\. Save the file\.

   For example, if you want to edit a trigger named *MyFirstTrigger* in the repository named *MyDemoRepo* so that it applies to all branches, replace `configurationId` with `repositoryName`, and remove the specified `master` and `preprod` branches in *red italic text*\. By default, if no branches are specified, the trigger applies to all branches in the repository:

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

1. At the terminal or command line, run the put\-repository\-triggers command\. This updates all triggers for the repository, including the changes you made to the *MyFirstTrigger* trigger:

   ```
   aws codecommit put-repository-triggers --repository-name MyDemoRepo file://MyTriggers.json
   ```

   This command returns a configuration ID, similar to the following:

   ```
   {
       "configurationId": "0123456-I-AM-AN-EXAMPLE"
   }
   ```