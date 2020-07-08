# Integrate Eclipse with AWS CodeCommit<a name="setting-up-ide-ec"></a>

You can use Eclipse to make code changes in a CodeCommit repository\. The Toolkit for Eclipse integration is designed to work with Git credentials and an IAM user\. You can clone existing repositories, create repositories, commit and push code changes to a repository, and more\. 

To use Toolkit for Eclipse with CodeCommit, you need the following:
+ Eclipse installed on your local computer\.
+ An IAM user with a valid set of credentials \(an access key and secret key\) configured for it\. This IAM user should also have: 

  One of the CodeCommit managed policies and the IAMSelfManageServiceSpecificCredentials managed policy applied to it\.

  OR

  If the IAM user already has Git credentials configured, one of the CodeCommit managed policies or equivalent permissions\.

  For more information, see [AWS managed \(predefined\) policies for CodeCommit](auth-and-access-control-iam-identity-based-access-control.md#managed-policies) and [Understanding and Getting Your Security Credentials](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html)\.
+ An active set of Git credentials configured for the user in IAM\. For more information, see [Step 3: Create Git credentials for HTTPS connections to CodeCommit](setting-up-gc.md#setting-up-gc-iam)\.

**Topics**
+ [Step 1: Get an access key and secret key for your IAM user](#setting-up-ide-ec-profile)
+ [Step 2: Install AWS Toolkit for Eclipse and connect to CodeCommit](#setting-up-ide-ec-connect)
+ [Clone a CodeCommit repository from Eclipse](#setting-up-ide-ec-checkout)
+ [Create a CodeCommit repository from Eclipse](#setting-up-ide-ec-create)
+ [Working with CodeCommit repositories](#setting-up-ide-ec-work)

## Step 1: Get an access key and secret key for your IAM user<a name="setting-up-ide-ec-profile"></a>

If you do not already have a credential profile set up on the computer where Eclipse is installed, you can [configure one with the AWS CLI and the aws configure command](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html#cli-quick-configuration)\. Alternatively, you can follow the steps in this procedure to create and download your credentials\. Provide them to the Toolkit for Eclipse when prompted\. 

Access keys consist of an access key ID and secret access key, which are used to sign programmatic requests that you make to AWS\. If you don't have access keys, you can create them from the AWS Management Console\. As a best practice, do not use the AWS account root user access keys for any task where it's not required\. Instead, [create a new administrator IAM user](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) with access keys for yourself\.

The only time that you can view or download the secret access key is when you create the keys\. You cannot recover them later\. However, you can create new access keys at any time\. You must also have permissions to perform the required IAM actions\. For more information, see [Permissions Required to Access IAM Resources](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_permissions-required.html) in the *IAM User Guide*\.

**To create access keys for an IAM user**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Users**\.

1. Choose the name of the user whose access keys you want to create, and then choose the **Security credentials** tab\.

1. In the **Access keys** section, choose **Create access key**\.

1. To view the new access key pair, choose **Show**\. You will not have access to the secret access key again after this dialog box closes\. Your credentials will look something like this:
   + Access key ID: AKIAIOSFODNN7EXAMPLE
   + Secret access key: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY

1. To download the key pair, choose **Download \.csv file**\. Store the keys in a secure location\. You will not have access to the secret access key again after this dialog box closes\.

   Keep the keys confidential in order to protect your AWS account and never email them\. Do not share them outside your organization, even if an inquiry appears to come from AWS or Amazon\.com\. No one who legitimately represents Amazon will ever ask you for your secret key\.

1. After you download the `.csv` file, choose **Close**\. When you create an access key, the key pair is active by default, and you can use the pair right away\.

**Related topics**
+ [What Is IAM?](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) in the *IAM User Guide*
+ [AWS Security Credentials](https://docs.aws.amazon.com/general/latest/gr/aws-security-credentials.html) in *AWS General Reference* 

## Step 2: Install AWS Toolkit for Eclipse and connect to CodeCommit<a name="setting-up-ide-ec-connect"></a>

The Toolkit for Eclipse is a software package you can add to Eclipse\. After you've installed it and configured it with your AWS credential profile, you can connect to CodeCommit from the AWS Explorer in Eclipse\. 

**To install the Toolkit for Eclipse with the AWS CodeCommit module and configure access to your project repository**

1. Install Toolkit for Eclipse on your local computer if you don't have a supported version already installed\. If you need to update your version of Toolkit for Eclipse, follow the instructions in [Set Up the Toolkit](https://docs.aws.amazon.com/AWSToolkitEclipse/latest/GettingStartedGuide/setup-install.html)\. 

1. In Eclipse, either follow the firstrun experience, or open **Preferences** from the Eclipse menu system \(the location varies depending on your version and operating system\) and choose **AWS Toolkit**\.

1. Do one of the following:
   + If you are following the firstrun experience, provide your AWS security credentials when prompted to set up your credential profile\.
   + If you are configuring in **Preferences** and have a credential profile already configured on your computer, choose it from **Default Profile**\.
   + If you are configuring in **Preferences** and you do not see the profile you want to use, or if the list is empty, choose **Add profile**\. In **Profile Details**, enter a name for the profile and the credentials for the IAM user \(access key and secret key\), or alternatively, enter the location of the credentials file\.
   + If you are configuring in **Preferences** and you do not have a profile configured, use the links for signing up for an account or managing your existing AWS security credentials\.

1. In Eclipse, expand the **AWS Toolkit** menu and choose **AWS CodeCommit**\. Choose your credential profile, and then enter the user name and password for your Git credentials or import them from the \.csv file\. Choose **Apply**, and then choose **OK**\.  
![\[Configuring Git credentials in Eclipse with the Toolkit for Eclipse installed.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-eclipse-pref.png)

After you are signed in with a profile, the AWS CodeCommit connection panel appears in Team Explorer with options to clone, create, or sign out\. Choosing **Clone** clones an existing CodeCommit repository to your local computer, so you can start working on code\. This is the most frequently used option\. 

 If you don't have any repositories, or want to create a repository, choose **Create**\. 

## Clone a CodeCommit repository from Eclipse<a name="setting-up-ide-ec-checkout"></a>

After you've configured your credentials, you can clone a repository to a local repo on your computer by checking it out in Eclipse\. Then you can start working with the code\.

1. In Eclipse, open **AWS Explorer**\. For information about where to find it, see [How to Access AWS Explorer](https://docs.aws.amazon.com/AWSToolkitEclipse/latest/GettingStartedGuide/open-aws-explorer.html)\. Expand **AWS CodeCommit**, and choose the CodeCommit repository you want to work in\. You can view the commit history and other details of the repository, which can help you determine if this is the repository and branch you want to clone\.
**Note**  
If you do not see your repository, choose the flag icon to open the AWS Regions menu, and choose the AWS Region where the repository was created\.  
![\[Choosing your CodeCommit repository in the AWS Explorer.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-eclipse-awsexplorer.png)

1. Choose **Check out**, and follow the instructions to clone the repository to your local computer\.

1. When you have finished cloning the project, you're ready to start editing your code in Eclipse and staging, committing, and pushing your changes to your project's repository in CodeCommit\. 

## Create a CodeCommit repository from Eclipse<a name="setting-up-ide-ec-create"></a>

You can create CodeCommit repositories from Eclipse with the Toolkit for Eclipse\. As part of creating the repository, you also clone it to a local repo on your computer, so you can start working with it right away\.

1. In AWS Explorer, right\-click **AWS CodeCommit**, and then choose **Create repository**\. 
**Note**  
Repositories are region\-specific\. Before you create the repository, make sure you have selected the correct AWS Region\. You cannot choose the AWS Region after you have started the repository creation process\.

1. In **Repository Name**, enter a name for this repository\. Repository names must be unique within an AWS account\. There are character and length limits\. For more information, see [Quotas](limits.md)\. In **Repository Description**, enter an optional description for this repository\. This helps others understand what this repository is for, and helps distinguish it from other repositories in the region\. Choose **OK**\.

1. In AWS Explorer, expand **AWS CodeCommit**, and then choose the CodeCommit repository you just created\. You see that this repository has no commit history\. Choose **Check out**, and follow the instructions to clone the repository to your local computer\.

## Working with CodeCommit repositories<a name="setting-up-ide-ec-work"></a>

After you have connected to CodeCommit, you can see a list of repositories associated with your account, by AWS Region, in AWS Explorer\. Choose the flag menu to change the region\. 

**Note**  
CodeCommit might not be available in all AWS Regions supported by Toolkit for Eclipse\.

 In Toolkit for Eclipse, you can browse the contents of these repositories from the **Navigation** and **Package Explorer** views\. To open a file, choose it from the list\.

Git operations in Toolkit for Eclipse for CodeCommit repositories work exactly as they do for any other Git\-based repository\. You can make changes to code, add files, and create local commits\. When you are ready to share, you use the **Git Staging** option to push your commits to the CodeCommit repository\. If you haven't configured your author and committer information in a Git profile, you can do this before you commit and push\. Because your Git credentials for your IAM user are already stored locally and associated with your connected AWS credential profile, you won’t be prompted to supply them again when you push to CodeCommit\.

For more information about working with Toolkit for Eclipse, see the [AWS Toolkit for Eclipse Getting Started Guide](https://docs.aws.amazon.com/AWSToolkitEclipse/latest/GettingStartedGuide/)\. 