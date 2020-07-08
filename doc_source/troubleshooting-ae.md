# Troubleshooting access errors and AWS CodeCommit<a name="troubleshooting-ae"></a>

The following information might help you troubleshoot access errors when connecting with AWS CodeCommit repositories\.

**Topics**
+ [Access error: I am prompted for a user name and password when I connect to a CodeCommit repository from Windows](#troubleshooting-ae1w)
+ [Access error: Public key denied when connecting to a CodeCommit repository](#troubleshooting-ae2)
+ [Access error: “Rate Exceeded” or “429” message when connecting to a CodeCommit repository](#troubleshooting-ae3)

## Access error: I am prompted for a user name and password when I connect to a CodeCommit repository from Windows<a name="troubleshooting-ae1w"></a>

**Problem:** When you try to use Git to communicate with a CodeCommit repository, you see a dialog box that prompts you for your user name and password\.

**Possible fixes:** This might be the built\-in credential management system for Windows\. Depending on your configuration, do one of the following:
+ If you are using HTTPS with Git credentials, your Git credentials are not yet stored in the system\. Provide the Git credentials and continue\. You should not be prompted again\. For more information, see [For HTTPS users using Git credentials](setting-up-gc.md)\.
+ If you are using HTTPS with the credential helper for AWS CodeCommit, it is not compatible with the Windows credential management system\. Choose **Cancel**\. 

  This might also be an indication that you installed the Git Credential Manager when you installed Git for Windows\. The Git Credential Manager is not compatible with the credential helper for CodeCommit included in the AWS CLI\. Consider uninstalling the Git Credential Manager\. You can also install and configure git\-remote\-codecommit as an alternative to using the credential helper for CodeCommit\. 

  For more information, see [Setup steps for HTTPS connections to AWS CodeCommit with git\-remote\-codecommit](setting-up-git-remote-codecommit.md), [For HTTPS connections on Windows with the AWS CLI credential helper](setting-up-https-windows.md), and [Git for Windows: I installed Git for Windows, but I am denied access to my repository \(403\)](troubleshooting-ch.md#troubleshooting-windowshttps)\.

## Access error: Public key denied when connecting to a CodeCommit repository<a name="troubleshooting-ae2"></a>

**Problem:** When you try to use an SSH endpoint to communicate with a CodeCommit repository, an error message appears containing the phrase `Error: public key denied`\.

**Possible fixes:** The most common reason for this error is that you have not completed setup for SSH connections\. Configure a public and private SSH key pair, and then associate the public key with your IAM user\. For more information about configuring SSH, see [For SSH connections on Linux, macOS, or Unix](setting-up-ssh-unixes.md) and [For SSH connections on Windows](setting-up-ssh-windows.md)\. 

## Access error: “Rate Exceeded” or “429” message when connecting to a CodeCommit repository<a name="troubleshooting-ae3"></a>

**Problem:** When you try to communicate with a CodeCommit repository, a message appears that says “Rate Exceeded” or with an error code of “429”\. Communication either slows significantly or fails\. 

**Cause:** All calls to CodeCommit, whether from an application, the AWS CLI, a Git client, or the AWS Management Console, are subject to a maximum number of requests per second and overall active requests\. You cannot exceed the maximum allowed request rate for an AWS account in any AWS Region\. If requests exceed the maximum rate, you receive an error and further calls are temporarily throttled for your AWS account\. During the throttling period, your connections to CodeCommit are slowed and might fail\.

**Possible fixes:** Take steps to reduce the number of connections or calls to CodeCommit or to spread out requests\. Some approaches to consider:
+ **Implement jitter in requests, particularly in periodic polling requests**

  If you have an application that is polling CodeCommit periodically and this application is running on multiple Amazon EC2 instances, introduce jitter \(a random amount of delay\) so that different Amazon EC2 instances do not poll at the same second\. We recommend a random number from 0 to 59 seconds to evenly distribute polling mechanisms across a one\-minute timeframe\.
+ **Use an event\-based architecture rather than polling**

  Rather than polling, use an event\-based architecture so that calls are only made when an event occurs\. Consider using CloudWatch Events notifications for [AWS CodeCommit events](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/EventTypes.html#codecommit_event_type) to trigger your workflow\.
+ **Implement error retries and exponential backoffs for APIs and automated Git actions**

  Error retries and exponential backoffs can help limit the rate of calls\. Each AWS SDK implements automatic retry logic and exponential backoff algorithms\. For automated Git push and Git pull, you might need to implement your own retry logic\. For more information, see [Error Retries and Exponential Backoff in AWS](https://docs.aws.amazon.com/general/latest/gr/api-retries.html)\.
+ **Request a CodeCommit service quota increase in the AWS Support Center**

  To receive a service limit increase, you must confirm that you have already followed the suggestions offered here, including implementation of error retries or exponential backoff methods\. In your request, you must also provide the AWS Region, AWS account, and timeframe affected by the throttling issues\. 