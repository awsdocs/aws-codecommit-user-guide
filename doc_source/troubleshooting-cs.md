# Troubleshooting Console Errors and AWS CodeCommit<a name="troubleshooting-cs"></a>

The following information might help you troubleshoot console errors you might see when using AWS CodeCommit repositories\.


+ [Access Error: Encryption Key Access Denied for an AWS CodeCommit Repository from the Console or the AWS CLI](#troubleshooting-ae3)
+ [Console Error: Cannot Browse the Code in an AWS CodeCommit Repository from the Console](#troubleshooting-cs1)

## Access Error: Encryption Key Access Denied for an AWS CodeCommit Repository from the Console or the AWS CLI<a name="troubleshooting-ae3"></a>

**Problem:** When you try to access AWS CodeCommit from the console or the AWS CLI, an error message appears containing the phrase `EncryptionKeyAccessDeniedException` or `User is not authorized for the KMS default master key for CodeCommit 'aws/codecommit' in your account`\.

**Possible fixes:** The most common cause for this error is that your AWS account is not subscribed to AWS Key Management Service, which is required for AWS CodeCommit\. Open the IAM console, choose **Encryption Keys**, and then choose **Get Started Now**\. If you see a message that you are not currently subscribed to the AWS Key Management Service service, follow the instructions on that page to subscribe\. For more information about AWS CodeCommit and AWS Key Management Service, see [AWS KMS and Encryption](encryption.md)\. 

## Console Error: Cannot Browse the Code in an AWS CodeCommit Repository from the Console<a name="troubleshooting-cs1"></a>

**Problem:** When you try to browse the contents of a repository from the console, an error message appears denying access\.

**Possible fixes:** The most common cause for this error is that an IAM policy applied to your AWS account denies one or more of the permissions required for browsing code from the AWS CodeCommit console\. For more information about AWS CodeCommit access permissions and browsing, see [Authentication and Access Control for AWS CodeCommit](auth-and-access-control.md)\. 