# Change AWS CodeCommit Repository Settings<a name="how-to-change-repository"></a>

To change the settings of an AWS CodeCommit repository, such as its description or name, you can use the AWS CLI and the AWS CodeCommit console\.

**Important**  
Changing a repository's name may break any local repos that use the old name in their remote URL\. Run the git remote set\-url command to update the remote URL to use the new repository's name\.

**Topics**
+ [Use the AWS CodeCommit Console to Change Repository Settings](#how-to-change-repository-console)
+ [Use the AWS CLI to Change AWS CodeCommit Repository Settings](#how-to-change-repository-cli)

## Use the AWS CodeCommit Console to Change Repository Settings<a name="how-to-change-repository-console"></a>

To use the AWS CodeCommit console to change an AWS CodeCommit repository's settings in AWS CodeCommit, follow these steps\.

1. Open the AWS CodeCommit console at [https://console\.aws\.amazon\.com/codecommit](https://console.aws.amazon.com/codecommit)\.

1. In the list of repositories, choose the name of the repository where you want to change settings\. 

1. In the navigation pane, choose **Settings**\. 

1. To change the name of the repository, in the **General** tab, in **Repository name**, type a new name in the **Name** text box, choose **Change name**, and then choose **Change name** again\.
**Important**  
Changing the name of the AWS CodeCommit repository will change the SSH and HTTPS URLs that users need to connect to the repository\. Users will not be able to connect to this repository until they update their connection settings\. Also, because the repository's ARN will change, changing the repository name will invalidate any IAM user policies that rely on this repository's ARN\.  
To connect to the repository after the name is changed, each user must use the git remote set\-url command and specify the new URL to use\. For example, if you changed the name of the repository from MyDemoRepo to MyRenamedDemoRepo, users who use HTTPS to connect to the repository would run the following Git command:  

   ```
   git remote set-url origin https://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyRenamedDemoRepo
   ```
Users who use SSH to connect to the repository would run the following Git command:  

   ```
   git remote set-url origin ssh://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyRenamedDemoRepo
   ```
For more options, see your Git documentation\.

1. To change the repository's description, modify the text in the **Description** text box, and then choose **Save changes**\.
**Note**  
The description field accepts all HTML characters and all valid Unicode characters\. If you are an application developer using the `GetRepository` or `BatchGetRepositories` APIs and plan to display the repository description field in a web browser, see the [AWS CodeCommit API Reference](http://docs.aws.amazon.com/codecommit/latest/APIReference/)\.

1. To change the default branch, choose a different branch from the **Default branch** drop\-down list, choose **Save changes**, and then choose **Change default**\.

1. To delete the repository, choose **Delete repository**\. In the box next to **Type the name of the repository to confirm deletion**, type the repository's name, and then choose **Delete**\.
**Important**  
After you delete this repository in AWS CodeCommit, you will no longer be able to clone it to any local repo or shared repo\. You will also no longer be able to pull data from it, or push data to it, from any local repo or shared repo\. This action cannot be undone\.

## Use the AWS CLI to Change AWS CodeCommit Repository Settings<a name="how-to-change-repository-cli"></a>

To use AWS CLI commands with AWS CodeCommit, install the AWS CLI\. For more information, see [Command Line Reference](cmd-ref.md)\. 

To use AWS CLI to change an AWS CodeCommit repository's settings in AWS CodeCommit, run one or more of the following commands:
+ [update\-repository\-description](#how-to-change-repository-cli-description) to change an AWS CodeCommit repository's description\.
+ [update\-repository\-name](#how-to-change-repository-cli-name) to change an AWS CodeCommit repository's name\.

### To change an AWS CodeCommit repository's description<a name="how-to-change-repository-cli-description"></a>

1. Run the update\-repository\-description command, specifying:
   +  The name of the AWS CodeCommit repository \(with the `--repository-name` option\)\.
**Tip**  
To get the name of the AWS CodeCommit repository, run the [list\-repositories](how-to-view-repository-details.md#how-to-view-repository-details-no-name-cli) command\.
   + The new repository description \(with the `--repository-description` option\)\.
**Note**  
The description field accepts all HTML characters and all valid Unicode characters\. If you are an application developer using the `GetRepository` or `BatchGetRepositories` APIs and plan to display the repository description field in a web browser, see the [AWS CodeCommit API Reference](http://docs.aws.amazon.com/codecommit/latest/APIReference/)\.

   For example, to change the description for the AWS CodeCommit repository named `MyDemoRepo` to `This description was changed`:

   ```
   aws codecommit update-repository-description --repository-name MyDemoRepo --repository-description "This description was changed"
   ```

   This command produces output only if there are errors\.

1. To verify the changed description, run the get\-repository command, specifying the name of the AWS CodeCommit repository whose description you changed with the `--repository-name` option\.

   The output of the command will show the changed text in `repositoryDescription`\.

### To change an AWS CodeCommit repository's name<a name="how-to-change-repository-cli-name"></a>

1. Run the update\-repository\-name command, specifying:
   + The current name of the AWS CodeCommit repository \(with the `--old-name` option\)\.
**Tip**  
To get the AWS CodeCommit repository's name, run the [list\-repositories](how-to-view-repository-details.md#how-to-view-repository-details-no-name-cli) command\.
   + The new name of the AWS CodeCommit repository \(with the `--new-name` option\)\. 

   For example, to change the repository named `MyDemoRepo` to `MyRenamedDemoRepo`:

   ```
   aws codecommit update-repository-name --old-name MyDemoRepo --new-name MyRenamedDemoRepo
   ```

   This command produces output only if there are errors\.
**Important**  
Changing the name of the AWS CodeCommit repository will change the SSH and HTTPS URLs that users need to connect to the repository\. Users will not be able to connect to this repository until they update their connection settings\. Also, because the repository's ARN will change, changing the repository name will invalidate any IAM user policies that rely on this repository's ARN\.

1. To verify the changed name, run the list\-repositories command and review the list of repository names\.