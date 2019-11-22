# Setup Steps for SSH Connections to AWS CodeCommit Repositories on Windows<a name="setting-up-ssh-windows"></a>

Before you can connect to AWS CodeCommit for the first time, you must complete the initial configuration steps\. This topic walks you through the steps for setting up your computer and AWS profile, connecting to a CodeCommit repository, and cloning that repository to your computer \(also known as creating a local repo\)\. If you're new to Git, you might also want to review the information in [Where Can I Learn More About Git?](welcome.md#welcome-get-started-with-git)\.

**Topics**
+ [Step 1: Initial Configuration for CodeCommit](#setting-up-ssh-windows-account)
+ [Step 2: Install Git](#setting-up-ssh-windows-install-git)
+ [SSH and Windows: Set Up the Public and Private Keys for Git and CodeCommit](#setting-up-ssh-windows-keys-windows)
+ [Step 4: Connect to the CodeCommit Console and Clone the Repository](#setting-up-ssh-windows-connect-console)
+ [Next Steps](#setting-up-ssh-windows-next-step)

## Step 1: Initial Configuration for CodeCommit<a name="setting-up-ssh-windows-account"></a>

Follow these steps to set up an AWS account, create an IAM user, and configure access to CodeCommit\. 

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

**Note**  
If you want to use AWS CLI commands with CodeCommit, install the AWS CLI\. For more information, see [Command Line Reference](cmd-ref.md)\.

## Step 2: Install Git<a name="setting-up-ssh-windows-install-git"></a>

To work with files, commits, and other information in CodeCommit repositories, you must install Git on your local machine\. CodeCommit supports Git versions 1\.7\.9 and later\.

To install Git, we recommend websites such as [Git Downloads](http://git-scm.com/downloads)\.

**Note**  
Git is an evolving, regularly updated platform\. Occasionally, a feature change might affect the way it works with CodeCommit\. If you encounter issues with a specific version of Git and CodeCommit, review the information in [Troubleshooting](troubleshooting.md)\.

If the version of Git you installed does not include a Bash emulator, such as Git Bash, install one\. You use this emulator instead of the Windows command line when you configure SSH connections\.

## SSH and Windows: Set Up the Public and Private Keys for Git and CodeCommit<a name="setting-up-ssh-windows-keys-windows"></a>

1. Open the Bash emulator\.
**Note**  
You might need to run the emulator with administrative permissions\.

   From the emulator, run the ssh\-keygen command, and follow the directions to save the file to the \.ssh directory for your profile\. 

   For example:

   ```
   $ ssh-keygen
   
   Generating public/private rsa key pair.
   Enter file in which to save the key (/drive/Users/user-name/.ssh/id_rsa): Type a file name here, for example /c/Users/user-name/.ssh/codecommit_rsa
   
   Enter passphrase (empty for no passphrase): <Type a passphrase, and then press Enter>
   Enter same passphrase again: <Type the passphrase again, and then press Enter>
   
   Your identification has been saved in drive/Users/user-name/.ssh/codecommit_rsa.
   Your public key has been saved in drive/Users/user-name/.ssh/codecommit_rsa.pub.
   The key fingerprint is:
   45:63:d5:99:0e:99:73:50:5e:d4:b3:2d:86:4a:2c:14 user-name@client-name
   The key's randomart image is:
   +--[ RSA 2048]----+
   |        E.+.o*.++|
   |        .o .=.=o.|
   |       . ..  *. +|
   |        ..o . +..|
   |        So . . . |
   |          .      |
   |                 |
   |                 |
   |                 |
   +-----------------+
   ```

   This generates: 
   + The *codecommit\_rsa* file, which is the private key file\.
   + The *codecommit\_rsa*\.pub file, which is the public key file\.

1. Run the following commands to display the value of the public key file \(*codecommit\_rsa*\.pub\):

   ```
   cd .ssh
   notepad codecommit_rsa.pub
   ```

   Copy the contents of the file, and then close Notepad without saving\. The contents of the file look similar to the following:

   ```
   ssh-rsa EXAMPLE-AfICCQD6m7oRw0uXOjANBgkqhkiG9w0BAQUFADCBiDELMAkGA1UEBhMCVVMxCzAJB
   gNVBAgTAldBMRAwDgYDVQQHEwdTZWF0dGxlMQ8wDQYDVQQKEwZBbWF6b24xFDASBgNVBAsTC0lBTSBDb2
   5zb2xlMRIwEAYDVQQDEwlUZXN0Q2lsYWMxHzAdBgkqhkiG9w0BCQEWEG5vb25lQGFtYXpvbi5jb20wHhc
   NMTEwNDI1MjA0NTIxWhcNMTIwNDI0MjA0NTIxWjCBiDELMAkGA1UEBhMCVVMxCzAJBgNVBAgTAldBMRAw
   DgYDVQQHEwdTZWF0dGxlMQ8wDQYDVQQKEwZBbWF6b24xFDAS=EXAMPLE user-name@computer-name
   ```

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.
**Note**  
You can directly view and manage your CodeCommit credentials in **My Security Credentials**\. For more information, see [View and Manage Your Credentials](setting-up.md#setting-up-view-credentials)\.

1. In the IAM console, in the navigation pane, choose **Users**, and from the list of users, choose your IAM user\. 

1. On the user details page, choose the **Security Credentials** tab, and then choose **Upload SSH public key**\.

1. Paste the contents of your SSH public key into the field, and then choose **Upload SSH public key**\. 

1. Copy or save the information in **SSH Key ID** \(for example, *APKAEIBAERJR2EXAMPLE*\)\.   
![\[The SSH Key ID in the IAM console\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-ssh-key-id-iam.png)![\[The SSH Key ID in the IAM console\]](http://docs.aws.amazon.com/codecommit/latest/userguide/)
**Note**  
If you have more than one SSH key IDs uploaded, the keys are listed alphabetically by key ID, not by upload date\. Make sure that you have copied the key ID that is associated with the correct upload date\.

1. In the Bash emulator, run the following commands to create a config file in the \~/\.ssh directory, or edit it if one already exists:

   ```
   notepad ~/.ssh/config
   ```

1. Add the following lines to the file, where the value for *User* is the SSH key ID you copied earlier, and the value for *IdentityFile* is the path to and name of the private key file:

   ```
   Host git-codecommit.*.amazonaws.com
     User APKAEIBAERJR2EXAMPLE
     IdentityFile ~/.ssh/codecommit_rsa
   ```
**Note**  
If you gave your private key file a name other than *codecommit\_rsa*, be sure to use it here\.

   Save the file as config \(not config\.txt\), and then close Notepad\.
**Important**  
The name of the file must be `config` with no file extension\. Otherwise, the SSH connections fail\.

1. Run the following command to test your SSH configuration:

   ```
   ssh git-codecommit.us-east-2.amazonaws.com
   ```

   You are asked to confirm the connection because `git-codecommit.us-east-2.amazonaws.com` is not yet included in your known hosts file\. The CodeCommit server fingerprint is displayed as part of the verification \(`a9:6d:03:ed:08:42:21:be:06:e1:e0:2a:d1:75:31:5e` for MD5 or `3lBlW2g5xn/NA2Ck6dyeJIrQOWvn7n8UEs56fG6ZIzQ` for SHA256\)\.
**Note**  
CodeCommit server fingerprints are unique for every AWS Region\. To view the server fingerprints for an AWS Region, see [Server Fingerprints for CodeCommit](regions.md#regions-fingerprints)\.

   After you have confirmed the connection, you should see confirmation that you have added the server to your known hosts file and a successful connection message\. If you do not see a success message, double\-check that you saved the `config` file in the \~/\.ssh directory of the IAM user you configured for access to CodeCommit, that the `config` file has no file extension \(for example, it must not be named config\.txt\), and that you specified the correct private key file \(*codecommit\_rsa*, not *codecommit\_rsa*\.pub\)\. 

   For information to help you troubleshoot problems, run the `ssh` command with the `-v` parameter:

   ```
   ssh -v git-codecommit.us-east-2.amazonaws.com
   ```

   For information to help you troubleshoot connection problems, see [Troubleshooting](troubleshooting.md)\.

## Step 4: Connect to the CodeCommit Console and Clone the Repository<a name="setting-up-ssh-windows-connect-console"></a>

If an administrator has already sent you the name and connection details for the CodeCommit repository, you can skip this step and clone the repository directly\.

**To connect to a CodeCommit repository**

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In the region selector, choose the AWS Region where the repository was created\. Repositories are specific to an AWS Region\. For more information, see [Regions and Git Connection Endpoints](regions.md)\.

1. Choose the repository you want to connect to from the list\. This opens the **Code** page for that repository\.

    If you see a **Welcome** page instead of a list of repositories, there are no repositories associated with your AWS account\. To create a repository, see [Create an AWS CodeCommit Repository](how-to-create-repository.md) or follow the steps in the [Getting Started with Git and CodeCommit](getting-started.md) tutorial\.

1. Choose **Clone URL**, and then copy the SSH URL\.

1. In the Bash emulator, using the SSH URL you just copied, run the git clone command to clone the repository\. This command creates the local repo in a subdirectory of the directory where you run the command\. For example, to clone a repository named *MyDemoRepo* to a local repo named *my\-demo\-repo* in the US East \(Ohio\) Region:

   ```
   git clone ssh://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo my-demo-repo
   ```

   Alternatively, open a command prompt, and using the URL and the SSH key ID for the public key you uploaded to IAM, run the git clone command\. The local repo is created in a subdirectory of the directory where you run the command\. For example, to clone a repository named *MyDemoRepo* to a local repo named *my\-demo\-repo*:

   ```
   git clone ssh://Your-SSH-Key-ID@git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo my-demo-repo
   ```

   For more information, see [Connect to the CodeCommit Repository by Cloning the Repository](how-to-connect.md#how-to-connect-http) and [Create a Commit](how-to-create-commit.md)\.

## Next Steps<a name="setting-up-ssh-windows-next-step"></a>

You have completed the prerequisites\. Follow the steps in [Getting Started with CodeCommit ](getting-started-cc.md) to start using CodeCommit\.