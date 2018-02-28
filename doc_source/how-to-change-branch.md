# Change Branch Settings in AWS CodeCommit<a name="how-to-change-branch"></a>

You can change the default branch to use in the AWS CodeCommit console\. You can also use the AWS CLI to change the default branch for a repository\. To change other branch settings, you can use Git from a local repo connected to the AWS CodeCommit repository\. 


+ [Use the AWS CodeCommit Console to Change the Default Branch](#how-to-change-branch-console)
+ [Use the AWS CLI to Change Branch Settings](#how-to-change-branch-cli)

## Use the AWS CodeCommit Console to Change the Default Branch<a name="how-to-change-branch-console"></a>

You can specify which branch is the default branch in an AWS CodeCommit repository in the AWS CodeCommit console\. 

1. Open the AWS CodeCommit console at [https://console\.aws\.amazon\.com/codecommit](https://console.aws.amazon.com/codecommit)\.

1. In the list of repositories, choose the name of the repository where you want to change settings\. 

1. In the navigation pane, choose **Branches**\.

1. Choose **Change default branch**\. In the **Default branch** drop\-down list, choose a different branch, and then choose **Change**\.  
![\[Changing the default branch used in an AWS CodeCommit repository.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-branches-change.png)

## Use the AWS CLI to Change Branch Settings<a name="how-to-change-branch-cli"></a>

To use AWS CLI commands with AWS CodeCommit, install the AWS CLI\. For more information, see [Command Line Reference](cmd-ref.md)\. 

To use the AWS CLI to change a repository's branch settings in an AWS CodeCommit repository, run the following command:

+ [update\-default\-branch](#how-to-change-branch-cli-default) to change the default branch\.

### To change the default branch<a name="how-to-change-branch-cli-default"></a>

1. Run the update\-default\-branch command, specifying:

   + The name of the AWS CodeCommit repository where the default branch will be updated \(with the \-\-repository\-name option\)\.
**Tip**  
To get the name of the AWS CodeCommit repository, run the [list\-repositories](how-to-view-repository-details.md#how-to-view-repository-details-no-name-cli) command\.

   + The name of the new default branch \(with the \-\-default\-branch\-name option\)\.
**Tip**  
To get the name of the branch, run the [list\-branches](how-to-view-branch-details.md#how-to-view-branch-details-cli) command\.

1. For example, to change the default branch to `MyNewBranch` in an AWS CodeCommit repository named `MyDemoRepo`:

   ```
   aws codecommit update-default-branch --repository-name MyDemoRepo --default-branch-name MyNewBranch
   ```

   This command produces output only if there are errors\.

For more options, see your Git documentation\.