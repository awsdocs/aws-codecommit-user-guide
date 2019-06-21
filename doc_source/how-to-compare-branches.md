# Compare Branches in AWS CodeCommit<a name="how-to-compare-branches"></a>

You can use the CodeCommit console to compare branches in a CodeCommit repository\. Comparing branches helps you quickly view the differences between a branch and the default branch, or view the differences between any two branches\.

**Topics**
+ [Compare a Branch to the Default Branch](#how-to-compare-branches-default)
+ [Compare Two Specific Branches](#how-to-compare-branches-two)

## Compare a Branch to the Default Branch<a name="how-to-compare-branches-default"></a>

Use the CodeCommit console to quickly view the differences between a branch and the default branch for your repository\.

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In **Repositories**, choose the name of the repository where you want to compare branches\. 

1. In the navigation pane, choose **Commits**, and then choose the **Compare commits** tab\.

1. In **Destination**, choose the name of the default branch\. In **Source**, choose the branch you want to compare to the default branch\. Choose **Compare**\.

## Compare Two Specific Branches<a name="how-to-compare-branches-two"></a>

Use the CodeCommit console to view the differences between two branches that you want to compare\.

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In **Repositories**, choose the name of the repository where you want to compare branches\. 

1. In the navigation pane, choose **Commits**, and then choose the **Compare commits** tab\.

1. In **Destination** and **Source**, choose the two branches to compare, and then choose **Compare**\. To view the list of changed files, expand the changed files list\. You can view changes in files side by side \(Split view\) or inline \(Unified view\)\.
**Note**  
If you are signed in as an IAM user, you can configure and save your preferences for viewing code and other console settings\. For more information, see [Working with User Preferences](user-preferences.md)\.  
![\[An abbreviated view of the differences between two branches.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-compare-branches.png)