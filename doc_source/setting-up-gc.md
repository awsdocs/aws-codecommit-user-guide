--------

 The procedures in this guide support the new console design\. If you choose to use the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\.

--------

# Setup for HTTPS Users Using Git Credentials<a name="setting-up-gc"></a>

The simplest way to set up connections to AWS CodeCommit repositories is to configure Git credentials for AWS CodeCommit in the IAM console, and then use those credentials for HTTPS connections\. You can also use these same credentials with any third\-party tool or individual development environment \(IDE\) that supports HTTPS authentication using a static user name and password\. For examples, see [For Connections from Development Tools](setting-up-ide.md)\.

**Note**  
If you have previously configured your local computer to use the credential helper for AWS CodeCommit, you must edit your \.gitconfig file to remove the credential helper information from the file before you can use Git credentials\. If your local computer is running macOS, you might need to clear cached credentials from Keychain Access\.

## Step 1: Initial Configuration for AWS CodeCommit<a name="setting-up-gc-account"></a>

Follow these steps to set up an AWS account, create an IAM user, and configure access to AWS CodeCommit\. 

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

If you want to use AWS CLI commands with AWS CodeCommit, install the AWS CLI\. For more information, see [Command Line Reference](cmd-ref.md)\.

## Step 2: Install Git<a name="setting-up-gc-install-git"></a>

To work with files, commits, and other information in AWS CodeCommit repositories, you must install Git on your local machine\. AWS CodeCommit supports Git versions 1\.7\.9 and later\.

To install Git, we recommend websites such as [Git Downloads](http://git-scm.com/downloads)\.

**Note**  
Git is an evolving, regularly updated platform\. Occasionally, a feature change might affect the way it works with AWS CodeCommit\. If you encounter issues with a specific version of Git and AWS CodeCommit, review the information in [Troubleshooting](troubleshooting.md)\.

## Step 3: Create Git Credentials for HTTPS Connections to AWS CodeCommit<a name="setting-up-gc-iam"></a>

After you have installed Git, create Git credentials for your IAM user in IAM\. For more information, see [Use Git Credentials and HTTPS with AWS CodeCommit](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_ssh-keys.html#git-credentials-code-commit) in the *IAM User Guide*\.

**To set up HTTPS Git Credentials for AWS CodeCommit**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

   Make sure to sign in as the IAM user who will create and use the Git credentials for connections to AWS CodeCommit\.

1. In the IAM console, in the navigation pane, choose **Users**, and from the list of users, choose your IAM user\. 

1. On the user details page, choose the **Security Credentials** tab, and in **HTTPS Git credentials for AWS CodeCommit**, choose **Generate**\.  
![\[Generating Git credentials in the IAM console\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-iam-gc1.png)
**Note**  
You cannot choose your own user name or password for Git credentials\. For more information, see [Use Git Credentials and HTTPS with AWS CodeCommit](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_ssh-keys.html#git-credentials-code-commit)\.

1. Copy the user name and password that IAM generated for you, either by showing, copying, and pasting this information into a secure file on your local computer, or by choosing **Download credentials** to download this information as a \.CSV file\. You will need this information to connect to AWS CodeCommit\.  
![\[Downloading Git credentials from the IAM console\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-iam-gc2.png)

   After you have saved your credentials, choose **Close**\.
**Important**  
This is your only chance to save the user name and password\. If you do not save them, you can copy the user name from the IAM console, but you cannot look up the password\. You must reset the password and then save it\.

## Step 4: Connect to the AWS CodeCommit Console and Clone the Repository<a name="setting-up-gc-connect-console"></a>

If an administrator has already sent you the name and connection details for the AWS CodeCommit repository, you can skip this step and clone the repository directly\.

**To connect to an AWS CodeCommit repository**

1. Open the AWS CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In the region selector, choose the region where the repository was created\. Repositories are specific to an AWS region\. For more information, see [Regions and Git Connection Endpoints](regions.md)\.

1. Choose the repository you want to connect to from the list\. This opens the **Code** page for that repository\.

    If you see a **Welcome** page instead of a list of repositories, there are no repositories associated with your AWS account\. To create a repository, see [Create an AWS CodeCommit Repository](how-to-create-repository.md) or follow the steps in the [Git with AWS CodeCommit Tutorial](getting-started.md) tutorial\.

1. Choose **Connect**\. Review the instructions and copy the URL to use when connecting to the repository\.

1. Open a terminal, command line, or Git shell\. Using the HTTPS URL you copied, run the git clone command to clone the repository\. For example, to clone a repository named *MyDemoRepo* to a local repo named *my\-demo\-repo* in the US East \(Ohio\) region:

   ```
   git clone https://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo my-demo-repo
   ```

   The first time you connect, you will be prompted to provide the user name and password for the repository\. Depending on the configuration of your local computer, this prompt will either originate from a credential management system for the operating system \(for example, Keychain Access for macOS\), a credential manager utility for your version of Git \(for example, the Git Credential Manager included in Git for Windows\), your IDE, or Git itself\. Provide the user name and password generated for Git credentials in IAM \(the ones you created in [Step 3: Create Git Credentials for HTTPS Connections to AWS CodeCommit](#setting-up-gc-iam)\)\. Depending on your operating system and other software, this information might be saved for you in a credential store or credential management utility\. If so, you should not be prompted again unless you change the password, inactivate the Git credentials, or delete the Git credentials in IAM\.

   If you do not have a credential store or credential management utility configured on your local computer, you can install one\. For more information about Git and how it manages credentials, see [Credential Storage](https://git-scm.com/book/en/v2/Git-Tools-Credential-Storage) in the Git documentation\.

   For more information, see [Connect to the AWS CodeCommit Repository by Cloning the Repository](how-to-connect.md#how-to-connect-http) and [Create a Commit](how-to-create-commit.md)\.

## Next Steps<a name="setting-up-gc-next-step"></a>

You have completed the prerequisites\. Follow the steps in [AWS CodeCommit Tutorial](getting-started-cc.md) to start using AWS CodeCommit\.