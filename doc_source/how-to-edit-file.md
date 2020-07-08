# Edit the contents of a file in an AWS CodeCommit repository<a name="how-to-edit-file"></a>

You can use the CodeCommit console, AWS CLI, or a Git client to edit the contents of a file in a CodeCommit repository\. 

![\[A view of editing a file in the CodeCommit console\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-edit-file.png)

**Topics**
+ [Edit a file \(console\)](#how-to-edit-file-console)
+ [Edit or delete a file \(AWS CLI\)](#how-to-edit-file-cli)
+ [Edit a file \(Git\)](#how-to-edit-file-git)

## Edit a file \(console\)<a name="how-to-edit-file-console"></a>

You can use the CodeCommit console to edit a file that has been added to a branch in a CodeCommit repository\. As part of editing the file, you can provide your user name and an email address\. You can also add a commit message so other users understand who made the change and why\.

**To edit a file in a repository**

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In **Repositories**, choose the repository where you want to edit a file\. 

1. In the **Code** view, choose the branch where you want to edit the file\. By default, the contents of the default branch are shown when you open the **Code** view\. 

   To change the view to a different branch, choose the view selector button\. Either choose a branch name from the drop\-down list, or in the filter box, enter the name of the branch, and then choose it from the list\.

1. Navigate the contents of the branch and choose the file you want to edit\. In the file view, choose **Edit**\.
**Note**  
If you choose a binary file, a warning message appears asking you to confirm that you want to display the contents\. You should not use the CodeCommit console to edit binary files\.

1. Edit the file, and provide information to other users about who made this change and why\. 
   + In **Author name**, enter your name\. This name is used as both the author name and the committer name in the commit information\. CodeCommit defaults to using your IAM user name or a derivation of your console login as the author name\.
   + In **Email address**, enter an email address so that other repository users can contact you about this change\. 
   + In **Commit message**, enter a brief description of your changes\.

1. Choose **Commit changes** to save your changes to the file and commit the changes to the repository\.

## Edit or delete a file \(AWS CLI\)<a name="how-to-edit-file-cli"></a>

You can use the AWS CLI and the put\-file command to make changes to a file in a CodeCommit repository\. You can also use the put\-file command to add a directory or path structure for the changed file, if you want to store the changed file in a location different from the original\. If you want to delete a file entirely, you can use the delete\-file command\.

**Note**  
To use AWS CLI commands with CodeCommit, install the AWS CLI\. For more information, see [Command line reference](cmd-ref.md)\. 

**To edit a file in a repository**

1. Using a local copy of the file, make the changes you want to add to the CodeCommit repository\.

1. At the terminal or command line, run the put\-file command, specifying:
   + The repository where you want to add the edited file\.
   + The branch where you want to add the edited file\.
   + The full commit ID of the most recent commit made to that branch, also known as the tip or head commit\.
   + The local location of the file\.
   + The name of the updated file you want to add, including the path where the updated file is stored in the repository, if any\.
   + The user name and email you want associated with this file change\.
   + A commit message that explains the change you made\.

   The user name, email address, and commit message are optional, but help other users know who made the change and why\. If you do not supply a user name, CodeCommit defaults to using your IAM user name or a derivation of your console login\.

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

To delete a file, use the delete\-file command\. For example, to delete a file named *README\.md* in a branch named *master* with a most recent commit ID of *c5709475EXAMPLE* in a repository named *MyDemoRepo*:

```
aws codecommit delete-file --repository-name MyDemoRepo --branch-name master --file-path README.md --parent-commit-id c5709475EXAMPLE
```

If successful, this command returns output similar to the following:

```
{
  "blobId":"559b44fEXAMPLE",
  "commitId":"353cf655EXAMPLE",
  "filePath":"README.md",
  "treeId":"6bc824cEXAMPLE"
}
```

## Edit a file \(Git\)<a name="how-to-edit-file-git"></a>

You can edit files in a local repo and push your changes to a CodeCommit repository\. For more information, see [Getting started with Git and AWS CodeCommit](getting-started.md)\.