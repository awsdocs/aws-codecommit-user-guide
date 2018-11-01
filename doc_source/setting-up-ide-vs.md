--------

 The procedures in this guide support the new console design\. If you choose to use the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\.

--------

# Integrate Visual Studio with AWS CodeCommit<a name="setting-up-ide-vs"></a>

You can use Visual Studio to make code changes in an AWS CodeCommit repository\. The AWS Toolkit for Visual Studio now includes features that make working with AWS CodeCommit easier and more convenient when working from within Visual Studio Team Explorer\. The Toolkit for Visual Studio integration is designed to work with Git credentials and an IAM user\. You can clone existing repositories, create repositories, commit and push code changes to a repository, and more\. 

**Important**  
The Toolkit for Visual Studio is available for installation on Windows operating systems only\.

If you've used the Toolkit for Visual Studio before, you're probably already familiar with setting up AWS credential profiles that contain an access key and secret key\. Credential profiles are used in the Toolkit for Visual Studio to enable calls to AWS service APIs \(for example, to Amazon S3 to list buckets or to AWS CodeCommit to list repositories\)\. To pull and push code to an AWS CodeCommit repository, you also need Git credentials\. If you don't have Git credentials, the Toolkit for Visual Studio can generate and apply those credentials for you\. This can save you a great deal of time\.

