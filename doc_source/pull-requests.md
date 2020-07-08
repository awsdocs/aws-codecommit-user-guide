# Working with pull requests in AWS CodeCommit repositories<a name="pull-requests"></a>

A pull request is the primary way you and other repository users can review, comment on, and merge code changes from one branch to another\. You can use pull requests to collaboratively review code changes for minor changes or fixes, major feature additions, or new versions of your released software\. Here is one possible workflow for a pull request:

Li Juan, a developer working in a repo named MyDemoRepo, wants to work on a new feature for an upcoming version of a product\. To keep her work separate from production\-ready code, she creates a branch off of the default branch and names it *feature\-randomizationfeature*\. She writes code, makes commits, and pushes the new feature code into this branch\. She wants other repository users to review the code for quality before she merges her changes into the default branch\. To do this, she creates a pull request\. The pull request contains the comparison between her working branch and the branch of the code where she intends to merge her changes \(in this case, the default branch\)\. She can also create an approval rule that requires a specified number of users to approve her pull request\. She can even specify an approval pool of users\. Other users review her code and changes, adding comments and suggestions\. She might update her working branch multiple times with code changes in response to comments\. Her changes are incorporated into the pull request every time she pushes them to that branch in CodeCommit\. She might also incorporate changes that have been made in the intended destination branch while the pull request is open, so users can be sure they're reviewing all of the proposed changes in context\. When she and her reviewers are satisfied, and the conditions for approval rules \(if any\) have been satisfied, she or one of her reviewers merges her code and closes the pull request\. 

![\[Creating a pull request\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-pull-request-create.png)

Pull requests require two branches: a source branch that contains the code you want reviewed, and a destination branch, where you merge the reviewed code\. The source branch contains the AFTER commit, which is the commit that contains the changes you want to merge into the destination branch\. The destination branch contains the BEFORE commit, which represents the state of the code before the pull request branch is merged into the destination branch\. The choice of merge strategy affects the details of how commits are merged between the source and destination branches in the CodeCommit console\. For more information about merge strategies in CodeCommit, see [Merge a pull request \(console\)](how-to-merge-pull-request.md#how-to-merge-pull-request-console)\.

![\[The source and destination branches for a pull requests, showing the relationship between before and after commits\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-pull-request-concepts.png)

The pull request displays the differences between the tip of the source branch and the latest commit on the destination branch when the pull request is created, so users can view and comment on the changes\. You can update the pull request in response to comments by committing and pushing changes to the source branch\. 

![\[Adding a comment on a line in a pull request.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-pull-request-comment.png)

When your code has been reviewed, and the approval rule requirements \(if any\) have been satisfied, you can close the pull request in one of several ways: 
+ Merge the branches locally and push your changes\. This closes the request automatically\.
+ Use the AWS CodeCommit console to close the pull request without merging, resolve conflicts in a merge, or, if there are no conflicts, close and merge the branches using one of the available merge strategies\.
+ Use the AWS CLI\.

Before you create a pull request:
+ Make sure that you have committed and pushed the code changes you want reviewed to a branch \(the source branch\)\.
+ Set up notifications for your repository, so other users can be notified about the pull request and changes to it\. \(This step is optional, but recommended\.\)
+ Create and associate approval rule templates with your repository, so that approval rules are automatically created for pull requests to help ensure code quality\. For more information, see [Working with approval rule templates](approval-rule-templates.md)\.

Pull requests are more effective when you've set up IAM users for your repository users in your AWS account\. It's easier to identify which user made which comment\. The other advantage is that IAM users can use Git credentials for repository access\. For more information, see [Step 1: Initial configuration for CodeCommit](setting-up-gc.md#setting-up-gc-account)\. You can use pull requests with other kinds of users, including federated access users\.

For information about working with other aspects of your repository in CodeCommit, see [Working with repositories](repositories.md), [Working with approval rule templates](approval-rule-templates.md), [Working with files](files.md), [Working with commits](commits.md), [Working with branches](branches.md), and [Working with user preferences](user-preferences.md)\. 

**Topics**
+ [Create a pull request](how-to-create-pull-request.md)
+ [Create an approval rule for a pull request](how-to-create-pull-request-approval-rule.md)
+ [View pull requests in an AWS CodeCommit repository](how-to-view-pull-request.md)
+ [Review a pull request](how-to-review-pull-request.md)
+ [Update a pull request](how-to-update-pull-request.md)
+ [Edit or delete an approval rule for a pull request](how-to-edit-delete-pull-request-approval-rule.md)
+ [Override approval rules on a pull request](how-to-override-approval-rules.md)
+ [Merge a pull request in an AWS CodeCommit repository](how-to-merge-pull-request.md)
+ [Resolve conflicts in a pull request in an AWS CodeCommit repository](how-to-resolve-conflict-pull-request.md)
+ [Close a pull request in an AWS CodeCommit repository](how-to-close-pull-request.md)