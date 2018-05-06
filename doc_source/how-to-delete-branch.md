# Delete a Branch in AWS CodeCommit<a name="how-to-delete-branch"></a>

You can use the AWS CodeCommit console to delete a branch in a repository\. Deleting a branch in AWS CodeCommit does not delete that branch in a local repo, so users might continue to have copies of that branch until the next time they pull changes\. To delete a branch locally and push that change to the AWS CodeCommit repository, use Git from a local repo connected to the AWS CodeCommit repository\. 

Deleting a branch does not delete any commits, but it does delete all references to the commits in that branch\. If you delete a branch that contains commits that have not been merged into another branch in the repository, you cannot retrieve those commits unless you have their full commit IDs\. 

**Note**  
You cannot use the instructions in this topic to delete a repository's default branch\. If you want to delete the default branch, you must create a new branch, make the new branch the default branch, and then delete the old branch\. To learn how to create a new branch, see [Create a Branch](how-to-create-branch.md)\. To learn how to make a branch the default branch, see [Change Branch Settings](how-to-change-branch.md)\.

**Topics**
+ [Use the AWS CodeCommit Console to Delete a Branch](#how-to-delete-branch-console)
+ [Use the AWS CLI to Delete a Branch](#how-to-delete-branch-cli)
+ [Use Git to Delete a Branch](#how-to-delete-branch-git)

## Use the AWS CodeCommit Console to Delete a Branch<a name="how-to-delete-branch-console"></a>

You can use the AWS CodeCommit console to delete a branch in an AWS CodeCommit repository\. 

1. Open the AWS CodeCommit console at [https://console\.aws\.amazon\.com/codecommit](https://console.aws.amazon.com/codecommit)\.

1. In the list of repositories, choose the name of the repository\. 

1. In the navigation pane, choose **Branches**\.

1. Find the name of the branch that you want to delete, and choose the delete icon\. In the confirmation dialog, choose **Delete**\.  
![\[Deleting a branch from a repository.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-branches-delete-2step.png)

## Use the AWS CLI to Delete a Branch<a name="how-to-delete-branch-cli"></a>

You can use the AWS CLI to delete a branch in an AWS CodeCommit repository, if that branch is not the default branch for the repository\. For more information about intalling and using the AWS CLI, see [Command Line Reference](cmd-ref.md)\. 

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

## Use Git to Delete a Branch<a name="how-to-delete-branch-git"></a>

To use Git from a local repo to delete a branch in an AWS CodeCommit repository, follow these steps\.

These steps assume you have already connected the local repo to the AWS CodeCommit repository\. For instructions, see [Connect to a Repository](how-to-connect.md)\.

1. To delete the branch from the local repo, run the git branch \-D *branch\-name* command where *branch\-name* is the name of the branch you want to delete\.
**Tip**  
To get a list of branch names, run git branch \-\-all\.

   For example, to delete a branch in the local repo named `MyNewBranch`:

   ```
   git branch -D MyNewBranch
   ```

1. To delete the branch from the AWS CodeCommit repository, run the git push *remote\-name* \-\-delete *branch\-name* command where *remote\-name* is the nickname the local repo uses for the AWS CodeCommit repository and *branch\-name* is the name of the branch you want to delete from the AWS CodeCommit repository\. 
**Tip**  
To get a list of AWS CodeCommit repository names along with their URLs, run the git remote \-v command\.

   For example, to delete a branch named `MyNewBranch` in the AWS CodeCommit repository named `origin`:

   ```
   git push origin --delete MyNewBranch
   ```
**Tip**  
This command will not delete a branch if it is the default branch\.

For more options, see your Git documentation\.