To use Visual Studio with AWS CodeCommit, you need the following:
+ An IAM user with a valid set of credentials \(an access key and secret key\) configured for it\. This IAM user should also have:

  One of the AWS CodeCommit managed policies and the IAMSelfManageServiceSpecificCredentials managed policy applied to it\.

  OR

  If the IAM user already has Git credentials configured, one of the AWS CodeCommit managed policies or equivalent permissions\.

   For more information, see [AWS Managed \(Predefined\) Policies for AWS CodeCommit](auth-and-access-control-iam-identity-based-access-control.md#managed-policies) and [Understanding and Getting Your Security Credentials](http://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html)\.
+ The AWS Toolkit for Visual Studio installed on the computer where you've installed Visual Studio\. For more information, see [Setting Up the AWS Toolkit for Visual Studio](http://docs.aws.amazon.com/AWSToolkitVS/latest/UserGuide/getting-set-up.html)\.

**Topics**
+ [Step 1: Get an Access Key and Secret Key for Your IAM User](#setting-up-ide-vs-profile)
+ [Step 2: Install AWS Toolkit for Visual Studio and Connect to AWS CodeCommit](#setting-up-ide-vs-connect)
+ [Clone an AWS CodeCommit Repository from Visual Studio](#setting-up-ide-vs-clone)
+ [Create an AWS CodeCommit Repository from Visual Studio](#setting-up-ide-vs-create)
+ [Working with AWS CodeCommit Repositories](#setting-up-ide-vs-work)

## Step 1: Get an Access Key and Secret Key for Your IAM User<a name="setting-up-ide-vs-profile"></a>

If you do not already have a credential profile set up on the computer where Visual Studio is installed, you can [configure one with the AWS CLI and the aws configure command](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html#cli-quick-configuration)\. Alternatively, you can follow this procedure to create and download your credentials\. Provide them to the Toolkit for Visual Studio when prompted\. 

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

## Step 2: Install AWS Toolkit for Visual Studio and Connect to AWS CodeCommit<a name="setting-up-ide-vs-connect"></a>

The Toolkit for Visual Studio is a software package you can add to Visual Studio\. After you've installed it, you can connect to AWS CodeCommit from Team Explorer in Visual Studio\. 

**To install the Toolkit for Visual Studio with the AWS CodeCommit module and configure access to your project repository**

1. Install Visual Studio on your local computer if you don't have a supported version already installed\. 

1. [Download and install the Toolkit for Visual Studio](https://aws.amazon.com/visualstudio/) and save the file to a local folder or directory\. Launch the installation wizard by opening the file\. When prompted on the **Getting Started with the AWS Toolkit for Visual Studio** page, type or import your AWS credentials \(your access key and secret key\), and then choose** Save and Close**\.

1. In Visual Studio, open **Team Explorer**\. In **Hosted Service Providers**, find **AWS CodeCommit**, and choose **Connect**\.  
![\[Connecting to AWS CodeCommit in Team Explorer with the Toolkit for Visual Studio installed.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-vs-te1.png)

1. Do one of the following:
   + If you have a single credential profile already configured on your computer, the Toolkit for Visual Studio will apply it automatically\. No action is required\. The AWS CodeCommit connection panel appears in Team Explorer\.
   + If you have more than one credential profile configured on your computer, you are prompted to choose the one you want to use\. Choose the profile associated with the IAM user you'll use for connecting to AWS CodeCommit repositories, and then choose **OK**\.
   + If you do not have a profile configured, a dialog box appears and asks for your AWS security credentials \(your access key and secret key\)\. Type or import them, and then choose **OK**\.

After you are signed in with a profile, the AWS CodeCommit connection panel appears in Team Explorer with options to clone, create, or sign out\. Choosing **Clone** clones an existing AWS CodeCommit repository to your local computer, so you can start working on code\. This is the most frequently used option\. 

 If you don't have repositories, or want to create a repository, choose **Create**\. For more information, see [Create an AWS CodeCommit Repository from Visual Studio](#setting-up-ide-vs-create)\. 

## Clone an AWS CodeCommit Repository from Visual Studio<a name="setting-up-ide-vs-clone"></a>

After you're connected to AWS CodeCommit, you can clone a repository to a local repo on your computer\. Then you can start working with the code\.

1. In **Manage Connections**, choose **Clone**\. In **Region**, choose the AWS region where the repository was created in AWS CodeCommit\. Choose your project's repository and the folder on your local computer you want to clone the repository into, and then choose **OK**\.

1. If you are prompted to create Git credentials, choose **Yes**\. The toolkit attempts to create credentials on your behalf\. You must have the IAMSelfManageServiceSpecificCredentials applied to your IAM user, or the equivalent permissions\. When prompted, save the credentials file in a secure location\. This is the only opportunity you will have to save these Git credentials\.

   If the toolkit cannot create Git credentials on your behalf, or if you chose **No**, you must create and provide your own Git credentials\. For more information, see [For HTTPS Users Using Git Credentials](setting-up-gc.md), or follow the online directions\.

1. When you have finished cloning the project, you're ready to start editing your code in Visual Studio and committing and pushing your changes to your project's repository in AWS CodeCommit\. 

## Create an AWS CodeCommit Repository from Visual Studio<a name="setting-up-ide-vs-create"></a>

You can create AWS CodeCommit repositories from Visual Studio with the Toolkit for Visual Studio\. As part of creating the repository, you also clone it to a local repo on your computer, so you can start working with it right away\.

1. In **Manage Connections**, choose **Create**\. 

1. In **Region**, choose the AWS region where you want to create the repository\. AWS CodeCommit repositories are organized by region\. 

1. In **Name**, type a name for this repository\. Repository names must be unique within an AWS account\. There are character and length limits\. For more information, see [Limits](limits.md)\. Optionally, in **Description**, type a description for this repository\. This helps others understand what the repository is for, and helps distinguish it from other repositories in the region\.

1. In **Clone into**, type or browse to the folder or directory where you want to clone this repository on your local computer\. Visual Studio automatically clones the repository after it's created and creates the local repo in the location you choose\. 

1. When you are satisfied with your choices, choose **OK**\.

1. If prompted to create Git credentials, choose **Yes**\. The toolkit attempts to create credentials on your behalf\. You must have the IAMSelfManageServiceSpecificCredentials applied to your IAM user, or the equivalent permissions\. When prompted, save the credentials file in a secure location\. This is the only opportunity you will have to save these Git credentials\.

   If the toolkit cannot create Git credentials on your behalf, or if you chose **No**, you must create and provide your own Git credentials\. For more information, see [For HTTPS Users Using Git Credentials](setting-up-gc.md), or follow the online directions\.

## Working with AWS CodeCommit Repositories<a name="setting-up-ide-vs-work"></a>

After you have connected to AWS CodeCommit, you can see a list of repositories associated with your AWS account\. You can browse the contents of these repositories in Visual Studio\. Open the context menu for the repository you're interested in, and choose **Browse in Console**\.

Git operations in Visual Studio for AWS CodeCommit repositories work exactly as they do for any other Git\-based repository\. You can make changes to code, add files, and create local commits\. When you are ready to share, you use the **Sync** option in Team Explorer to push your commits to the AWS CodeCommit repository\. Because your Git credentials for your IAM user are already stored locally and associated with your connected AWS credential profile, you won’t be prompted to supply them again when you push to AWS CodeCommit\.

For more information about working with Toolkit for Visual Studio, see the [AWS Toolkit for Visual Studio User Guide](http://docs.aws.amazon.com/AWSToolkitVS/latest/UserGuide/)\.