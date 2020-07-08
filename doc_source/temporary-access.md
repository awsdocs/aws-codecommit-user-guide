# Connecting to AWS CodeCommit repositories with rotating credentials<a name="temporary-access"></a>

You can give users access to your AWS CodeCommit repositories without configuring IAM users for them or using an access key and secret key\.  Instead of creating an IAM user, you can use existing identities from AWS Directory Service, your enterprise user directory, or a web identity provider\. These are known as *federated users*\. AWS assigns a role to a federated user when access is requested through an [identity provider](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers.html)\. For more information about federated users, see [Federated Users and Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction_access-management.html#intro-access-roles) in the *IAM User Guide*\. You can also configure role\-based access for IAM users to access CodeCommit repositories in separate AWS accounts \(a technique known as *cross\-account access*\)\. For a walkthrough of configuring cross\-account access to a repository, see [Configure cross\-account access to an AWS CodeCommit repository using roles](cross-account.md)\. 

You can configure access for users who want or must authenticate through methods such as:
+ Security Assertion Markup Language \(SAML\)
+ Multi\-factor authentication \(MFA\)
+ Federation
+ Login with Amazon
+ Amazon Cognito
+ Facebook
+ Google
+ OpenID Connect \(OIDC\)\-compatible identity provider

**Note**  
The following information applies only to the use of git\-remote\-codecommit or the AWS CLI credential helper to connect to CodeCommit repositories\. Because the recommended approach for temporary or federated access to CodeCommit is to set up git\-remote\-codecommit, this topic provides examples using that utility\. For more information, see [Setup steps for HTTPS connections to AWS CodeCommit with git\-remote\-codecommit](setting-up-git-remote-codecommit.md)\.   
You cannot use SSH or Git credentials and HTTPS to connect to CodeCommit repositories with rotating or temporary access credentials\. 

You do not need to complete these steps if all of the following requirements are true:
+ You are signed in to an Amazon EC2 instance\.
+ You are using Git and HTTPS with the AWS CLI credential helper to connect from the Amazon EC2 instance to CodeCommit repositories\.
+ The Amazon EC2 instance has an attached IAM instance profile that contains the access permissions described in [For HTTPS connections on Linux, macOS, or Unix with the AWS CLI credential helper](setting-up-https-unixes.md) or [For HTTPS connections on Windows with the AWS CLI credential helper](setting-up-https-windows.md)\.
+ You have installed and configured the Git credential helper on the Amazon EC2 instance, as described in [For HTTPS connections on Linux, macOS, or Unix with the AWS CLI credential helper](setting-up-https-unixes.md) or [For HTTPS connections on Windows with the AWS CLI credential helper](setting-up-https-windows.md)\.

Amazon EC2 instances that meet the preceding requirements are already set up to communicate temporary access credentials to CodeCommit on your behalf\.

**Note**  
You can configure and use git\-remote\-codecommit on Amazon EC2 instances\.

To give users temporary access to your CodeCommit repositories, complete the following steps\.

## Step 1: Complete the prerequisites<a name="temporary-access-prerequisites"></a>

Complete the setup steps to provide a user with access to your CodeCommit repositories using rotating credentials: 
+ For cross\-account access, see [Walkthrough: Delegating Access Across AWS Accounts Using IAM Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/roles-walkthrough-crossacct.html) and [Configure cross\-account access to an AWS CodeCommit repository using roles](cross-account.md)\.
+ For SAML and federation, see [ Using Your Organization's Authentication System to Grant Access to AWS Resources](https://docs.aws.amazon.com/STS/latest/UsingSTS/STSUseCases.html#IdentityBrokerApplication) and [About AWS STS SAML 2\.0\-based Federation](https://docs.aws.amazon.com/STS/latest/UsingSTS/CreatingSAML.html)\.
+ For MFA, see [Using Multi\-Factor Authentication \(MFA\) Devices with AWS](https://docs.aws.amazon.com/IAM/latest/UserGuide/Using_ManagingMFA.html) and [Creating Temporary Security Credentials to Enable Access for IAM Users](https://docs.aws.amazon.com/STS/latest/UsingSTS/CreatingSessionTokens.html)\.
+ For Login with Amazon, Amazon Cognito, Facebook, Google, or any OIDC\-compatible identity provider, see [About AWS STS Web Identity Federation](https://docs.aws.amazon.com/STS/latest/UsingSTS/web-identity-federation.html)\.

Use the information in [Authentication and access control for AWS CodeCommit](auth-and-access-control.md) to specify the CodeCommit permissions you want to grant the user\.

## Step 2: Get role name or access credentials<a name="temporary-access-get-credentials"></a>

If you want your users to access repositories by assuming a role, provide your users with the Amazon Resource Name \(ARN\) of that role\. Otherwise, depending on the way you set up access, your user can get rotating credentials in one of the following ways:
+ For cross\-account access, call the AWS CLI [assume\-role](https://docs.aws.amazon.com/cli/latest/reference/sts/assume-role.html) command or call the AWS STS [AssumeRole](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) API\.
+ For SAML, call the AWS CLI [assume\-role\-with\-saml](https://docs.aws.amazon.com/cli/latest/reference/sts/assume-role-with-saml.html) command or the AWS STS [AssumeRoleWithSAML](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRoleWithSAML.html) API\.
+ For federation, call the AWS CLI [assume\-role](https://docs.aws.amazon.com/cli/latest/reference/sts/assume-role.html) or [get\-federation\-token](https://docs.aws.amazon.com/cli/latest/reference/sts/get-federation-token.html) commands or the AWS STS [AssumeRole](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) or [GetFederationToken](https://docs.aws.amazon.com/STS/latest/APIReference/API_GetFederationToken.html) APIs\.
+ For MFA, call the AWS CLI [get\-session\-token](https://docs.aws.amazon.com/cli/latest/reference/sts/get-session-token.html) command or the AWS STS [GetSessionToken](https://docs.aws.amazon.com/STS/latest/APIReference/API_GetSessionToken.html) API\.
+ For Login with Amazon, Amazon Cognito, Facebook, Google, or any OIDC\-compatible identity provider, call the AWS CLI [assume\-role\-with\-web\-identity](https://docs.aws.amazon.com/cli/latest/reference/sts/assume-role-with-web-identity.html) command or the AWS STS [AssumeRoleWithWebIdentity](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRoleWithWebIdentity.html) API\.

## Step 3: Install git\-remote\-codecommit and configure the AWS CLI<a name="temporary-access-configure-credentials"></a>

You must configure your local computer to use the access credentials by installing [https://pypi.org/project/git-remote-codecommit/](https://pypi.org/project/git-remote-codecommit/) and configuring a profile in the AWS CLI\.

1. Follow the instructions in [Setting up ](setting-up.md) to set up the AWS CLI\. Use the aws configure command to configure one or more profiles\. Consider creating a named profile to use when you connect to CodeCommit repositories using rotating credentials\.

1. You can associate the credentials with the user's AWS CLI named profile in one of the following ways\. 
   + If you are assuming a role to access CodeCommit, configure a named profile with the information required to assume that role\. For example, if you want to assume a role named *CodeCommitAccess* in the AWS account 111111111111, you can configure a default profile to use when working with other AWS resources and a named profile to use when assuming that role\. The following commands create a named profile named *CodeAccess* that assumes a role named *CodeCommitAccess*\. The user name *Maria Garcia* is associated with the session and the default profile is set as the source of its AWS credentials:

     ```
     aws configure set role_arn arn:aws:iam:::111111111111:role/CodeCommitAccess --profile CodeAccess
     aws configure set source_profile default --profile CodeAccess
     aws configure set role_session_name "Maria Garcia" --profile CodeAccess
     ```

     If you want to verify the changes, manually view or edit the `~/.aws/config` file \(for Linux\) or the `%UserProfile%.aws\config` file \(for Windows\) and review the information under the named profile\. For example, your file might look similar to the following:

     ```
     [default]
     region = us-east-1
     output = json
     
     [profile CodeAccess]
     source_profile = default
     role_session_name = Maria Garcia
     role_arn = arn:aws:iam:::111111111111:role/CodeCommitAccess
     ```

      After you have configured your named profile, you can then clone CodeCommit repositories with the git\-remote\-codecommit utility using the named profile\. For example, to clone a repository named *MyDemoRepo*: 

     ```
     git clone codecommit://CodeAccess@MyDemoRepo
     ```
   + If you are using web identity federation and OpenID Connect \(OIDC\), configure a named profile that makes the AWS Security Token Service \(AWS STS\) `AssumeRoleWithWebIdentity` API call on your behalf to refresh temporary credentials\. Use the aws configure set command or manually edit the `~/.aws/credentials` file \(for Linux\) or the `%UserProfile%.aws\credentials` file \(for Windows\) to add an AWS CLI named profile with the required setting values\. For example, to create a profile that assumes the *CodeCommitAccess* role and uses a web idenity token file \~/*my\-credentials**/my\-token\-file*:

     ```
     [CodeCommitWebIdentity]
     role_arn = arn:aws:iam:::111111111111:role/CodeCommitAccess
     web_identity_token_file=~/my-credentials/my-token-file
     role_session_name = Maria Garcia
     ```

   For more information, see [Configuring the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html) and [Using an IAM Role in the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-role.html) in the *AWS Command Line Interface User Guide*\.

## Step 4: Access the CodeCommit repositories<a name="temporary-access-use-credentials"></a>

Assuming your user has followed the instructions in [Connect to a repository](how-to-connect.md) to connect to the CodeCommit repositories, the user then uses the extended functionality provided by git\-remote\-codecommit and Git to call git clone, git push, and git pull to clone, push to, and pull from, the CodeCommit repositories to which he or she has access\. For example, to clone a repository:

```
git clone codecommit://CodeAccess@MyDemoRepo
```

Git commit, push, and pull commands use regular Git syntax\. 

When the user uses the AWS CLI and specifies the AWS CLI named profile associated with the rotating access credentials, results scoped to that profile are returned\.