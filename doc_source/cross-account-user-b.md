# Cross\-Account Repository Access: Actions for the Repository User in AccountB<a name="cross-account-user-b"></a>

To access the repository in AccountA, users in the AccountB group must configure their local computers for repository access\. The following sections provide steps and examples\.

**Topics**
+ [Step 1: Configure the AWS CLI and Git for an AccountB User to Access the Repository in AccountA](#cross-account-configure-credentials)
+ [Step 2: Clone and Access the CodeCommit Repository in AccountA](#cross-account-clone-and-use)

## Step 1: Configure the AWS CLI and Git for an AccountB User to Access the Repository in AccountA<a name="cross-account-configure-credentials"></a>

You must use the credential helper, not SSH keys or Git credentials, to access repositories in another AWS account\. AccountB users must configure their computers to use the credential helper to access the shared CodeCommit repository in AccountA\. You cannot use SSH keys or Git credentials to access repositories in another AWS account\. However, you can continue to use SSH keys or Git credentials when accessing repositories in AccountB\.<a name="cross-account-configure-cli-git"></a>

**To configure the AWS CLI and Git for cross\-account access**

1. Install the AWS CLI on the local computer\. See instructions for your operating system in [Installing the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/installing.html)\.

1. Install Git on the local computer\. To install Git, we recommend websites such as [Git Downloads](http://git-scm.com/downloads) or [Git for Windows](http://msysgit.github.io/)\. 

   When you install Git, do not install the Git Credential Manager\. The Git Credential Manager is not compatible with the credential helper included with the AWS CLI\.
**Note**  
CodeCommit supports Git versions 1\.7\.9 and later\. Git is an evolving, regularly updated platform\. Occasionally, a feature change might affect the way it works with CodeCommit\. If you encounter issues with a specific version of Git and CodeCommit, review the information in [Troubleshooting](troubleshooting.md)\.

1. From the terminal or command line, at the directory location where you want to clone the repository, run the git config \-\-local user\.name and git config \-\-local user\.email commands to set the user name and email for the commits you will make to the repository\. For example:

   ```
   git config --local user.name "Saanvi Sarkar"
   git config --local user.email saanvi_sarkar@example.com
   ```

   These commands return nothing, but the email and user name you specify is associated with the commits you make to the repository in AccountA\.

1. Run the aws configure \-\-profile command to configure a default profile to use when connecting to resources in AccountB\. When prompted, provide the access key and secret key for your IAM user\.
**Note**  
If you have already installed the AWS CLI and configured a profile, you can skip this step\. 

    For example, run the following command to create a default AWS CLI profile that you use to access AWS resources in AccountA in US East \(Ohio\) \(us\-east\-2\):

   ```
   aws configure
   ```

   When prompted, provide the following information:

   ```
   AWS Access Key ID [None]: Your-IAM-User-Access-Key
   AWS Secret Access Key ID [None]: Your-IAM-User-Secret-Access-Key
   Default region name ID [None]: us-east-2
   Default output format [None]: json
   ```

1. Run the aws configure \-\-profile command again to configure a named profile to use when connecting to the repository in AccountA\. When prompted, provide the access key and secret key for your IAM user\. For example, run the following command to create an AWS CLI profile named *MyCrossAccountAccessProfile* that you use to access a repository in AccountA in US East \(Ohio\) \(us\-east\-2\):

   ```
   aws configure --profile MyCrossAccountAccessProfile
   ```

   When prompted, provide the following information:

   ```
   AWS Access Key ID [None]: Your-IAM-User-Access-Key
   AWS Secret Access Key ID [None]: Your-IAM-User-Secret-Access-Key
   Default region name ID [None]: us-east-2
   Default output format [None]: json
   ```

1. In a plain\-text editor, open the `config` file, also known as the AWS CLI configuration file\. Depending on your operating system, this file might be located at `~/.aws/config` on Linux, macOS, or Unix, or at *drive*:\\Users\\*USERNAME*\\\.aws\\config on Windows\. 

1. In the file, find the entry that corresponds to the *MyCrossAccountAccessProfile* profile you just created\. It should look similar to the following:

   ```
   [profile MyCrossAccountAccessProfile]
   region = US East (Ohio)
   output = json
   ```

   Add two lines to the profile configuration, `role_arn` and `source_profile`\. Provide the ARN of the role in AccountA that you assume to access the repository in the other account and the name of your default AWS CLI profile in AccountB\. For example:

   ```
   [profile MyCrossAccountAccessProfile]
   region = US East (Ohio)
   role_arn = arn:aws:iam::111122223333:role/MyCrossAccountRepositoryContributorRole
   source_profile = default
   output = json
   ```

   Save your changes, and close the plain\-text editor\.

1. Run the git config command twice: once to configure Git to use the AWS CLI credential helper with the AWS CLI profile you just created, and again to use HTTP\. For example:

   ```
   git config --global credential.helper '!aws --profile MyCrossAccountAccessProfile codecommit credential-helper $@'
   ```

   ```
   git config --global credential.UseHttpPath true
   ```
**Note**  
The syntax for the first git config command will vary slightly depending on your operating system\. Linux, macOS, or Unix users should use single quotes \(`'`\) where Windows users should use double quotes \(`"`\)\.

1. Run the git config \-l command to verify your configuration\. A successful configuration contains lines similar to the following:

   ```
   user.name=Saanvi Sarkar
   user.email=saanvi_sarkar@example.com
   credential.helper=!aws --profile MyCrossAccountAccessProfile codecommit credential-helper $@
   credential.usehttppath=true
   ```
**Note**  
If you see the following line in your configuration file, another credential manager is configured for your system, which can prevent the AWS CLI credential helper from working:  

   ```
   credential.helper=manager
   ```
To fix this problem, run the following command:  

   ```
   git config --system --unset credential.helper
   ```

## Step 2: Clone and Access the CodeCommit Repository in AccountA<a name="cross-account-clone-and-use"></a>

Run git clone, git push, and git pull to clone, push to, and pull from, the cross\-account CodeCommit repository\. You can also sign in to the AWS Management Console, switch roles, and use the CodeCommit console to interact with the repository in the other account\.

**Note**  
Depending on how the IAM role was configured, you might be able to view repositories on the default page for CodeCommit\. If you cannot view the repositories, ask the repository administrator to email you a URL link to the **Code** page for that repository in the CodeCommit console\. The URL is similar to the following:  

```
https://console.aws.amazon.com/codecommit/home?region=us-east-2#/repository/MySharedDemoRepo/browse/HEAD/--/
```<a name="cross-account-clone-cross-account-repo"></a>

**To clone the cross\-account repository to your local computer**

1. At the command line or terminal, in the directory where you want to clone the repository, run the git clone command with the HTTPS clone URL\. For example:

   ```
   git clone https://git-codecommit.us-east-2.amazonaws.com/v1/repos/MySharedDemoRepo
   ```

   Unless you specify otherwise, the repository is cloned into a subdirectory with the same name as the repository\.

1. Change directories to the cloned repository, and either add or make a change to a file\. For example, you can add a file named *NewFile\.txt*\.

1. Add the file to the tracked changes for the local repo, commit the change, and push the file to the CodeCommit repository\. For example:

   ```
   git add NewFile.txt
   git commit -m "Added a file to test cross-account access to this repository"
   git push
   ```

   For more information, see [Getting Started with Git and AWS CodeCommit](getting-started.md)\.

Now that you've added a file, go to the CodeCommit console to view your commit, review other users' changes to the repo, participate in pull requests, and more\.<a name="cross-account-console"></a>

**To access the cross\-account repository in the CodeCommit console**

1. Sign in to the AWS Management Console in AccountB \(*888888888888*\) as the IAM user who has been granted cross\-account access to the repository in AccountA\.

1. Choose your user name on the navigation bar, and in the drop\-down list, choose **Switch Role**\. 
**Note**  
If this is the first time you have selected this option, review the information on the page, and then choose **Switch Role** again\.

1. On the **Switch Role** page, do the following:
   + In **Account**, enter the account ID for AccountA \( for example, *111122223333*\)\. 
   + In **Role**, enter the name of the role you want to assume for access to the repository in AccountA \(for example, *MyCrossAccountRepositoryContributorRole*\)\.
   + In **Display Name**, enter a friendly name for this role\. This name appears in the console when you are assuming this role\. It also appears in the list of assumed roles the next time you want to switch roles in the console\.
   + \(Optional\) In **Color**, choose a color label for the display name\.
   + Choose **Switch Role**\.

   For more information, see [Switching to a Role \(AWS Management Console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-console.html)\.

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

   If the assumed role has permission to view the names of repositories in AccountA, you see a list of repositories and an error message that informs you that you do not have permissions to view their status\. This is expected behavior\. Choose the name of the shared repository from the list\.

   If the assumed role does not have permission to view the names of repositories in AccountA, you see an error message and a blank list with no repositories\. Paste the URL link to the repository or modify the console link and change `/list` to the name of the shared repository \(for example, `/MySharedDemoRepo`\)\.

1. In **Code**, find the name of the file you added from your local computer\. Choose it to browse the code in the file, and then browse the rest of the repository and start using its features\. 

   For more information, see [Getting Started with AWS CodeCommit ](getting-started-cc.md)\.