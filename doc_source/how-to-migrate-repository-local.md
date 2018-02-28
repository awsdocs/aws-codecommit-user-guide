# Migrate Local or Unversioned Content to AWS CodeCommit<a name="how-to-migrate-repository-local"></a>

The procedures in this topic walk you through the process of migrating an existing project or local content on your computer to an AWS CodeCommit repository\. As part of this process, you will:

+ Complete the initial setup required for AWS CodeCommit\.

+ Create an AWS CodeCommit repository\.

+ Place a local folder under Git version control and push the contents of that folder to the AWS CodeCommit repository\.

+ View files in the AWS CodeCommit repository\.

+ Share the AWS CodeCommit repository with your team\.

![\[Migrating a local project to AWS CodeCommit\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-migrate-local.png)![\[Migrating a local project to AWS CodeCommit\]](http://docs.aws.amazon.com/codecommit/latest/userguide/)


+ [Step 0: Setup Required for Access to AWS CodeCommit](#how-to-migrate-local-setup)
+ [Step 1: Create an AWS CodeCommit Repository](#how-to-migrate-local-create)
+ [Step 2: Migrate Local Content to the AWS CodeCommit Repository](#how-to-migrate-local-version)
+ [Step 3: View Files in AWS CodeCommit](#how-to-migrate-local-view)
+ [Step 4: Share the AWS CodeCommit Repository](#how-to-migrate-local-share)

## Step 0: Setup Required for Access to AWS CodeCommit<a name="how-to-migrate-local-setup"></a>

Before you can migrate local content to AWS CodeCommit, you must create and configure an IAM user for AWS CodeCommit and configure your local computer for access\. You should also install the AWS CLI to manage AWS CodeCommit\. Although you can perform most AWS CodeCommit tasks without it, the AWS CLI offers flexibility when working with Git\. 

If you are already set up for AWS CodeCommit, you can skip ahead to [Step 1: Create an AWS CodeCommit Repository](#how-to-migrate-local-create)\.

**To create and configure an IAM user for accessing AWS CodeCommit**

1. Create an AWS account by going to [http://aws\.amazon\.com](http://aws.amazon.com) and choosing **Sign Up**\.

1. Create an IAM user, or use an existing one, in your AWS account\. Make sure you have an access key ID and a secret access key associated with that IAM user\. For more information, see [Creating an IAM User in Your AWS Account](http://docs.aws.amazon.com/IAM/latest/UserGuide/Using_SettingUpUser.html)\.
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

1. On your local machine, download and install the AWS CLI\. This is a prerequisite for interacting with AWS CodeCommit from the command line\. For more information, see [Getting Set Up with the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-set-up.html)\.
**Note**  
AWS CodeCommit works only with AWS CLI versions 1\.7\.38 and later\. To determine which version of the AWS CLI you have installed, run the `aws --version` command\.  
To upgrade an older version of the AWS CLI to the latest version, see [Installing the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/installing.html)\.

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

   For more information about AWS CodeCommit and regions, see [Regions and Git Connection Endpoints](regions.md)\. For more information about IAM, access keys, and secret keys, see [How Do I Get Credentials?](http://docs.aws.amazon.com/IAM/latest/UserGuide/IAM_Introduction.html#IAM_SecurityCredentials) and [Managing Access Keys for IAM Users](http://docs.aws.amazon.com/IAM/latest/UserGuide/ManagingCredentials.html)\.

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

AWS CodeCommit supports both HTTPS and SSH authentication\. To complete setup, you must configure either Git credentials for AWS CodeCommit \(HTTPS, recommended for most users\), an SSH key pair \(SSH\) to use when accessing AWS CodeCommit, or the credential helper included in the AWS CLI\. 

+ For Git credentials on all supported operating systems, see [Step 3: Create Git Credentials for HTTPS Connections to AWS CodeCommit](setting-up-gc.md#setting-up-gc-iam)\.

+ For SSH on Linux, macOS, or Unix, see [SSH and Linux, macOS, or Unix: Set Up the Public and Private Keys for Git and AWS CodeCommit](setting-up-ssh-unixes.md#setting-up-ssh-unixes-keys-unixes)\.

+  For SSH on Windows, see [SSH and Windows: Set Up the Public and Private Keys for Git and AWS CodeCommit](setting-up-ssh-windows.md#setting-up-ssh-windows-keys-windows)\.

+ For the credential helper on Linux, macOS, or Unix, see [Set Up the Credential Helper \(Linux, macOS, or Unix\)](setting-up-https-unixes.md#setting-up-https-unixes-ch-config)\.

+ For the credential helper on Windows, see [Set Up the Credential Helper \(Windows\)](setting-up-https-windows.md#setting-up-https-windows-ch-config)\.

## Step 1: Create an AWS CodeCommit Repository<a name="how-to-migrate-local-create"></a>

In this section, you will use the AWS CodeCommit console to create the AWS CodeCommit repository you will use for the rest of this tutorial\. To use the AWS CLI to create the repository, see [Use the AWS CLI to Create an AWS CodeCommit Repository](how-to-create-repository.md#how-to-create-repository-cli)\.

1. Open the AWS CodeCommit console at [https://console\.aws\.amazon\.com/codecommit](https://console.aws.amazon.com/codecommit)\.

1. In the region selector, choose the region where you will create the repository\. For more information, see [Regions and Git Connection Endpoints](regions.md)\.

1. On the **Dashboard** page, choose **Create repository**\. \(If a welcome page appears instead of the **Dashboard** page, choose **Get Started Now**\.\) 

1. On the **Create repository** page, in **Repository name**, type a name for the repository\.
**Note**  
This name must be unique in the region for your AWS account\.

1. \(Optional\) In the **Description** box, type a description for the repository\. This can help you and other users identify the purpose of the repository\. 
**Note**  
The description field accepts all HTML characters and all valid Unicode characters\. If you are an application developer using the `GetRepository` or `BatchGetRepositories` APIs and plan to display the repository description field in a web browser, see the [AWS CodeCommit API Reference](http://docs.aws.amazon.com/codecommit/latest/APIReference/)\.

1. Choose **Create repository**\. 

1. In **Configure email notifications**, configure notifications so that repository users receive emails about important repository events\. This step is optional, but recommended\. You can choose the event types \(for example, comments on code\) and whether to use an existing Amazon SNS topic or create one specifically for this purpose\. You can choose to skip this step and configure notifications at a later time\. For more information, see [Configuring Notifications for Events in an AWS CodeCommit Repository](how-to-repository-email.md)\. 

![\[Creating a repository for migrating local content to AWS CodeCommit\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-create-repo-migrate-local.png)![\[Creating a repository for migrating local content to AWS CodeCommit\]](http://docs.aws.amazon.com/codecommit/latest/userguide/)

After it is created, the repository will appear in the list of repositories in your dashboard\. In the URL column, choose the copy icon, and then choose the protocol \(HTTPS or SSH\) you will use to connect to AWS CodeCommit\. Copy the URL\.

For example, if you named your repository *MyFirstRepo* and you are using HTTPS, the URL would look like the following:

```
https://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyFirstRepo
```

You will need this URL later in [Step 2: Migrate Local Content to the AWS CodeCommit Repository](#how-to-migrate-local-version)\.

## Step 2: Migrate Local Content to the AWS CodeCommit Repository<a name="how-to-migrate-local-version"></a>

Now that you have an AWS CodeCommit repository, you can choose a directory on your local computer to convert into a local Git repository\. The git init command can be used to either convert existing, unversioned content to a Git repository or, if you do not yet have files or content, to initialize a new, empty repository\.

1. From the terminal or command line on your local computer, change directories to the directory you want to use as the source for your repository\.

1. Run the git init command to initialize Git version control in the directory\. This will create a \.git subdirectory in the root of the directory that enables version control tracking\. The \.git folder also contains all of the required metadata for the repository\. 

   ```
   git init
   ```

1. Add the files you want to add to version control\. In this tutorial, you will run the `git add` command with the `.` specifier to add all of the files in this directory\. For other options, consult your Git documentation\. 

   ```
   git add .
   ```

1. Create a commit for the added files with a commit message\. 

   ```
   git commit –m "Initial commit"
   ```

1. Run the git push command, specifying the URL and name of the destination AWS CodeCommit repository and the `--all` option\. \(This is the URL you copied in [Step 1: Create an AWS CodeCommit Repository](#how-to-migrate-local-create)\.\)

   For example, if you named your repository *MyFirstRepo* and you are set up to use HTTPS, you would type the following command:

   ```
   git push https://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyFirstRepo --all
   ```

## Step 3: View Files in AWS CodeCommit<a name="how-to-migrate-local-view"></a>

After you have pushed the contents of your directory, you can use the AWS CodeCommit console to quickly view all of the files in the repository\.

1. Open the AWS CodeCommit console at [https://console\.aws\.amazon\.com/codecommit](https://console.aws.amazon.com/codecommit)\.

1. Choose the name of the repository from the list \(for example, *MyFirstRepository*\)\. 

1. View the files in the repository for the branches, the clone URLs, the settings, and more\.  
![\[View of the contents of a repository in AWS CodeCommit\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-view-migrate-local.png)![\[View of the contents of a repository in AWS CodeCommit\]](http://docs.aws.amazon.com/codecommit/latest/userguide/)

## Step 4: Share the AWS CodeCommit Repository<a name="how-to-migrate-local-share"></a>

When you create a repository in AWS CodeCommit, two endpoints are generated: one for HTTPS connections and one for SSH connections\. Both provide secure connections over a network\. Your users can use either protocol\. Both endpoints remain active no matter which protocol you recommend to your users\. Before you can share your repository with others, you must create IAM policies that allow access to your repository to other users\. Provide those access instructions to your users\. 

**Create a customer managed policy for your repository**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the **Dashboard** navigation area, choose **Policies**, and then choose **Create Policy**\. 

1. On the **Create Policy** page, next to **Copy an AWS Managed Policy**, choose **Select**\.

1. On the **Copy an AWS Managed Policy** page, type **AWSCodeCommitPowerUser** in the **Search Policies** search box\. Choose **Select** next to that policy name\.

1. On the **Review Policy** page, in **Policy Name**, type a new name for the policy \(for example, *AWSCodeCommitPowerUser\-MyDemoRepo*\)\.

   In the **Policy Document** text box, replace the "\*" portion of the `Resource` line with the Amazon Resource Name \(ARN\) of the AWS CodeCommit repository\. For example:

   ```
   "Resource": [
    "arn:aws:codecommit:us-east-2:80398EXAMPLE:MyDemoRepo"
    ]
   ```
**Tip**  
To find the ARN for the AWS CodeCommit repository, go to the AWS CodeCommit console and choose the repository name from the list\. For more information, see [View Repository Details](how-to-view-repository-details.md)\.

   If you want this policy to apply to more than one repository, add each repository as a resource by specifying its ARN\. Include a comma between each resource statement, as shown in the following example:

   ```
   "Resource": [
    "arn:aws:codecommit:us-east-2:80398EXAMPLE:MyDemoRepo",
    "arn:aws:codecommit:us-east-2:80398EXAMPLE:MyOtherDemoRepo"
    ]
   ```

1. Choose **Validate Policy**\. After it is validated, choose **Create Policy**\.
**Tip**  
Creating a managed policy for a repository does not supply additional permissions required for individual users to set up Git credentials or SSH keys in IAM\. You must apply these managed policies to individual IAM users\.  
To allow users to use Git credentials to connect to AWS CodeCommit, select the **IAMSelfManageServiceSpecificCredentials** and **IAMReadOnlyAccess** managed policies and apply them to your users\.
To allow users to use SSH to connect to AWS CodeCommit, select the **IAMUserSSHKeys** and **IAMReadOnlyAccess** managed policies and apply them to your users\.

To manage access to your repository, create an IAM group for its users, add IAM users to that group, and then attach the customer managed policy you created in the previous step, as well as any additional policies required for access, such as IAMSelfManageServiceSpecificCredentials or IAMUserSSHKeys\. 

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the **Dashboard** navigation area, choose **Groups**, and then choose **Create New Group**\. 

1. On the **Set Group Name** page, in the **Group Name** box, type a name for the group \(for example, *MyDemoRepoGroup*\), and then choose **Next Step**\. Consider including the repository name as part of the group name\.
**Note**  
This name must be unique across an AWS account\.

1. Select the check box next to the customer managed policy you created in the previous section \(for example, **AWSCodeCommitPowerUser\-MyDemoRepo**\)\. 

   + If your users will use HTTPS and Git credentials to connect to AWS CodeCommit, select the check boxes next to **IAMSelfManageServiceSpecificCredentials** and **IAMReadOnlyAccess**, and then choose **Next Step**\. 

   + If your users will use SSH to connect to your repository, select the check boxes next to **IAMUserSSHKeys** and **IAMReadOnlyAccess**, and then choose **Next Step**\.

1. On the **Review** page, choose **Create Group**\. The group will be created in IAM with the specified policies already attached\. It will appear in the list of groups associated with your AWS account\.

1. Choose your group from the list\. 

1. On the group summary page, choose the **Users** tab, and then choose **Add Users to Group**\. On the list that shows all users associated with your AWS account, select the check boxes next to the users to whom you want to allow access to the AWS CodeCommit repository, and then choose **Add Users**\.
**Tip**  
You can use the Search box to quickly find users by name\.

1. When you have added your users, close the IAM console\.

After you have created an IAM user that will access AWS CodeCommit using the policy group and policies you configured, send that user the connection information they will use to connect to the repository\.

1. Open the AWS CodeCommit console at [https://console\.aws\.amazon\.com/codecommit](https://console.aws.amazon.com/codecommit)\.

1. In the region selector, choose the region where the repository was created\. Repositories are specific to an AWS region\. For more information, see [Regions and Git Connection Endpoints](regions.md)\.

1. On the **Dashboard** page, choose the name of the repository you want to share\. 

1. On the **Code** page, choose **Clone URL**, and then choose the protocol you want your users to use\. 

1. Copy the displayed URL for the connection protocol your users will use when connecting to your AWS CodeCommit repository\. 

1. Send your users the connection information along with any other instructions, such as installing the AWS CLI, configuring a profile, or installing Git\. Make sure to include the configuration information for the connection protocol \(for example, for HTTPS, configuring the credential helper for Git\)\. 