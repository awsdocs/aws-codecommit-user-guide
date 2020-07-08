# Working with branches in AWS CodeCommit repositories<a name="branches"></a>

What is a branch? In Git, branches are simply pointers or references to a commit\. In development, they're a convenient way to organize your work\. You can use branches to separate work on a new or different version of files without impacting work in other branches\. You can use branches to develop new features, store a specific version of your project from a particular commit, and more\.

In CodeCommit, you can change the default branch for your repository\. This default branch is the one used as the base or default branch in local repos when users clone the repository\. You can also create and delete branches and view details about a branch\. You can quickly compare differences between a branch and the default branch \(or any two branches\)\. To view the history of branches and merges in your repository, you can use the [Commit Visualizer](how-to-view-commit-details.md#how-to-view-commit-details-console-visualizer)\.

![\[A view of branches in a repository\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-branches.png)

For information about working with other aspects of your repository in CodeCommit, see [Working with repositories](repositories.md), [Working with files](files.md), [Working with pull requests](pull-requests.md), [Working with commits](commits.md), and [Working with user preferences](user-preferences.md)\. 

**Topics**
+ [Create a branch in AWS CodeCommit](how-to-create-branch.md)
+ [Limit pushes and merges to branches in AWS CodeCommit](how-to-conditional-branch.md)
+ [View branch details in AWS CodeCommit](how-to-view-branch-details.md)
+ [Compare and merge branches in AWS CodeCommit](how-to-compare-branches.md)
+ [Change branch settings in AWS CodeCommit](how-to-change-branch.md)
+ [Delete a branch in AWS CodeCommit](how-to-delete-branch.md)