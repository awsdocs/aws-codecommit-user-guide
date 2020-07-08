# Delete triggers from an AWS CodeCommit repository<a name="how-to-notify-delete"></a>

You might want to delete triggers if they are no longer being used\. You cannot undo the deletion of a trigger, but you can create one again\.

**Note**  
If you configured one or more triggers for your repository, deleting the repository does not delete the Amazon SNS topics or Lambda functions you configured as the targets of those triggers\. Be sure to delete those resources, too, if they are no longer needed\.

**Topics**
+ [Delete a trigger from a repository \(console\)](#how-to-notify-delete-console)
+ [Delete a trigger from a repository \(AWS CLI\)](#how-to-notify-delete-cli)

## Delete a trigger from a repository \(console\)<a name="how-to-notify-delete-console"></a>

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In **Repositories**, choose the repository where you want to delete triggers for repository events\.

1. In the navigation pane for the repository, choose **Settings**\. In **Settings**, choose **Triggers**\.

1. Choose the trigger you want to delete from the list of triggers, and then choose **Delete**\.

1. In the dialog box, type **delete** to confirm\.

## Delete a trigger from a repository \(AWS CLI\)<a name="how-to-notify-delete-cli"></a>

1. At a terminal \(Linux, macOS, or Unix\) or command prompt \(Windows\), run the get\-repository\-triggers command to create a JSON file with the structure of all of the triggers configured for your repository\. For example, to create a JSON file named *MyTriggers\.json* with the structure of all of the triggers configured for a repository named MyDemoRepo:

   ```
   aws codecommit get-repository-triggers --repository-name MyDemoRepo >MyTriggers.json
   ```

   This command creates a file named *MyTriggers\.json* in the directory where you ran the command\.

1. Edit the JSON file in a plain\-text editor and remove the trigger block for the trigger you want to delete\. Replace the `configurationId` pair with a `repositoryName` pair\. Save the file\.

   For example, if you want to remove a trigger named *MyFirstTrigger* from the repository named *MyDemoRepo*, you would replace `configurationId` with `repositoryName`, and remove the statement in *red italic text*:

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
           }, 
           {
               "destinationArn": "arn:aws:lambda:us-east-2:80398EXAMPLE:function:MyCodeCommitJSFunction", 
               "branches": [], 
               "name": "MyLambdaTrigger", 
               "events": [
                   "all"
               ]
           }  
       ]
   }
   ```

1. At the terminal or command line, run the put\-repository\-triggers command\. This updates the triggers for the repository and deletes the *MyFirstTrigger* trigger:

   ```
   aws codecommit put-repository-triggers --repository-name MyDemoRepo file://MyTriggers.json
   ```

   This command returns a configuration ID, similar to the following:

   ```
   {
       "configurationId": "0123456-I-AM-AN-EXAMPLE"
   }
   ```
**Note**  
To delete all triggers for a repository named *MyDemoRepo*, your JSON file would look similar to this:  

   ```
   {
       "repositoryName": "MyDemoRepo",
       "triggers": []
   }
   ```