# Change branch settings in AWS CodeCommit<a name="how-to-change-branch"></a>

You can change which branch to use as the default branch in the AWS CodeCommit console or with the AWS CLI\. For example, if you created your first commit using a Git client that set the default branch to *master*, you could create a branch named *main*, and then change the branch settings so that the new branch is set as the default branch for the repository\. To change other branch settings, you can use Git from a local repo connected to the CodeCommit repository\. 

**Topics**
+ [Change the default branch \(console\)](#how-to-change-branch-console)
+ [Change the default branch \(AWS CLI\)](#how-to-change-branch-cli)

## Change the default branch \(console\)<a name="how-to-change-branch-console"></a>

You can specify which branch is the default branch in a CodeCommit repository in the AWS CodeCommit console\. 

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In **Repositories**, choose the name of the repository where you want to change settings\. 

1. In the navigation pane, choose **Settings**\.

1. In **Default branch**, choose the branch drop\-down list and choose a different branch\. Choose **Save**\.
**Tip**  
If you do not see another branch in the drop\-down list, you have not created any additional branches\. You cannot change the default branch of a repository if the repository has only one branch\. For more information, see [Create a branch in AWS CodeCommit](how-to-create-branch.md)\.
If you do not see the **Default branch** section but instead see items for notification rules and connections, you are in the general settings menu for the console\. The settings menu for repositories is listed under **Repositories** at the same level as **Code** and **Pull requests**\.

## Change the default branch \(AWS CLI\)<a name="how-to-change-branch-cli"></a>

To use AWS CLI commands with CodeCommit, install the AWS CLI\. For more information, see [Command line reference](cmd-ref.md)\. 

To use the AWS CLI to change a repository's branch settings in a CodeCommit repository, run the following command:
+ [update\-default\-branch](#how-to-change-branch-cli-default) to change the default branch\.

### To change the default branch<a name="how-to-change-branch-cli-default"></a>

1. Run the update\-default\-branch command, specifying:
   + The name of the CodeCommit repository where the default branch is updated \(with the \-\-repository\-name option\)\.
**Tip**  
To get the name of the CodeCommit repository, run the [list\-repositories](how-to-view-repository-details.md#how-to-view-repository-details-no-name-cli) command\.
   + The name of the new default branch \(with the \-\-default\-branch\-name option\)\.
**Tip**  
To get the name of the branch, run the [list\-branches](how-to-view-branch-details.md#how-to-view-branch-details-cli) command\.

1. For example, to change the default branch to `MyNewBranch` in a CodeCommit repository named `MyDemoRepo`:

   ```
   aws codecommit update-default-branch --repository-name MyDemoRepo --default-branch-name MyNewBranch
   ```

   This command produces output only if there are errors\.

For more options, see your Git documentation\.