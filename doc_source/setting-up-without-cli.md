# Setup for SSH users not using the AWS CLI<a name="setting-up-without-cli"></a>

If you want to use SSH connections for your repository, you can connect to AWS CodeCommit without installing the AWS CLI\. The AWS CLI includes commands that are useful when you use and manage CodeCommit repositories, but it is not required for initial setup\.

This topic assumes:
+ You have set up an IAM user with the policies or permissions required for CodeCommit and the **IAMUserSSHKeys** managed policy or equivalent permissions required for uploading keys\. For more information, see [Using identity\-based policies \(IAM Policies\) for CodeCommit](auth-and-access-control-iam-identity-based-access-control.md)\.
+ You already have, or know how to create, a public\-private key pair\. We strongly recommend that you use a secure passphrase for your SSH key\. 
+ You are familiar with SSH, your Git client, and its configuration files\. 
+ If you are using Windows, you have installed a command\-line utility, such as Git Bash, that emulates the bash shell\. 

If you need more guidance, follow the instructions in [For SSH connections on Linux, macOS, or Unix](setting-up-ssh-unixes.md) or [For SSH connections on Windows](setting-up-ssh-windows.md)\.

**Topics**
+ [Step 1: Associate your public key with your IAM user](#setting-up-without-cli-add-key)
+ [Step 2: Add CodeCommit to your SSH configuration](#setting-up-without-cli-configure-client)
+ [Next steps](#setting-up-without-cli-next-step)

## Step 1: Associate your public key with your IAM user<a name="setting-up-without-cli-add-key"></a>

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the IAM console, in the navigation pane, choose **Users**, and from the list of users, choose your IAM user\. 

1. On the **Security Credentials** tab, choose **Upload SSH public key**\.

1. Paste the contents of your SSH public key into the field, and then choose **Upload SSH Key**\. 
**Tip**  
The public\-private key pair must be SSH\-2 RSA, in OpenSSH format, and contain 2048 bits\. The key looks similar to this:  

   ```
   ssh-rsa EXAMPLE-AfICCQD6m7oRw0uXOjANBgkqhkiG9w0BAQUFADCBiDELMAkGA1UEBhMCVVMxCzAJB
   gNVBAgTAldBMRAwDgYDVQQHEwdTZWF0dGxlMQ8wDQYDVQQKEwZBbWF6b24xFDASBgNVBAsTC0lBTSBDb2
   5zb2xlMRIwEAYDVQQDEwlUZXN0Q2lsYWMxHzAdBgkqhkiG9w0BCQEWEG5vb25lQGFtYXpvbi5jb20wHhc
   NMTEwNDI1MjA0NTIxWhcNMTIwNDI0MjA0NTIxWjCBiDELMAkGA1UEBhMCVVMxCzAJBgNVBAgTAldBMRAw
   DgYDVQQHEwdTZWF0dGxlMQ8wDQYDVQQKEwZBbWF6b24xFDAS=EXAMPLE user-name@ip-192-0-2-137
   ```
IAM accepts public keys in the OpenSSH format only\. If you provide your public key in another format, you see an error message that says the key format is not valid\. 

1. Copy the SSH key ID \(for example, *APKAEIBAERJR2EXAMPLE*\) and close the console\.  
![\[The SSH key ID in the IAM console\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-ssh-key-id-iam.png)![\[The SSH key ID in the IAM console\]](http://docs.aws.amazon.com/codecommit/latest/userguide/)

## Step 2: Add CodeCommit to your SSH configuration<a name="setting-up-without-cli-configure-client"></a>

1. At the terminal \(Linux, macOS, or Unix\) or bash emulator \(Windows\), edit your SSH configuration file by typing cat>> \~/\.ssh/config:

   ```
   Host git-codecommit.*.amazonaws.com
   User Your-SSH-Key-ID, such as APKAEIBAERJR2EXAMPLE
   IdentityFile Your-Private-Key-File, such as ~/.ssh/codecommit_rsa or ~/.ssh/id_rsa
   ```
**Tip**  
If you have more than one SSH configuration, make sure you include the blank lines before and after the content\. Save the file by pressing the `Ctrl` and `d` keys simultaneously\.

1. Run the following command to test your SSH configuration:

   ```
   ssh git-codecommit.us-east-2.amazonaws.com
   ```

   Enter the passphrase for your SSH key file when prompted\. If everything is configured correctly, you should see the following success message:

   ```
   You have successfully authenticated over SSH. You can use Git to interact with CodeCommit. 
   Interactive shells are not supported. Connection to git-codecommit.us-east-2.amazonaws.com closed by remote host.
   ```

## Next steps<a name="setting-up-without-cli-next-step"></a>

You have completed the prerequisites\. Follow the steps in [Getting started with CodeCommit ](getting-started-cc.md) to start using CodeCommit\.

To connect to a repository, follow the steps in [Connect to a repository](how-to-connect.md)\. To create a repository, follow the steps in [Create a repository](how-to-create-repository.md)\.