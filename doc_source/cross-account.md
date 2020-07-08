# Configure cross\-account access to an AWS CodeCommit repository using roles<a name="cross-account"></a>

You can configure access to CodeCommit repositories for IAM users and groups in another AWS account\. This is often referred to as *cross\-account access*\. This section provides examples and step\-by\-step instructions for configuring cross\-account access for a repository named *MySharedDemoRepo* in the US East \(Ohio\) Region in an AWS account \(referred to as AccountA\) to IAM users who belong to an IAM group named *DevelopersWithCrossAccountRepositoryAccess* in another AWS account \(referred to as AccountB\)\.

This section is divided into three parts:
+ Actions for the Administrator in AccountA\.
+ Actions for the Administrator in AccountB\.
+ Actions for the repository user in AccountB\.

To configure cross\-account access:
+ The administrator in AccountA signs in as an IAM user with the permissions required to create and manage repositories in CodeCommit and create roles in IAM\. If you are using managed policies, apply IAMFullAccess and AWSCodeCommitFullAccess to this IAM user\.

  The example account ID for AccountA is *111122223333*\.
+ The administrator in AccountB signs in as an IAM user with the permissions required to create and manage IAM users and groups, and to configure policies for users and groups\. If you are using managed policies, apply IAMFullAccess to this IAM user\.

  The example account ID for AccountB is *888888888888*\.
+ The repository user in AccountB, to emulate the activities of a developer, signs in as an IAM user who is a member of the IAM group created to allow access to the CodeCommit repository in AccountA\. This account must be configured with: 
  + AWS Management Console access\.
  + An access key and secret key to use when connecting to AWS resources and the ARN of the role to assume when accessing repositories in AccountA\.
  + The git\-remote\-codecommit utility on the local computer where the repository is cloned\. This utility requires Python and its installer, pip\. You can download the utility from [https://pypi.org/project/git-remote-codecommit/](https://pypi.org/project/git-remote-codecommit/) on the Python Package Index website\.

  For more information, see [Setup steps for HTTPS connections to AWS CodeCommit with git\-remote\-codecommit](setting-up-git-remote-codecommit.md) and [IAM users](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction_identity-management.html#intro-identity-users)\.

**Topics**
+ [Cross\-account repository access: Actions for the administrator in AccountA](cross-account-administrator-a.md)
+ [Cross\-account repository access: Actions for the administrator in AccountB](cross-account-administrator-b.md)
+ [Cross\-account repository access: Actions for the repository user in AccountB](cross-account-user-b.md)