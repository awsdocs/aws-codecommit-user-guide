# Troubleshooting Git credentials and HTTPS connections to AWS CodeCommit<a name="troubleshooting-gc"></a>

The following information might help you troubleshoot common issues when using Git credentials and HTTPS to connect to AWS CodeCommit repositories\.

**Topics**
+ [Git credentials for AWS CodeCommit: I keep seeing a prompt for credentials when I connect to my CodeCommit repository at the terminal or command line](#troubleshooting-gc1)
+ [Git credentials for AWS CodeCommit: I set up Git credentials, but my system is not using them](#troubleshooting-gc2)

## Git credentials for AWS CodeCommit: I keep seeing a prompt for credentials when I connect to my CodeCommit repository at the terminal or command line<a name="troubleshooting-gc1"></a>

**Problem:** When you try to push, pull, or otherwise interact with a CodeCommit repository from the terminal or command line, you are prompted to provide a user name and password, and you must supply the Git credentials for your IAM user\.

**Possible fixes:** The most common causes for this error are that your local computer is running an operating system that does not support credential management, or it does not have a credential management utility installed, or the Git credentials for your IAM user have not been saved to one of these credential management systems\. Depending on your operating system and local environment, you might need to install a credential manager, configure the credential manager that is included in your operating system, or customize your local environment to use credential storage\. For example, if your computer is running macOS, you can use the Keychain Access utility to store your credentials\. If your computer is running Windows, you can use the Git Credential Manager that is installed with Git for Windows\. For more information, see [For HTTPS users using Git credentials](setting-up-gc.md) and [Credential Storage](https://git-scm.com/book/en/v2/Git-Tools-Credential-Storage) in the Git documentation\. 

## Git credentials for AWS CodeCommit: I set up Git credentials, but my system is not using them<a name="troubleshooting-gc2"></a>

**Problem:** When you try to use CodeCommit with a Git client, the client does not appear to use the Git credentials for your IAM user\.

**Possible fixes:** The most common cause for this error is that you previously set up your computer to use the credential helper that is included with the AWS CLI\. Check your \.gitconfig file for configuration sections similar to the following, and remove them: 

```
[credential "https://git-codecommit.*.amazonaws.com"]
    helper = !aws codecommit credential-helper $@ 
    UseHttpPath = true
```

Save the file, and then open a new command line or terminal session before you attempt to connect again\.

You may also have multiple credential helpers or managers set up on your computer, and your system might be defaulting to another configuration\. To reset which credential helper is used as the default, you can use the \-\-system option instead of \-\-global or \-\-local when running the git config command\.

For more information, see [For HTTPS users using Git credentials](setting-up-gc.md) and [Credential Storage](https://git-scm.com/book/en/v2/Git-Tools-Credential-Storage) in the Git documentation\. 