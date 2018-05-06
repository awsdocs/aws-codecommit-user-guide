# Setup Steps for SSH Connections to AWS CodeCommit Repositories on Linux, macOS, or Unix<a name="setting-up-ssh-unixes"></a>

Before you can connect to AWS CodeCommit for the first time, you must complete the initial configuration steps\. This topic walks you through the steps for setting up your computer and AWS profile, connecting to an AWS CodeCommit repository, and cloning that repository to your computer \(also known as creating a local repo\)\. If you're new to Git, you might also want to review the information in [Where Can I Learn More About Git?](welcome.md#welcome-get-started-with-git)\.

**Topics**
+ [Step 1: Initial Configuration for AWS CodeCommit](#setting-up-ssh-unixes-account)
+ [Step 2: Install Git](#setting-up-ssh-unixes-install-git)
+ [Step 3: Configure Credentials on Linux, macOS, or Unix](#setting-up-ssh-unixes-keys)
+ [Step 4: Connect to the AWS CodeCommit Console and Clone the Repository](#setting-up-ssh-unixes-connect-console)
+ [Next Steps](#setting-up-ssh-unixes-next-step)

## Step 1: Initial Configuration for AWS CodeCommit<a name="setting-up-ssh-unixes-account"></a>

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

**Note**  
If you want to use AWS CLI commands with AWS CodeCommit, install the AWS CLI\. For more information, see [Command Line Reference](cmd-ref.md)\.

## Step 2: Install Git<a name="setting-up-ssh-unixes-install-git"></a>

To work with files, commits, and other information in AWS CodeCommit repositories, you must install Git on your local machine\. AWS CodeCommit supports Git versions 1\.7\.9 and later\.

To install Git, we recommend websites such as [Git Downloads](http://git-scm.com/downloads)\.

**Note**  
Git is an evolving, regularly updated platform\. Occasionally, a feature change might affect the way it works with AWS CodeCommit\. If you encounter issues with a specific version of Git and AWS CodeCommit, review the information in [Troubleshooting](troubleshooting.md)\.

## Step 3: Configure Credentials on Linux, macOS, or Unix<a name="setting-up-ssh-unixes-keys"></a>

### SSH and Linux, macOS, or Unix: Set Up the Public and Private Keys for Git and AWS CodeCommit<a name="setting-up-ssh-unixes-keys-unixes"></a>

1. From the terminal on your local machine, run the ssh\-keygen command, and follow the directions to save the file to the \.ssh directory for your profile\. 
**Note**  
Be sure to check with your system administrator about where key files should be stored and which file naming pattern should be used\.

   For example:

   ```
   $ ssh-keygen
   
   Generating public/private rsa key pair.
   Enter file in which to save the key (/home/user-name/.ssh/id_rsa): Type /home/your-user-name/.ssh/ and a file name here, for example /home/your-user-name/.ssh/codecommit_rsa
   
   Enter passphrase (empty for no passphrase): <Type a passphrase, and then press Enter>
   Enter same passphrase again: <Type the passphrase again, and then press Enter>
   
   Your identification has been saved in /home/user-name/.ssh/codecommit_rsa.
   Your public key has been saved in /home/user-name/.ssh/codecommit_rsa.pub.
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

1. Run the following command to display the value of the public key file \(*codecommit\_rsa*\.pub\):

   ```
   cat ~/.ssh/codecommit_rsa.pub
   ```

   Copy this value\. It will look similar to the following:

   ```
   ssh-rsa EXAMPLE-AfICCQD6m7oRw0uXOjANBgkqhkiG9w0BAQUFADCBiDELMAkGA1UEBhMCVVMxCzAJB
   gNVBAgTAldBMRAwDgYDVQQHEwdTZWF0dGxlMQ8wDQYDVQQKEwZBbWF6b24xFDASBgNVBAsTC0lBTSBDb2
   5zb2xlMRIwEAYDVQQDEwlUZXN0Q2lsYWMxHzAdBgkqhkiG9w0BCQEWEG5vb25lQGFtYXpvbi5jb20wHhc
   NMTEwNDI1MjA0NTIxWhcNMTIwNDI0MjA0NTIxWjCBiDELMAkGA1UEBhMCVVMxCzAJBgNVBAgTAldBMRAw
   DgYDVQQHEwdTZWF0dGxlMQ8wDQYDVQQKEwZBbWF6b24xFDAS=EXAMPLE user-name@ip-192-0-2-137
   ```

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the IAM console, in the navigation pane, choose **Users**, and from the list of users, choose your IAM user\. 

1. On the user details page, choose the **Security Credentials** tab, and then choose **Upload SSH public key**\.

1. Paste the contents of your SSH public key into the field, and then choose **Upload SSH public key**\. 

1. Copy or save the information in **SSH Key ID** \(for example, *APKAEIBAERJR2EXAMPLE*\)\.   
![\[The SSH Key ID in the IAM console\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-ssh-key-id-iam.png)![\[The SSH Key ID in the IAM console\]](http://docs.aws.amazon.com/codecommit/latest/userguide/)

1. On your local machine, use a text editor to create a config file in the \~/\.ssh directory, and then add the following lines to the file, where the value for *User* is the SSH key ID you copied earlier:

   ```
   Host git-codecommit.*.amazonaws.com
     User APKAEIBAERJR2EXAMPLE
     IdentityFile ~/.ssh/codecommit_rsa
   ```
**Note**  
If you gave your private key file a name other than *codecommit\_rsa*, be sure to use it here\.

   Save and name this file `config`\.

1. From the terminal, run the following command to change the permissions for the config file:

   ```
   chmod 600 config
   ```

1. Run the following command to test your SSH configuration:

   ```
   ssh git-codecommit.us-east-2.amazonaws.com
   ```

   You will be asked to confirm the connection because `git-codecommit.us-east-2.amazonaws.com` is not yet included in your known hosts file\. The AWS CodeCommit server fingerprint is displayed as part of the verification \(`a9:6d:03:ed:08:42:21:be:06:e1:e0:2a:d1:75:31:5e` for MD5 or `3lBlW2g5xn/NA2Ck6dyeJIrQOWvn7n8UEs56fG6ZIzQ` for SHA256\)\.
**Note**  
AWS CodeCommit server fingerprints are unique for every region\. To view the server fingerprints for a specific region, see [Server Fingerprints for AWS CodeCommit](regions.md#regions-fingerprints)\.

   After you have confirmed the connection, you should see confirmation that you have added the server to your known hosts file and a successful connection message\. If you do not see a success message, double\-check that you saved the `config` file in the \~/\.ssh directory of the IAM user you configured for access to AWS CodeCommit, and that you specified the correct private key file\. 

   For information to help you troubleshoot problems, run the `ssh` command with the `-v` parameter:

   ```
   ssh -v git-codecommit.us-east-2.amazonaws.com
   ```

   You can find more information to help you troubleshoot connection problems in [Troubleshooting](troubleshooting.md)\.

## Step 4: Connect to the AWS CodeCommit Console and Clone the Repository<a name="setting-up-ssh-unixes-connect-console"></a>

If an administrator has already sent you the name and connection details for the AWS CodeCommit repository, you can skip this step and clone the repository directly\.

**To connect to an AWS CodeCommit repository**

1. Open the AWS CodeCommit console at [https://console\.aws\.amazon\.com/codecommit](https://console.aws.amazon.com/codecommit)\.

1. In the region selector, choose the region where the repository was created\. Repositories are specific to an AWS region\. For more information, see [Regions and Git Connection Endpoints](regions.md)\.

1. Choose the repository you want to connect to from the list\. This opens the **Code** page for that repository\.

    If you see a **Welcome** page instead of a list of repositories, there are no repositories associated with your AWS account\. To create a repository, see [Create an AWS CodeCommit Repository](how-to-create-repository.md) or follow the steps in the [Git with AWS CodeCommit Tutorial](getting-started.md) tutorial\.

1. Copy the SSH URL to use when connecting to the repository\.

1. Open a terminal\. From the /tmp directory, using the SSH URL you copied, run the git clone command to clone the repository\. For example, to clone a repository named *MyDemoRepo* to a local repo named *my\-demo\-repo* in the US East \(Ohio\) region:

   ```
   git clone ssh://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo my-demo-repo
   ```
**Note**  
 If you successfully tested your connection, but the clone command fails, you might not have the necessary access to your config file, or another setting might be in conflict with your config file\. Try connecting again, this time including the SSH key ID in the command\. For example:  

   ```
   git clone ssh://Your-SSH-Key-ID@git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo my-demo-repo
   ```
For more information, see [Access Error: Public Key Is Uploaded Successfully to IAM but Connection Fails on Linux, macOS, or Unix Systems](troubleshooting-ssh.md#troubleshooting-ae4)\.

   For more information about how to connect to repositories, see [Connect to the AWS CodeCommit Repository by Cloning the Repository](how-to-connect.md#how-to-connect-http)\.

## Next Steps<a name="setting-up-ssh-unixes-next-step"></a>

You have completed the prerequisites\. Follow the steps in [AWS CodeCommit Tutorial](getting-started-cc.md) to start using AWS CodeCommit\.