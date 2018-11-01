--------

 The procedures in this guide support the new console design\. If you choose to use the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\.

--------

# Troubleshooting Configuration Errors and AWS CodeCommit<a name="troubleshooting-cf"></a>

The following information might help you troubleshoot configuration errors you might see when connecting with AWS CodeCommit repositories\.

**Topics**
+ [Configuration Error: Cannot Configure AWS CLI Credentials on macOS](#troubleshooting-cf1)

## Configuration Error: Cannot Configure AWS CLI Credentials on macOS<a name="troubleshooting-cf1"></a>

**Problem:** When you run `aws configure` to configure the AWS CLI, you see a `ConfigParseError` message\.

**Possible fixes:** The most common cause for this error is that a credentials file already exists\. Browse to \~/\.aws and look for a file named `credentials`\. Rename or delete that file, and then run aws configure again\.