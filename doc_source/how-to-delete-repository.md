--------

 The procedures in this guide support the new console design\. If you choose to use the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\.

--------

# Delete an AWS CodeCommit Repository<a name="how-to-delete-repository"></a>

You can use the AWS CodeCommit console or the AWS CLI to delete an AWS CodeCommit repository\.

**Note**  
Deleting a repository does not delete any local copies of that repository \(local repos\)\. To delete a local repo, use your local machine's directory and file management tools\.

**Topics**
+ [Delete a Repository \(Console\)](#how-to-delete-repository-console)
+ [Delete a Local Repo](#how-to-delete-repository-git)
+ [Delete an AWS CodeCommit Repository \(AWS CLI\)](#how-to-delete-repository-cli)

## Delete a Repository \(Console\)<a name="how-to-delete-repository-console"></a>

Follow these steps to use the AWS CodeCommit console to delete an AWS CodeCommit repository\.

**Important**  
After you delete an AWS CodeCommit repository, you are no longer able to clone it to any local repo or shared repo\. You are also no longer able to pull data from it, or push data to it, from any local repo or shared repo\. This action cannot be undone\.

1. Open the AWS CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In **Repositories**, choose the name of the repository you want to delete\. 

1. In the navigation pane, choose **Settings**\.

1. On the **Repository settings** page, in **Delete repository**, choose **Delete repository**\. Enter **delete**, and then choose **Delete**\. The repository is permanently deleted\.
**Note**  
Deleting the repository in AWS CodeCommit does not delete any local repos\. 

## Delete a Local Repo<a name="how-to-delete-repository-git"></a>

Use your local machine's directory and file management tools to delete the directory that contains the local repo\.

Deleting a local repo does not delete any AWS CodeCommit repository to which it might be connected\. 

## Delete an AWS CodeCommit Repository \(AWS CLI\)<a name="how-to-delete-repository-cli"></a>

To use AWS CLI commands with AWS CodeCommit, install the AWS CLI\. For more information, see [Command Line Reference](cmd-ref.md)\. 

To use the AWS CLI to delete an AWS CodeCommit repository, run the delete\-repository command, specifying the name of the AWS CodeCommit repository to delete \(with the `--repository-name` option\)\.

**Important**  
After you delete an AWS CodeCommit repository, you are no longer able to clone it to any local repo or shared repo\. You are also no longer able to pull data from it, or push data to it, from any local repo or shared repo\. This action cannot be undone\.

**Tip**  
To get the name of the AWS CodeCommit repository, run the [list\-repositories](how-to-view-repository-details.md#how-to-view-repository-details-no-name-cli) command\.

For example, to delete a repository named `MyDemoRepo`:

```
aws codecommit delete-repository --repository-name MyDemoRepo
```

If successful, the ID of the AWS CodeCommit repository that was permanently deleted appears in the output:

```
{
    "repositoryId": "f7579e13-b83e-4027-aaef-650c0EXAMPLE"
}
```

Deleting an AWS CodeCommit repository does not delete any local repos that might be connected to it\. 