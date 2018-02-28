# Compare Commits in AWS CodeCommit<a name="how-to-compare-commits"></a>

You can use the AWS CodeCommit console to view the differences between commit specifiers in an AWS CodeCommit repository\. You can quickly view the difference between a commit and its parent\. You can also compare any two references, including commit IDs\. 


+ [Compare a Commit to Its Parent](#how-to-compare-commits-parent)
+ [Compare Any Two Commit Specifiers](#how-to-compare-commits-compare)

## Compare a Commit to Its Parent<a name="how-to-compare-commits-parent"></a>

You can quickly view the difference between a commit and its parent to review the commit message, the committer, and exactly what changed\.

1. Open the AWS CodeCommit console at [https://console\.aws\.amazon\.com/codecommit](https://console.aws.amazon.com/codecommit)\.

1. On the **Dashboard** page, from the list of repositories, choose the repository where you want to view the difference between a commit and its parent\. 

1. In the navigation pane, choose **Commits**\.

1. Choose the abbreviated commit ID of any commit in the list\. The view changes to show details for this commit, including the differences between it and its parent commit\.  
![\[Choose the abbreviated commit ID to show differences between this commit and its parent\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-commit-changes1.png)

   You can show changes side by side \(**Split** view\) or inline \(**Unified** view\)\. You can also hide or show white space changes\. You can also add comments\. For more information, see [Comment on a Commit](how-to-commit-comment.md)\.
**Note**  
If you are signed in as an IAM user, you can configure and save your preferences for viewing code and other console settings\. For more information, see [Working with User Preferences](user-preferences.md)\.  
![\[Changes shown in Split view, with white space changes visible\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-commit-changes2c.png)  
![\[Adding a comment to a changed line in a commit.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-commenting-savelinecomment.png)
**Note**  
 Depending on line ending style, your code editor, and other factors, you might see entire lines added or deleted instead of specific changes in a line\. The level of detail matches what's returned in the git show or git diff commands\.  

![\[Changes shown in Split view, with white space changes visible\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-commit-changes2b.png)

1. To compare a commit to its parent from the **Commit Visualizer page**, choose a reference point on the graph, and then choose **View differences between this commit and its parent**\.  
![\[The option to view differences between a commit and its parent in Commit Visualizer\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-commit-changes-visualizer.png)

## Compare Any Two Commit Specifiers<a name="how-to-compare-commits-compare"></a>

You can view the differences between any two commit specifiers in the AWS CodeCommit console\. Commit specifiers are references, such as branches, tags, and commit IDs\. 

1. Open the AWS CodeCommit console at [https://console\.aws\.amazon\.com/codecommit](https://console.aws.amazon.com/codecommit)\.

1. On the **Dashboard** page, from the list of repositories, choose the repository where you want to compare commits, branches, or tagged commits\. 

1. In the navigation pane, choose **Compare**\.  
![\[Compare any two commit specifiers\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-compare-1.png)

1. Use the **Choose** buttons to compare two commit specifiers\. 

   + To compare the tip of a branch, choose the branch name\. This selects the most recent commit from that branch for the comparison\.

   + To compare a commit with a specific tag associated with it, choose the tag name\. This selects the tagged commit for the comparison\.

   + To compare a specific commit, paste the commit ID in the text box\. To get the full commit ID, choose **Commits** in the navigation bar, and copy the commit ID from the list\. On the **Compare** page, paste the full commit ID in the text box, and press **Enter**\. You can repeat this to copy and paste a second commit ID, if you want to compare two commit IDs\.  
![\[Compare branches, tags, or commit IDs\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-compare-2.png)

1. After you have selected the specifiers, choose **Compare**\.   
![\[The comparison view between two commit specifiers\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-compare-3.png)

   You can show differences side by side \(**Split** view\) or inline \(**Unified** view\)\. You can also hide or show white space changes\.

1. To reverse the comparison, choose the Flip button \(![\[The flip button for changing the order of comparison.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-compare-flip.png)\), and then choose **Compare**\. 

1. To clear your comparison choices, choose **Clear**\.