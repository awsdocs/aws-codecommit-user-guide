# Setting up for AWS CodeCommit<a name="setting-up"></a>

You can sign in to the AWS Management Console and [upload, add, or edit a file](files.md) to a repository directly from the AWS CodeCommit console\. This is a quick way to make a change\. However, if you want to work with multiple files, files across branches, and so on, consider setting up your local computer to work with repositories\. The easiest way to set up CodeCommit is to configure HTTPS Git credentials for AWS CodeCommit\. This HTTPS authentication method: 
+ Uses a static user name and password\.
+ Works with all operating systems supported by CodeCommit\.
+ Is also compatible with integrated development environments \(IDEs\) and other development tools that support Git credentials\.

You can use other methods if you do not want to or cannot use Git credentials for operational reasons\. For example, if you access CodeCommit repositories using federated access, temporary credentials, or a web identity provider, you cannot use Git credentials\. We recommend that you set up your local computer using the `git-remote-codecommit` command\. Review these options carefully, to decide which alternative method works best for you\.
+ [Setting up using Git credentials](#setting-up-standard)
+ [Setting up using other methods](#setting-up-other)
+ [Compatibility for CodeCommit, Git, and other components](#setting-up-compat)

For information about using CodeCommit and Amazon Virtual Private Cloud, see [Using AWS CodeCommit with interface VPC endpoints](codecommit-and-interface-VPC.md) \.

## View and manage your credentials<a name="setting-up-view-credentials"></a>

You can view and manage your CodeCommit credentials from the AWS console through **My Security Credentials**\.

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation bar on the upper right, choose your user name, and then choose **My Security Credentials**\.   
![\[AWS Management Console My Security Credentials link\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/security-credentials-user.shared.console.png)

1. Choose the **AWS CodeCommit credentials** tab\.

## Setting up using Git credentials<a name="setting-up-standard"></a>

With HTTPS connections and Git credentials, you generate a static user name and password in IAM\. You then use these credentials with Git and any third\-party tool that supports Git user name and password authentication\. This method is supported by most IDEs and development tools\. It is the simplest and easiest connection method to use with CodeCommit\. 
+ [For HTTPS users using Git credentials](setting-up-gc.md): Follow these instructions to set up connections between your local computer and CodeCommit repositories using Git credentials\.
+ [For connections from development tools](setting-up-ide.md): Follow these guidelines to set up connections between your IDE or other development tools and CodeCommit repositories using Git credentials\. IDEs that support Git credentials include \(but are not limited to\) Visual Studio, Eclipse, Xcode, and IntelliJ\.

## Setting up using other methods<a name="setting-up-other"></a>

You can use the SSH protocol instead of HTTPS to connect to your CodeCommit repository\. With SSH connections, you create public and private key files on your local machine that Git and CodeCommit use for SSH authentication\. You associate the public key with your IAM user\. You store the private key on your local machine\. Because SSH requires manual creation and management of public and private key files, you might find Git credentials simpler and easier to use with CodeCommit\.

Unlike Git credentials, SSH connection setup varies, depending on the operating system on your local computer\. 
+ [For SSH users not using the AWS CLI](setting-up-without-cli.md): Follow these abbreviated instructions if you already have a public\-private key pair and are familiar with SSH connections on your local computer\.
+ [For SSH connections on Linux, macOS, or Unix](setting-up-ssh-unixes.md): Follow these instructions for a step\-by\-step walkthrough of creating a public\-private key pair and setting up connections on Linux, macOS, or Unix operating systems\.
+ [For SSH connections on Windows](setting-up-ssh-windows.md): Follow these instructions for a step\-by\-step walkthrough of creating public\-private key pair and setting up connections on Windows operating systems\.

If you are connecting to CodeCommit and AWS using federated access, an identity provider, or temporary credentials, or if you do not want to configure IAM users or Git credentials for IAM users, you can set up connections to CodeCommit repositories in one of two ways: 
+ Install and use git\-remote\-codecommit \(recommended\)\.
+ Install and use the credential helper included in the AWS CLI\.

 Both methods support accessing CodeCommit repositories without requiring an IAM user, which means that you can connect to repositories using federated access and temporary credentials\. The git\-remote\-codecommit utility is the recommended approach\. It extends Git and is compatible with a variety of Git versions and credential helpers\. However, not all IDEs support the clone URL format used by `git-remote-codecommit`\. You might have to manually clone repositories to your local computer before you can work with them in your IDE\.
+ Follow the instructions in [Setup Steps for HTTPS Connections to AWS CodeCommit Repositories with git\-remote\-codecommit](setting-up-git-remote-codecommit.md) to install and set up git\-remote\-codecommit on Windows, Linux, macOS, or Unix\.

The credential helper included in the AWS CLI allows Git to use HTTPS and a cryptographically signed version of your IAM user credentials or Amazon EC2 instance role whenever Git needs to authenticate with AWS to interact with CodeCommit repositories\. Some operating systems and Git versions have their own credential helpers, which conflict with the credential helper included in the AWS CLI\. They can cause connectivity issues for CodeCommit\. 
+ [For HTTPS connections on Linux, macOS, or Unix with the AWS CLI credential helper](setting-up-https-unixes.md): Follow these instructions for a step\-by\-step walkthrough of installing and setting up the credential helper on Linux, macOS, or Unix systems\.
+ [For HTTPS connections on Windows with the AWS CLI credential helper](setting-up-https-windows.md): Follow these instructions for a step\-by\-step walkthrough of installing and setting up the credential helper on Windows systems\.

If you are connecting to a CodeCommit repository that is hosted in another AWS account, you can configure access and set up connections using roles, policies, and the credential helper included in the AWS CLI\.
+ [Configure cross\-account access to an AWS CodeCommit repository using roles](cross-account.md): Follow these instructions for a step\-by\-step walkthrough of configuring cross\-account access in one AWS account to users in an IAM group in another AWS account\.

## Compatibility for CodeCommit, Git, and other components<a name="setting-up-compat"></a>

When you work with CodeCommit, you use Git\. You might use other programs, too\. The following table provides the latest guidance for version compatibility\. As a best practice, we recommend that you use the latest versions of Git and other software\.


**Version compatibility information for AWS CodeCommit**  

| Component | Version | 
| --- | --- | 
| Git | CodeCommit supports Git versions 1\.7\.9 and later\. We recommend using a recent version of Git\.  | 
| Curl | CodeCommit requires curl 7\.33 and later\. However, there is a known issue with HTTPS and curl update 7\.41\.0\. For more information, see [Troubleshooting](troubleshooting.md)\. | 
| Python \(git\-remote\-codecommit only\) | git\-remote\-codecommit requires version 3 and later\. | 
| Pip \(git\-remote\-codecommit only\) | git\-remote\-codecommit requires version 9\.0\.3 and later\. | 