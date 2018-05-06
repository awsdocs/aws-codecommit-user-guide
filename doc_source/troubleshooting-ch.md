# Troubleshooting the Credential Helper and HTTPS Connections to AWS CodeCommit<a name="troubleshooting-ch"></a>

The following information might help you troubleshoot common issues when using the credential helper included with the AWS CLI and HTTPS to connect to AWS CodeCommit repositories\.

**Topics**
+ [Git for macOS: I Configured the Credential Helper Successfully, but Now I Am Denied Access to My Repository \(403\)](#troubleshooting-macoshttps)
+ [Git for Windows: I Installed Git for Windows, but I Am Denied Access to My Repository \(403\)](#troubleshooting-windowshttps)

## Git for macOS: I Configured the Credential Helper Successfully, but Now I Am Denied Access to My Repository \(403\)<a name="troubleshooting-macoshttps"></a>

**Problem:** On macOS, the credential helper does not seem to access or use your credentials as expected\. This can be caused by two different problems:
+ The AWS CLI is configured for a different AWS region than the one where the repository exists\.
+ The Keychain Access utility has saved credentials which have since expired\.

**Possible fixes:** To verify whether the AWS CLI is configured for the correct region, run the aws configure command, and review the displayed information\. If the AWS CodeCommit repository is in a different region than the one shown for the AWS CLI, you must run the aws configure command and change the values to the appropriate ones for that region\. For more information, see [Step 1: Initial Configuration for AWS CodeCommit](setting-up-https-unixes.md#setting-up-https-unixes-account)\.

 The default version of Git released on OS X and macOS uses the Keychain Access utility to save generated credentials\. For security reasons, the password generated for access to your AWS CodeCommit repository is temporary, so the credentials stored in the keychain will stop working after about 15 minutes\. If you are only accessing Git with AWS CodeCommit, try the following:

1. In Terminal, run the git config command to find the Git configuration file where the Keychain Access utility is defined:

   ```
   $ git config -l --show-origin
   ```

   In the output from this command, find a line that contains the following option:

   ```
   helper = osxkeychain
   ```

   The file listed at the beginning of this line is the Git configuration file you must edit\.

1. To edit the Git configuration file, use a plain\-text editor or run the following command:

   ```
   $ nano /usr/local/git/etc/gitconfig
   ```

1. Comment out the following line of text:

   ```
   # helper = osxkeychain
   ```

   Alternatively, if you want to continue to use the Keychain Access utility to cache credentials for other Git repositories, modify the header instead of commenting out the line\. For example, to allow cached credentials for GitHub, you could modify the header as follows:

   ```
   [credential "https://github.com"]
      helper = osxkeychain
   ```

If you are accessing other repositories with Git, you can configure the Keychain Access utility so that it does not supply credentials for your AWS CodeCommit repositories\. To configure the Keychain Access utility:

1. Open the Keychain Access utility\. \(You can use Finder to locate it\.\)

1. Search for `git-codecommit.us-east-2.amazonaws.com`\. Highlight the row, open the context \(right\-click\) menu, and then choose **Get Info**\.

1. Choose the **Access Control** tab\.

1. In **Confirm before allowing access**, choose `git-credential-osxkeychain`, and then choose the minus sign to remove it from the list\.
**Note**  
After removing `git-credential-osxkeychain` from the list, you will see a pop\-up dialog box whenever you run a Git command\. Choose **Deny** to continue\. If you find the pop\-ups too disruptive, here are some alternatives:  
Connect to AWS CodeCommit using SSH instead of HTTPS\. For more information, see [For SSH Connections on Linux, macOS, or Unix](setting-up-ssh-unixes.md)\. 
In the Keychain Access utility, on the **Access Control** tab for `git-codecommit.us-east-2.amazonaws.com`, choose the **Allow all applications to access this item \(access to this item is not restricted\)** option\. This will prevent the pop\-ups, but the credentials will eventually expire \(on average, this takes about 15 minutes\) and you will see a 403 error message\. When this happens, you must delete the keychain item in order to restore functionality\.
Install a version of Git that does not use the keychain by default\.
Consider a scripting solution for deleting the keychain item\. To view a community\-generated sample of a scripted solution, see [Mac OS X Script to Periodically Delete Cached Credentials in the OS X Certificate Store](integrations.md#integrations-community-code) in [Product and Service Integrations](integrations.md)\.

## Git for Windows: I Installed Git for Windows, but I Am Denied Access to My Repository \(403\)<a name="troubleshooting-windowshttps"></a>

**Problem:** On Windows, the credential helper does not seem to access or use your credentials as expected\. This can be caused by different problems:
+ The AWS CLI is configured for a different AWS region than the one where the repository exists\.
+ By default, Git for Windows installs a Git Credential Manager utility that is not compatible with AWS CodeCommit connections that use the AWS credential helper\. When installed, it will cause connections to repository to fail even thought the credential helper has been installed with the AWS CLI and configured for connections to AWS CodeCommit\.
+ Some versions of Git for Windows might not be in full compliance with [RFC 2617](https://tools.ietf.org/html/rfc2617#page-5) and [RFC 4559](https://tools.ietf.org/html/rfc4559#page-2), which could potentially cause issues with both Git credentials and the credential helper included with the AWS CLI\. For more information, see [Version 2\.11\.0\(3\) does not ask for username/password](https://github.com/git-for-windows/git/issues/1034)\.

**Possible fixes:** 
+ If you are attempting to use the credential helper included with the AWS CLI, consider connecting with Git credentials over HTTPS instead of using the credential helper\. Git credentials configured for your IAM user are compatible with the Git Credential Manager for Windows, unlike the credential helper for AWS CodeCommit\. For more information, see [For HTTPS Users Using Git Credentials](setting-up-gc.md)\. 

  If you want to use the credential helper, to verify whether the AWS CLI is configured for the correct region, run the aws configure command, and review the displayed information\. If the AWS CodeCommit repository is in a different region than the one shown for the AWS CLI, you must run the aws configure command and change the values to the appropriate ones for that region\. For more information, see [Step 1: Initial Configuration for AWS CodeCommit](setting-up-https-windows.md#setting-up-https-windows-account)\.
+ If possible, uninstall and reinstall Git for Windows\. When installing Git for Windows, clear the check box for the option for installing the Git Credential Manager utility\. This credential manager is not compatible with the credential helper for AWS CodeCommit\. If you installed the Git Credential Manager or another credential management utility and you do not want to uninstall it, you can modify your \.gitconfig file and add specific credential management for AWS CodeCommit:

  1. Open **Control Panel**, choose **Credential Manager**, and remove any stored credentials for AWS CodeCommit\.

  1. Open your \.gitconfig file in any plain\-text editor, such as Notepad\.
**Note**  
If you work with multiple Git profiles, you might have both local and global \.gitconfig files\. Be sure to edit the appropriate file\.

  1. Add the following section to your \.gitconfig file:

     ```
     [credential "https://git-codecommit.*.amazonaws.com"]
         helper = !aws codecommit credential-helper $@ 
         UseHttpPath = true
     ```

  1. Save the file, and then open a new command line session before you attempt to connect again\.

  You can also use this approach if you want to use the credential helper for AWS CodeCommit when connecting to AWS CodeCommit repositories and another credential management system when connecting to other hosted repositories, such as GitHub repositories\. 

  To reset which credential helper is used as the default, you can use the \-\-system option instead of \-\-global or \-\-local when running the git config command\.
+ If you are using Git credentials on a Windows computer, you can try to work around any RFC noncompliance issues by including your Git credential user name as part of the connection string\. For example, to work around the issue and clone a repository named *MyDemoRepo* in the US East \(Ohio\) region:

  ```
  git clone https://Your-Git-Credential-Username@git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo my-demo-repo
  ```
**Note**  
This approach will not work if you have an `@` character in your Git credentials username\. You must URL encode \(also known as URL escaping or [percent\-encoding](https://en.wikipedia.org/wiki/Percent-encoding)\) the character before it will work\.