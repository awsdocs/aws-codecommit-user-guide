# Compare Branches in AWS CodeCommit<a name="how-to-compare-branches"></a>

You can compare branches in an AWS CodeCommit repository by using the AWS CodeCommit console\. Comparing branches helps you quickly view the differences between a branch and the default branch, or view the differences between any two branches\.

**Topics**
+ [Compare a Branch to the Default Branch](#how-to-compare-branches-default)
+ [Compare Two Specific Branches](#how-to-compare-branches-two)

## Compare a Branch to the Default Branch<a name="how-to-compare-branches-default"></a>

Use the AWS CodeCommit console to quickly view the differences between a branch and the default branch for your repository\.

1. Open the AWS CodeCommit console at [https://console\.aws\.amazon\.com/codecommit](https://console.aws.amazon.com/codecommit)\.

1. In the list of repositories, choose the name of the repository\. 

1. In the navigation pane, choose **Branches**\.

1. In the list of branches, find the branch you want to compare to the default branch, and then choose **Compare**\.   
![\[The list of branches in a repository, including information about the branches and the option to compare other branches to the default branch.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-branches.png)

   The **Compare** view opens and displays the differences between the branch you chose and the default branch\.  
![\[An abbreviated view of the differences between a branch and the default branch in a repository, including the number of changed files and the changes in those files.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-branches-compare1.png)

## Compare Two Specific Branches<a name="how-to-compare-branches-two"></a>

Use the AWS CodeCommit console to view the differences between two branches that you want to compare\.

1. Open the AWS CodeCommit console at [https://console\.aws\.amazon\.com/codecommit](https://console.aws.amazon.com/codecommit)\.

1. In the list of repositories, choose the name of the repository\. 

1. In the navigation pane, choose **Compare**\.

1. Choose the two branches to compare, and then choose **Compare**\. To view the list of changed files, expand the changed files list\. You can view changes in files side by side \(Split view\) or inline \(Unified view\)\.
**Note**  
If you are signed in as an IAM user, you can configure and save your preferences for viewing code and other console settings\. For more information, see [Working with User Preferences](user-preferences.md)\.  
![\[An abbreviated view of the differences between two branches, including the number of changed files and the changes in those files.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-branches-compare1.png)