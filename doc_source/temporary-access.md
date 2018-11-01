--------

 The procedures in this guide support the new console design\. If you choose to use the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\.

--------

# Temporary Access to AWS CodeCommit Repositories<a name="temporary-access"></a>

You can allow users temporary access to your AWS CodeCommit repositories\. For example, you might do this to allow IAM users to access AWS CodeCommit repositories in separate AWS accounts \(a technique known as *cross\-account access*\)\. For a walkthrough of configuring cross\-account access to a repository, see [Configure Cross\-Account Access to an AWS CodeCommit Repository](cross-account.md)\. 

You can also configure access for users who want or must authenticate through methods such as:
+ Security Assertion Markup Language \(SAML\)
+ Multi\-factor authentication \(MFA\)
+ Federation
+ Login with Amazon
+ Amazon Cognito
+ Facebook
+ Google
+ OpenID Connect \(OIDC\)\-compatible identity provider

**Note**  
The following information applies only to the use of the AWS CLI credential helper to connect to AWS CodeCommit repositories\. You cannot use SSH or Git credentials and HTTPS to connect to AWS CodeCommit repositories with temporary access credentials\. 

To give users temporary access to your AWS CodeCommit repositories, complete the following steps\.

Do not complete these steps if all of the following requirements are true:
+ You are signed in to an Amazon EC2 instance\.
+ You are using Git and HTTPS with the AWS CLI credential helper to connect from the Amazon EC2 instance to AWS CodeCommit repositories\.
+ The Amazon EC2 instance has an attached IAM instance profile that contains the access permissions described in [For HTTPS Connections on Linux, macOS, or Unix with the AWS CLI Credential Helper](setting-up-https-unixes.md) or [For HTTPS Connections on Windows with the AWS CLI Credential Helper](setting-up-https-windows.md)\.
+ You have installed and configured the Git credential helper on the Amazon EC2 instance, as described in [For HTTPS Connections on Linux, macOS, or Unix with the AWS CLI Credential Helper](setting-up-https-unixes.md) or [For HTTPS Connections on Windows with the AWS CLI Credential Helper](setting-up-https-windows.md)\.

Amazon EC2 instances that meet the preceding requirements are already set up to communicate temporary access credentials to AWS CodeCommit on your behalf\.

## Step 1: Complete the Prerequisites<a name="temporary-access-prerequisites"></a>

