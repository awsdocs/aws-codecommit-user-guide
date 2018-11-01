--------

 The procedures in this guide support the new console design\. If you choose to use the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\.

--------

# Setting Up for AWS CodeCommit<a name="setting-up"></a>

You can sign in to the AWS Management Console and [upload, add, or edit a file](files.md) to a repository directly from the AWS CodeCommit console\. This is a quick way to make a change\. However, if you want to work with multiple files, files across branches, and so on, consider setting up your local computer to work with repositories\. The easiest way to set up AWS CodeCommit is to configure HTTPS Git credentials for AWS CodeCommit\. This HTTPS authentication method: 
+ Uses a static user name and password\.
+ Works with all operating systems supported by AWS CodeCommit\.
+ Is also compatible with integrated development environments \(IDEs\) and other development tools that support Git credentials\.

You can use other methods if you do not want to or cannot use Git credentials for operational reasons\. Read through these other options carefully, to decide which alternate method will work best for you\.
+ [Setting Up Using Git Credentials](#setting-up-standard)
+ [Setting Up Using Other Methods](#setting-up-other)
+ [Compatibility for AWS CodeCommit, Git, and Other Components](#setting-up-compat)

## Setting Up Using Git Credentials<a name="setting-up-standard"></a>

With HTTPS connections and Git credentials, you generate a static user name and password in IAM\. You then use these credentials with Git and any third\-party tool that supports Git user name and password authentication\. This method is supported by most IDEs and development tools\. It is the simplest and easiest connection method to use with AWS CodeCommit\. 
+ [For HTTPS Users Using Git Credentials](setting-up-gc.md): Follow these instructions to set up connections between your local computer and AWS CodeCommit repositories using Git credentials\.
+ [For Connections from Development Tools](setting-up-ide.md): Follow these guidelines to set up connections between your IDE or other development tools and AWS CodeCommit repositories using Git credentials\. IDEs that support Git credentials include \(but are not limited to\) Visual Studio, Eclipse, Xcode, and IntelliJ\.

## Setting Up Using Other Methods<a name="setting-up-other"></a>

You can use the SSH protocol instead of HTTPS to connect to your AWS CodeCommit repository\. With SSH connections, you create public and private key files on your local machine that Git and AWS CodeCommit use for SSH authentication\. You associate the public key with your IAM user\. You store the private key on your local machine\. Because SSH requires manual creation and management of public and private key files, you might find Git credentials simpler and easier to use with AWS CodeCommit\.

Unlike Git credentials, SSH connection setup will vary, depending on the operating system on your local computer\. 
+ [For SSH Users Not Using the AWS CLI](setting-up-without-cli.md): Follow these abbreviated instructions if you already have a public\-private key pair and are familiar with SSH connections on your local computer\.
+ [For SSH Connections on Linux, macOS, or Unix](setting-up-ssh-unixes.md): Follow these instructions for a step\-by\-step walkthrough of creating a public\-private key pair and setting up connections on Linux, macOS, or Unix operating systems\.
+ [For SSH Connections on Windows](setting-up-ssh-windows.md): Follow these instructions for a step\-by\-step walkthrough of creating public\-private key pair and setting up connections on Windows operating systems\.

If you are connecting to AWS CodeCommit and AWS using federated access or temporary credentials, or if you do not want to configure IAM users, you can set up connections to AWS CodeCommit repositories using the credential helper included in the AWS CLI\. The credential helper allows Git to use HTTPS and a cryptographically signed version of your IAM user credentials or Amazon EC2 instance role whenever Git needs to authenticate with AWS to interact with AWS CodeCommit repositories\. **This is the only connection method for AWS CodeCommit repositories that does not require an IAM user, so it is the only method that supports federated access and temporary credentials\.** Some operating systems and Git versions have their own credential helpers, which conflict with the credential helper included in the AWS CLI\. They can cause connectivity issues for AWS CodeCommit\. For ease of use, consider creating IAM users and configuring Git credentials with HTTPS connections instead of using the credential helper\.
+ [For HTTPS Connections on Linux, macOS, or Unix with the AWS CLI Credential Helper](setting-up-https-unixes.md): Follow these instructions for a step\-by\-step walkthrough of installing and setting up the credential helper on Linux, macOS, or Unix systems\.
+ [For HTTPS Connections on Windows with the AWS CLI Credential Helper](setting-up-https-windows.md): Follow these instructions for a step\-by\-step walkthrough of installing and setting up the credential helper on Windows systems\.

If you are connecting to an AWS CodeCommit repository that is hosted in another AWS account, you can configure access and set up connections using roles, policies, and the credential helper included in the AWS CLI\.
+ [Configure Cross\-Account Access to an AWS CodeCommit Repository](cross-account.md): Follow these instructions for a step\-by\-step walkthrough of configuring cross\-account access in one AWS account to users in an IAM group in another AWS account\.

## Compatibility for AWS CodeCommit, Git, and Other Components<a name="setting-up-compat"></a>

When working with AWS CodeCommit, you will use Git\. You might use other programs, too\. The following table provides the latest guidance for version compatibility\.


**Version Compatibility Information for AWS CodeCommit**  

| Component | Version | 
| --- | --- | 
| Git | AWS CodeCommit supports Git versions 1\.7\.9 and later\.  | 
| Curl | AWS CodeCommit requires curl 7\.33 and later\. However, there is a known issue with HTTPS and curl update 7\.41\.0\. For more information, see [Troubleshooting](troubleshooting.md)\. | 