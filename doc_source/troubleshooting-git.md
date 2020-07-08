# Troubleshooting Git clients and AWS CodeCommit<a name="troubleshooting-git"></a>

The following information might help you troubleshoot common issues when using Git with AWS CodeCommit repositories\. For troubleshooting problems related to Git clients when using HTTPS or SSH, also see [Troubleshooting Git credentials \(HTTPS\)](troubleshooting-gc.md), [Troubleshooting SSH connections](troubleshooting-ssh.md), and [Troubleshooting the credential helper \(HTTPS\)](troubleshooting-ch.md)\.

**Topics**
+ [Git error: Error: RPC failed; result=56, HTTP code = 200 fatal: The remote end hung up unexpectedly](#troubleshooting-ge1)
+ [Git error: Too many reference update commands](#troubleshooting-ge2)
+ [Git error: Push via HTTPS is broken in some versions of Git](#troubleshooting-ge3)
+ [Git error: 'gnutls\_handshake\(\) failed'](#troubleshooting-ge4)
+ [Git error: Git cannot find the CodeCommit repository or does not have permission to access the repository](#troubleshooting-ge5)
+ [Git on Windows: No supported authentication methods available \(publickey\)](#troubleshooting-gw1)

## Git error: Error: RPC failed; result=56, HTTP code = 200 fatal: The remote end hung up unexpectedly<a name="troubleshooting-ge1"></a>

**Problem:** When pushing a large change, a large number of changes, or a large repository, long\-running HTTPS connections are often terminated prematurely due to networking issues or firewall settings\. 

**Possible fixes:** Push with SSH instead, or when you are migrating a large repository, follow the steps in [Migrate a repository in increments](how-to-push-large-repositories.md)\. Also, make sure you are not exceeding the size limits for individual files\. For more information, see [Quotas](limits.md)\.

## Git error: Too many reference update commands<a name="troubleshooting-ge2"></a>

**Problem:** The maximum number of reference updates per push is 4,000\. This error appears when the push contains more than 4,000 reference updates\. 

**Possible fixes:** Try pushing branches and tags individually with `git push --all` and `git push --tags`\. If you have too many tags, split the tags into multiple pushes\. For more information, see [Quotas](limits.md)\.

## Git error: Push via HTTPS is broken in some versions of Git<a name="troubleshooting-ge3"></a>

**Problem:** An issue with the curl update to 7\.41\.0 causes SSPI\-based digest authentication to fail\. Known affected versions of Git include 1\.9\.5\.msysgit\.1\. Some versions of Git for Windows might not be in full compliance with [RFC 2617](https://tools.ietf.org/html/rfc2617#page-5) and [RFC 4559](https://tools.ietf.org/html/rfc4559#page-2), which could potentially cause issues with HTTPS connections using either Git credentials or the credential helper included with the AWS CLI\. 

**Possible fixes:** Check your version of Git for known issues or use an earlier or later version\. For more information about mysysgit, see [ Push to HTTPS Is Broken](https://github.com/msysgit/git/issues/332) in the GitHub forums\. For more information about Git for Windows version issues, see [Version 2\.11\.0\(3\) does not ask for username/password](https://github.com/git-for-windows/git/issues/1034)\.

## Git error: 'gnutls\_handshake\(\) failed'<a name="troubleshooting-ge4"></a>

**Problem:** In Linux, when you try to use Git to communicate with a CodeCommit repository, an error message appears containing the phrase `error: gnutls_handshake() failed`\.

**Possible fixes:** Compile Git against OpenSSL\. For one approach, see ["Error: gnutls\_handshake\(\) failed" When Connecting to HTTPS Servers](http://askubuntu.com/questions/186847/error-gnutls-handshake-falied-when-connecting-to-https-servers) in the Ask Ubuntu forums\.

Alternatively, use SSH instead of HTTPS to communicate with CodeCommit repositories\. 

## Git error: Git cannot find the CodeCommit repository or does not have permission to access the repository<a name="troubleshooting-ge5"></a>

**Problem:** A trailing slash in the connection string can cause connection attempts to fail\. 

**Possible fixes:** Make sure that you have provided the correct name and connection string for the repository, and that there are no trailing slashes\. For more information, see [Connect to a repository](how-to-connect.md)\.

## Git on Windows: No supported authentication methods available \(publickey\)<a name="troubleshooting-gw1"></a>

**Problem:** After you configure SSH access for Windows, you see an access denied error when you attempt to use commands such as git pull, git push, or git clone\.

**Possible fixes:** The most common cause for this error is that a GIT\_SSH environment variable exists on your computer and is configured to support another connection utility, such as PuTTY\. To fix this problem, try one of the following:
+ Open a Bash emulator and add the `GIT_SSH_COMMAND="ssh"` parameter before the Git command\. For example, if you are attempting to clone a repository, instead of running git clone ssh://git\-codecommit\.us\-east\-2\.amazonaws\.com/v1/repos/MyDemoRepo my\-demo\-repo, run: 

  ```
  GIT_SSH_COMMAND="ssh" git clone ssh://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo my-demo-repo
  ```
+ Rename or delete the `GIT_SSH` environment variable if you are no longer using it\. Then open a new command prompt or Bash emulator session, and try your command again\.

For more information about troubleshooting Git issues on Windows when using SSH, see [Troubleshooting SSH connections](troubleshooting-ssh.md)\.