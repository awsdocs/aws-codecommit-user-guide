# Working with Pull Requests in AWS CodeCommit Repositories<a name="pull-requests"></a>

A pull request is the primary way you and other repository users can review, comment on, and merge code changes from one branch to another\. You can use pull requests to collaboratively review code changes for minor changes or fixes, major feature additions, or new versions of your released software\. Here is one possible workflow for a pull request:

Li Juan, a developer working in a repo named MyDemoRepo, wants to work on a new feature for an upcoming version of a product\. To keep her work separate from production\-ready code, she creates a branch off of the default branch and names it *feature\-randomizationfeature*\.  She writes code, makes commits, and pushes the new feature code into this branch\. She wants other repository users to review the code for quality before she merges her changes into the default branch\. To do this, she creates a pull request\. The pull request contains the comparison between her working branch and the branch of the code where she intends to merge her changes \(in this case, the default branch\)\. Other users review her code and changes, adding comments and suggestions\. She might update her working branch multiple times with code changes in response to comments\. Her changes are incorporated into the pull request every time she pushes them to that branch in AWS CodeCommit\. She might also incorporate changes that have been made in the intended destination branch while the pull request is open, so users can be sure they're reviewing all the proposed changes in context\. When she and her reviewers are satisfied, she merges her code and closes the pull request\. 

![\[Creating a pull request\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-pull-request-create.png)

Pull requests require two branches: a source branch that contains the code you want reviewed, and a destination branch, where you merge the reviewed code\. The source branch contains the AFTER commit, which is the commit that contains the changes you want to merge into the destination branch\. The destination branch contains the BEFORE commit, which represents the "before" state of the code \(before the pull request branch is merged into the destination branch\)\. 

![\[The source and destination branches for a pull requests, showing the relationship between before and after commits\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-pull-request-concepts.png)

The pull request displays the differences between these two branches, so users can view and comment on the changes\. You can update the pull request in response to comments by committing and pushing changes to the source branch\. 

![\[Adding a comment on a line in a pull request.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-pull-request-comment.png)

When your code has been reviewed, you can close the pull request in one of several ways: 

+ Merge the branches locally and push your changes\. This closes the request automatically\.

+ Use the AWS CodeCommit console to either close the pull request without merging or, if there are no conflicts, to close and merge the branches\.

+ Use the AWS CLI\.

Before you create a pull request:

+ Create a branch that contains the code you want reviewed \(the source branch\)\.

+ Commit and push the code you want reviewed to the source branch\.

+ Set up notifications for your repository, so other users can be notified about the pull request and changes to it\. \(This step is optional, but recommended\.\)

Pull requests are more effective when you've set up IAM users for your repository users in your AWS account\. It's easier to distinguish which user made which comment\. IAM users also have the advantage of being able to use Git credentials for repository access\. For more information, see [Step 1: Initial Configuration for AWS CodeCommit](setting-up-gc.md#setting-up-gc-account)\. However, you can use pull requests with other kinds of users, including federated access users\.

For information about working with other aspects of your repository in AWS CodeCommit, see [Working with Repositories](repositories.md), [Working with Files](files.md), [Working with Commits](commits.md), [Working with Branches](branches.md), and [Working with User Preferences](user-preferences.md)\. 


+ [Create a Pull Request](how-to-create-pull-request.md)
+ [View Pull Requests in an AWS CodeCommit Repository](how-to-view-pull-request.md)
+ [Review a Pull Request](how-to-review-pull-request.md)
+ [Update a Pull Request](how-to-update-pull-request.md)
+ [Close a Pull Request in an AWS CodeCommit Repository](how-to-close-pull-request.md)