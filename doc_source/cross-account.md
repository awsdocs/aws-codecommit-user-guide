# Configure Cross\-Account Access to an AWS CodeCommit Repository<a name="cross-account"></a>

You can configure access to AWS CodeCommit repositories for IAM users and groups in another AWS account\. This is often referred to as *cross\-account access*\. This section provides examples and step\-by\-step instructions for configuring cross\-account access for a repository named *MySharedDemoRepo* in the US East \(Ohio\) Region in an AWS account \(referred to as AccountA\) to IAM users who belong to an IAM group named *DevelopersWithCrossAccountRepositoryAccess* in another AWS account \(referred to as AccountB\)\.

This section is divided into three parts:
+ Actions for the Administrator in AccountA\.
+ Actions for the Administrator in AccountB\.
+ Actions for the repository user in AccountB\.

To configure cross\-account access:
+ The administrator in AccountA signs in as an IAM user with the permissions required to create and manage repositories in AWS CodeCommit and create roles in IAM\. If you are using managed policies, apply IAMFullAccess and AWSCodeCommitFullAccess to this IAM user\.

  The example account ID for AccountA is *111122223333*\.
+ The administrator in AccountB, signs in as an IAM user with the permissions required to create and manage IAM users and groups, and to configure policies for users and groups\. If you are using managed policies, apply IAMFullAccess to this IAM user\.

  The example account ID for AccountB is *888888888888*\.
+ The repository user in AccountB, to emulate the activities of a developer, signs in as an IAM user who is a member of the IAM group created to allow access to the AWS CodeCommit repository in AccountA\. This account must be configured with: 
  + AWS console access\.
  + An access key and secret key to use when installing and configuring the AWS CLI credential helper on a local computer or Amazon EC2 instance, as described in [For HTTPS Connections on Linux, macOS, or Unix with the AWS CLI Credential Helper](setting-up-https-unixes.md) or [For HTTPS Connections on Windows with the AWS CLI Credential Helper](setting-up-https-windows.md)\.

  For more information, see [IAM users](http://docs.aws.amazon.com/IAM/latest/UserGuide/introduction_identity-management.html#intro-identity-users)\.

**Topics**
+ [Cross\-Account Repository Access: Actions for the Administrator in AccountA](cross-account-administrator-a.md)
+ [Cross Account Repository Access: Actions for the Administrator in AccountB](cross-account-administrator-b.md)
+ [Cross\-Account Repository Access: Actions for the Repository User in AccountB](cross-account-user-b.md)