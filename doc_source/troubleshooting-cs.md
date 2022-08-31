# Troubleshooting console errors and AWS CodeCommit<a name="troubleshooting-cs"></a>

The following information might help you troubleshoot console errors when using AWS CodeCommit repositories\.

**Topics**
+ [Access error: Encryption key access denied for a CodeCommit repository from the console or AWS CLI](#troubleshooting-ae3)
+ [Console error: Cannot browse the code in a CodeCommit repository from the console](#troubleshooting-cs1)
+ [Display error: Cannot view a file or a comparison between files](#troubleshooting-dd1)

## Access error: Encryption key access denied for a CodeCommit repository from the console or AWS CLI<a name="troubleshooting-ae3"></a>

**Problem:** When you try to access CodeCommit from the console or the AWS CLI, an error message appears containing the phrase `EncryptionKeyAccessDeniedException` or `User is not authorized for the KMS default master key for CodeCommit 'aws/codecommit' in your account`\.

**Possible fixes:** The most common cause for this error is that your Amazon Web Services account is not subscribed to AWS Key Management Service, which is required for CodeCommit\. Open the AWS KMS console, choose **AWS managed keys**, and then choose **Get Started Now**\. If you see a message that you are not currently subscribed to the AWS Key Management Service service, follow the instructions on that page to subscribe\. For more information about CodeCommit and AWS Key Management Service, see [AWS KMS and encryption](encryption.md)\. 

## Console error: Cannot browse the code in a CodeCommit repository from the console<a name="troubleshooting-cs1"></a>

**Problem:** When you try to browse the contents of a repository from the console, an error message appears denying access\.

**Possible fixes:** The most common cause for this error is that an IAM policy applied to your Amazon Web Services account denies one or more of the permissions required for browsing code from the CodeCommit console\. For more information about CodeCommit access permissions and browsing, see [Authentication and access control for AWS CodeCommit](auth-and-access-control.md)\. 

## Display error: Cannot view a file or a comparison between files<a name="troubleshooting-dd1"></a>

**Problem:** When you try to view a file or a comparison between two versions of a file in the CodeCommit console, an error appears stating that the file or difference is too large to display\.

**Possible fixes:** The most common cause for this error is that either the file is too large to display, contains one or more lines that exceeds the character limit for a single line in a file, or that the difference between the two versions of the file exceeds the line limit\. For more information, see [Quotas](limits.md)\. To view the file or the differences between the version of the file, you can open the file locally in your preferred IDE, use a Git diff tool, or run the git diff command\. 