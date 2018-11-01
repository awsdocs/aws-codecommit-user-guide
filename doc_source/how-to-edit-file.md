--------

 The procedures in this guide support the new console design\. If you choose to use the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\.

--------

# Edit the Contents of a File in an AWS CodeCommit Repository<a name="how-to-edit-file"></a>

You can use the AWS CodeCommit console, AWS CLI, or a Git client to edit the contents of a file in an AWS CodeCommit repository\. 

![\[A view of editing a file in the AWS CodeCommit console\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-edit-file.png)

**Topics**
+ [Edit a File \(Console\)](#how-to-edit-file-console)
+ [Edit a File \(AWS CLI\)](#how-to-edit-file-cli)
+ [Edit a File \(Git\)](#how-to-edit-file-git)

## Edit a File \(Console\)<a name="how-to-edit-file-console"></a>

You can use the AWS CodeCommit console to edit a file that has been added to a branch in an AWS CodeCommit repository\. As part of editing the file, you can provide your user name and an email address\. You can also add a commit message so other users understand who made the change and why\.

**To edit a file in a repository**

1. Open the AWS CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In **Repositories**, choose the repository where you want to edit a file\. 

1. In the **Code** view, choose the branch where you want to edit the file\. By default, the contents of the default branch are shown when you open the **Code** view\. 

   To change the view to a different branch, choose the view selector button\. Either choose a branch name from the drop\-down list, or in the filter box, enter the name of the branch, and then choose it from the list\.

1. Navigate the contents of the branch and choose the file you want to edit\. In the file view, choose **Edit**\.
**Note**  
If you choose a binary file, a warning message appears asking you to confirm that you want to display the contents\. You should not use the AWS CodeCommit console to edit binary files\.

1. Edit the file, and provide information to other users about who made this change and why\. 
   + In **Author name**, enter your name\. This name is used as both the author name and the committer name in the commit information\. AWS CodeCommit defaults to using your IAM user name or a derivation of your console login as the author name\.
   + In **Email address**, enter an email address so that other repository users can contact you about this change\. 
   + In **Commit message**, enter a brief description of your changes\.

1. Choose **Commit changes** to save your changes to the file and commit the changes to the repository\.

## Edit a File \(AWS CLI\)<a name="how-to-edit-file-cli"></a>

You can use the AWS CLI and the put\-file command to make changes to a file in an AWS CodeCommit repository\. You can also use the put\-file command to add a directory or path structure for the changed file, if you want to store the changed file in a location different from the original\.

**Note**  
To use AWS CLI commands with AWS CodeCommit, install the AWS CLI\. For more information, see [Command Line Reference](cmd-ref.md)\. 

**To edit a file in a repository**

1. Using a local copy of the file, make the changes you want to add to the AWS CodeCommit repository\.

1. At the terminal or command line, run the put\-file command, specifying:
   + The repository where you want to add the edited file\.
   + The branch where you want to add the edited file\.
   + The full commit ID of the most recent commit made to that branch, also known as the tip or head commit\.
   + The local location of the file\.
   + The name of the updated file you want to add, including the path where the updated file is stored in the repository, if any\.
   + The user name and email you want associated with this file change\.
   + A commit message that explains the change you made\.

   The user name, email address, and commit message are optional, but help other users know who made the change and why\. If you do not supply a user name, AWS CodeCommit defaults to using your IAM user name or a derivation of your console login\.

   For example, to add edits made to a file named *ExampleSolution\.py* to a repository named *MyDemoRepo* to a branch named *feature\-randomizationfeature* whose most recent commit has an ID of *4c925148EXAMPLE*:

   ```
   aws codecommit put-file --repository-name MyDemoRepo --branch-name feature-randomizationfeature --file-content file://MyDirectory/ExampleSolution.py --file-path /solutions/ExampleSolution.py --parent-commit-id 4c925148EXAMPLE --name "María García" --email "maría_garcía@example.com" --commit-message "I fixed the bug Mary found."
   ```
**Note**  
If you want to add a changed binary file, make sure to use `--file-content` with the notation `fileb://MyDirectory/MyFile.raw`\.

   If successful, this command returns output similar to the following:

   ```
   {
      "blobId": "2eb4af3bEXAMPLE",
      "commitId": "317f8570EXAMPLE",
      "treeId": "347a3408EXAMPLE"
   }
   ```

## Edit a File \(Git\)<a name="how-to-edit-file-git"></a>

You can edit files in a local repo and push your changes to an AWS CodeCommit repository\. For more information, see [Git with AWS CodeCommit Tutorial](getting-started.md)\.