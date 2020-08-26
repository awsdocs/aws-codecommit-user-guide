# Troubleshooting the credential helper and HTTPS connections to AWS CodeCommit<a name="troubleshooting-ch"></a>

The following information might help you troubleshoot common issues when you use the credential helper included with the AWS CLI and HTTPS to connect to CodeCommit repositories\.

**Note**  
Although the credential helper is a supported method for connecting to CodeCommit using federated access, an identity provider, or temporary credentials, the recommended method is to install and use the git\-remote\-codecommit utility\. For more information, see [Setup steps for HTTPS connections to AWS CodeCommit with git\-remote\-codecommit](setting-up-git-remote-codecommit.md)\.

**Topics**
+ [I get a command not found error in Windows when using the credential helper](#troubleshooting-py3)
+ [I am prompted for a user name when I connect to a CodeCommit repository](#troubleshooting-ae1)
+ [Git for macOS: I configured the credential helper successfully, but now I am denied access to my repository \(403\)](#troubleshooting-macoshttps)
+ [Git for Windows: I installed Git for Windows, but I am denied access to my repository \(403\)](#troubleshooting-windowshttps)

## I get a command not found error in Windows when using the credential helper<a name="troubleshooting-py3"></a>

**Problem:** After updating the AWS CLI, credential helper connections to CodeCommit repositories fail with `aws codecommit credential-helper $@ get: aws: command not found`\.

**Cause**: The most common reason for this error is that your AWS CLI version has been updated to a version that uses Python 3\. There is a known issue with the MSI package\. To verify whether you have one of the affected versions, open a command line and run the following command: `aws --version`

If the output Python version begins with a 3, you have an affected version\. For example: 

```
aws-cli/1.16.62 Python/3.6.2 Darwin/16.7.0 botocore/1.12.52
```

**Possible fixes:** You can work around this issue by doing one of the following:
+ Install and configure the AWS CLI on Windows using Python and pip instead of the MSI\. For more information, see [Install Python, pip, and the AWS CLI on Windows](https://docs.aws.amazon.com/cli/latest/userguide/install-windows.html#awscli-install-windows-pip)\.
+ Manually edit your `.gitconfig` file to change the `[credential]` section to explicitly point to `aws.cmd` on your local computer\. For example:

  ```
  [credential]    
      helper = !"\C:\\Program Files\\Amazon\\AWSCLI\\bin\\aws.cmd\" codecommit credential-helper $@ 
      UseHttpPath = true
  ```
+ Run the git config command to update your `.gitconfig` file to explicitly reference `aws.cmd`, and manually update your PATH environment variable to include the path to the command as needed\. For example: 

  ```
  git config --global credential.helper "!aws.cmd codecommit credential-helper $@"
  git config --global credential.UseHttpPath true
  ```

## I am prompted for a user name when I connect to a CodeCommit repository<a name="troubleshooting-ae1"></a>

**Problem:** When you try to use the credential helper to communicate with a CodeCommit repository, a message appears prompting you for your user name\.

**Possible fixes:** Configure your AWS profile or make sure the profile you are using is the one you configured for working with CodeCommit\. For more information about setting up, see [Setup steps for HTTPS connections to AWS CodeCommit repositories on Linux, macOS, or Unix with the AWS CLI credential helper](setting-up-https-unixes.md) or [Setup steps for HTTPS connections to AWS CodeCommit repositories on Windows with the AWS CLI credential helper](setting-up-https-windows.md)\. For more information about IAM, access keys, and secret keys, see [Managing Access Keys for IAM Users](https://docs.aws.amazon.com/IAM/latest/UserGuide/ManagingCredentials.html) and [How Do I Get Credentials?](https://docs.aws.amazon.com/IAM/latest/UserGuide/IAM_Introduction.html#IAM_SecurityCredentials)

## Git for macOS: I configured the credential helper successfully, but now I am denied access to my repository \(403\)<a name="troubleshooting-macoshttps"></a>

**Problem:** On macOS, the credential helper does not seem to access or use your credentials as expected\. This can be caused by two different problems:
+ The AWS CLI is configured for an AWS Region different from the one where the repository exists\.
+ The Keychain Access utility has saved credentials that have since expired\.

**Possible fixes:** To verify whether the AWS CLI is configured for the correct region, run the aws configure command, and review the displayed information\. If the CodeCommit repository is in an AWS Region different from the one shown for the AWS CLI, you must run the aws configure command and change the values to ones appropriate for that Region\. For more information, see [Step 1: Initial configuration for CodeCommit](setting-up-https-unixes.md#setting-up-https-unixes-account)\.

 The default version of Git released on OS X and macOS uses the Keychain Access utility to save generated credentials\. For security reasons, the password generated for access to your CodeCommit repository is temporary, so the credentials stored in the keychain stop working after about 15 minutes\. If you are only accessing Git with CodeCommit, try the following:

1. In Terminal, run the git config command to find the Git configuration file \(`gitconfig`\) where the Keychain Access utility is defined\. Depending on your local system and preferences, you might have more than one `gitconfig` file\. 

   ```
   $ git config -l --show-origin | grep credential
   ```

   In the output from this command, search for results similar to:

   ```
   file:/path/to/gitconfig  credential.helper=osxkeychain
   ```

   The file listed at the beginning of this line is the Git configuration file you must edit\.

1. To edit the Git configuration file, use a plain\-text editor or run the following command:

   ```
   $ nano /usr/local/git/etc/gitconfig
   ```

1. Modify the configuration using one of the following strategies:
   + Comment out or delete the credential section that contains `helper = osxkeychain`\. For example:

     ```
     # helper = osxkeychain
     ```
   + Update both the `aws credential helper` and `osxkeychain` credential helper sections to have context\. For example, if `osxkeychain` is used to authenticate to GitHub:

     ```
     [credential "https://git-codecommit.us-east-1.amazonaws\.com"]
       helper = !aws --profile CodeCommitProfile codecommit credential-helper $@
       UseHttpPath = true
     [credential "https://github.com"]
       helper = osxkeychain
     ```

     In this configuration, Git will use the `osxkeychain` helper when the remote host matches "`https://github.com`" and the credential helper when the remote host matches "`https://git-codecommit\.us-east-1\.amazonaws.com`"\.
   + Include an empty string helper before the credential helper\. For example:

     ```
     [credential]
       helper =
       helper = !aws --profile CodeCommitProfile codecommit credential-helper $@
       UseHttpPath = true
     ```

   Alternatively, if you want to continue to use the Keychain Access utility to cache credentials for other Git repositories, modify the header instead of commenting out the line\. For example, to allow cached credentials for GitHub, you could modify the header as follows:

   ```
   [credential "https://github.com"]
      helper = osxkeychain
   ```

If you are accessing other repositories with Git, you can configure the Keychain Access utility so that it does not supply credentials for your CodeCommit repositories\. To configure the Keychain Access utility:

1. Open the Keychain Access utility\. \(You can use Finder to locate it\.\)

1. Search for `git-codecommit.us-east-2.amazonaws.com` and replace *us\-east\-2* with the AWS Region where the repository exists\. Highlight the row, open the context \(right\-click\) menu, and then choose **Get Info**\.

1. Choose the **Access Control** tab\.

1. In **Confirm before allowing access**, choose `git-credential-osxkeychain`, and then choose the minus sign to remove it from the list\.
**Note**  
After removing `git-credential-osxkeychain` from the list, you see a dialog box whenever you run a Git command\. Choose **Deny** to continue\. If you find the pop\-ups too disruptive, here are some alternatives:  
Connect to CodeCommit using SSH instead of HTTPS\. For more information, see [For SSH connections on Linux, macOS, or Unix](setting-up-ssh-unixes.md)\. 
In the Keychain Access utility, on the **Access Control** tab for `git-codecommit.us-east-2.amazonaws.com`, choose the **Allow all applications to access this item \(access to this item is not restricted\)** option\. This prevents the pop\-ups, but the credentials eventually expire \(on average, this takes about 15 minutes\) and you then see a 403 error message\. When this happens, you must delete the keychain item to restore functionality\.
Install a version of Git that does not use the keychain by default\.
Consider a scripting solution for deleting the keychain item\. To view a community\-generated sample of a scripted solution, see [Mac OS X Script to Periodically Delete Cached Credentials in the OS X Certificate Store](integrations.md#integrations-community-code) in [Product and service integrations](integrations.md)\.

If you want to stop Git from using the Keychain Access utility entirely, you can configure Git to stop using osxkeychain as the credential helper\. For example, if you open a terminal and run the command `git config --system credential.helper`, and it returns `osxkeychain`, Git is set to use the Keychain Access utility\. You can change this by running the following command:

```
git config --system --unset credential.helper
```

Be aware that by running this command with the `--system` option changes the Git behavior system\-wide for all users, and this might have unintended consequences for other users, or for other repositories if you're using other repository services in addition to CodeCommit\. Also be aware that this approach might require the use of `sudo`, and that your account might not have sufficient system permissions to apply this change\. Make sure to verify that the command applied successfully by running the `git config --system credential.helper` command again\. For more information, see [Customizing Git \- Git Configuration](https://git-scm.com/book/en/v2/Customizing-Git-Git-Configuration) and [this article on Stack Overflow](https://stackoverflow.com/questions/16052602/disable-git-credential-osxkeychain)\.

## Git for Windows: I installed Git for Windows, but I am denied access to my repository \(403\)<a name="troubleshooting-windowshttps"></a>

**Problem:** On Windows, the credential helper does not seem to access or use your credentials as expected\. This can be caused by different problems:
+ The AWS CLI is configured for an AWS Region different from the one where the repository exists\.
+ By default, Git for Windows installs a Git Credential Manager utility that is not compatible with CodeCommit connections that use the AWS credential helper\. When installed, it causes connections to the repository to fail even though the credential helper has been installed with the AWS CLI and configured for connections to CodeCommit\.
+ Some versions of Git for Windows might not be in full compliance with [RFC 2617](https://tools.ietf.org/html/rfc2617#page-5) and [RFC 4559](https://tools.ietf.org/html/rfc4559#page-2), which could potentially cause issues with both Git credentials and the credential helper included with the AWS CLI\. For more information, see [Version 2\.11\.0\(3\) does not ask for username/password](https://github.com/git-for-windows/git/issues/1034)\.

**Possible fixes:** 
+ If you are attempting to use the credential helper included with the AWS CLI, consider connecting with Git credentials over HTTPS instead of using the credential helper\. Git credentials configured for your IAM user are compatible with the Git Credential Manager for Windows, unlike the credential helper for AWS CodeCommit\. For more information, see [For HTTPS users using Git credentials](setting-up-gc.md)\. 

  If you want to use the credential helper, to verify whether the AWS CLI is configured for the correct AWS Region, run the aws configure command, and review the displayed information\. If the CodeCommit repository is in an AWS Region different from the one shown for the AWS CLI, you must run the aws configure command and change the values to ones appropriate for that Region\. For more information, see [Step 1: Initial configuration for CodeCommit](setting-up-https-windows.md#setting-up-https-windows-account)\.
+ If possible, uninstall and reinstall Git for Windows\. When you install Git for Windows, clear the check box for the option to install the Git Credential Manager utility\. This credential manager is not compatible with the credential helper for AWS CodeCommit\. If you installed the Git Credential Manager or another credential management utility and you do not want to uninstall it, you can modify your `.gitconfig` file and add credential management for CodeCommit:

  1. Open **Control Panel**, choose **Credential Manager**, and remove any stored credentials for CodeCommit\.

  1. Open your `.gitconfig` file in any plain\-text editor, such as Notepad\.
**Note**  
If you work with multiple Git profiles, you might have both local and global `.gitconfig` files\. Be sure to edit the appropriate file\.

  1. Add the following section to your `.gitconfig` file:

     ```
     [credential "https://git-codecommit.*.amazonaws.com"]
         helper = !aws codecommit credential-helper $@ 
         UseHttpPath = true
     ```

  1. Save the file, and then open a new command line session before you attempt to connect again\.

  You can also use this approach if you want to use the credential helper for AWS CodeCommit when you connect to CodeCommit repositories and another credential management system when you connect to other hosted repositories, such as GitHub repositories\. 

  To reset which credential helper is used as the default, you can use the \-\-system option instead of \-\-global or \-\-local when you run the git config command\.
+ If you are using Git credentials on a Windows computer, you can try to work around any RFC noncompliance issues by including your Git credential user name as part of the connection string\. For example, to work around the issue and clone a repository named *MyDemoRepo* in the US East \(Ohio\) Region:

  ```
  git clone https://Your-Git-Credential-Username@git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo my-demo-repo
  ```
**Note**  
This approach does not work if you have an `@` character in your Git credentials user name\. You must URL\-encode \(also known as URL escaping or [percent\-encoding](https://en.wikipedia.org/wiki/Percent-encoding)\) the character\.