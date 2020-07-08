# View branch details in AWS CodeCommit<a name="how-to-view-branch-details"></a>

You can use the CodeCommit console to view details about the branches in a CodeCommit repository\. You can view the date of the last commit to a branch, the commit message, and more\. You can also use the AWS CLI or Git from a local repo connected to the CodeCommit repository\.

**Topics**
+ [View branch details \(console\)](#how-to-view-branch-details-console)
+ [View branch details \(Git\)](#how-to-view-branch-details-git)
+ [View branch details \(AWS CLI\)](#how-to-view-branch-details-cli)

## View branch details \(console\)<a name="how-to-view-branch-details-console"></a>

Use the CodeCommit console to quickly view a list of branches for your repository and details about the branches\.

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In **Repositories**, choose the name of the repository where you want to view branch details\. 

1. In the navigation pane, choose **Branches**\.  
![\[A view of branches in a repository.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-branches.png)

1. The name of the branch used as the default for the repository is displayed next to **Default branch**\. To view details about the most recent commit to a branch, choose the branch, and then choose **View last commit**\. To view the files and code in a branch, choose the branch name\. 

## View branch details \(Git\)<a name="how-to-view-branch-details-git"></a>

To use Git from a local repo to view details about both the local and remote tracking branches for a CodeCommit repository, run the git branch command\.

The following steps are written with the assumption that you have already connected the local repo to the CodeCommit repository\. For instructions, see [Connect to a repository](how-to-connect.md)\.

1. Run the git branch command, specifying the \-\-all option:

   ```
   git branch --all
   ```

1. If successful, this command returns output similar to the following:

   ```
     MyNewBranch
   * master
     remotes/origin/MyNewBranch
     remotes/origin/master
   ```

   The asterisk \(`*`\) appears next to the currently open branch\. The entries after that are remote tracking references\.
**Tip**  
git branch shows local branches\.  
git branch \-r shows remote branches\.  
git checkout *existing\-branch\-name* switches to the specified branch name and, if git branch is run immediately afterward, displays it with an asterisk \(`*`\)\.  
git remote update *remote\-name* updates your local repo with the list of available CodeCommit repository branches\. \(To get a list of CodeCommit repository names and their URLs, run the git remote \-v command\.\)

For more options, see your Git documentation\.

## View branch details \(AWS CLI\)<a name="how-to-view-branch-details-cli"></a>

To use AWS CLI commands with CodeCommit, install the AWS CLI\. For more information, see [Command line reference](cmd-ref.md)\. 

To use the AWS CLI to view details about the branches in a CodeCommit repository, run one or more of the following commands:
+ To view a list of branch names, run [list\-branches](#how-to-view-branch-details-cli)\.
+ To view information about a specific branch, run [get\-branch](#how-to-view-branch-details-cli-details)\.

### To view a list of branch names<a name="how-to-view-branch-details-cli-list"></a>

1. Run the list\-branches command, specifying the name of the CodeCommit repository \(with the `--repository-name` option\)\.
**Tip**  
To get the name of the CodeCommit repository, run the [list\-repositories](how-to-view-repository-details.md#how-to-view-repository-details-no-name-cli) command\.

   For example, to view details about the branches in a CodeCommit repository named `MyDemoRepo`:

   ```
   aws codecommit list-branches --repository-name MyDemoRepo
   ```

1. If successful, this command outputs a `branchNameList` object, with an entry for each branch\.

   Here is some example output based on the preceding example command:

   ```
   {
       "branches": [
           "MyNewBranch",
           "master"
       ]
   }
   ```

### To view information about a branch<a name="how-to-view-branch-details-cli-details"></a>

1. Run the get\-branch command, specifying:
   + The repository name \(with the \-\-repository\-name option\)\.
   + The branch name \(with the \-\-branch\-name option\)\.

   For example, to view information about a branch named `MyNewBranch` in a CodeCommit repository named `MyDemoRepo`:

   ```
   aws codecommit get-branch --repository-name MyDemoRepo --branch-name MyNewBranch
   ```

1. If successful, this command outputs the name of the branch and the ID of the last commit made to the branch\.

   Here is some example output based on the preceding example command:

   ```
   {
       "branch": {
             "branchName": "MyNewBranch",
             "commitID": "317f8570EXAMPLE"
       }
   }
   ```