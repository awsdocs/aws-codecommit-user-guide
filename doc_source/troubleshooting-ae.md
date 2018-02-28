# Troubleshooting Access Errors and AWS CodeCommit<a name="troubleshooting-ae"></a>

The following information might help you troubleshoot access errors you might see when connecting with AWS CodeCommit repositories\.


+ [Access Error: Prompted for AWS User Name When Connecting to an AWS CodeCommit Repository](#troubleshooting-ae1)
+ [Access Error: Prompted for User Name and Password When Connecting to an AWS CodeCommit Repository from Windows](#troubleshooting-ae1w)
+ [Access Error: Public Key Denied When Connecting to an AWS CodeCommit Repository](#troubleshooting-ae2)

## Access Error: Prompted for AWS User Name When Connecting to an AWS CodeCommit Repository<a name="troubleshooting-ae1"></a>

**Problem:** When you try to use Git to communicate with an AWS CodeCommit repository, a message appears prompting you for your AWS user name\.

**Possible fixes:** Configure your AWS profile or make sure the profile you are using is the one you configured for working with AWS CodeCommit\. For more information about setting up, see [Setting Up ](setting-up.md)\. For more information about IAM, access keys, and secret keys, see [Managing Access Keys for IAM Users](http://docs.aws.amazon.com/IAM/latest/UserGuide/ManagingCredentials.html) and [How Do I Get Credentials?](http://docs.aws.amazon.com/IAM/latest/UserGuide/IAM_Introduction.html#IAM_SecurityCredentials)\.

## Access Error: Prompted for User Name and Password When Connecting to an AWS CodeCommit Repository from Windows<a name="troubleshooting-ae1w"></a>

**Problem:** When you try to use Git to communicate with an AWS CodeCommit repository, you see a pop\-up dialog box asking for your user name and password\.

**Possible fixes:** This might be the built\-in credential management system for Windows\. Depending on your configuration, do one of the following:

+ If you are using HTTPS with Git credentials, your Git credentials are not yet stored in the system\. Provide the Git credentials and continue\. You should not be prompted again\. For more information, see [For HTTPS Users Using Git Credentials](setting-up-gc.md)\.

+ If you are using HTTPS with the credential helper for AWS CodeCommit, it is not compatible with the Windows credential management system\. Choose **Cancel**\. 

  This might also be an indication that you installed the Git Credential Manager as part of installing Git for Windows\. The Git Credential Manager is not compatible with AWS CodeCommit\. Consider uninstalling it\. 

  For more information, see [For HTTPS Connections on Windows with the AWS CLI Credential Helper](setting-up-https-windows.md) and [Git for Windows: I Installed Git for Windows, but I Am Denied Access to My Repository \(403\)](troubleshooting-ch.md#troubleshooting-windowshttps)\.

## Access Error: Public Key Denied When Connecting to an AWS CodeCommit Repository<a name="troubleshooting-ae2"></a>

**Problem:** When you try to use an SSH endpoint to communicate with an AWS CodeCommit repository, an error message appears containing the phrase `Error: public key denied`\.

**Possible fixes:** The most common reason for this error is that you have not completed set up for SSH connections\. Configure a public and private SSH key pair, and then associate the public key with your IAM user\. For more information about configuring SSH, see [For SSH Connections on Linux, macOS, or Unix](setting-up-ssh-unixes.md) and [For SSH Connections on Windows](setting-up-ssh-windows.md)\. 