# Delete an AWS CodeCommit repository<a name="how-to-delete-repository"></a>

You can use the CodeCommit console or the AWS CLI to delete a CodeCommit repository\.

**Note**  
Deleting a repository does not delete any local copies of that repository \(local repos\)\. To delete a local repo, use your local machine's directory and file management tools\.

**Topics**
+ [Delete a CodeCommit repository \(console\)](#how-to-delete-repository-console)
+ [Delete a local repo](#how-to-delete-repository-git)
+ [Delete a CodeCommit repository \(AWS CLI\)](#how-to-delete-repository-cli)

## Delete a CodeCommit repository \(console\)<a name="how-to-delete-repository-console"></a>

Follow these steps to use the CodeCommit console to delete a CodeCommit repository\.

**Important**  
After you delete a CodeCommit repository, you are no longer able to clone it to any local repo or shared repo\. You are also no longer able to pull data from it, or push data to it, from any local repo or shared repo\. This action cannot be undone\.

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In **Repositories**, choose the name of the repository you want to delete\. 

1. In the navigation pane, choose **Settings**\.

1. On the **General** tab, in **Delete repository**, choose **Delete repository**\. Enter **delete**, and then choose **Delete**\. The repository is permanently deleted\.
**Note**  
Deleting the repository in CodeCommit does not delete any local repos\. 

## Delete a local repo<a name="how-to-delete-repository-git"></a>

Use your local machine's directory and file management tools to delete the directory that contains the local repo\.

Deleting a local repo does not delete any CodeCommit repository to which it might be connected\. 

## Delete a CodeCommit repository \(AWS CLI\)<a name="how-to-delete-repository-cli"></a>

To use AWS CLI commands with CodeCommit, install the AWS CLI\. For more information, see [Command line reference](cmd-ref.md)\. 

To use the AWS CLI to delete a CodeCommit repository, run the delete\-repository command, specifying the name of the CodeCommit repository to delete \(with the `--repository-name` option\)\.

**Important**  
After you delete a CodeCommit repository, you are no longer able to clone it to any local repo or shared repo\. You are also no longer able to pull data from it, or push data to it, from any local repo or shared repo\. This action cannot be undone\.

**Tip**  
To get the name of the CodeCommit repository, run the [list\-repositories](how-to-view-repository-details.md#how-to-view-repository-details-no-name-cli) command\.

For example, to delete a repository named `MyDemoRepo`:

```
aws codecommit delete-repository --repository-name MyDemoRepo
```

If successful, the ID of the CodeCommit repository that was permanently deleted appears in the output:

```
{
    "repositoryId": "f7579e13-b83e-4027-aaef-650c0EXAMPLE"
}
```

Deleting a CodeCommit repository does not delete any local repos that might be connected to it\. 