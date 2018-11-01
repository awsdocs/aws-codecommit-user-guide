--------

 The procedures in this guide support the new console design\. If you choose to use the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\.

--------

# Working with Commits in AWS CodeCommit Repositories<a name="commits"></a>

Commits to a repository are snapshots of the contents and changes to the contents of your repository\. Every time a user commits and pushes a change, that information is saved and stored\. So, too, is information that includes who committed the change, the date and time of the commit, and the changes made as part of the commit\. You can also add tags to commits, to easily identify specific commits\. In AWS CodeCommit, you can:
+ Review commits\.
+ View the history of commits in a graph\.
+ Compare a commit to its parent or to another specifier\.
+ Add comments to your commits and reply to comments made by others\.

![\[Adding a comment to a changed line in a commit.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-commenting-addlinecomment.png)

Before you can push commits to an AWS CodeCommit repository, you must set up your local computer to connect to the repository\. For the simplest method, see [For HTTPS Users Using Git Credentials](setting-up-gc.md)\. 

For information about working with other aspects of your repository in AWS CodeCommit, see [Working with Repositories](repositories.md), [Working with Files](files.md), [Working with Pull Requests](pull-requests.md) , [Working with Branches](branches.md), and [Working with User Preferences](user-preferences.md)\. 

**Topics**
+ [Create a Commit in AWS CodeCommit](how-to-create-commit.md)
+ [View Commit Details in AWS CodeCommit](how-to-view-commit-details.md)
+ [Compare Commits in AWS CodeCommit](how-to-compare-commits.md)
+ [Comment on a Commit in AWS CodeCommit](how-to-commit-comment.md)
+ [Create a Tag in AWS CodeCommit](how-to-create-tag.md)
+ [View Tag Details in AWS CodeCommit](how-to-view-tag-details.md)
+ [Delete a Tag in AWS CodeCommit](how-to-delete-tag.md)