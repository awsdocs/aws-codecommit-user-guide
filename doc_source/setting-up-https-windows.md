# Setup Steps for HTTPS Connections to AWS CodeCommit Repositories on Windows with the AWS CLI Credential Helper<a name="setting-up-https-windows"></a>

Before you can connect to AWS CodeCommit for the first time, you must complete the initial configuration steps\. For most users, this can be done most easily by following the steps in [For HTTPS Users Using Git Credentials](setting-up-gc.md)\. However, if you want to connect to CodeCommit using a root account, federated access, or temporary credentials, you must use the credential helper that is included in the AWS CLI\.

This topic walks you through the steps to install the AWS CLI, set up your computer and AWS profile, connect to a CodeCommit repository, and clone that repository to your computer, also known as creating a local repo\. If you're new to Git, you might also want to review the information in [Where Can I Learn More About Git?](welcome.md#welcome-get-started-with-git)\.

**Topics**
+ [Step 1: Initial Configuration for CodeCommit](#setting-up-https-windows-account)
+ [Step 2: Install Git](#setting-up-https-windows-install-git)
+ [Step 3: Set Up the Credential Helper](#setting-up-https-windows-credential-helper)
+ [Step 4: Connect to the CodeCommit Console and Clone the Repository](#setting-up-https-windows-connect-console)
+ [Next Steps](#setting-up-https-windows-next-step)

## Step 1: Initial Configuration for CodeCommit<a name="setting-up-https-windows-account"></a>

Follow these steps to set up an AWS account, create and configure an IAM user, and install the AWS CLI\. The AWS CLI includes a credential helper that you configure for HTTPS connections to your CodeCommit repositories\. 

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

   For more information about CodeCommit and AWS Regions, see [Regions and Git Connection Endpoints](regions.md)\. For more information about IAM, access keys, and secret keys, see [How Do I Get Credentials?](https://docs.aws.amazon.com/IAM/latest/UserGuide/IAM_Introduction.html#IAM_SecurityCredentials) and [Managing Access Keys for IAM Users](https://docs.aws.amazon.com/IAM/latest/UserGuide/ManagingCredentials.html)\.

## Step 2: Install Git<a name="setting-up-https-windows-install-git"></a>

To work with files, commits, and other information in CodeCommit repositories, you must install Git on your local machine\. CodeCommit supports Git versions 1\.7\.9 and later\.

To install Git, we recommend websites such as [Git for Windows](https://gitforwindows.org/)\. If you use this link to install Git, you can accept all of the installation default settings except for the following: 
+ When prompted during the **Adjusting your PATH environment** step, select the **Use Git from the Windows Command Prompt** option\.
+ \(Optional\) If you intend to use HTTPS with the credential helper that is included in the AWS CLI instead of configuring Git credentials for CodeCommit, on the **Configuring extra options** page, make sure the **Enable Git Credential Manager** option is cleared\. The Git Credential Manager is only compatible with CodeCommit if IAM users configure Git credentials\. For more information, see [For HTTPS Users Using Git Credentials](setting-up-gc.md) and [Git for Windows: I installed Git for Windows, but I am denied access to my repository \(403\)](troubleshooting-ch.md#troubleshooting-windowshttps)\.

**Note**  
Git is an evolving, regularly updated platform\. Occasionally, a feature change might affect the way it works with CodeCommit\. If you encounter issues with a specific version of Git and CodeCommit, review the information in [Troubleshooting](troubleshooting.md)\.

## Step 3: Set Up the Credential Helper<a name="setting-up-https-windows-credential-helper"></a>

The AWS CLI includes a Git credential helper you can use with CodeCommit\. The Git credential helper requires an AWS credential profile, which stores a copy of an IAM user's AWS access key ID and AWS secret access key \(along with a default AWS Region name and default output format\)\. The Git credential helper uses this information to automatically authenticate with CodeCommit so you don't need to enter this information every time you use Git to interact with CodeCommit\.  <a name="setting-up-https-windows-ch-config"></a>

1. Open a command prompt and use Git to run git config, specifying the use of the Git credential helper with the AWS credential profile, which enables the Git credential helper to send the path to repositories:

   ```
   git config --global credential.helper "!aws codecommit credential-helper $@"
   git config --global credential.UseHttpPath true
   ```

   The Git credential helper writes the following to the \.gitconfig file:

   ```
   [credential]    
       helper = !aws codecommit credential-helper $@ 
       UseHttpPath = true
   ```
**Important**  
If you are using a Bash emulator instead of the Windows command line, you must use single quotes instead of double quotes\.
The credential helper uses the default AWS profile or the Amazon EC2 instance role\. If you have created an AWS credential profile to use, such as *CodeCommitProfile*, you can modify the command as follows to use it instead:  

     ```
     git config --global credential.helper "!aws codecommit credential-helper --profile CodeCommitProfile $@"
     ```
This writes the following to the \.gitconfig file:  

     ```
     [credential]    
         helper = !aws codecommit credential-helper --profile=CodeCommitProfile $@ 
         UseHttpPath = true
     ```
If your profile name contains spaces, you must edit your \.gitconfig file after you run this command to enclose it in single quotation marks \('\)\. Otherwise, the credential helper does not work\. 
If your installation of Git for Windows included the Git Credential Manager utility, you see 403 errors or prompts to provide credentials into the Credential Manager utility after the first few connection attempts\. The most reliable way to solve this problem is to uninstall and then reinstall Git for Windows without the option for the Git Credential Manager utility, because it is not compatible with CodeCommit\. If you want to keep the Git Credential Manager utility, you must perform additional configuration steps to also use CodeCommit, including manually modifying the \.gitconfig file to specify the use of the credential helper for AWS CodeCommit when connecting to CodeCommit\. Remove any stored credentials from the Credential Manager utility \(you can find this utility in Control Panel\)\. After you have removed any stored credentials, add the following to your \.gitconfig file, save it, and then try connecting again from a new command prompt window:  

     ```
     [credential "https://git-codecommit.us-east-2.amazonaws.com"]
         helper = !aws codecommit credential-helper $@ 
         UseHttpPath = true
     
     [credential "https://git-codecommit.us-east-1.amazonaws.com"]
         helper = !aws codecommit credential-helper $@ 
         UseHttpPath = true
     ```
You might also have to reconfigure your git config settings by specifying \-\-system instead of \-\-global or \-\-local before all connections work as expected\.
If you want to use different IAM users on the same local machine for CodeCommit, you should specify git config \-\-local instead of git config \-\-global, and run the configuration for each AWS credential profile\. 

1. Run git config \-\-global \-\-edit to verify the preceding values have been written to the \.gitconfig file for your user profile \(by default, `%HOME%\.gitconfig` or `drive:\Users\UserName\.gitconfig`\)\. If successful, you should see the preceding values \(in addition to values that might already exist in the Git global configuration file\)\. To exit, typically you would type **:q** and then press Enter\.

## Step 4: Connect to the CodeCommit Console and Clone the Repository<a name="setting-up-https-windows-connect-console"></a>

If an administrator has already sent you the name and connection details for the CodeCommit repository, you can skip this step and clone the repository directly\.

**To connect to a CodeCommit repository**

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In the region selector, choose the AWS Region where the repository was created\. Repositories are specific to an AWS Region\. For more information, see [Regions and Git Connection Endpoints](regions.md)\.

1. Choose the repository you want to connect to from the list\. This opens the **Code** page for that repository\.

    If you see a **Welcome** page instead of a list of repositories, there are no repositories associated with your AWS account\. To create a repository, see [Create an AWS CodeCommit Repository](how-to-create-repository.md) or follow the steps in the [Git with CodeCommit Tutorial](getting-started.md) tutorial\.

1. Copy the HTTPS URL to use when connecting to the repository\.

1. Open a command prompt and use the URL to clone the repository with the git clone command\. The local repo is created in a subdirectory of the directory where you run the command\. For example, to clone a repository named *MyDemoRepo* to a local repo named *my\-demo\-repo* in the US East \(Ohio\) Region:

   ```
   git clone https://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo my-demo-repo
   ```

   On some versions of Windows, you might see a pop\-up message asking for your user name and password\. This is the built\-in credential management system for Windows, but it is not compatible with the credential helper for AWS CodeCommit\. Choose **Cancel**\. 

   For more information about how to connect to repositories, see [Connect to the CodeCommit Repository by Cloning the Repository](how-to-connect.md#how-to-connect-http)\.

## Next Steps<a name="setting-up-https-windows-next-step"></a>

You have completed the prerequisites\. Follow the steps in [CodeCommit Tutorial](getting-started-cc.md) to start using CodeCommit\.