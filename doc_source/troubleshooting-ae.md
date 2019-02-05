--------

 The procedures in this guide support the new console design\. If you choose to use the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\.

--------

# Troubleshooting Access Errors and AWS CodeCommit<a name="troubleshooting-ae"></a>

The following information might help you troubleshoot access errors you might see when connecting with AWS CodeCommit repositories\.

**Topics**
+ [Access Error: Prompted for User Name and Password When Connecting to an AWS CodeCommit Repository from Windows](#troubleshooting-ae1w)
+ [Access Error: Public Key Denied When Connecting to an AWS CodeCommit Repository](#troubleshooting-ae2)
+ [Access Error: “Rate Exceeded” or “429” Message When Connecting to an AWS CodeCommit Repository](#troubleshooting-ae3)

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

## Access Error: “Rate Exceeded” or “429” Message When Connecting to an AWS CodeCommit Repository<a name="troubleshooting-ae3"></a>

**Problem:** When you try to communicate with an AWS CodeCommit repository, a message appears stating “Rate Exceeded” or with an error code of “429”\. Communication either slows significantly or fails\. 

**Cause:** All calls to AWS CodeCommit, whether from an application, the AWS CLI, a Git client, or the AWS Management Console, are subject to a maximum number of requests per second and overall active requests\. You cannot exceed the maximum allowed request rate for an AWS account in any AWS Region\. If requests exceed the maximum rate, you receive an error and further calls are temporarily throttled for your AWS account\. During the throttling period, your connections to AWS CodeCommit will be slowed and might fail\.

**Possible fixes:** Take steps to reduce the number of connections or calls to AWS CodeCommit or to spread out requests\. Some approaches to consider:
+ **Implement jitter in requests, particularly in periodic polling requests**

  If you have an application that is polling AWS CodeCommit periodically and this application is running on multiple Amazon EC2 instances, introduce jitter \(a random amount of delay\) so that different Amazon EC2 instances will not poll at the same second\. We recommend a random number from 0 to 59 seconds to evenly distribute polling mechanisms across a one\-minute timeframe\.
+ **Use an event\-based architecture rather than polling**

  Rather than polling, use an event\-based architecture so that calls are only made when an event occurs\. Consuder using CloudWatch Events notifications for [AWS CodeCommit events](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/EventTypes.html#codecommit_event_type) to trigger your workflow\.
+ **Implement error retries and exponential backoffs for APIs and automated Git actions**

  Error retries and exponential backoffs can help limit the rate of calls\. Each AWS SDK impements automatic retry logic and exponential backoff alorithms\. For automated Git push and Git pull, you might need to implement your own retry logic\. For more information, see [Error Retries and Exponential Backoff in AWS](https://docs.aws.amazon.com/general/latest/gr/api-retries.html)\.
+ **Request an AWS CodeCommit service limit increase in the AWS Support Center**

  To receive a service limit increase, you must confirm that you have already followed the above suggestions, including implementation of error retries or exponential backoff methods\. In your request, you must also provide the AWS Region, AWS account, and timeframe affected by the throttling issues\. 