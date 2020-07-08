# Change AWS CodeCommit repository settings<a name="how-to-change-repository"></a>

You can use the AWS CLI and the AWS CodeCommit console to change the settings of an CodeCommit repository, such as its description or name\.

**Important**  
Changing a repository's name may break any local repos that use the old name in their remote URL\. Run the git remote set\-url command to update the remote URL to use the new repository's name\.

**Topics**
+ [Change repository settings \(console\)](#how-to-change-repository-console)
+ [Change AWS CodeCommit repository settings \(AWS CLI\)](#how-to-change-repository-cli)

## Change repository settings \(console\)<a name="how-to-change-repository-console"></a>

To use the AWS CodeCommit console to change a CodeCommit repository's settings in AWS CodeCommit, follow these steps\.

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In **Repositories**, choose the name of the repository where you want to change settings\. 

1. In the navigation pane, choose **Settings**\. 

1. To change the name of the repository, in **Repository name**, enter a new name in the **Name** text box and choose **Save**\. When prompted, verify your choice\. 
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

1. To change the repository's description, modify the text in the **Description** text box, and then choose **Save**\.
**Note**  
The description field displays Markdown in the console and accepts all HTML characters and valid Unicode characters\. If you are an application developer who is using the `GetRepository` or `BatchGetRepositories` APIs and you plan to display the repository description field in a web browser, see the [CodeCommit API Reference](https://docs.aws.amazon.com/codecommit/latest/APIReference/)\.

1. To change the default branch, in **Default branch**, choose the branch drop\-down list and choose a different branch\. Choose **Save**\.

1. To delete the repository, choose **Delete repository**\. In the box next to **Type the name of the repository to confirm deletion**, enter **delete**, and then choose **Delete**\.
**Important**  
After you delete this repository in AWS CodeCommit, you will no longer be able to clone it to any local repo or shared repo\. You will also no longer be able to pull data from it, or push data to it, from any local repo or shared repo\. This action cannot be undone\.

## Change AWS CodeCommit repository settings \(AWS CLI\)<a name="how-to-change-repository-cli"></a>

To use AWS CLI commands with CodeCommit, install the AWS CLI\. For more information, see [Command line reference](cmd-ref.md)\. 

To use AWS CLI to change a CodeCommit repository's settings in AWS CodeCommit, run one or more of the following commands:
+ [update\-repository\-description](#how-to-change-repository-cli-description) to change the description of an CodeCommit repository\.
+ [update\-repository\-name](#how-to-change-repository-cli-name) to change the name of an CodeCommit repository\.

### To change a CodeCommit repository's description<a name="how-to-change-repository-cli-description"></a>

1. Run the update\-repository\-description command, specifying:
   +  The name of the CodeCommit repository \(with the `--repository-name` option\)\.
**Tip**  
To get the name of the CodeCommit repository, run the [list\-repositories](how-to-view-repository-details.md#how-to-view-repository-details-no-name-cli) command\.
   + The new repository description \(with the `--repository-description` option\)\.
**Note**  
The description field displays Markdown in the console and accepts all HTML characters and valid Unicode characters\. If you are an application developer who is using the `GetRepository` or `BatchGetRepositories` APIs and you plan to display the repository description field in a web browser, see the [CodeCommit API Reference](https://docs.aws.amazon.com/codecommit/latest/APIReference/)\.

   For example, to change the description for the CodeCommit repository named `MyDemoRepo` to `This description was changed`:

   ```
   aws codecommit update-repository-description --repository-name MyDemoRepo --repository-description "This description was changed"
   ```

   This command produces output only if there are errors\.

1. To verify the changed description, run the get\-repository command, specifying the name of the CodeCommit repository whose description you changed with the `--repository-name` option\.

   The output of the command shows the changed text in `repositoryDescription`\.

### To change a CodeCommit repository's name<a name="how-to-change-repository-cli-name"></a>

1. Run the update\-repository\-name command, specifying:
   + The current name of the CodeCommit repository \(with the `--old-name` option\)\.
**Tip**  
To get the CodeCommit repository's name, run the [list\-repositories](how-to-view-repository-details.md#how-to-view-repository-details-no-name-cli) command\.
   + The new name of the CodeCommit repository \(with the `--new-name` option\)\. 

   For example, to change the repository named `MyDemoRepo` to `MyRenamedDemoRepo`:

   ```
   aws codecommit update-repository-name --old-name MyDemoRepo --new-name MyRenamedDemoRepo
   ```

   This command produces output only if there are errors\.
**Important**  
Changing the name of the AWS CodeCommit repository changes the SSH and HTTPS URLs that users need to connect to the repository\. Users cannot connect to this repository until they update their connection settings\. Also, because the repository's ARN changes, changing the repository name invalidates any IAM user policies that rely on this repository's ARN\.

1. To verify the changed name, run the list\-repositories command and review the list of repository names\.