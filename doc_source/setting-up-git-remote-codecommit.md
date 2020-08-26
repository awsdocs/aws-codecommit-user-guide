# Setup steps for HTTPS connections to AWS CodeCommit with git\-remote\-codecommit<a name="setting-up-git-remote-codecommit"></a>

If you want to connect to CodeCommit using a root account, federated access, or temporary credentials, you should set up access using git\-remote\-codecommit\. This utility provides a simple method for pushing and pulling code from CodeCommit repositories by extending Git\. It is the recommended method for supporting connections made with federated access, identity providers, and temporary credentials\.  Instead of creating an IAM user, you can use existing identities from AWS Directory Service, your enterprise user directory, or a web identity provider\. These are known as *federated users*\. AWS assigns a role to a federated user when access is requested through an [identity provider](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers.html)\. For more information about federated users, see [Federated Users and Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction_access-management.html#intro-access-roles) in the *IAM User Guide*\. 

You can also use git\-remote\-codecommit with an IAM user\. Unlike other HTTPS connection methods, git\-remote\-codecommit does not require setting up Git credentials for the user\.

**Note**  
Some IDEs do not support the clone URL format used by `git-remote-codecommit`\. You might have to manually clone repositories to your local computer before you can work with them in your preferred IDE\. For more information, see [Troubleshooting git\-remote\-codecommit and AWS CodeCommit](troubleshooting-grc.md)\.

These procedures are written with the assumption that you have an AWS account, have created at least one repository in CodeCommit, and use an IAM user with a managed policy when connecting to CodeCommit repositories\. For information about how to configure access for federated users and other rotating credential types, see [Connecting to AWS CodeCommit repositories with rotating credentials](temporary-access.md)\.

**Topics**
+ [Step 0: Install prerequisites for git\-remote\-codecommit](#setting-up-git-remote-codecommit-prereq)
+ [Step 1: Initial configuration for CodeCommit](#setting-up-git-remote-codecommit-account)
+ [Step 2: Install git\-remote\-codecommit](#setting-up-git-remote-codecommit-install)
+ [Step 3: Connect to the CodeCommit console and clone the repository](#setting-up-git-remote-codecommit-connect-console)
+ [Next steps](#setting-up-git-remote-codecommit-next-step)

## Step 0: Install prerequisites for git\-remote\-codecommit<a name="setting-up-git-remote-codecommit-prereq"></a>

Before you can use git\-remote\-codecommit, you must install some prerequisites on your local computer\. These include:
+ Python \(version 3 or later\) and its package manager, pip, if they are not already installed\. To download and install the latest version of Python, visit [the Python website](https://www.python.org/)\.
+ Git

**Note**  
When you install Python on Windows, make sure that you choose the option to add Python to the path\. 

git\-remote\-codecommit requires pip version 9\.0\.3 or later\. To check your version of pip, open a terminal or command line and run the following command:

```
pip --version
```

You can run the following two commands to update your version of pip to the latest version:

```
curl -O https://bootstrap.pypa.io/get-pip.py
python3 get-pip.py --user
```

To work with files, commits, and other information in CodeCommit repositories, you must install Git on your local machine\. CodeCommit supports Git versions 1\.7\.9 and later\. We recommend using a recent version of Git\.

To install Git, we recommend websites such as [Git Downloads](http://git-scm.com/downloads)\.

**Note**  
Git is an evolving, regularly updated platform\. Occasionally, a feature change might affect the way it works with CodeCommit\. If you encounter issues with a specific version of Git and CodeCommit, review the information in [Troubleshooting](troubleshooting.md)\.

## Step 1: Initial configuration for CodeCommit<a name="setting-up-git-remote-codecommit-account"></a>

Follow these steps to create an IAM user, configure it with the appropriate policies, obtain an access key and secret key, and install and configure the AWS CLI\. 

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

## Step 2: Install git\-remote\-codecommit<a name="setting-up-git-remote-codecommit-install"></a>

Follow these steps to install git\-remote\-codecommit\.

**To install git\-remote\-codecommit**

1. At the terminal or command line, run the following command:

   ```
   pip install git-remote-codecommit
   ```
**Note**  
Depending on your operating system and configuration, you might need to run this command with elevated permissions, such as sudo:  

   ```
   sudo pip install git-remote-codecommit
   ```

1. Monitor the installation process until you see a success message similar to the following:

   ```
   Successfully built git-remote-codecommit
   ```

## Step 3: Connect to the CodeCommit console and clone the repository<a name="setting-up-git-remote-codecommit-connect-console"></a>

If an administrator has already sent you the clone URL to use with git\-remote\-codecommit for the CodeCommit repository, you can skip connecting to the console and clone the repository directly\.

**To connect to a CodeCommit repository**

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In the region selector, choose the AWS Region where the repository was created\. Repositories are specific to an AWS Region\. For more information, see [Regions and Git connection endpoints](regions.md)\.

1. Find the repository you want to connect to from the list and choose it\. Choose **Clone URL**, and then choose the protocol you want to use when cloning or connecting to the repository\. This copies the clone URL\.
   + Copy the HTTPS URL if you are using either Git credentials with your IAM user or the credential helper included with the AWS CLI\.
   + Copy the HTTPS \(GRC\) URL if you are using the git\-remote\-codecommit command on your local computer\.
   + Copy the SSH URL if you are using an SSH public/private key pair with your IAM user\.
**Note**  
 If you see a **Welcome** page instead of a list of repositories, there are no repositories associated with your AWS account in the AWS Region where you are signed in\. To create a repository, see [Create an AWS CodeCommit repository](how-to-create-repository.md) or follow the steps in the [Getting started with Git and CodeCommit](getting-started.md) tutorial\.

1. At the terminal or command prompt, clone the repository with the git clone command\. Use the HTTPS git\-remote\-codecommit URL you copied and the name of the AWS CLI profile, if you created a named profile\. If you do not specify a profile, the command assumes the default profile\. The local repo is created in a subdirectory of the directory where you run the command\. For example, to clone a repository named *MyDemoRepo* to a local repo named *my\-demo\-repo*:

   ```
   git clone codecommit://MyDemoRepo my-demo-repo
   ```

   To clone the same repository using a profile named *CodeCommitProfile*: 

   ```
   git clone codecommit://CodeCommitProfile@MyDemoRepo my-demo-repo
   ```

   To clone a repository in a different AWS Region than the one configured in your profile, include the AWS Region name\. For example:

   ```
   git clone codecommit::ap-northeast-1://MyDemoRepo my-demo-repo
   ```

## Next steps<a name="setting-up-git-remote-codecommit-next-step"></a>

You have completed the prerequisites\. Follow the steps in [Getting started with CodeCommit ](getting-started-cc.md) to start using CodeCommit\.

To learn how to create and push your first commit, see [Create a commit in AWS CodeCommit](how-to-create-commit.md)\. If you're new to Git, you might also want to review the information in [Where can I learn more about Git?](welcome.md#welcome-get-started-with-git) and [Getting started with Git and AWS CodeCommit](getting-started.md)\.