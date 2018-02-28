# Create a Branch in AWS CodeCommit<a name="how-to-create-branch"></a>

You can use the AWS CodeCommit console to create branches for your repository\. This is a quick way to separate work on a new or different version of files without impacting work in the default branch\. After creating a branch in the AWS CodeCommit console, you'll need to pull that change to your local repo\. Alternatively, you can create a branch locally and push that change to an AWS CodeCommit repository by using Git from a local repo connected to the AWS CodeCommit repository\. You can also use the AWS CLI\.


+ [Use the AWS CodeCommit Console to Create a Branch](#how-to-create-branch-console)
+ [Use Git to Create a Branch](#how-to-create-branch-git)
+ [Use the AWS CLI to Create a Branch](#how-to-create-branch-cli)

## Use the AWS CodeCommit Console to Create a Branch<a name="how-to-create-branch-console"></a>

You can use the AWS CodeCommit console to create a branch in an AWS CodeCommit repository\. When users next pull changes from the repository, they will see the new branch\.

1. Open the AWS CodeCommit console at [https://console\.aws\.amazon\.com/codecommit](https://console.aws.amazon.com/codecommit)\.

1. In the list of repositories, choose the name of the repository\. 

1. In the navigation pane, choose **Branches**\.

1. Choose **Create branch**\.   
![\[Creating a branch in the AWS CodeCommit console.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-branches-create.png)

   Type a name for the branch in **Branch name**\. In **Branch from**, the default branch is selected\. If you want to branch from a different branch or from a specific commit, expand the branch list, and either choose a branch from the list, or paste a specific commit ID\. Choose **Create**\.

## Use Git to Create a Branch<a name="how-to-create-branch-git"></a>

To use Git from a local repo to create a branch in an local repo and then push that branch to the AWS CodeCommit repository, follow these steps\.

These steps assume you have already connected the local repo to the AWS CodeCommit repository\. For instructions, see [Connect to a Repository](how-to-connect.md)\.

1. Create a new branch in your local repo by running the git checkout \-b *new\-branch\-name* command, where *new\-branch\-name* is the name of the new branch\.

   For example, the following command creates a new branch named `MyNewBranch` in the local repo:

   ```
   git checkout -b MyNewBranch
   ```

1. To push the new branch from the local repo to the AWS CodeCommit repository, run the git push command, specifying both the *remote\-name* and the *new\-branch\-name*\. 

   For example, to push a new branch in the local repo named `MyNewBranch` to the AWS CodeCommit repository with the nickname `origin`:

   ```
   git push origin MyNewBranch
   ```

**Note**  
If you add the `-u` option to git push \(for example, git push \-u origin master\), then in the future you can run git push without *remote\-name* *branch\-name*\. Upstream tracking information will be set\. To get upstream tracking information, run git remote show *remote\-name* \(for example, git remote show origin\)\.  
To see a list of all of your local and remote tracking branches, run git branch \-\-all\.  
To set up a branch in the local repo that is connected to an existing branch in the AWS CodeCommit repository, run git checkout *remote\-branch\-name*\.

For more options, see your Git documentation\.

## Use the AWS CLI to Create a Branch<a name="how-to-create-branch-cli"></a>

To use AWS CLI commands with AWS CodeCommit, install the AWS CLI\. For more information, see [Command Line Reference](cmd-ref.md)\. 

To use the AWS CLI to create a branch in an AWS CodeCommit repository and then push that branch to the AWS CodeCommit repository, follow these steps\.

1. Run the create\-branch command, specifying:

   + The name of the AWS CodeCommit repository where the branch will be created \(with the \-\-repository\-name option\)\.
**Note**  
To get the name of the AWS CodeCommit repository, run the [list\-repositories](how-to-view-repository-details.md#how-to-view-repository-details-no-name-cli) command\.

   + The name of the new branch \(with the \-\-branch\-name option\)\.

   + The ID of the commit to which the new branch will point \(with the \-\-commit\-id option\)\.

   For example, to create a new branch named `MyNewBranch` that points to commit ID `317f8570EXAMPLE` in an AWS CodeCommit repository named `MyDemoRepo`:

   ```
   aws codecommit create-branch --repository-name MyDemoRepo --branch-name MyNewBranch --commit-id 317f8570EXAMPLE
   ```

   This command produces output only if there are errors\.

1. To update your local repo's list of available AWS CodeCommit repository branches with the new remote branch name, run git remote update *remote\-name*\.

   For example, to update your local repo's list of available branches for the AWS CodeCommit repository with the nickname `origin`:

   ```
   git remote update origin 
   ```
**Note**  
Alternatively, you can run the git fetch command\. You can also view all remote branches by running git branch \-\-all, but until you update your local repo's list, the remote branch you created will not appear in the list\.   
For more options, see your Git documentation\.

1. To set up a branch in the local repo that is connected to the new branch in the AWS CodeCommit repository, run git checkout *remote\-branch\-name*\.

**Note**  
 To get a list of AWS CodeCommit repository names, along with their URLs, run the git remote \-v command\.