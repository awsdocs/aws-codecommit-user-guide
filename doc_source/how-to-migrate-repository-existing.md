# Migrate a Git repository to AWS CodeCommit<a name="how-to-migrate-repository-existing"></a>

You can migrate an existing Git repository to a CodeCommit repository\. The procedures in this topic show you how to migrate a project hosted on another Git repository to CodeCommit\. As part of this process, you:
+ Complete the initial setup required for CodeCommit\.
+ Create a CodeCommit repository\.
+ Clone the repository and push it to CodeCommit\.
+ View files in the CodeCommit repository\.
+ Share the CodeCommit repository with your team\.

![\[Migrating a Git repository to CodeCommit\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-migrate-existing.png)![\[Migrating a Git repository to CodeCommit\]](http://docs.aws.amazon.com/codecommit/latest/userguide/)

**Topics**
+ [Step 0: Setup required for access to CodeCommit](#how-to-migrate-existing-setup)
+ [Step 1: Create a CodeCommit repository](#how-to-migrate-existing-create)
+ [Step 2: Clone the repository and push to the CodeCommit repository](#how-to-migrate-existing-clone)
+ [Step 3: View files in CodeCommit](#how-to-migrate-existing-view)
+ [Step 4: Share the CodeCommit repository](#how-to-migrate-existing-share)

## Step 0: Setup required for access to CodeCommit<a name="how-to-migrate-existing-setup"></a>

Before you can migrate a repository to CodeCommit, you must create and configure an IAM user for CodeCommit and configure your local computer for access\. You should also install the AWS CLI to manage CodeCommit\. Although you can perform most CodeCommit tasks without it, the AWS CLI offers flexibility when working with Git at the command line or terminal\. 

If you are already set up for CodeCommit, you can skip ahead to [Step 1: Create a CodeCommit repository](#how-to-migrate-existing-create)\.

**To create and configure an IAM user for accessing CodeCommit**

1. Create an AWS account by going to [http://aws\.amazon\.com](http://aws.amazon.com) and choosing **Sign Up**\.

1. Create an IAM user, or use an existing one, in your AWS account\. Make sure you have an access key ID and a secret access key associated with that IAM user\. For more information, see [Creating an IAM User in Your AWS Account](https://docs.aws.amazon.com/IAM/latest/UserGuide/Using_SettingUpUser.html)\.
**Note**  
CodeCommit requires AWS Key Management Service\. If you are using an existing IAM user, make sure there are no policies attached to the user that expressly deny the AWS KMS actions required by CodeCommit\. For more information, see [AWS KMS and encryption](encryption.md)\.

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the IAM console, in the navigation pane, choose **Users**, and then choose the IAM user you want to configure for CodeCommit access\.

1. On the **Permissions** tab, choose **Add Permissions**\. 

1. In **Grant permissions**, choose **Attach existing policies directly**\.

1. From the list of policies, select **AWSCodeCommitPowerUser** or another managed policy for CodeCommit access\. For more information, see [AWS managed \(predefined\) policies for CodeCommit](auth-and-access-control-iam-identity-based-access-control.md#managed-policies)\.

   After you have selected the policy you want to attach, choose **Next: Review** to review the list of policies to attach to the IAM user\. If the list is correct, choose **Add permissions**\.

    For more information about CodeCommit managed policies and sharing access to repositories with other groups and users, see [Share a repository](how-to-share-repository.md) and [Authentication and access control for AWS CodeCommit](auth-and-access-control.md)\.

**To install and configure the AWS CLI**

1. On your local machine, download and install the AWS CLI\. This is a prerequisite for interacting with CodeCommit from the command line\. For more information, see [Getting Set Up with the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-set-up.html)\.
**Note**  
CodeCommit works only with AWS CLI versions 1\.7\.38 and later\. As a best practice, install or upgrade the AWS CLI to the latest version available\. To determine which version of the AWS CLI you have installed, run the aws \-\-version command\.  
To upgrade an older version of the AWS CLI to the latest version, see [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html)\.

1.  Run this command to verify the CodeCommit commands for the AWS CLI are installed:

   ```
   aws codecommit help
   ```

   This command should return a list of CodeCommit commands\.

1. Configure the AWS CLI with a profile by using the configure command, as follows:

   ```
   aws configure
   ```

   When prompted, specify the AWS access key and AWS secret access key of the IAM user to use with CodeCommit\. Also, be sure to specify the AWS Region where the repository exists, such as `us-east-2`\. When prompted for the default output format, specify `json`\. For example:

   ```
   AWS Access Key ID [None]: Type your target AWS access key ID here, and then press Enter
   AWS Secret Access Key [None]: Type your target AWS secret access key here, and then press Enter
   Default region name [None]: Type a supported region for CodeCommit here, and then press Enter
   Default output format [None]: Type json here, and then press Enter
   ```

   For more information about creating and configuring profiles to use with the AWS CLI, see the following:
   + [Named Profiles](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-profiles.html)
   + [Using an IAM Role in the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-role.html)
   + [Set command](https://docs.aws.amazon.com/cli/latest/reference/set.html)
   + [Connecting to AWS CodeCommit repositories with rotating credentials](temporary-access.md)

   To connect to a repository or a resource in another AWS Region, you must reconfigure the AWS CLI with the default Region name\. Supported default Region names for CodeCommit include:
   + us\-east\-2
   + us\-east\-1
   + eu\-west\-1
   + us\-west\-2
   + ap\-northeast\-1
   + ap\-southeast\-1
   + ap\-southeast\-2
   + eu\-central\-1
   + ap\-northeast\-2
   + sa\-east\-1
   + us\-west\-1
   + eu\-west\-2
   + ap\-south\-1
   + ca\-central\-1
   + us\-gov\-west\-1
   + us\-gov\-east\-1
   + eu\-north\-1
   + ap\-east\-1
   + me\-south\-1
   + cn\-north\-1
   + cn\-northwest\-1

   For more information about CodeCommit and AWS Regions, see [Regions and Git connection endpoints](regions.md)\. For more information about IAM, access keys, and secret keys, see [How Do I Get Credentials?](https://docs.aws.amazon.com/IAM/latest/UserGuide/IAM_Introduction.html#IAM_SecurityCredentials) and [Managing Access Keys for IAM Users](https://docs.aws.amazon.com/IAM/latest/UserGuide/ManagingCredentials.html)\. For more information about the AWS CLI and profiles, see [Named Profiles](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-profiles.html)\.

Next, you must install Git\. 
+ **For Linux, macOS, or Unix**:

  To work with files, commits, and other information in CodeCommit repositories, you must install Git on your local machine\. CodeCommit supports Git versions 1\.7\.9 and later\. We recommend using a recent version of Git\.

  To install Git, we recommend websites such as [Git Downloads](http://git-scm.com/downloads)\.
**Note**  
Git is an evolving, regularly updated platform\. Occasionally, a feature change might affect the way it works with CodeCommit\. If you encounter issues with a specific version of Git and CodeCommit, review the information in [Troubleshooting](troubleshooting.md)\.
+ **For Windows:** 

  To work with files, commits, and other information in CodeCommit repositories, you must install Git on your local machine\. CodeCommit supports Git versions 1\.7\.9 and later\. We recommend using a recent version of Git\.

  To install Git, we recommend websites such as [Git for Windows](https://gitforwindows.org/)\. If you use this link to install Git, you can accept all of the installation default settings except for the following: 
  + When prompted during the **Adjusting your PATH environment** step, choose the option to use Git from the command line\.
  + \(Optional\) If you intend to use HTTPS with the credential helper that is included in the AWS CLI instead of configuring Git credentials for CodeCommit, on the **Configuring extra options** page, make sure the **Enable Git Credential Manager** option is cleared\. The Git Credential Manager is only compatible with CodeCommit if IAM users configure Git credentials\. For more information, see [For HTTPS users using Git credentials](setting-up-gc.md) and [Git for Windows: I installed Git for Windows, but I am denied access to my repository \(403\)](troubleshooting-ch.md#troubleshooting-windowshttps)\.
**Note**  
Git is an evolving, regularly updated platform\. Occasionally, a feature change might affect the way it works with CodeCommit\. If you encounter issues with a specific version of Git and CodeCommit, review the information in [Troubleshooting](troubleshooting.md)\.

CodeCommit supports both HTTPS and SSH authentication\. To complete setup, you must configure Git credentials for CodeCommit \(HTTPS, recommended for most users\), an SSH key pair to use when accessing CodeCommit \(SSH\), git\-remote\-codecommit \(recommended for users who use federated access\), or the credential helper included in the AWS CLI \(HTTPS\)\. 
+ For Git credentials on all supported operating systems, see [Step 3: Create Git credentials for HTTPS connections to CodeCommit](setting-up-gc.md#setting-up-gc-iam)\.
+ For SSH on Linux, macOS, or Unix, see [SSH and Linux, macOS, or Unix: Set up the public and private keys for Git and CodeCommit](setting-up-ssh-unixes.md#setting-up-ssh-unixes-keys-unixes)\.
+  For SSH on Windows, see [SSH and Windows: Set up the public and private keys for Git and CodeCommit](setting-up-ssh-windows.md#setting-up-ssh-windows-keys-windows)\.
+ For git\-remote\-codecommit, see [Setup steps for HTTPS connections to AWS CodeCommit with git\-remote\-codecommit](setting-up-git-remote-codecommit.md)\.
+ For the credential helper on Linux, macOS, or Unix, see [Set Up the Credential Helper \(Linux, macOS, or Unix\)](setting-up-https-unixes.md#setting-up-https-unixes-ch-config)\.
+ For the credential helper on Windows, see [Set Up the Credential Helper \(Windows\)](setting-up-https-windows.md#setting-up-https-windows-ch-config)\.

## Step 1: Create a CodeCommit repository<a name="how-to-migrate-existing-create"></a>

In this section, you use the CodeCommit console to create the CodeCommit repository you use for the rest of this tutorial\. To use the AWS CLI to create the repository, see [Create a repository \(AWS CLI\)](how-to-create-repository.md#how-to-create-repository-cli)\.

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In the region selector, choose the AWS Region where you want to create the repository\. For more information, see [Regions and Git connection endpoints](regions.md)\.

1. On the **Repositories** page, choose **Create repository**\. 

1. On the **Create repository** page, in **Repository name**, enter a name for the repository\.
**Note**  
Repository names are case sensitive\. The name must be unique in the AWS Region for your AWS account\.

1. \(Optional\) In **Description**, enter a description for the repository\. This can help you and other users identify the purpose of the repository\. 
**Note**  
The description field displays Markdown in the console and accepts all HTML characters and valid Unicode characters\. If you are an application developer who is using the `GetRepository` or `BatchGetRepositories` APIs and you plan to display the repository description field in a web browser, see the [CodeCommit API Reference](https://docs.aws.amazon.com/codecommit/latest/APIReference/)\.

1. \(Optional\) Choose **Add tag** to add one or more repository tags \(a custom attribute label that helps you organize and manage your AWS resources\) to your repository\. For more information, see [Tagging repositories in AWS CodeCommit](how-to-tag-repository.md)\.

1. \(Optional\) Select **Enable Amazon CodeGuru Reviewer for Java** if this repository contains Java code, and you want CodeGuru Reviewer to analyze it\. CodeGuru Reviewer uses multiple machine learning models to find Java code defects and to suggest improvements and fixes in pull requests\. For more information, see the [https://docs.aws.amazon.com/codeguru/latest/reviewer-ug/Welcome.html](https://docs.aws.amazon.com/codeguru/latest/reviewer-ug/Welcome.html)\.

1. Choose **Create**\. 

![\[Creating a repository for migrating a Git repository to CodeCommit\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-create-repo-migrate-existing.png)

After it is created, the repository appears in the **Repositories** list\. In the URL column, choose the copy icon, and then choose the protocol \(SSH or HTTPS\) to be used to connect to CodeCommit\. Copy the URL\.

For example, if you named your repository *MyClonedRepository* and you are using Git credentials with HTTPS in the US East \(Ohio\) Region, the URL looks like the following:

```
https://git-codecommit.us-east-2.amazonaws.com/MyClonedRepository
```

You need this URL later in [Step 2: Clone the repository and push to the CodeCommit repository](#how-to-migrate-existing-clone)\.

## Step 2: Clone the repository and push to the CodeCommit repository<a name="how-to-migrate-existing-clone"></a>

In this section, you clone a Git repository to your local computer, creating what is called a local repo\. You then push the contents of the local repo to the CodeCommit repository you created earlier\.

1. From the terminal or command prompt on your local computer, run the git clone command with the `--mirror` option to clone a bare copy of the remote repository into a new folder named *aws\-codecommit\-demo*\. This is a bare repo meant only for migration\. It is not the local repo for interacting with the migrated repository in CodeCommit\. You can create that later, after the migration to CodeCommit is complete\.

   The following example clones a demo application hosted on GitHub \(*https://github\.com/awslabs/aws\-demo\-php\-simple\-app\.git*\) to a local repo in a directory named *aws\-codecommit\-demo*\. 

   ```
   git clone --mirror https://github.com/awslabs/aws-demo-php-simple-app.git aws-codecommit-demo
   ```

1. Change directories to the directory where you made the clone\.

   ```
   cd aws-codecommit-demo
   ```

1. Run the git push command, specifying the URL and name of the destination CodeCommit repository and the \-\-all option\. \(This is the URL you copied in [Step 1: Create a CodeCommit repository](#how-to-migrate-existing-create)\)\.

   For example, if you named your repository *MyClonedRepository* and you are set up to use HTTPS, you would run the following command:

   ```
   git push https://git-codecommit.us-east-2.amazonaws.com/v1/repos//MyClonedRepository --all
   ```
**Note**  
The \-\-all option only pushes all branches for the repository\. It does not push other references, such as tags\. If you want to push tags, wait until the initial push is complete, and then push again, this time using the \-\-tags option:  

   ```
   git push ssh://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyClonedRepository --tags
   ```
For more information, see [Git push](https://git-scm.com/docs/git-push) on the Git website\. For information about pushing large repositories, especially when pushing all references at once \(for example, with the \-\-mirror option\), see [Migrate a repository in increments](how-to-push-large-repositories.md)\.

You can delete the *aws\-codecommit\-demo* folder and its contents after you have migrated the repository to CodeCommit\. To create a local repo with all the correct references for working with the repository in CodeCommit, run the `git clone` command without the `--mirror` option:

```
git clone https://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyClonedRepository
```

## Step 3: View files in CodeCommit<a name="how-to-migrate-existing-view"></a>

After you have pushed the contents of your directory, you can use the CodeCommit console to quickly view all of the files in that repository\.

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In **Repositories**, choose the name of the repository \(for example, *MyClonedRepository*\)\. 

1. View the files in the repository for the branches, the clone URLs, the settings, and more\.  
![\[View of a cloned repository in CodeCommit\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-cloned-repo-url.png)

## Step 4: Share the CodeCommit repository<a name="how-to-migrate-existing-share"></a>

When you create a repository in CodeCommit, two endpoints are generated: one for HTTPS connections and one for SSH connections\. Both provide secure connections over a network\. Your users can use either protocol\. Both endpoints remain active no matter which protocol you recommend to your users\. Before you can share your repository with others, you must create IAM policies that allow other users access to your repository\. Provide those access instructions to your users\. 

**Create a customer managed policy for your repository**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the **Dashboard** navigation area, choose **Policies**, and then choose **Create Policy**\. 

1. On the **Create Policy** page,, choose **Import managed policy**\.

1. On the **Import managed policies** page, in **Filter policies**, enter **AWSCodeCommitPowerUser**\. Choose the button next to the policy name and then choose **Import**\.

1. On the **Create policy** page, choose **JSON**\. Replace the "\*" portion of the `Resource` line for CodeCommit actions with the Amazon Resource Name \(ARN\) of the CodeCommit repository, as shown here:

   ```
   "Resource": [
    "arn:aws:codecommit:us-east-2:111111111111:MyDemoRepo"
    ]
   ```
**Tip**  
To find the ARN for the CodeCommit repository, go to the CodeCommit console and choose the repository name from the list\. For more information, see [View repository details](how-to-view-repository-details.md)\.

   If you want this policy to apply to more than one repository, add each repository as a resource by specifying its ARN\. Include a comma between each resource statement, as shown here:

   ```
   "Resource": [
    "arn:aws:codecommit:us-east-2:111111111111:MyDemoRepo",
    "arn:aws:codecommit:us-east-2:111111111111:MyOtherDemoRepo"
    ]
   ```

   When you are finished editing, choose **Review policy**\.

1. On the **Review Policy** page, in **Name**, enter a new name for the policy \(for example, *AWSCodeCommitPowerUser\-MyDemoRepo*\)\. Optionally provide a description for this policy\.

1. Choose **Create Policy**\.

To manage access to your repository, create an IAM group for its users, add IAM users to that group, and then attach the customer managed policy you created in the previous step\. Attach any other policies required for access, such as IAMUserSSHKeys or IAMSelfManageServiceSpecificCredentials\. 

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the **Dashboard** navigation area, choose **Groups**, and then choose **Create New Group**\. 

1. On the **Set Group Name** page, in **Group Name**, enter a name for the group \(for example, *MyDemoRepoGroup*\), and then choose **Next Step**\. Consider including the repository name as part of the group name\.
**Note**  
This name must be unique across an AWS account\.

1. Select the box next to the customer managed policy you created in the previous section \(for example, **AWSCodeCommitPowerUser\-MyDemoRepo**\)\. 

1. On the **Review** page, choose **Create Group**\. IAM creates this group with the specified policies already attached\. The group appears in the list of groups associated with your AWS account\.

1. Choose your group from the list\. 

1. On the group summary page, choose the **Users** tab, and then choose **Add Users to Group**\. On the list that shows all users associated with your AWS account, select the boxes next to the users to whom you want to allow access to the CodeCommit repository, and then choose **Add Users**\.
**Tip**  
You can use the Search box to quickly find users by name\.

1. When you have added your users, close the IAM console\.

After you have created an IAM user to access CodeCommit using the policy group and policies you configured, send that user the information required to connect to the repository\.

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In the region selector, choose the AWS Region where the repository was created\. Repositories are specific to an AWS Region\. For more information, see [Regions and Git connection endpoints](regions.md)\.

1. On the **Repositories** page, choose the repository you want to share\. 

1. In **Clone URL**, choose the protocol that you want your users to use\. This copies the clone URL for the connection protocol\. 

1. Send your users the clone URL along with any other instructions, such as installing the AWS CLI, configuring a profile, or installing Git\. Make sure to include the configuration information for the connection protocol \(for example, HTTPS\)\. 