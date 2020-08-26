# Create a branch in AWS CodeCommit<a name="how-to-create-branch"></a>

You can use the CodeCommit console or the AWS CLI to create branches for your repository\. This is a quick way to separate work on a new or different version of files without impacting work in the default branch\. After you create a branch in the CodeCommit console, you must pull that change to your local repo\. Alternatively, you can create a branch locally and then use Git from a local repo connected to the CodeCommit repository to push that change\. 

**Topics**
+ [Create a branch \(console\)](#how-to-create-branch-console)
+ [Create a branch \(Git\)](#how-to-create-branch-git)
+ [Create a branch \(AWS CLI\)](#how-to-create-branch-cli)

## Create a branch \(console\)<a name="how-to-create-branch-console"></a>

You can use the CodeCommit console to create a branch in a CodeCommit repository\. The next time users pull changes from the repository, they see the new branch\.

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In **Repositories**, choose the name of the repository where you want to create a branch\. 

1. In the navigation pane, choose **Branches**\.

1. Choose **Create branch**\.   
![\[Creating a branch in the CodeCommit console.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-branches-create.png)

   In **Branch name**, enter a name for the branch\. In **Branch from**, choose a branch or tag from the list, or paste a commit ID\. Choose **Create branch**\.

## Create a branch \(Git\)<a name="how-to-create-branch-git"></a>

Follow these steps to use Git from a local repo to create a branch in a local repo and then push that branch to the CodeCommit repository\.

These steps are written with the assumption that you have already connected the local repo to the CodeCommit repository\. For instructions, see [Connect to a repository](how-to-connect.md)\.

1. Create a branch in your local repo by running the git checkout \-b *new\-branch\-name* command, where *new\-branch\-name* is the name of the new branch\.

   For example, the following command creates a branch named `MyNewBranch` in the local repo:

   ```
   git checkout -b MyNewBranch
   ```

1. To push the new branch from the local repo to the CodeCommit repository, run the git push command, specifying both the *remote\-name* and the *new\-branch\-name*\. 

   For example, to push a new branch in the local repo named `MyNewBranch` to the CodeCommit repository with the nickname `origin`:

   ```
   git push origin MyNewBranch
   ```

**Note**  
If you add the `-u` option to git push \(for example, git push \-u origin master\), then in the future you can run git push without *remote\-name* *branch\-name*\. Upstream tracking information is set\. To get upstream tracking information, run git remote show *remote\-name* \(for example, git remote show origin\)\.  
To see a list of all of your local and remote tracking branches, run git branch \-\-all\.  
To set up a branch in the local repo that is connected to a branch in the CodeCommit repository, run git checkout *remote\-branch\-name*\.

For more options, see your Git documentation\.

## Create a branch \(AWS CLI\)<a name="how-to-create-branch-cli"></a>

To use AWS CLI commands with CodeCommit, install the AWS CLI\. For more information, see [Command line reference](cmd-ref.md)\. 

Follow these steps to use the AWS CLI to create a branch in a CodeCommit repository and then push that branch to the CodeCommit repository\. For steps to create an initial commit and specify the name of the default branch for an empty repository, see [Create the first commit for a repository using the AWS CLI](how-to-create-commit.md#create-first-commit)\.

1. Run the create\-branch command, specifying:
   + The name of the CodeCommit repository where the branch is created \(with the \-\-repository\-name option\)\.
**Note**  
To get the name of the CodeCommit repository, run the [list\-repositories](how-to-view-repository-details.md#how-to-view-repository-details-no-name-cli) command\.
   + The name of the new branch \(with the \-\-branch\-name option\)\.
   + The ID of the commit to which the new branch points \(with the \-\-commit\-id option\)\.

   For example, to create a branch named `MyNewBranch` that points to commit ID `317f8570EXAMPLE` in a CodeCommit repository named `MyDemoRepo`:

   ```
   aws codecommit create-branch --repository-name MyDemoRepo --branch-name MyNewBranch --commit-id 317f8570EXAMPLE
   ```

   This command produces output only if there are errors\.

1. To update the list of available CodeCommit repository branches in your local repo with the new remote branch name, run git remote update *remote\-name*\.

   For example, to update the list of available branches for the CodeCommit repository with the nickname `origin`:

   ```
   git remote update origin 
   ```
**Note**  
Alternatively, you can run the git fetch command\. You can also view all remote branches by running git branch \-\-all, but until you update the list of your local repo, the remote branch you created does not appear in the list\.   
For more options, see your Git documentation\.

1. To set up a branch in the local repo that is connected to the new branch in the CodeCommit repository, run git checkout *remote\-branch\-name*\.

**Note**  
 To get a list of CodeCommit repository names and their URLs, run the git remote \-v command\.