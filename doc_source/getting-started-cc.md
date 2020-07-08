# Getting started with AWS CodeCommit<a name="getting-started-cc"></a>

This tutorial shows you how to use some key CodeCommit features\. First, you create a repository and commit some changes to it\. Then, you browse the files and view the changes\. You can also create a pull request so other users can review and comment on changes to your code\. 

If you are unfamiliar with Git, consider completing [Getting started with Git and CodeCommit](getting-started.md) too\. After you complete these tutorials, you should have enough practice to start using CodeCommit for your own projects and in team environments\.

The CodeCommit console includes helpful information in a collapsible panel that you can open from the information icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-info-icon.png)\) or any **Info** link on the page\. You can close this panel at any time\.

![\[Viewing additional guidance in the console\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-guidance-open.png)

The CodeCommit console also provides a way to quickly search for your resources, such as repositories, build projects, deployment applications, and pipelines\. Choose **Go to resource** or press the `/` key, and then type the name of the resource\. Any matches appear in the list\. Searches are case insensitive\. You only see resources that you have permissions to view\. For more information, see [Viewing resources in the console](auth-and-access-control-iam-identity-based-access-control.md#console-resources)\. 

## Prerequisites<a name="getting-started-cc-prereqs"></a>

Before you begin, you must complete the [prerequisites and setup](setting-up.md) procedure, including:
+ Assigning permissions to the IAM user\.
+ Setting up credential management for HTTPS or SSH connections on the local machine you use for this tutorial\.
+ Configuring the AWS CLI if you want to use the command line or terminal for all operations, including to create the repository\.

**Topics**
+ [Prerequisites](#getting-started-cc-prereqs)
+ [Step 1: Create a CodeCommit repository](#getting-started-cc-create-repo)
+ [Step 2: Add files to your repository](#getting-started-cc-add)
+ [Step 3: Browse the contents of your repository](#getting-started-cc-browse)
+ [Step 4: Create and collaborate on a pull request](#getting-started-cc-pullrequest)
+ [Step 5: Clean up](#getting-started-cc-clean-up)
+ [Step 6: Next steps](#getting-started-cc-next)

## Step 1: Create a CodeCommit repository<a name="getting-started-cc-create-repo"></a>

You can use the CodeCommit console to create a CodeCommit repository\. If you already have a repository you want to use for this tutorial, you can skip this step\. 

**Note**  
Depending on your usage, you might be charged for creating or accessing a repository\. For more information, see [Pricing](http://aws.amazon.com/codecommit/pricing) on the CodeCommit product information page\.

**To create the CodeCommit repository**

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. Use the region selector to choose the AWS Region where you want to create the repository\. For more information, see [Regions and Git connection endpoints](regions.md)\.

1. On the **Repositories** page, choose **Create repository**\. 

1. On the **Create repository** page, in **Repository name**, enter a name for your repository \(for example, **MyDemoRepo**\)\.
**Note**  
Repository names are case sensitive and can be no longer than 100 characters\. For more information, see [Limits](limits.md#limits-repository-names)\.

1. \(Optional\) In **Description**, enter a description \(for example, **My demonstration repository**\)\. This can help you and other users identify the purpose of the repository\.

1. \(Optional\) Choose **Add tag** to add one or more repository tags \(a custom attribute label that helps you organize and manage your AWS resources\) to your repository\. For more information, see [Tagging repositories in AWS CodeCommit](how-to-tag-repository.md)\.

1. \(Optional\) Select **Enable Amazon CodeGuru Reviewer for Java** if this repository will contain Java code, and you want to have CodeGuru Reviewer analyze that Java code\. CodeGuru Reviewer uses multiple machine learning models to find Java code defects and to automatically suggest improvements and fixes in pull requests\. For more information, see the Amazon CodeGuru Reviewer User Guide\.

1. Choose **Create**\. 

![\[Creating a repository from the console\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-create-repository.png)

**Note**  
If you use a name other than `MyDemoRepo` for your repository, be sure to use it in the remaining steps\.

When the repository opens, you see information about how to add files directly from the CodeCommit console\. 

## Step 2: Add files to your repository<a name="getting-started-cc-add"></a>

You can add files to your repository by:
+ Creating a file in the CodeCommit console\.
+ Uploading a file from your local computer using the CodeCommit console\.
+ Using a Git client to clone the repository to your local computer, and then adding, committing, and pushing files to the CodeCommit repository\.

The simplest way to get started is to open the CodeCommit console and add a file\.

**To add a file to the repository**

1. In the navigation bar for the repository, choose **Code**\.

1. Choose **Add file**, and then choose whether to create a file or upload a file from your computer\. This tutorial shows you how to do both\.

1. To add a file, do the following:

   1. In the drop\-down list of branches, choose the branch where you want to add the file\. The default branch is selected automatically for you\. In the example shown here, the default branch is named *master*\. If you want to add the file to a different branch, choose a different branch\. 

   1.  In **File name**, enter a name for the file\. In the code editor, enter the code for the file\. 

   1. In **Author name**, enter the name you want displayed to other repository users\. 

   1. In **Email address**, enter an email address\. 

   1. \(Optional\) In **Commit message**, enter a brief message\. Although this is optional, we recommend that you add a commit message to help your team members understand why you added this file\. If you do not enter a commit message, a default message is used\.

   1. Choose **Commit changes**\.

   To upload a file, do the following: 
   + If you're uploading a file, choose the file you want to upload\.   
![\[A view of uploading a file in the CodeCommit console\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-commit-first-file.png)
   + In **Author name**, enter the name you want displayed to other repository users\. 
   + In **Email address**, enter an email address\.
   + \(Optional\) In **Commit message**, enter a brief message\. Although this is optional, we recommend that you add a commit message to help your team members understand why you added this file\. If you do not enter a commit message, a default message is used\.
   + Choose **Commit changes**\.

For more information, see [Working with files in AWS CodeCommit repositories](files.md)\.

To use a Git client to clone the repository, install Git on your local computer, and then clone the CodeCommit repository\. Add some files to the local repo and push them to the CodeCommit repository\. For an in\-depth introduction, try the [Getting started with Git and CodeCommit](getting-started.md)\. If you are familiar with Git, but are not sure how to do this with a CodeCommit repository, you can view examples and instructions in [Create a commit](how-to-create-commit.md), [Step 2: Create a local repo](getting-started.md#getting-started-set-up-folders), or [Connect to a repository](how-to-connect.md)\. 

After you have added some files to the CodeCommit repository, you can view them in the console\.

## Step 3: Browse the contents of your repository<a name="getting-started-cc-browse"></a>

You can use the CodeCommit console to review the files in a repository or quickly read the contents of a file\. This helps you determine which branch to check out or whether to create a local copy of a repository\. 

**To browse the repository**

1. From **Repositories**, choose MyDemoRepo\.

1. The page displays the contents in the default branch of your repository\. To view another branch or to view the code at a specific tag, choose the branch or tag you want to view from the list\. In the following screenshot, the view is set to the **master** branch\.  
![\[Browse the contents of a repository\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-code-browse.png)

1. To view the contents of a file in your repository, choose the file from the list\. To change the color of the displayed code, choose the settings icon\.  
![\[View the contents of a file\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-code-browse-file.png)

   For more information, see [Browse files in a repository](how-to-browse.md)\.

1. To browse the commit history of the repository, choose **Commits**\. The console displays the commit history for the default branch, in reverse chronological order\. Review the commit details by author, date, and more\.  
![\[The commit history view in the console\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-code-history.png)

1. To view the commit history by [branch](branches.md) or by [Git tag](how-to-view-tag-details.md), choose the branch or tag you want to view from the list\. 

1. To view the differences between a commit and its parent commit, choose the abbreviated commit ID\. You can choose how the changes are displayed, including showing or hiding white space changes, and whether to view changes inline \(**Unified** view\) or side by side \(**Split** view\)\. 
**Note**  
Your preferences for viewing code and other console settings are saved as browser cookies whenever you change them\. For more information, see [Working with user preferences](user-preferences.md)\.  
![\[Changes shown in Unified view, with white space changes visible\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-commit-changes2c.png)

1. To view all comments on a commit, choose the commit and then scroll through the changes to view them inline\. You can also add your own comments and reply to the comments made by others\. 

   For more information, see [Comment on a commit](how-to-commit-comment.md)\. 

1. To view the differences between any two commits specifiers, including tags, branches, and commit IDs, in the navigation pane, choose **Commits**, and then choose **Compare commits**\.   
![\[Comparing a commit to the tip of a branch in Split view\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-compare-4.png)

   For more information, see [Browse the commit history of a repository](how-to-view-commit-details.md#how-to-view-commit-details-console-history) and [Compare commits](how-to-compare-commits.md)\. 

1. In **Commits**, choose the **Commit visualizer** tab\.   
![\[A graphical view of a repository in the console\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-cv-complex1.png)

   The commit graph is displayed, with the subject line for each commit shown next to its point in the graph\. The subject line display is limited to 80 characters\.

1. To see more details about a commit, choose its abbreviated commit ID\. To render the graph from a specific commit, choose that point in the graph\. For more information, see [View a graph of the commit history of a repository ](how-to-view-commit-details.md#how-to-view-commit-details-console-visualizer)\.

## Step 4: Create and collaborate on a pull request<a name="getting-started-cc-pullrequest"></a>

When you work in a repository with other users, you can collaborate on code and review changes\. You can create a pull request so that other users can review and comment on your code changes in a branch\. You can also create one or more approval rules for the pull request\. For example, you can create an approval rule that requires at least two other users to approve the pull request before it can be merged\. After the pull request is approved, you can merge those changes into its destination branch\. If you set up notifications for your repository, repository users can receive emails about repository events \(for example, for pull requests or when someone comments on code\)\. For more information, see [Configuring notifications for events in an AWS CodeCommit repository](how-to-repository-email.md)\.

**Important**  
Before you can create a pull request, you must create a branch that contains the code changes you want to review\. For more information, see [Create a branch](how-to-create-branch.md)\.

**To create and collaborate on a pull request**

1. In the navigation pane, choose **Pull requests**\.

1. In **Pull request**, choose **Create pull request**\. 
**Tip**  
You can also create pull requests from **Branches** and **Code**\.

   In **Create pull request**, in **Source**, choose the branch that contains the changes you want reviewed\. In **Destination**, choose the branch where you want the reviewed code to be merged when the pull request is closed\. Choose **Compare**\. 

1. Review the merge details and changes to confirm that the pull request contains the changes and commits you want reviewed\. If so, in **Title**, enter a title for this review\. This is the title that appears in the list of pull requests for the repository\. In **Description**, enter details about what this review is about and any other useful information for reviewers\. Choose **Create**\.  
![\[Creating a pull request\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-pull-request-create.png)

1. Your pull request appears in the list of pull requests for the repository\. You can filter the view to show only open requests, closed requests, requests that you created, and more\.   
![\[Viewing pull requests in a repository\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-pull-request-view.png)

1. You can add an approval rule to your pull request to ensure that certain conditions are met before it can be merged\. To add an approval rule to your pull request, choose the pull request from the list\. On the **Approvals** tab, choose **Create approval rule**\. 

1. In **Rule name**, give the rule a descriptive name\. For example, if you want to require two people to approve a pull request before it can be merged, you might name the rule **Require two approvals before merge**\. In **Number of approvals needed**, enter **2**, the number you want\. The default is 1\. Choose **Submit**\. To learn more about approval rules and approval pool members, see [Create an approval rule for a pull request](how-to-create-pull-request-approval-rule.md)\.  
![\[Creating an approval rule for a pull request\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-create-approval-rule.png)

1. If you configured notifications for your repository and chose to notify users of pull request events, users receive email about your new pull request\. Users can view the changes and comment on specific lines of code, files, and the pull request itself\. They can also reply to comments with text and emojis\. If necessary, you can push changes to the pull request branch, which updates the pull request\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-commenting-commenttab.png)

1. If you are satisfied about the changes made in the request, choose **Approve**\. You can choose to approve a pull request even if no approval rules are configured for that pull request\. This provides a clear record of your having reviewed the pull request and your approval of the changes\. You can also choose to revoke your approval if you change your mind\.   
![\[Viewing approvals on a pull request\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-pull-request-approvals.png)
**Note**  
You cannot approve a pull request if you created it\.

1. When you are satisfied that all the code changes have been reviewed and agreed to, from the pull request, do one of the following:
   + If you want to close the pull request without merging branches, choose **Close pull request**\.
   + If you want to merge the branches and close the pull request, choose **Merge**\. You can choose between the merge strategies available for your code, which depend on the differences between the source and destination branches, and whether to automatically delete the source branch after the merge is complete\. After you have made your choices, choose **Merge pull request** to complete the merge\.  
![\[A pull request showing the merge strategies available for the merge in the CodeCommit console.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-merge-squash.png)
   + If there are merge conflicts in the branches that cannot be resolved automatically, you can resolve them in the CodeCommit console, or you can use your local Git client to merge the branches and then push the merge\. For more information, see [Resolve conflicts in a pull request in an AWS CodeCommit repository](how-to-resolve-conflict-pull-request.md)\.
**Note**  
You can always manually merge branches, including pull request branches, by using the git merge command in your local repo and pushing your changes\. 

For more information, see [Working with pull requests](pull-requests.md) and [Working with approval rule templates](approval-rule-templates.md)\.

## Step 5: Clean up<a name="getting-started-cc-clean-up"></a>

If you no longer need the CodeCommit repository, you should delete the CodeCommit repository and other resources you used in this exercise so you won't continue to be charged for the storage space\. 

**Important**  
This action cannot be undone\. After you delete this repository, you can no longer clone it to any local repo or shared repo\. You also can no longer pull data from or push data to it, or perform any Git operations, from any local repo or shared repo\.   
If you configured notifications for your repository, deleting the repository also deletes the Amazon CloudWatch Events rule created for the repository\. It does not delete the Amazon SNS topic used as a target for that rule\.  
If you configured triggers for your repository, deleting the repository does not delete the Amazon SNS topics or Lambda functions you configured as the targets of those triggers\. Be sure to delete those resources if you don't need them\. For more information, see [Delete triggers from a repository](how-to-notify-delete.md)\.

**To delete the CodeCommit repository**

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In **Repositories**, choose the repository you want to delete\. If you followed the naming convention in this topic, it is named **MyDemoRepo**\. 

1. In the navigation pane, choose **Settings**\. 

1. On the **Settings** page, in **Delete repository**, choose **Delete repository**\.

1. Type **delete**, and then choose **Delete**\. The repository is permanently deleted\. 

## Step 6: Next steps<a name="getting-started-cc-next"></a>

Now that you have familiarized yourself with CodeCommit and some of its features, consider doing the following:
+ If you are new to Git and CodeCommit or want to review examples of using Git with CodeCommit, continue to the [Getting started with Git and CodeCommit](getting-started.md) tutorial\.
+ If you want to work with others in a CodeCommit repository, see [Share a repository](how-to-share-repository.md)\. \(If you want to share your repository with users in another AWS account, see [Configure cross\-account access to an AWS CodeCommit repository using roles](cross-account.md)\.\)
+ If you want to migrate a repository to CodeCommit, follow the steps in [Migrate to CodeCommit](how-to-migrate-repository.md)\.
+ If you want to add your repository to a continuous delivery pipeline, follow the steps in [Simple Pipeline Walkthrough](https://docs.aws.amazon.com/codepipeline/latest/userguide/getting-started-cc.html)\.
+ If you want to learn more about products and services that integrate with CodeCommit, including examples from the community, see [Product and service integrations](integrations.md)\.