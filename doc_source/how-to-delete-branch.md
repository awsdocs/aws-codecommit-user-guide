--------

 The procedures in this guide support the new console design\. If you choose to use the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\.

--------

# Delete a Branch in AWS CodeCommit<a name="how-to-delete-branch"></a>

You can use the AWS CodeCommit console to delete a branch in a repository\. Deleting a branch in AWS CodeCommit does not delete that branch in a local repo, so users might continue to have copies of that branch until the next time they pull changes\. To delete a branch locally and push that change to the AWS CodeCommit repository, use Git from a local repo connected to the AWS CodeCommit repository\. 

Deleting a branch does not delete any commits, but it does delete all references to the commits in that branch\. If you delete a branch that contains commits that have not been merged into another branch in the repository, you cannot retrieve those commits unless you have their full commit IDs\. 

**Note**  
You cannot use the instructions in this topic to delete a repository's default branch\. If you want to delete the default branch, you must create a branch, make the new branch the default branch, and then delete the old branch\. For more information, see [Create a Branch](how-to-create-branch.md) and [Change Branch Settings](how-to-change-branch.md)\.

**Topics**
+ [Delete a Branch \(Console\)](#how-to-delete-branch-console)
+ [Delete a Branch \(AWS CLI\)](#how-to-delete-branch-cli)
+ [Delete a Branch \(Git\)](#how-to-delete-branch-git)

## Delete a Branch \(Console\)<a name="how-to-delete-branch-console"></a>

You can use the AWS CodeCommit console to delete a branch in an AWS CodeCommit repository\. 

1. Open the AWS CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In **Repositories**, choose the name of the repository where you want to delete a branch\. 

1. In the navigation pane, choose **Branches**\.

1. Find the name of the branch that you want to delete, choose **Delete branch**, and confirm your choice\.

## Delete a Branch \(AWS CLI\)<a name="how-to-delete-branch-cli"></a>

You can use the AWS CLI to delete a branch in an AWS CodeCommit repository, if that branch is not the default branch for the repository\. For more information about installing and using the AWS CLI, see [Command Line Reference](cmd-ref.md)\. 

1. At the terminal or command line, run the delete\-branch command, specifying:
   + The name of the AWS CodeCommit repository where the branch will be deleted \(with the \-\-repository\-name option\)\.
**Tip**  
To get the name of the AWS CodeCommit repository, run the [list\-repositories](how-to-view-repository-details.md#how-to-view-repository-details-no-name-cli) command\.
   + The name of the branch to delete \(with the branch\-name option\)\.
**Tip**  
To get the name of the branch, run the [list\-branches](how-to-view-branch-details.md#how-to-view-branch-details-cli) command\.

1. For example, to delete a branch named `MyNewBranch` in an AWS CodeCommit repository named `MyDemoRepo`:

   ```
   aws codecommit delete-branch --repository-name MyDemoRepo --branch-name MyNewBranch
   ```

   This command returns information about the deleted branch, including the name of the deleted branch and the full commit ID of the commit that was the head of the branch\. For example:

   ```
   "deletedBranch": {
       "branchName": "MyNewBranch",
       "commitId": "317f8570EXAMPLE"
   }
   ```

## Delete a Branch \(Git\)<a name="how-to-delete-branch-git"></a>

Follow these steps to use Git from a local repo to delete a branch in an AWS CodeCommit repository\.

These steps are written with the assumption that you have already connected the local repo to the AWS CodeCommit repository\. For instructions, see [Connect to a Repository](how-to-connect.md)\.

1. To delete the branch from the local repo, run the git branch \-D *branch\-name* command where *branch\-name* is the name of the branch you want to delete\.
**Tip**  
To get a list of branch names, run git branch \-\-all\.

   For example, to delete a branch in the local repo named `MyNewBranch`:

   ```
   git branch -D MyNewBranch
   ```

1. To delete the branch from the AWS CodeCommit repository, run the git push *remote\-name* \-\-delete *branch\-name* command where *remote\-name* is the nickname the local repo uses for the AWS CodeCommit repository and *branch\-name* is the name of the branch you want to delete from the AWS CodeCommit repository\. 
**Tip**  
To get a list of AWS CodeCommit repository names and their URLs, run the git remote \-v command\.

   For example, to delete a branch named `MyNewBranch` in the AWS CodeCommit repository named `origin`:

   ```
   git push origin --delete MyNewBranch
   ```
**Tip**  
This command does not delete a branch if it is the default branch\.

For more options, see your Git documentation\.