# Setup Steps for HTTPS Connections to AWS CodeCommit Repositories on Linux, macOS, or Unix with the AWS CLI Credential Helper<a name="setting-up-https-unixes"></a>

Before you can connect to AWS CodeCommit for the first time, you must complete the initial configuration steps\. This topic walks you through the steps to set up your computer and AWS profile, connect to a CodeCommit repository, and clone that repository to your computer, also known as creating a local repo\. If you're new to Git, you might also want to review the information in [Where Can I Learn More About Git?](welcome.md#welcome-get-started-with-git)\.

**Topics**
+ [Step 1: Initial Configuration for CodeCommit](#setting-up-https-unixes-account)
+ [Step 2: Install Git](#setting-up-https-unixes-install-git)
+ [Step 3: Set Up the Credential Helper](#setting-up-https-unixes-credential-helper)
+ [Step 4: Connect to the CodeCommit Console and Clone the Repository](#setting-up-https-unixes-connect-console)
+ [Next Steps](#setting-up-https-unixes-next-step)

## Step 1: Initial Configuration for CodeCommit<a name="setting-up-https-unixes-account"></a>

Follow these steps to set up an AWS account, create and configure an IAM user, and install the AWS CLI\. 

**To create and configure an IAM user for accessing CodeCommit**

1. Create an AWS account by going to [http://aws\.amazon\.com](http://aws.amazon.com) and choosing **Sign Up**\.

1. Create an IAM user, or use an existing one, in your AWS account\. Make sure you have an access key ID and a secret access key associated with that IAM user\. For more information, see [Creating an IAM User in Your AWS Account](https://docs.aws.amazon.com/IAM/latest/UserGuide/Using_SettingUpUser.html)\.
**Note**  
CodeCommit requires AWS Key Management Service\. If you are using an existing IAM user, make sure there are no policies attached to the user that expressly deny the AWS KMS actions required by CodeCommit\. For more information, see [AWS KMS and Encryption](encryption.md)\.

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the IAM console, in the navigation pane, choose **Users**, and then choose the IAM user you want to configure for CodeCommit access\.

1. On the **Permissions** tab, choose **Add Permissions**\. 

1. In **Grant permissions**, choose **Attach existing policies directly**\.

1. From the list of policies, select **AWSCodeCommitFullAccess** or another managed policy for CodeCommit access\. For more information, see [AWS Managed \(Predefined\) Policies for CodeCommit](auth-and-access-control-iam-identity-based-access-control.md#managed-policies)\.

   After you have selected the policy you want to attach, choose** Next: Review** to review the list of policies that will be attached to the IAM user\. If the list is correct, choose **Add permissions**\.

    For more information about CodeCommit managed policies and sharing access to repositories with other groups and users, see [Share a Repository](how-to-share-repository.md) and [Authentication and Access Control for AWS CodeCommit](auth-and-access-control.md)\.

**To install and configure the AWS CLI**

1. On your local machine, download and install the AWS CLI\. This is a prerequisite for interacting with CodeCommit from the command line\. For more information, see [Getting Set Up with the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-set-up.html)\.
**Note**  
CodeCommit works only with AWS CLI versions 1\.7\.38 and later\. To determine which version of the AWS CLI you have installed, run the aws \-\-version command\.  
To upgrade an older version of the AWS CLI to the latest version, see [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html)\.

1.  Run this command to verify the CodeCommit commands for the AWS CLI are installed:

   ```
   aws codecommit help
   ```

   This command should return a list of CodeCommit commands\.

1. Configure the AWS CLI with the configure command, as follows:

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
   + me\-south\-1

   For more information about CodeCommit and AWS Regions, see [Regions and Git Connection Endpoints](regions.md)\. For more information about IAM, access keys, and secret keys, see [How Do I Get Credentials?](https://docs.aws.amazon.com/IAM/latest/UserGuide/IAM_Introduction.html#IAM_SecurityCredentials) and [Managing Access Keys for IAM Users](https://docs.aws.amazon.com/IAM/latest/UserGuide/ManagingCredentials.html)\.

## Step 2: Install Git<a name="setting-up-https-unixes-install-git"></a>

To work with files, commits, and other information in CodeCommit repositories, you must install Git on your local machine\. CodeCommit supports Git versions 1\.7\.9 and later\.

To install Git, we recommend websites such as [Git Downloads](http://git-scm.com/downloads)\.

**Note**  
Git is an evolving, regularly updated platform\. Occasionally, a feature change might affect the way it works with CodeCommit\. If you encounter issues with a specific version of Git and CodeCommit, review the information in [Troubleshooting](troubleshooting.md)\.

## Step 3: Set Up the Credential Helper<a name="setting-up-https-unixes-credential-helper"></a><a name="setting-up-https-unixes-ch-config"></a>

1. From the terminal, use Git to run git config, specifying the use of the Git credential helper with the AWS credential profile, and enabling the Git credential helper to send the path to repositories:

   ```
   git config --global credential.helper '!aws codecommit credential-helper $@'
   git config --global credential.UseHttpPath true
   ```
**Tip**  
The credential helper uses the default AWS credential profile or the Amazon EC2 instance role\. You can specify a profile to use, such as `CodeCommitProfile`, if you have created an AWS credential profile to use with CodeCommit:   

   ```
   git config --global credential.helper '!aws --profile CodeCommitProfile codecommit credential-helper $@'    
   ```
If your profile name contains spaces, make sure you enclose the name in quotation marks \("\)\.  
You can configure profiles per repository instead of globally by using `--local` instead of `--global`\.

   The Git credential helper writes the following value to `~/.gitconfig`:

   ```
   [credential]    
       helper = !aws --profile CodeCommitProfile codecommit credential-helper $@
       UseHttpPath = true
   ```
**Important**  
If you want to use a different IAM user on the same local machine for CodeCommit, you must run the git config command again and specify a different AWS credential profile\.

1. Run git config \-\-global \-\-edit to verify the preceding value has been written to `~/.gitconfig`\. If successful, you should see the preceding value \(in addition to values that might already exist in the Git global configuration file\)\. To exit, typically you would type **:q**, and then press Enter\. 

   If you experience problems after you configure your credential helper, see [Troubleshooting](troubleshooting.md)\.
**Important**  
If you are using macOS, use the following steps to ensure the credential helper is configured correctly\. 

1. If you are using macOS, use HTTPS to [connect to an CodeCommit repository](how-to-connect.md)\. After you connect to a CodeCommit repository with HTTPS for the first time, subsequent access fails after about fifteen minutes\. The default Git version on macOS uses the Keychain Access utility to store credentials\. For security measures, the password generated for access to your CodeCommit repository is temporary, so the credentials stored in the keychain stop working after about 15 minutes\. To prevent these expired credentials from being used, you must either:
   + Install a version of Git that does not use the keychain by default\.
   + Configure the Keychain Access utility to not provide credentials for CodeCommit repositories\.

   1. Open the Keychain Access utility\. \(You can use Finder to locate it\.\)

   1. Search for `git-codecommit.us-east-2.amazonaws.com`\. Highlight the row, open the context menu or right\-click it, and then choose **Get Info**\.

   1. Choose the **Access Control** tab\.

   1. In **Confirm before allowing access**, choose `git-credential-osxkeychain`, and then choose the minus sign to remove it from the list\.
**Note**  
After you remove `git-credential-osxkeychain` from the list, you see a pop\-up message whenever you run a Git command\. Choose **Deny** to continue\. If you find the pop\-ups too disruptive, here are some other options:  
Connect to CodeCommit using SSH instead of HTTPS\. For more information, see [For SSH Connections on Linux, macOS, or Unix](setting-up-ssh-unixes.md)\. 
In the Keychain Access utility, on the **Access Control** tab for `git-codecommit.us-east-2.amazonaws.com`, choose the **Allow all applications to access this item \(access to this item is not restricted\)** option\. This prevents the pop\-ups, but the credentials eventually expire \(on average, in about 15 minutes\) and you see a 403 error message\. When this happens, you must delete the keychain item to restore functionality\.
Install a version of Git that does not use the keychain by default\.

## Step 4: Connect to the CodeCommit Console and Clone the Repository<a name="setting-up-https-unixes-connect-console"></a>

If an administrator has already sent you the name and connection details for the CodeCommit repository, you can skip this step and clone the repository directly\.

**To connect to a CodeCommit repository**

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In the region selector, choose the AWS Region where the repository was created\. Repositories are specific to an AWS Region\. For more information, see [Regions and Git Connection Endpoints](regions.md)\.

1. Choose the repository you want to connect to from the list\. This opens the **Code** page for that repository\.

    If you see a **Welcome** page instead of a list of repositories, there are no repositories associated with your AWS account\. To create a repository, see [Create an AWS CodeCommit Repository](how-to-create-repository.md) or follow the steps in the [Getting Started with Git and CodeCommit](getting-started.md) tutorial\.

1. Copy the HTTPS URL to use when connecting to the repository\.

1. Open a terminal and from the /tmp directory, use the URL to clone the repository with the git clone command\. For example, to clone a repository named *MyDemoRepo* to a local repo named *my\-demo\-repo* in the US East \(Ohio\) Region:

   ```
   git clone https://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo my-demo-repo
   ```

   For more information, see [Connect to the CodeCommit Repository by Cloning the Repository](how-to-connect.md#how-to-connect-http) and [Create a Commit](how-to-create-commit.md)\.

## Next Steps<a name="setting-up-https-unixes-next-step"></a>

You have completed the prerequisites\. Follow the steps in [Getting Started with CodeCommit ](getting-started-cc.md) to start using CodeCommit\.