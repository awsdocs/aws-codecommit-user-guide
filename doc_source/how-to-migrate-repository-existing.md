--------

 The procedures in this guide support the new console design\. If you choose to use the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\.

--------

# Migrate a Git Repository to AWS CodeCommit<a name="how-to-migrate-repository-existing"></a>

You can migrate an existing Git repository to an AWS CodeCommit repository\. The procedures in this topic show you how to migrate a project hosted on another Git repository to AWS CodeCommit\. As part of this process, you:
+ Complete the initial setup required for AWS CodeCommit\.
+ Create an AWS CodeCommit repository\.
+ Clone the repository and push it to AWS CodeCommit\.
+ View files in the AWS CodeCommit repository\.
+ Share the AWS CodeCommit repository with your team\.

![\[Migrating a Git repository to AWS CodeCommit\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-migrate-existing.png)![\[Migrating a Git repository to AWS CodeCommit\]](http://docs.aws.amazon.com/codecommit/latest/userguide/)

**Topics**
+ [Step 0: Setup Required for Access to AWS CodeCommit](#how-to-migrate-existing-setup)
+ [Step 1: Create an AWS CodeCommit Repository](#how-to-migrate-existing-create)
+ [Step 2: Clone the Repository and Push to the AWS CodeCommit Repository](#how-to-migrate-existing-clone)
+ [Step 3: View Files in AWS CodeCommit](#how-to-migrate-existing-view)
+ [Step 4: Share the AWS CodeCommit Repository](#how-to-migrate-existing-share)

## Step 0: Setup Required for Access to AWS CodeCommit<a name="how-to-migrate-existing-setup"></a>

Before you can migrate a repository to AWS CodeCommit, you must create and configure an IAM user for AWS CodeCommit and configure your local computer for access\. You should also install the AWS CLI to manage AWS CodeCommit\. Although you can perform most AWS CodeCommit tasks without it, the AWS CLI offers flexibility when working with Git at the command line or terminal\. 

If you are already set up for AWS CodeCommit, you can skip ahead to [Step 1: Create an AWS CodeCommit Repository](#how-to-migrate-existing-create)\.

**To create and configure an IAM user for accessing AWS CodeCommit**

1. Create an AWS account by going to [http://aws\.amazon\.com](http://aws.amazon.com) and choosing **Sign Up**\.

1. Create an IAM user, or use an existing one, in your AWS account\. Make sure you have an access key ID and a secret access key associated with that IAM user\. For more information, see [Creating an IAM User in Your AWS Account](https://docs.aws.amazon.com/IAM/latest/UserGuide/Using_SettingUpUser.html)\.
**Note**  
AWS CodeCommit requires AWS Key Management Service\. If you are using an existing IAM user, make sure there are no policies attached to the user that expressly deny the AWS KMS actions required by AWS CodeCommit\. For more information, see [AWS KMS and Encryption](encryption.md)\.

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the IAM console, in the navigation pane, choose **Users**, and then choose the IAM user you want to configure for AWS CodeCommit access\.

1. On the **Permissions** tab, choose **Add Permissions**\. 

1. In **Grant permissions**, choose **Attach existing policies directly**\.

1. Select **AWSCodeCommitFullAccess** from the list of policies, or another managed policy for AWS CodeCommit access\. For more information about managed policies for AWS CodeCommit, see [AWS Managed \(Predefined\) Policies for AWS CodeCommit](auth-and-access-control-iam-identity-based-access-control.md#managed-policies)\.

   After you have selected the policy you want to attach, choose** Next: Review** to review the list of policies that will be attached to the IAM user\. If the list is correct, choose **Add permissions**\.

    For more information about AWS CodeCommit managed policies and sharing access to repositories with other groups and users, see [Share a Repository](how-to-share-repository.md) and [Authentication and Access Control for AWS CodeCommit](auth-and-access-control.md)\.

**To install and configure the AWS CLI**

1. On your local machine, download and install the AWS CLI\. This is a prerequisite for interacting with AWS CodeCommit from the command line\. For more information, see [Getting Set Up with the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-set-up.html)\.
**Note**  
AWS CodeCommit works only with AWS CLI versions 1\.7\.38 and later\. To determine which version of the AWS CLI you have installed, run the `aws --version` command\.  
To upgrade an older version of the AWS CLI to the latest version, see [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html)\.

1.  Run this command to verify the AWS CodeCommit commands for the AWS CLI are installed:

   ```
   aws codecommit help
   ```

   This command should return a list of AWS CodeCommit commands\.

1. Configure the AWS CLI with the configure command, as follows:

   ```
   aws configure
   ```

   When prompted, specify the AWS access key and AWS secret access key of the IAM user you will use with AWS CodeCommit\. Also, be sure to specify the region where the repository exists, such as `us-east-2`\. When prompted for the default output format, specify `json`\. For example:

   ```
   AWS Access Key ID [None]: Type your target AWS access key ID here, and then press Enter
   AWS Secret Access Key [None]: Type your target AWS secret access key here, and then press Enter
   Default region name [None]: Type a supported region for AWS CodeCommit here, and then press Enter
   Default output format [None]: Type json here, and then press Enter
   ```

   To connect to a repository or a resource in another region, you must re\-configure the AWS CLI with the default region name for that region\. Supported default region names for AWS CodeCommit include:
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

   For more information about AWS CodeCommit and regions, see [Regions and Git Connection Endpoints](regions.md)\. For more information about IAM, access keys, and secret keys, see [How Do I Get Credentials?](https://docs.aws.amazon.com/IAM/latest/UserGuide/IAM_Introduction.html#IAM_SecurityCredentials) and [Managing Access Keys for IAM Users](https://docs.aws.amazon.com/IAM/latest/UserGuide/ManagingCredentials.html)\.

Next, you must install Git\. 
+ **For Linux, macOS, or Unix**:

  To work with files, commits, and other information in AWS CodeCommit repositories, you must install Git on your local machine\. AWS CodeCommit supports Git versions 1\.7\.9 and later\.

  To install Git, we recommend websites such as [Git Downloads](http://git-scm.com/downloads)\.
**Note**  
Git is an evolving, regularly updated platform\. Occasionally, a feature change might affect the way it works with AWS CodeCommit\. If you encounter issues with a specific version of Git and AWS CodeCommit, review the information in [Troubleshooting](troubleshooting.md)\.
+ **For Windows:** 

  To work with files, commits, and other information in AWS CodeCommit repositories, you must install Git on your local machine\. AWS CodeCommit supports Git versions 1\.7\.9 and later\.

  To install Git, we recommend websites such as [Git for Windows](http://msysgit.github.io/)\. If you use this link to install Git, you can accept all of the installation default settings except for the following: 
  + When prompted during the **Adjusting your PATH environment** step, select the **Use Git from the Windows Command Prompt** option\.
  + \(Optional\) If you intend to use HTTPS with the credential helper that is included in the AWS CLI instead of configuring Git credentials for AWS CodeCommit, on the **Configuring extra options** page, make sure the **Enable Git Credential Manager** option is cleared\. The Git Credential Manager is only compatible with AWS CodeCommit if IAM users configure Git credentials\. For more information, see [For HTTPS Users Using Git Credentials](setting-up-gc.md) and [Git for Windows: I Installed Git for Windows, but I Am Denied Access to My Repository \(403\)](troubleshooting-ch.md#troubleshooting-windowshttps)\.
**Note**  
Git is an evolving, regularly updated platform\. Occasionally, a feature change might affect the way it works with AWS CodeCommit\. If you encounter issues with a specific version of Git and AWS CodeCommit, review the information in [Troubleshooting](troubleshooting.md)\.

AWS CodeCommit supports both HTTPS and SSH authentication\. To complete setup, you must configure Git credentials for AWS CodeCommit \(HTTPS, recommended for most users\), an SSH key pair to use when accessing AWS CodeCommit \(SSH\), or the credential helper included in the AWS CLI \(HTTPS\)\. 
+ For Git credentials on all supported operating systems, see [Step 3: Create Git Credentials for HTTPS Connections to AWS CodeCommit](setting-up-gc.md#setting-up-gc-iam)\.
+ For SSH on Linux, macOS, or Unix, see [SSH and Linux, macOS, or Unix: Set Up the Public and Private Keys for Git and AWS CodeCommit](setting-up-ssh-unixes.md#setting-up-ssh-unixes-keys-unixes)\.
+  For SSH on Windows, see [SSH and Windows: Set Up the Public and Private Keys for Git and AWS CodeCommit](setting-up-ssh-windows.md#setting-up-ssh-windows-keys-windows)\.
+ For the credential helper on Linux, macOS, or Unix, see [Set Up the Credential Helper \(Linux, macOS, or Unix\)](setting-up-https-unixes.md#setting-up-https-unixes-ch-config)\.
+ For the credential helper on Windows, see [Set Up the Credential Helper \(Windows\)](setting-up-https-windows.md#setting-up-https-windows-ch-config)\.

## Step 1: Create an AWS CodeCommit Repository<a name="how-to-migrate-existing-create"></a>

In this section, you use the AWS CodeCommit console to create the AWS CodeCommit repository you use for the rest of this tutorial\. To use the AWS CLI to create the repository, see [Create an AWS CodeCommit Repository \(AWS CLI\)](how-to-create-repository.md#how-to-create-repository-cli)\.

1. Open the AWS CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In the region selector, choose the region where you want to create the repository\. For more information, see [Regions and Git Connection Endpoints](regions.md)\.

1. On the **Repositories** page, choose **Create repository**\. 

1. On the **Create repository** page, in **Repository name**, enter a name for the repository\.
**Note**  
This name must be unique in the region for your AWS account\.

1. \(Optional\) In the **Description** box, enter a description for the repository\. This can help you and other users identify the purpose of the repository\. 
**Note**  
The description field displays Markdown in the console and accepts all HTML characters and valid Unicode characters\. If you are an application developer who is using the `GetRepository` or `BatchGetRepositories` APIs and you plan to display the repository description field in a web browser, see the [AWS CodeCommit API Reference](https://docs.aws.amazon.com/codecommit/latest/APIReference/)\.

1. Choose **Create**\. 

![\[Creating a repository for migrating a Git repository to AWS CodeCommit\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-create-repo-migrate-existing.png)

After it is created, the repository appears in the **Repositories** list\. In the URL column, choose the copy icon, and then choose the protocol \(SSH or HTTPS\) to be used to connect to AWS CodeCommit\. Copy the URL\.

For example, if you named your repository *MyClonedRepository* and you are using Git credentials with HTTPS in the US West \(Oregon\) region, the URL would look like the following:

```
https://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyClonedRepository
```

You need this URL later in [Step 2: Clone the Repository and Push to the AWS CodeCommit Repository](#how-to-migrate-existing-clone)\.

## Step 2: Clone the Repository and Push to the AWS CodeCommit Repository<a name="how-to-migrate-existing-clone"></a>

In this section, you clone an existing Git repository to your local computer, creating what is called a local repo\. You then push the contents of the local repo to the AWS CodeCommit repository you created earlier\.

1. From the terminal or command prompt on your local computer, run the git clone command with the `--mirror` option to clone a bare copy of the remote repository into a new folder named *aws\-codecommit\-demo*\. This is a bare repo meant only for migration\. It is not the local repo for interacting with the migrated repository in AWS CodeCommit\. You can create that later, after the migration to AWS CodeCommit is complete\.

   The following example clones a sample application created for AWS demonstration purposes and hosted on GitHub \(*https://github\.com/awslabs/aws\-demo\-php\-simple\-app\.git*\) to a local repo in a directory named *aws\-codecommit\-demo*\. 

   ```
   git clone --mirror https://github.com/awslabs/aws-demo-php-simple-app.git aws-codecommit-demo
   ```

1. Change directories to the directory where you made the clone\.

   ```
   cd aws-codecommit-demo
   ```

1. Run the git push command, specifying the URL and name of the destination AWS CodeCommit repository and the \-\-all option\. \(This is the URL you copied in [Step 1: Create an AWS CodeCommit Repository](#how-to-migrate-existing-create)\)\.

   For example, if you named your repository *MyClonedRepository* and you are set up to use HTTPS, you would type the following command:

   ```
   git push https://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyClonedRepository --all
   ```
**Note**  
The \-\-all option only pushes all branches for the repository\. It does not push other references, such as tags\. If you want to push tags, wait until the initial push is complete, and then push again, this time using the \-\-tags option:  

   ```
   git push ssh://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyClonedRepository --tags
   ```
For more information about git push and its options, see [Git push](https://git-scm.com/docs/git-push) on the Git website\. For information about pushing large repositories, especially when pushing all references at once \(for example, with the \-\-mirror option\), see [Migrate a Repository in Increments](how-to-push-large-repositories.md)\.

You can delete the *aws\-codecommit\-demo* folder and its contents after you have migrated the repository to AWS CodeCommit\. To create a local repo with all the correct references for working with the repository in AWS CodeCommit, run the `git clone` command without the `--mirror` option:

```
git clone https://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyClonedRepository
```

## Step 3: View Files in AWS CodeCommit<a name="how-to-migrate-existing-view"></a>

After you have pushed the contents of your directory, you can use the AWS CodeCommit console to quickly view all of the files in that repository\.

1. Open the AWS CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In **Repositories**, choose the name of the repository \(for example, *MyClonedRepository*\)\. 

1. View the files in the repository for the branches, the clone URLs, the settings, and more\.  
![\[View of a cloned repository in AWS CodeCommit\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-cloned-repo-url.png)

## Step 4: Share the AWS CodeCommit Repository<a name="how-to-migrate-existing-share"></a>

When you create a repository in AWS CodeCommit, two endpoints are generated: one for HTTPS connections and one for SSH connections\. Both provide secure connections over a network\. Your users can use either protocol\. Both endpoints remain active no matter which protocol you recommend to your users\. Before you can share your repository with others, you must create IAM policies that allow other users access to your repository\. Provide those access instructions to your users\. 

**Create a customer managed policy for your repository**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the **Dashboard** navigation area, choose **Policies**, and then choose **Create Policy**\. 

1. On the **Create Policy** page, next to **Copy an AWS Managed Policy**, choose **Select**\.

1. On the **Copy an AWS Managed Policy** page, in **Search Policies**, enter **AWSCodeCommitPowerUser**\. Choose **Select** next to the policy name\.

1. On the **Review Policy** page, in **Policy Name**, enter a new name for the policy \(for example, *AWSCodeCommitPowerUser\-MyDemoRepo*\)\.

   In **Policy Document**, replace the "\*" portion of the `Resource` line with the Amazon Resource Name \(ARN\) of the AWS CodeCommit repository, as shown here:

   ```
   "Resource": [
    "arn:aws:codecommit:us-east-2:80398EXAMPLE:MyDemoRepo"
    ]
   ```
**Tip**  
To find the ARN for the AWS CodeCommit repository, go to the AWS CodeCommit console and choose the repository name from the list\. For more information, see [View Repository Details](how-to-view-repository-details.md)\.

   If you want this policy to apply to more than one repository, add each repository as a resource by specifying its ARN\. Include a comma between each resource statement, as shown here:

   ```
   "Resource": [
    "arn:aws:codecommit:us-east-2:80398EXAMPLE:MyDemoRepo",
    "arn:aws:codecommit:us-east-2:80398EXAMPLE:MyOtherDemoRepo"
    ]
   ```

1. Choose **Validate Policy**\. After the policy is validated, choose **Create Policy**\.

To manage access to your repository, create an IAM group for its users, add IAM users to that group, and then attach the customer managed policy you created in the previous step\. Attach any other policies required for access, such as IAMUserSSHKeys or IAMSelfManageServiceSpecificCredentials\. 

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the **Dashboard** navigation area, choose **Groups**, and then choose **Create New Group**\. 

1. On the **Set Group Name** page, in **Group Name**, enter a name for the group \(for example, *MyDemoRepoGroup*\), and then choose **Next Step**\. Consider including the repository name as part of the group name\.
**Note**  
This name must be unique across an AWS account\.

1. Select the check box next to the customer managed policy you created in the previous section \(for example, **AWSCodeCommitPowerUser\-MyDemoRepo**\)\. 

1. On the **Review** page, choose **Create Group**\. IAM creates this group with the specified policies already attached\. The group appears in the list of groups associated with your AWS account\.

1. Choose your group from the list\. 

1. On the group summary page, choose the **Users** tab, and then choose **Add Users to Group**\. On the list that shows all users associated with your AWS account, select the check boxes next to the users to whom you want to allow access to the AWS CodeCommit repository, and then choose **Add Users**\.
**Tip**  
You can use the Search box to quickly find users by name\.

1. When you have added your users, close the IAM console\.

After you have created an IAM user to access AWS CodeCommit using the policy group and policies you configured, send that user the information required to connect to the repository\.

1. Open the AWS CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In the region selector, choose the region where the repository was created\. Repositories are specific to an AWS region\. For more information, see [Regions and Git Connection Endpoints](regions.md)\.

1. On the **Repositories** page, find the name of the repository you want to share\. 

1. In **Clone URL**, choose the protocol \(HTTPS or SSH\) that you want your users to use\. This copies the clone URL for the connection protocol\. 

1. Send your users the clone URL along with any other instructions, such as installing the AWS CLI, configuring a profile, or installing Git\. Make sure to include the configuration information for the connection protocol \(for example, for HTTPS, configuring the credential helper for Git\)\. 