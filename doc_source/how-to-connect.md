# Connect to an AWS CodeCommit repository<a name="how-to-connect"></a>

When you connect to a CodeCommit repository for the first time, you typically clone its contents to your local machine\. You can also [add files](how-to-create-file.md#how-to-create-file-console) to and [edit files](how-to-edit-file.md#how-to-edit-file-console) in a repository directly from the CodeCommit console\. Alternatively, if you already have a local repo, you can add a CodeCommit repository as a remote\. This topic provides instructions for connecting to a CodeCommit repository\. If you want to migrate an existing repository to CodeCommit, see [Migrate to CodeCommit](how-to-migrate-repository.md)\.

**Note**  
Depending on your usage, you might be charged for creating or accessing a repository\. For more information, see [Pricing](http://aws.amazon.com/codecommit/pricing) on the CodeCommit product information page\.

**Topics**
+ [Prerequisites for connecting to a CodeCommit repository](#how-to-connect-prerequisites)
+ [Connect to the CodeCommit repository by cloning the repository](#how-to-connect-http)
+ [Connect a local repo to the CodeCommit repository](#how-to-connect-local)

## Prerequisites for connecting to a CodeCommit repository<a name="how-to-connect-prerequisites"></a>

Before you can clone a CodeCommit repository or connect a local repo to an CodeCommit repository:
+ You must have configured your local computer with the software and settings required to connect to CodeCommit\. This includes installing and configuring Git\. For more information, see [Setting up ](setting-up.md) and [Getting started with Git and AWS CodeCommit](getting-started.md)\.
+ You must have the clone URL of the CodeCommit repository to which you want to connect\. For more information, see [View repository details](how-to-view-repository-details.md)\.

  If you have not yet created a CodeCommit repository, follow the instructions in [Create a repository](how-to-create-repository.md), copy the clone URL of the CodeCommit repository, and return to this page\.

  If you have a CodeCommit repository but you do not know its name, follow the instructions in [View repository details](how-to-view-repository-details.md)\.
+ You must have a location on your local machine to store a local copy of the CodeCommit repository you connect to\. \(This local copy of the CodeCommit repository is known as a *local repo*\.\) You then switch to and run Git commands from that location\. For example, you could use `/tmp` \(for Linux, macOS, or Unix\) or `c:\temp` \(for Windows\) if you are making a temporary clone for testing purposes\. That is the directory path used in these examples\.
**Note**  
You can use any directory you want\. If you are cloning a repository for long\-term use, consider creating the clone from a working directory and not one used for temporary files\. If you are using a directory different from `/tmp` or `c:\temp`, be sure to substitute that directory for ours when you follow these instructions\.

## Connect to the CodeCommit repository by cloning the repository<a name="how-to-connect-http"></a>

If you do not already have a local repo, follow the steps in this procedure to clone the CodeCommit repository to your local machine\.

1. Complete the prerequisites, including [Setting up ](setting-up.md)\.
**Important**  
If you have not completed setup, you cannot connect to or clone the repository\.

1. From the `/tmp` directory or the `c:\temp` directory, use Git to run the clone command\. The following examples show how to clone a repository named *MyDemoRepo* in the US East \(Ohio\) Region\.

   For HTTPS using [Git credentials](setting-up-gc.md) or the credential helper included with the AWS CLI:

   ```
   git clone https://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo my-demo-repo
   ```

   For HTTPS using [git\-remote\-codecommit](setting-up-git-remote-codecommit.md), assuming the default profile and AWS Region configured in the AWS CLI:

   ```
   git clone codecommit://MyDemoRepo my-demo-repo
   ```

   For SSH:

   ```
   git clone ssh://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo my-demo-repo
   ```

   In this example, `git-codecommit.us-east-2.amazonaws.com` is the Git connection point for the US East \(Ohio\) Region where the repository exists, `MyDemoRepo` represents the name of your CodeCommit repository, and `my-demo-repo` represents the name of the directory Git creates in the `/tmp` directory or the `c:\temp` directory\. For more information about the AWS Regions that support CodeCommit and the Git connections for those AWS Regions, see [Regions and Git connection endpoints](regions.md)\.
**Note**  
When you use SSH on Windows operating systems to clone a repository, you might need to add the SSH key ID to the connection string as follows:  

   ```
   git clone ssh://Your-SSH-Key-ID@git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo my-demo-repo
   ```
For more information, see [For SSH connections on Windows](setting-up-ssh-windows.md) and [Troubleshooting](troubleshooting.md)\.

   After Git creates the directory, it pulls down a copy of your CodeCommit repository into the newly created directory\.

   If the CodeCommit repository is new or otherwise empty, you see a message that you are cloning an empty repository\. This is expected\.
**Note**  
If you receive an error that Git can't find the CodeCommit repository or that you don't have permission to connect to the CodeCommit repository, make sure you completed the [prerequisites](setting-up.md), including assigning permissions to the IAM user and setting up your IAM user credentials for Git and CodeCommit on the local machine\. Also, make sure you specified the correct repository name\.

After you successfully connect your local repo to your CodeCommit repository, you are now ready to start running Git commands from the local repo to create commits, branches, and tags and push to and pull from the CodeCommit repository\.

## Connect a local repo to the CodeCommit repository<a name="how-to-connect-local"></a>

Complete the following steps if you already have a local repo and want to add a CodeCommit repository as the remote repository\. If you already have a remote repository and want to push your commits to CodeCommit and that other remote repository, follow the steps in [Push commits to two repositories](how-to-mirror-repo-pushes.md)\.

1. Complete the [prerequisites](#how-to-connect-prerequisites)\.

1. From the command prompt or terminal, switch to your local repo directory and run the git remote add command to add the CodeCommit repository as a remote repository for your local repo\.

   For example, the following command adds the remote nicknamed **origin** to https://git\-codecommit\.us\-east\-2\.amazonaws\.com/v1/repos/MyDemoRepo:

   For HTTPS:

   ```
   git remote add origin https://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo
   ```

   For SSH:

   ```
   git remote add origin ssh://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo
   ```

   This command returns nothing\.

1. To verify that you have added the CodeCommit repository as a remote for your local repo, run the git remote \-v command , which should create output similar to the following:

   For HTTPS:

   ```
   origin  https://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo (fetch)      
   origin  https://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo (push)
   ```

   For SSH:

   ```
   origin  ssh://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo (fetch)       
   origin  ssh://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo (push)
   ```

After you successfully connect your local repo to your CodeCommit repository, you are ready to start running Git commands from the local repo to create commits, branches, and tags, and to push to and pull from the CodeCommit repository\.