Complete the setup steps to provide a user with temporary access to your AWS CodeCommit repositories: 
+ For cross\-account access, see [Walkthrough: Delegating Access Across AWS Accounts Using IAM Roles](http://docs.aws.amazon.com/IAM/latest/UserGuide/roles-walkthrough-crossacct.html)\.
+ For SAML and federation, see [ Using Your Organization's Authentication System to Grant Access to AWS Resources](http://docs.aws.amazon.com/STS/latest/UsingSTS/STSUseCases.html#IdentityBrokerApplication) and [About AWS STS SAML 2\.0\-based Federation](http://docs.aws.amazon.com/STS/latest/UsingSTS/CreatingSAML.html)\.
+ For MFA, see [Using Multi\-Factor Authentication \(MFA\) Devices with AWS](http://docs.aws.amazon.com/IAM/latest/UserGuide/Using_ManagingMFA.html) and [Creating Temporary Security Credentials to Enable Access for IAM Users](http://docs.aws.amazon.com/STS/latest/UsingSTS/CreatingSessionTokens.html)\.
+ For Login with Amazon, Amazon Cognito, Facebook, Google, or any OIDC\-compatible identity provider, see [About AWS STS Web Identity Federation](http://docs.aws.amazon.com/STS/latest/UsingSTS/web-identity-federation.html)\.

Use the information in [Authentication and Access Control for AWS CodeCommit](auth-and-access-control.md) to specify the AWS CodeCommit permissions you want to temporarily grant the user\.

## Step 2: Get Temporary Access Credentials<a name="temporary-access-get-credentials"></a>

Depending on the way you set up temporary access, your user can get temporary access credentials in one of the following ways:
+ For cross\-account access, call the AWS CLI [assume\-role](http://docs.aws.amazon.com/cli/latest/reference/sts/assume-role.html) command or call the AWS STS [AssumeRole](http://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) API\.
+ For SAML, call the AWS CLI [assume\-role\-with\-saml](http://docs.aws.amazon.com/cli/latest/reference/sts/assume-role-with-saml.html) command or the AWS STS [AssumeRoleWithSAML](http://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRoleWithSAML.html) API\.
+ For federation, call the AWS CLI [assume\-role](http://docs.aws.amazon.com/cli/latest/reference/sts/assume-role.html) or [get\-federation\-token](http://docs.aws.amazon.com/cli/latest/reference/sts/get-federation-token.html) commands or the AWS STS [AssumeRole](http://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) or [GetFederationToken](http://docs.aws.amazon.com/STS/latest/APIReference/API_GetFederationToken.html) APIs\.
+ For MFA, call the AWS CLI [get\-session\-token](http://docs.aws.amazon.com/cli/latest/reference/sts/get-session-token.html) command or the AWS STS [GetSessionToken](http://docs.aws.amazon.com/STS/latest/APIReference/API_GetSessionToken.html) API\.
+ For Login with Amazon, Amazon Cognito, Facebook, Google, or any OIDC\-compatible identity provider, call the AWS CLI [assume\-role\-with\-web\-identity](http://docs.aws.amazon.com/cli/latest/reference/sts/assume-role-with-web-identity.html) command or the AWS STS [AssumeRoleWithWebIdentity](http://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRoleWithWebIdentity.html) API\.

Your user should receive a set of temporary access credentials, which include an AWS access key ID, a secret access key, and a session token\. Your user should make a note of these three values because they are used in the next step\.

## Step 3: Configure the AWS CLI with Your Temporary Access Credentials<a name="temporary-access-configure-credentials"></a>

Your user must configure the development machine to use those temporary access credentials\.

1. Follow the instructions in [Setting Up ](setting-up.md) to set up the AWS CLI\. Use the aws configure command to configure a profile\.
**Note**  
Before you continue, make sure the git config file is configured to use the AWS profile you configured in the AWS CLI\.

1. You can associate the temporary access credentials with the user's AWS CLI named profile in one of the following ways\. Do not use the aws configure command\.
   + In the `~/.aws/credentials` file \(for Linux\) or the `%UserProfile%.aws\credentials` file \(for Windows\), add to the user's AWS CLI named profile the `aws_access_key_id`, `aws_secret_access_key`, and `aws_session_token` setting values:

     ```
     [CodeCommitProfileName]
     aws_access_key_id=TheAccessKeyID
     aws_secret_access_key=TheSecretAccessKey
     aws_session_token=TheSessionToken
     ```

     \-OR\-
   + Set the **AWS\_ACCESS\_KEY\_ID**, **AWS\_SECRET\_ACCESS\_KEY**, and **AWS\_SESSION\_TOKEN** environment variables:

     For Linux, macOS, or Unix:

     ```
     export AWS_ACCESS_KEY_ID=TheAccessKey
     export AWS_SECRET_ACCESS_KEY=TheSecretAccessKey
     export AWS_SESSION_TOKEN=TheSessionToken
     ```

     For Windows:

     ```
     set AWS_ACCESS_KEY_ID=TheAccessKey
     set AWS_SECRET_ACCESS_KEY=TheSecretAccessKey
     set AWS_SESSION_TOKEN=TheSessionToken
     ```

   For more information, see [Configuring the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html) in the *AWS Command Line Interface User Guide*\.

1. Set up the Git credential helper with the AWS CLI named profile associated with the temporary access credentials\. 
   + [Linux, macOS, or Unix](setting-up-https-unixes.md)
   + [Windows](setting-up-https-windows.md)

   As you follow these steps, do not call the aws configure command\. You already specified temporary access credentials through the credentials file or the environment variables\. If you use environment variables instead of the credentials file, in the Git credential helper, specify `default` as the profile name\. 

## Step 4: Access the AWS CodeCommit Repositories<a name="temporary-access-use-credentials"></a>

Assuming your user has followed the instructions in [Connect to a Repository](how-to-connect.md) to connect to the AWS CodeCommit repositories, the user then uses Git to call git clone, git push, and git pull to clone, push to, and pull from, the AWS CodeCommit repositories to which he or she has temporary access\.

When the user uses the AWS CLI and specifies the AWS CLI named profile associated with the temporary access credentials, results scoped to that profile are returned\.

If the user receives the `403: Forbidden` error in response to calling a Git command or a command in the AWS CLI, it's likely the temporary access credentials have expired\. The user must go back to [step 2](#temporary-access-get-credentials) and get a new set of temporary access credentials\.