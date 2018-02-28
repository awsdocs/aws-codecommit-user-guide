# Delete an AWS CodeCommit Repository<a name="how-to-delete-repository"></a>

To delete a local repo, use your local machine's directory and file management tools\. To delete an AWS CodeCommit repository, use the AWS CLI or the AWS CodeCommit console\.


+ [Use the AWS CodeCommit Console to Delete a Repository](#how-to-delete-repository-console)
+ [Delete a Local Repo](#how-to-delete-repository-git)
+ [Use the AWS CLI to Delete an AWS CodeCommit Repository](#how-to-delete-repository-cli)

## Use the AWS CodeCommit Console to Delete a Repository<a name="how-to-delete-repository-console"></a>

To use the AWS CodeCommit console to delete an AWS CodeCommit repository, follow these steps\.

**Important**  
After you delete an AWS CodeCommit repository, you will no longer be able to clone it to any local repo or shared repo\. You will also no longer be able to pull data from it, or push data to it, from any local repo or shared repo\. This action cannot be undone\.

1. Open the AWS CodeCommit console at [https://console\.aws\.amazon\.com/codecommit](https://console.aws.amazon.com/codecommit)\.

1. In the list of repositories, choose the name of the repository you want to delete\. 

1. In the navigation pane, choose **Settings**\.

1. In **General**, choose **Delete repository**\. In the box next to **Type the name of the repository to confirm deletion**, type the repository's name, and then choose **Delete**\. The repository is permanently deleted\.

## Delete a Local Repo<a name="how-to-delete-repository-git"></a>

Use your local machine's directory and file management tools to delete the directory that contains the local repo\.

Deleting a local repo does not delete any AWS CodeCommit repository to which it might be connected\. 

## Use the AWS CLI to Delete an AWS CodeCommit Repository<a name="how-to-delete-repository-cli"></a>

To use AWS CLI commands with AWS CodeCommit, install the AWS CLI\. For more information, see [Command Line Reference](cmd-ref.md)\. 

To use the AWS CLI to delete an AWS CodeCommit repository, run the delete\-repository command, specifying the name of the AWS CodeCommit repository to delete \(with the `--repository-name` option\)\.

**Important**  
After you delete an AWS CodeCommit repository, you will no longer be able to clone it to any local repo or shared repo\. You will also no longer be able to pull data from it, or push data to it, from any local repo or shared repo\. This action cannot be undone\.

**Tip**  
To get the AWS CodeCommit repository's name, run the [list\-repositories](how-to-view-repository-details.md#how-to-view-repository-details-no-name-cli) command\.

For example, to delete a repository named `MyDemoRepo`:

For Linux, macOS, or Unix:

```
aws codecommit delete-repository \
  --repository-name MyDemoRepo
```

For Windows:

```
aws codecommit delete-repository --repository-name MyDemoRepo
```

If successful, the ID of the AWS CodeCommit repository that was permanently deleted will appear in the output:

```
{
    "repositoryId": "f7579e13-b83e-4027-aaef-650c0EXAMPLE"
}
```

Deleting an AWS CodeCommit repository does not delete any local repos that may be connected to it\. 