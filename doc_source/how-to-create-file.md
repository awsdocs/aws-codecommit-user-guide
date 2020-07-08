# Create or add a file to an AWS CodeCommit repository<a name="how-to-create-file"></a>

You can use the CodeCommit console, AWS CLI, or a Git client to add a file to a repository\. You can upload a file from your local computer to the repository, or you can use the code editor in the console to create the file\. The editor is a quick and easy way to add a simple file, such as a readme\.md file, to a branch in a repository\. 

![\[A view of uploading a file in the CodeCommit console\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-commit-first-file.png)

**Topics**
+ [Create or upload a file \(console\)](#how-to-create-file-console)
+ [Add a file \(AWS CLI\)](#how-to-create-file-cli)
+ [Add a file \(Git\)](#how-to-create-file-git)

## Create or upload a file \(console\)<a name="how-to-create-file-console"></a>

You can use the CodeCommit console to create a file and add it to a branch in a CodeCommit repository\. As part of creating the file, you can provide your user name and an email address\. You can also add a commit message so other users understand who added the file and why\. You can also upload a file directly from your local computer to a branch in a repository\.

**To add a file to a repository**

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In **Repositories**, choose the repository where you want to add a file\.

1. In the **Code** view, choose the branch where you want to add the file\. By default, the contents of the default branch are shown when you open the **Code** view\. 

   To change the view to a different branch, choose the view selector button\. Either choose a branch name from the drop\-down list, or in the filter box, enter the name of the branch, and then choose it from the list\.

1. Choose **Add file**, and then choose one of the following options:
   +  To use the code editor to create the contents of a file and add it to the repository, choose **Create file**\.
   + To upload a file from your local computer to the repository, choose **Upload file**\.

1. Provide information to other users about who added this file to the repository and why\. 
   + In **Author name**, enter your name\. This name is used as both the author name and the committer name in the commit information\. CodeCommit defaults to using your IAM user name or a derivation of your console login as the author name\.
   + In **Email address**, enter an email address so that other repository users can contact you about this change\. 
   + In **Commit message**, enter a brief description\. This is optional, but highly recommended\. Otherwise, a default commit message is used\.

1. Do one of the following:
   + If you are uploading a file, choose the file from your local computer\.
   + If you are creating a file, enter the content you want to add in the code editor, and provide a name for the file\.

1. Choose **Commit changes**\.

## Add a file \(AWS CLI\)<a name="how-to-create-file-cli"></a>

You can use the AWS CLI and the put\-file command to add a file in an CodeCommit repository\. You can also use the put\-file command to add a directory or path structure for the file\.

**Note**  
To use AWS CLI commands with CodeCommit, install the AWS CLI\. For more information, see [Command line reference](cmd-ref.md)\. 

**To add a file to a repository**

1. On your local computer, create the file you want to add to the CodeCommit repository\.

1. At the terminal or command line, run the put\-file command, specifying:
   + The repository where you want to add the file\.
   + The branch where you want to add the file\.
   + The full commit ID of the most recent commit made to that branch, also known as the tip or head commit\.
   + The local location of the file\. The syntax used for this location varies, depending on your local operating system\.
   + The name of the file you want to add, including the path where the updated file is stored in the repository, if any\.
   + The user name and email you want associated with this file\.
   + A commit message that explains why you added this file\.

   The user name, email address, and commit message are optional, but help other users know who made the change and why\. If you do not supply a user name, CodeCommit defaults to using your IAM user name or a derivation of your console login as the author name\.

   For example, to add a file named *ExampleSolution\.py* to a repository named *MyDemoRepo* to a branch named *feature\-randomizationfeature* whose most recent commit has an ID of *4c925148EXAMPLE*:

   ```
   aws codecommit put-file --repository-name MyDemoRepo --branch-name feature-randomizationfeature --file-content file://MyDirectory/ExampleSolution.py --file-path /solutions/ExampleSolution.py --parent-commit-id 4c925148EXAMPLE --name "María García" --email "maría_garcía@example.com" --commit-message "I added a third randomization routine."
   ```
**Note**  
When you add binary files, make sure that you use `fileb://` to specify the local location of the file\.

   If successful, this command returns output similar to the following:

   ```
   {
      "blobId": "2eb4af3bEXAMPLE",
      "commitId": "317f8570EXAMPLE",
      "treeId": "347a3408EXAMPLE"
   }
   ```

## Add a file \(Git\)<a name="how-to-create-file-git"></a>

You can add files in a local repo and push your changes to a CodeCommit repository\. For more information, see [Getting started with Git and AWS CodeCommit](getting-started.md)\.