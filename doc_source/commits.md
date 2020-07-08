# Working with commits in AWS CodeCommit repositories<a name="commits"></a>

Commits are snapshots of the contents and changes to the contents of your repository\. Every time a user commits and pushes a change, that information is saved and stored\. So, too, is information that includes who committed the change, the date and time of the commit, and the changes made as part of the commit\. You can also add tags to commits, to easily identify specific commits\. In CodeCommit, you can:
+ Review commits\.
+ View the history of commits in a graph\.
+ Compare a commit to its parent or to another specifier\.
+ Add comments to your commits and reply to comments made by others\.

![\[Adding a comment to a changed line in a commit.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-commenting-addlinecomment.png)

Before you can push commits to a CodeCommit repository, you must set up your local computer to connect to the repository\. For the simplest method, see [For HTTPS users using Git credentials](setting-up-gc.md)\. 

For information about working with other aspects of your repository in CodeCommit, see [Working with repositories](repositories.md), [Working with files](files.md), [Working with pull requests](pull-requests.md) , [Working with branches](branches.md), and [Working with user preferences](user-preferences.md)\. 

**Topics**
+ [Create a commit in AWS CodeCommit](how-to-create-commit.md)
+ [View commit details in AWS CodeCommit](how-to-view-commit-details.md)
+ [Compare commits in AWS CodeCommit](how-to-compare-commits.md)
+ [Comment on a commit in AWS CodeCommit](how-to-commit-comment.md)
+ [Create a Git tag in AWS CodeCommit](how-to-create-tag.md)
+ [View Git tag details in AWS CodeCommit](how-to-view-tag-details.md)
+ [Delete a Git tag in AWS CodeCommit](how-to-delete-tag.md)