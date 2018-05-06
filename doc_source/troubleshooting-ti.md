# Troubleshooting Triggers and AWS CodeCommit<a name="troubleshooting-ti"></a>

The following information might help you troubleshoot issues with triggers you might see in AWS CodeCommit\.

**Topics**
+ [Trigger Error: A Repository Trigger Does Not Run When Expected](#troubleshooting-ti1)

## Trigger Error: A Repository Trigger Does Not Run When Expected<a name="troubleshooting-ti1"></a>

**Problem:** One or more triggers configured for a repository does not appear to run or does not run as expected\.

**Possible fixes:** If the target of the trigger is a AWS Lambda function, make sure you have configured the function's resource policy for access by AWS CodeCommit\. For more information, see [Example 2: Create a Policy for AWS Lambda Integration](auth-and-access-control-iam-identity-based-access-control.md#access-permissions-lambda-int)\.

Alternatively, edit the trigger and make sure the events for which you want to trigger actions have been selected and that the branches for the trigger include the branch where you want to see responses to actions\. Try changing the settings for the trigger to **All repository events** and **All branches** and then testing the trigger\. For more information, see [Edit Triggers for a Repository](how-to-notify-edit.md)\.