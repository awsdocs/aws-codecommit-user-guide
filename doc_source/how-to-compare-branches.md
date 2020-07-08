# Compare and merge branches in AWS CodeCommit<a name="how-to-compare-branches"></a>

You can use the CodeCommit console to compare branches in a CodeCommit repository\. Comparing branches helps you quickly view the differences between a branch and the default branch, or view the differences between any two branches\.

**Topics**
+ [Compare a branch to the default branch](#how-to-compare-branches-default)
+ [Compare two specific branches](#how-to-compare-branches-two)
+ [Merge two branches \(AWS CLI\)](#how-to-merge-branches-cli)

## Compare a branch to the default branch<a name="how-to-compare-branches-default"></a>

Use the CodeCommit console to quickly view the differences between a branch and the default branch for your repository\.

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In **Repositories**, choose the name of the repository where you want to compare branches\. 

1. In the navigation pane, choose **Commits**, and then choose the **Compare commits** tab\.

1. In **Destination**, choose the name of the default branch\. In **Source**, choose the branch you want to compare to the default branch\. Choose **Compare**\.

## Compare two specific branches<a name="how-to-compare-branches-two"></a>

Use the CodeCommit console to view the differences between two branches that you want to compare\.

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In **Repositories**, choose the name of the repository where you want to compare branches\. 

1. In the navigation pane, choose **Commits**, and then choose the **Compare commits** tab\.

1. In **Destination** and **Source**, choose the two branches to compare, and then choose **Compare**\. To view the list of changed files, expand the changed files list\. You can view changes in files side by side \(Split view\) or inline \(Unified view\)\.
**Note**  
If you are signed in as an IAM user, you can configure and save your preferences for viewing code and other console settings\. For more information, see [Working with user preferences](user-preferences.md)\.  
![\[An abbreviated view of the differences between two branches.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-compare-branches.png)

## Merge two branches \(AWS CLI\)<a name="how-to-merge-branches-cli"></a>

You can merge two branches in a CodeCommit repository using the AWS CLI using one of the available merge strategies by running one of the following commands:
+ To merge two branches using the fast\-forward merge strategy, run the [merge\-branches\-by\-fast\-forward](#merge-branches-by-fast-forward) command\.
+ To merge two branches using the squash merge strategy, run the [merge\-branches\-by\-squash](#merge-branches-by-squash) command\.
+ To merge two branches using the three\-way merge strategy, run the [merge\-branches\-by\-three\-way](#merge-branches-by-three-way) command\.

You can also test merges by running the create\-unreferenced\-merge\-commit command\. For more information, see [Resolve Conflicts in a Pull Request](how-to-resolve-conflict-pull-request.md#create-unreferenced-merge-commit)\.

**Note**  
To use AWS CLI commands with CodeCommit, install the AWS CLI\. For more information, see [Command line reference](cmd-ref.md)\. 

**To use the AWS CLI to merge two branches in a CodeCommit repository**

1. <a name="merge-branches-by-fast-forward"></a>To merge two branches using the fast\-forward merge strategy, run the merge\-branches\-by\-fast\-forward command, specifying: 
   + The name of the source branch that contains the changes you want to merge \(with the \-\-source\-commit\-specifier option\)\. 
   + The name of the destination branch where you want to merge your changes \(with the \-\-destination\-commit\-specifier option\)\. 
   + The name of the repository \(with the \-\-repository\-name option\)\.

    For example, to merge a source branch named *bugfix\-1234* into a destination branch named *preprod* in a repository named *MyDemoRepo*:

   ```
   aws codecommit merge-branches-by-fast-forward --source-commit-specifier bugfix-bug1234 --destination-commit-specifier preprod --repository-name MyDemoRepo
   ```

   If successful, this command produces output similar to the following:

   ```
   {
       "commitId": "4f178133EXAMPLE",
       "treeId": "389765daEXAMPLE"
   }
   ```

1. <a name="merge-branches-by-squash"></a>To merge two branches using the squash merge strategy, run the merge\-branches\-by\-squash command, specifying:
   + The name of the source branch that contains the changes you want to merge \(with the \-\-source\-commit\-specifier option\)\. 
   + The name of the destination branch where you want to merge your changes \(with the \-\-destination\-commit\-specifier option\)\. 
   + The name of the repository \(with the \-\-repository\-name option\)\.
   + The commit message to include \(with the \-\-commit\-message option\)\.
   + The name to use for the commit \(with the \-\-name option\)\.
   + The email address to use for the commit \(with the \-\-email option\)\.

   For example, to merge a source branch named *bugfix\-bug1234* with a destination branch named *bugfix\-quarterly* in a repository named *MyDemoRepo*:

   ```
   aws codecommit merge-branches-by-squash --source-commit-specifier bugfix-bug1234 --destination-commit-specifier bugfix-quarterly --author-name "Maria Garcia" --email "maria_garcia@example.com" --commit-message "Merging in fix branches to prepare for a general patch." --repository-name MyDemoRepo
   ```

   If successful, this command produces output similar to the following:

   ```
   {
       "commitId": "4f178133EXAMPLE",
       "treeId": "389765daEXAMPLE"
   }
   ```

1. <a name="merge-branches-by-three-way"></a>To merge two branches using the three\-way merge strategy, run the merge\-branches\-by\-three\-way command, specifying:
   + The name of the source branch that contains the changes you want to merge \(with the \-\-source\-commit\-specifier option\)\. 
   + The name of the destination branch where you want to merge your changes \(with the \-\-destination\-commit\-specifier option\)\. 
   + The name of the repository \(with the \-\-repository\-name option\)\.
   + The commit message to include \(with the \-\-commit\-message option\)\.
   + The name to use for the commit \(with the \-\-name option\)\.
   + The email address to use for the commit \(with the \-\-email option\)\.

   For example, to merge a source branch named *master* with a destination branch named *bugfix\-1234* in a repository named *MyDemoRepo*:

   ```
   aws codecommit merge-branches-by-three-way --source-commit-specifier master --destination-commit-specifier bugfix-bug1234 --author-name "Jorge Souza" --email "jorge_souza@example.com" --commit-message "Merging changes from master to bugfix branch before additional testing."  --repository-name MyDemoRepo
   ```

   If successful, this command produces output similar to the following:

   ```
   {
       "commitId": "4f178133EXAMPLE",
       "treeId": "389765daEXAMPLE"
   }
   ```