# Integrate Eclipse with AWS CodeCommit<a name="setting-up-ide-ec"></a>

You can use Eclipse to make code changes in an AWS CodeCommit repository\. The Toolkit for Eclipse integration is designed to work with Git credentials and an IAM user\. You can clone existing repositories, create repositories, commit and push code changes to a repository, and more\. 

To use Toolkit for Eclipse with AWS CodeCommit, you need the following:
+ Eclipse installed on your local computer\.
+ An IAM user with a valid set of credentials \(an access key and secret key\) configured for it\. This IAM user should also have: 

  One of the AWS CodeCommit managed policies and the IAMSelfManageServiceSpecificCredentials managed policy applied to it\.

  OR

  If the IAM user already has Git credentials configured, one of the AWS CodeCommit managed policies or equivalent permissions \.

  For more information, see [AWS Managed \(Predefined\) Policies for AWS CodeCommit](auth-and-access-control-iam-identity-based-access-control.md#managed-policies) and [Understanding and Getting Your Security Credentials](http://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html)\.
+ An active set of Git credentials configured for the user in IAM\. For more information, see [Step 3: Create Git Credentials for HTTPS Connections to AWS CodeCommit](setting-up-gc.md#setting-up-gc-iam)\.

**Topics**
+ [Step 1: Get an Access Key and Secret Key for Your IAM User](#setting-up-ide-ec-profile)
+ [Step 2: Install AWS Toolkit for Eclipse and Connect to AWS CodeCommit](#setting-up-ide-ec-connect)
+ [Clone an AWS CodeCommit Repository from Eclipse](#setting-up-ide-ec-checkout)
+ [Create an AWS CodeCommit Repository from Eclipse](#setting-up-ide-ec-create)
+ [Working with AWS CodeCommit Repositories](#setting-up-ide-ec-work)

## Step 1: Get an Access Key and Secret Key for Your IAM User<a name="setting-up-ide-ec-profile"></a>

If you do not already have a credential profile set up on the computer where Eclipse is installed, you can [configure one with the AWS CLI and the aws configure command](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html#cli-quick-configuration)\. Alternatively, you can follow this procedure to create and download your credentials\. Provide them to the Toolkit for Eclipse when prompted\. 

**To get the access key ID and secret access key for an IAM user**

Access keys consist of an access key ID and secret access key, which are used to sign programmatic requests that you make to AWS\. If you don't have access keys, you can create them from the AWS Management Console\. We recommend that you use IAM access keys instead of AWS account root user access keys\. IAM lets you securely control access to AWS services and resources in your AWS account\.

The only time that you can view or download the secret access keys is when you create the keys\. You cannot recover them later\. However, you can create new access keys at any time\. You must also have permissions to perform the required IAM actions\. For more information, see [Permissions Required to Access IAM Resources](http://docs.aws.amazon.com/IAM/latest/UserGuide/access_permissions-required.html) in the *IAM User Guide*\.

1. Open the [IAM console](https://console.aws.amazon.com/iam/home?#home)\.

1. In the navigation pane of the console, choose **Users**\.

1. Choose your IAM user name \(not the check box\)\.

1. Choose the **Security credentials** tab and then choose **Create access key**\.

1. To see the new access key, choose **Show**\. Your credentials will look something like this:
   + Access key ID: AKIAIOSFODNN7EXAMPLE
   + Secret access key: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY

1. To download the key pair, choose **Download \.csv file**\. Store the keys in a secure location\.

   Keep the keys confidential in order to protect your AWS account, and never email them\. Do not share them outside your organization, even if an inquiry appears to come from AWS or Amazon\.com\. No one who legitimately represents Amazon will ever ask you for your secret key\.

**Related topics**
+ [What Is IAM?](http://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) in the *IAM User Guide*
+ [AWS Security Credentials](http://docs.aws.amazon.com/general/latest/gr/aws-security-credentials.html) in *AWS General Reference* 

## Step 2: Install AWS Toolkit for Eclipse and Connect to AWS CodeCommit<a name="setting-up-ide-ec-connect"></a>

The Toolkit for Eclipse is a software package you can add to Eclipse\. After you've installed it and configured it with your AWS credential profile, you can connect to AWS CodeCommit from the AWS Explorer in Eclipse\. 

**To install the Toolkit for Eclipse with the AWS CodeCommit module and configure access to your project repository**

1. Install Toolkit for Eclipse on your local computer if you don't have a supported version already installed\. If you need to update your version of Toolkit for Eclipse, follow the instructions in [Set Up the Toolkit](http://docs.aws.amazon.com/AWSToolkitEclipse/latest/GettingStartedGuide/setup-install.html)\. 

1. In Eclipse, either follow the firstrun experience, or open **Preferences** from the Eclipse menu system \(the precise location will vary depending on your version and operating system\) and choose **AWS Toolkit**\.

1. Do one of the following:
   + If you are following the firstrun experience, provide your AWS security credentials when prompted to set up your credential profile\.
   + If you are configuring in **Preferences** and have a credential profile already configured on your computer, choose it from the list in **Default Profile**\.
   + If you are configuring in **Preferences** and you do not see the profile you want to use, or if the list is empty, choose **Add profile**\. In **Profile Details**, provide a name for the proifle and the credentials for the IAM user \(access key and secret key\), or alternatively provide the location of the credentials file\.
   + If you are configuring in **Preferences** and you do not have a profile configured, use the links for signing up for a new account or managing your existing AWS security credentials\.

1. In Eclipse, expand the **AWS Toolkit** menu and choose **AWS CodeCommit**\. Choose your credential profile, and then type the user name and password for your Git credentials or import them from the \.csv file\. Choose **Apply**, and then choose **OK**\.  
![\[Configuring Git credentials in Eclipse with the Toolkit for Eclipse installed.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-eclipse-pref.png)

After you are signed in with a profile, the AWS CodeCommit connection panel appears in Team Explorer with options to clone, create, or sign out\. Choosing **Clone** clones an existing AWS CodeCommit repository to your local computer, so you can start working on code\. This is the most frequently used option\. 

 If you don't have any repositories, or want to create a repository, choose **Create**\. 

## Clone an AWS CodeCommit Repository from Eclipse<a name="setting-up-ide-ec-checkout"></a>

After you've configured your credentials, you can clone a repository to a local repo on your computer by checking it out in Eclipse\. Then you can start working with the code\.

1. In Eclipse, open **AWS Explorer**\. For more information about where to find it, see [How to Access AWS Explorer](http://docs.aws.amazon.com/AWSToolkitEclipse/latest/GettingStartedGuide/open-aws-explorer.html)\. Expand **AWS CodeCommit**, and choose the AWS CodeCommit repository you want to work in\. You can view the commit history and other details of the repository, which can help you determine if this is the repository and branch you want to clone\.
**Note**  
If you do not see your repository, choose the flag icon to open the AWS regions menu, and choose the region where the repository was created\.  
![\[Choosing your AWS CodeCommit repository in the AWS Explorer.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-eclipse-awsexplorer.png)

1. Choose **Check out**, and follow the instructions to clone the repository to your local computer\.

1. When you have finished cloning the project, you're ready to start editing your code in Eclipse and staging, committing, and pushing your changes to your project's repository in AWS CodeCommit\. 

## Create an AWS CodeCommit Repository from Eclipse<a name="setting-up-ide-ec-create"></a>

You can create AWS CodeCommit repositories from Eclipse with the Toolkit for Eclipse\. As part of creating the repository, you'll also clone it to a local repo on your computer, so you can start working with it right away\.

1. In AWS Explorer, right\-click **AWS CodeCommit**, and then choose **Create repository**\. 
**Note**  
Repositories are region\-specific\. Before you create the repository, make sure you have selected the correct AWS region\. You cannot choose the region after you have started the repository creation process\.

1. In **Repository Name**, type a name for this repository\. Repository names must be unique within an AWS account\. There are character and length limits\. For more information, see [Limits](limits.md)\. Optionally, in **Repository Description**, type a description for this repository\. This helps others understand what this repository is for, and helps distinguish it from other repositories in the region\. Choose **OK**\.

1. In AWS Explorer, expand **AWS CodeCommit**, and then choose the AWS CodeCommit repository you just created\. You will see that this repository has no commit history\. Choose **Check out**, and follow the instructions to clone the repository to your local computer\.

## Working with AWS CodeCommit Repositories<a name="setting-up-ide-ec-work"></a>

After you have connected to AWS CodeCommit, you can see a list of repositories associated with your AWS account, by region, in AWS Explorer\. Choose the flag menu to change the region\. 

**Note**  
AWS CodeCommit might not be available in all regions supported by Toolkit for Eclipse\.

 In Toolkit for Eclipse, you can browse the contents of these repositories from the **Navigation** and **Package Explorer** views\. To open a file, choose it from the list\.

Git operations in Toolkit for Eclipse for AWS CodeCommit repositories work exactly as they do for any other Git\-based repository\. You can make changes to code, add files, and create local commits\. When you are ready to share, you use the **Git Staging** option to push your commits to the AWS CodeCommit repository\. If you haven't configured your author and committer information in a Git profile, you can do this before you commit and push\. Because your Git credentials for your IAM user are already stored locally and associated with your connected AWS credential profile, you won’t be prompted to supply them again when you push to AWS CodeCommit\.

For more information about working with Toolkit for Eclipse, see the [AWS Toolkit for Eclipse Getting Started Guide](http://docs.aws.amazon.com/AWSToolkitEclipse/latest/GettingStartedGuide/)\. 