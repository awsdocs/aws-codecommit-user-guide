--------

 The procedures in this guide support the new console design\. If you choose to use the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\.

--------

# Getting Started with AWS CodeCommit Tutorial<a name="getting-started-cc"></a>

If you're new to AWS CodeCommit, this tutorial helps you learn how to use its features\. In this tutorial, you create a repository in AWS CodeCommit\. After you commit some changes to the AWS CodeCommit repository, you browse the files and view the changes\. You can also create a pull request for others to review and comment on changes to your code\.

The AWS CodeCommit console includes helpful information in a collapsible panel that you can open from the information icon or any “Info” link on the page\. \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-info-icon.png)\)\. You can close this panel at any time\.

![\[Viewing additional guidance in the console\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-guidance-open.png)

If you are not familiar with Git, you might want to complete the [Git with AWS CodeCommit Tutorial](getting-started.md) in addition to this tutorial\. After you finish this tutorial, you should have enough practice to start using AWS CodeCommit for your own projects and in team environments\.

**Important**  
Before you begin, you must complete the [prerequisites and setup](setting-up.md), including:  
Assigning permissions to the IAM user\.
Setting up credential management for HTTPS or SSH connections on the local machine you use for this tutorial\.
Configuring the AWS CLI if you want to use the command line or terminal for all operations, including to create the repository\.

**Topics**
+ [Step 1: Create an AWS CodeCommit Repository](#getting-started-cc-create-repo)
+ [Step 2: Add Files to Your Repository](#getting-started-cc-add)
+ [Step 3: Browse the Contents of Your Repository](#getting-started-cc-browse)
+ [Step 4: Create and Collaborate on a Pull Request](#getting-started-cc-pullrequest)
+ [Step 5: Next Steps](#getting-started-cc-next)
+ [Step 6: Clean Up](#getting-started-cc-clean-up)

## Step 1: Create an AWS CodeCommit Repository<a name="getting-started-cc-create-repo"></a>

You can use the AWS CodeCommit console to create an AWS CodeCommit repository\. If you already have a repository you want to use for this tutorial, you can skip this step\. 

**Note**  
Depending on your usage, you might be charged for creating or accessing a repository\. For more information, see [Pricing](http://aws.amazon.com/codecommit/pricing) on the AWS CodeCommit product information page\.

**To create the AWS CodeCommit repository \(console\)**

1. Open the AWS CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In the region selector, choose the region where you want to create the repository\. For more information, see [Regions and Git Connection Endpoints](regions.md)\.

1. On the **Repositories** page, choose **Create repository**\. 

1. On the **Create repository** page, in **Repository name**, enter a name \(for example, **MyDemoRepo**\)\.
**Note**  
Repository names can be no longer than 100 characters\. For more information, see [Limits](limits.md#limits-repository-names)\.

1. \(Optional\) In **Description**, enter a description \(for example, **My demonstration repository\.**\)\. This can help you and other users identify the purpose of the repository\.

1. Choose **Create**\. 

![\[Creating a repository from the console\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-create-repository.png)

**Note**  
If you use a name other than `MyDemoRepo` for your repository, be sure to use it in the remaining steps of this tutorial\.

When the repository opens, you see information about how to add files directly from the AWS CodeCommit console\. 

## Step 2: Add Files to Your Repository<a name="getting-started-cc-add"></a>

You can add files to your repository by:
+ Creating a file directly in the AWS CodeCommit console\.
+ Uploading a file from your local computer using the AWS CodeCommit console\.
+ Using a Git client to clone the repository to your local computer, and then adding, committing, and pushing files to the AWS CodeCommit repository\.

The simplest way to get started is to add a file from the AWS CodeCommit console\.

1. In the navigation bar for the repository, choose **Code**\.

1. Choose **Add file**, and then choose to create or upload a file from your computer\.

1. Do the following:
   + If you want to add the file to a different branch, from the drop\-down list of branches, choose the branch where you want to add the file\. The default branch is selected automatically for you\. In the example shown here, the default branch is named *master*\.
   + In **Author name**, enter the name you want displayed to other repository users\. 
   + In **Email address**, enter an email address\. 
   + \(Optional\) In **Commit message**, enter a brief message\. Although this is optional, we recommend you add a commit message to help your team members understand why you added this file\. If you do not enter a commit message, a default message is used\.
   + If you're uploading a file, choose the file you want to upload\. 
   + If you're creating a file, in **File name**, enter a name for the file, and then in the code editor, enter the code you want to add\.   
![\[A view of uploading a file in the AWS CodeCommit console\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-commit-first-file.png)

1. Choose **Commit changes**\.

For more information, see [Working with Files in AWS CodeCommit Repositories](files.md)\.

To use a Git client to clone the repository, install Git on your local computer, and then clone the AWS CodeCommit repository\. Add some files to the local repo and push them to the AWS CodeCommit repository\. For an in\-depth introduction, try the [Git with AWS CodeCommit Tutorial](getting-started.md)\. If you are familiar with Git, but are not sure how to do this with an AWS CodeCommit repository, you can view examples and instructions in [Create a Commit](how-to-create-commit.md), [Step 2: Create a Local Repo](getting-started.md#getting-started-set-up-folders), or [Connect to a Repository](how-to-connect.md)\. 

After you have added some files to the AWS CodeCommit repository, you can view them in the console\.

## Step 3: Browse the Contents of Your Repository<a name="getting-started-cc-browse"></a>

You can use the AWS CodeCommit console to review the files in a repository or to quickly read the contents of a file\. This helps you determine which branch to check out or whether to create a local copy of a repository\. 

1. From **Repositories**, choose MyDemoRepo\.

1. The contents of the repository are displayed in the default branch for your repository\. To change the view to another branch, or to view the code at a specific tag, choose the view selector button, and then choose the branch or tag you want to view from the list\. Here the view is set to the **master** branch\.  
![\[Browse the contents of a repository\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-code-browse.png)

1. To view the contents of a file in your repository, choose the file from the list\. Choose the gear icon to change the color display option of the displayed code\.  
![\[View the contents of a file\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-code-browse-file.png)

For more information, see [Browse Files in a Repository](how-to-browse.md)\.

You can also browse the commit history of a repository\. This helps you identify changes made in a repository, including when and by whom those changes were made\.

1. In the navigation pane for a repository, choose **Commits**\. A history of commits for the repository in the default branch is displayed, in reverse chronological order\.  
![\[The commit history view in the console\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-code-history.png)

1. Review the commit history by [branch](branches.md) or by [tag](how-to-view-tag-details.md), and get details about commits by author, date, and more\. 

1. To view the differences between a commit and its parent, choose the abbreviated commit ID\. You can choose how the changes are displayed, including showing or hiding white space changes, and whether to view changes inline \(**Unified** view\) or side by side \(**Split** view\)\. 
**Note**  
Your preferences for viewing code and other console settings are saved as browser cookies whenever you change them\. For more information, see [Working with User Preferences](user-preferences.md)\.  
![\[Changes shown in Unified view, with white space changes visible\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-commit-changes2c.png)

1. To view all comments on a commit, choose the commit and then scroll through the changes to view them inline\. You can also add your own comments and reply to the comments made by others\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-commenting-commenttab.png)

   For more information, see [Comment on a Commit](how-to-commit-comment.md)\. 

1. To view the differences between any two commits specifiers, including tags, branches, and commit IDs, in the navigation pane, choose **Commits**, and then choose **Compare commits**\.   
![\[Comparing a commit to the tip of a branch in Split view\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-compare-4.png)

   For more information, see [Browse the Commit History of a Repository](how-to-view-commit-details.md#how-to-view-commit-details-console-history) and [Compare Commits](how-to-compare-commits.md)\. 

1. In **Commits**, choose the **Commit visualizer** tab\.   
![\[A graphical view of a repository in the console\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-cv-complex1.png)

   The commit graph is displayed, with the subject line for each commit shown next to its point in the graph\. The subject line display is limited to 80 characters\.

1. To see more details about a commit, choose its abbreviated commit ID\. To render the graph from a specific commit, choose that point in the graph\. For more information, see [View a Graph of the Commit History of a Repository ](how-to-view-commit-details.md#how-to-view-commit-details-console-visualizer)\.

If you set up notifications for your repository, repository users can receive emails about repository events \(for example, for pull requests or when someone comments on code\)\. For more information, see [Configuring Notifications for Events in an AWS CodeCommit Repository](how-to-repository-email.md)\.

## Step 4: Create and Collaborate on a Pull Request<a name="getting-started-cc-pullrequest"></a>

When you work with other repository users, you might want to collaborate on code and review changes\. You can create a pull request so that other users can review and comment on your code changes in a branch before you merge those changes into another branch\.

**Important**  
Before you can create a pull request, you must create a branch that contains the code changes you want to review\. For more information, see [Create a Branch](how-to-create-branch.md)\.

1. In the navigation pane, choose **Pull requests**\.

1. In **Pull request**, choose **Create pull request**\. 
**Tip**  
You can also create pull requests from **Branches** and **Code**\.

   In **Create pull request**, in **Source**, choose the branch that contains the changes you want reviewed\. In **Destination**, choose the branch where you want the reviewed code to be merged when the pull request is closed\. Choose **Compare**\. 

1. Review the merge details and changes to confirm that the pull request contains the changes and commits you want reviewed\. If so, in **Title**, enter a title for this review\. This is the title that appears in the list of pull requests for the repository\. In **Description**, enter details about what this review is about and any other useful information for reviewers\. Choose **Create**\.  
![\[Creating a pull request\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-pull-request-create.png)

1. Your pull request appears in the list of pull requests for the repository\. You can filter the view to show only open requests, closed requests, requests that you created, and more\.   
![\[Viewing pull requests in a repository\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-pull-request-view.png)

1. If you configured notifications for your repository and chose to notify users of pull request events, users receive email about your new pull request\. Users can view the changes and comment on specific lines of code, files, and the pull request itself\. They can also reply to comments\. If necessary, you can push changes to the pull request branch, which updates the pull request\.  
![\[Viewing activity in a pull request\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-pull-request-reply-activity.png)

1. When you are satisfied that all the code changes have been reviewed and agreed to, from the pull request, do one of the following:
   + If you want to use the fast\-forward merge option to automatically merge the branches as part of closing the pull request, choose **Merge**\.
   + If you want to close the pull request without using the automatic fast\-forward merge option to merge branches, choose **Close pull request**\.
   + If there are merge conflicts in the branches that cannot be automatically resolved, use your local Git client to merge the branches and then push the merge\. This closes the pull request\.
**Note**  
You can always manually merge branches, including pull request branches, by using the git merge command in your local repo and pushing your changes\. AWS CodeCommit closes the pull request when you push the merged code\.

For more information, see [Working with Pull Requests](pull-requests.md)\.

## Step 5: Next Steps<a name="getting-started-cc-next"></a>

Now that you have familiarized yourself with AWS CodeCommit and some of its features, consider doing the following:
+ If you are new to Git and AWS CodeCommit or want to review examples of using Git with AWS CodeCommit, continue to the [Git with AWS CodeCommit Tutorial](getting-started.md) tutorial\.
+ If you want to work with others in an AWS CodeCommit repository, see [Share a Repository](how-to-share-repository.md)\. \(If you want to share your repository with users in another AWS account, see [Configure Cross\-Account Access to an AWS CodeCommit Repository](cross-account.md)\.\)
+ If you want to migrate a repository to AWS CodeCommit, follow the steps in [Migrate to AWS CodeCommit](how-to-migrate-repository.md)\.
+ If you want to add your repository to a continuous delivery pipeline, follow the steps in [Simple Pipeline Walkthrough](https://docs.aws.amazon.com/codepipeline/latest/userguide/getting-started-cc.html)\.
+ If you want to learn more about products and services that integrate with AWS CodeCommit, including examples from the community, see [Product and Service Integrations](integrations.md)\.

## Step 6: Clean Up<a name="getting-started-cc-clean-up"></a>

If you are done with exploring AWS CodeCommit and no longer need the repository, you should delete the AWS CodeCommit repository and other resources you used in this tutorial so you won't continue to be charged for the storage space\. 

**Important**  
After you delete this repository, you can no longer clone it to any local repo or shared repo\. You also can no longer pull data from it, push data to it, or perform any Git operations, from any local repo or shared repo\. This action cannot be undone\.  
If you configured notifications for your repository, deleting the repository also deletes the Amazon CloudWatch Events rule created for the repository\. It does not delete the Amazon SNS topic used as a target for that rule\.  
If you configured triggers for your repository, deleting the repository does not delete the Amazon SNS topics or Lambda functions you configured as the targets of those triggers\. Be sure to delete those resources if you don't need them\. For more information, see [Delete Triggers from a Repository](how-to-notify-delete.md)\.

**To delete the AWS CodeCommit repository**

1. Open the AWS CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In **Repositories**, choose the repository you want to delete\. If you followed the naming convention in this topic, it is named **MyDemoRepo**\. 

1. In the navigation pane, choose **Settings**\. 

1. On the **Settings** page, in **Delete repository**, choose **Delete repository**\.

1. Type **delete**, and then choose **Delete**\. The repository is permanently deleted\. 