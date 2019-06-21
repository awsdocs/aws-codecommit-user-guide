# AWS CodeCommit Command Line Reference<a name="cmd-ref"></a>

This reference helps you learn how to use the AWS CLI\.

**To install and configure the AWS CLI**

1. On your local machine, download and install the AWS CLI\. This is a prerequisite for interacting with CodeCommit from the command line\. For more information, see [Getting Set Up with the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-set-up.html)\.
**Note**  
CodeCommit works only with AWS CLI versions 1\.7\.38 and later\. To determine which version of the AWS CLI you have installed, run the aws \-\-version command\.  
To upgrade an older version of the AWS CLI to the latest version, see [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html)\.

1.  Run this command to verify the CodeCommit commands for the AWS CLI are installed:

   ```
   aws codecommit help
   ```

   This command should return a list of CodeCommit commands\.

1. Configure the AWS CLI with the configure command, as follows:

   ```
   aws configure
   ```

   When prompted, specify the AWS access key and AWS secret access key of the IAM user to use with CodeCommit\. Also, be sure to specify the AWS Region where the repository exists, such as `us-east-2`\. When prompted for the default output format, specify `json`\. For example:

   ```
   AWS Access Key ID [None]: Type your target AWS access key ID here, and then press Enter
   AWS Secret Access Key [None]: Type your target AWS secret access key here, and then press Enter
   Default region name [None]: Type a supported region for CodeCommit here, and then press Enter
   Default output format [None]: Type json here, and then press Enter
   ```

   To connect to a repository or a resource in another AWS Region, you must reconfigure the AWS CLI with the default Region name\. Supported default Region names for CodeCommit include:
   + us\-east\-2
   + us\-east\-1
   + eu\-west\-1
   + us\-west\-2
   + ap\-northeast\-1
   + ap\-southeast\-1
   + ap\-southeast\-2
   + eu\-central\-1
   + ap\-northeast\-2
   + sa\-east\-1
   + us\-west\-1
   + eu\-west\-2
   + ap\-south\-1
   + ca\-central\-1
   + us\-gov\-west\-1
   + us\-gov\-east\-1

   For more information about CodeCommit and AWS Regions, see [Regions and Git Connection Endpoints](regions.md)\. For more information about IAM, access keys, and secret keys, see [How Do I Get Credentials?](https://docs.aws.amazon.com/IAM/latest/UserGuide/IAM_Introduction.html#IAM_SecurityCredentials) and [Managing Access Keys for IAM Users](https://docs.aws.amazon.com/IAM/latest/UserGuide/ManagingCredentials.html)\.

To view a list of all available CodeCommit commands, run the following command:

```
aws codecommit help
```

To view information about a specific CodeCommit command, run the following command, where *command\-name* is the name of the command \(for example, create\-repository\):

```
aws codecommit command-name help
```

See the following sections to view descriptions and example usage of the commands in the AWS CLI:
+ [batch\-describe\-merge\-conflicts](how-to-resolve-conflict-pull-request.md#batch-describe-merge-conflicts)
+ [batch\-get\-repositories](how-to-view-repository-details.md#how-to-view-repository-details-with-names-cli)
+ [create\-branch](how-to-create-branch.md#how-to-create-branch-cli)
+ [create\-commit](how-to-create-commit.md#how-to-create-commit-cli)
+ [create\-pull\-request](how-to-create-pull-request.md#how-to-create-pull-request-cli)
+ [create\-repository](how-to-create-repository.md#how-to-create-repository-cli)
+ [create\-unreferenced\-merge\-commit](how-to-resolve-conflict-pull-request.md#create-unreferenced-merge-commit)
+ [delete\-branch](how-to-delete-branch.md#how-to-delete-branch-cli)
+ [delete\-comment\-content](how-to-commit-comment.md#how-to-commit-comment-cli-commit-delete)
+ [delete\-file](how-to-edit-file.md#how-to-edit-file-cli)
+ [delete\-repository](how-to-delete-repository.md#how-to-delete-repository-cli)
+ [describe\-merge\-conflicts](how-to-resolve-conflict-pull-request.md#describe-merge-conflicts)
+ [describe\-pull\-request\-events](how-to-view-pull-request.md#describe-pull-request-events)
+ [get\-blob](how-to-view-commit-details.md#how-to-view-commit-details-cli-blob)
+ [get\-branch](how-to-view-branch-details.md#how-to-view-branch-details-cli-details)
+ [get\-comment](how-to-commit-comment.md#how-to-commit-comment-cli-get-comment-info)
+ [get\-comments\-for\-compared\-commit](how-to-commit-comment.md#how-to-commit-comment-cli-get-comments)
+ [get\-comments\-for\-pull\-request](how-to-review-pull-request.md#get-comments-for-pull-request)
+ [get\-commit](how-to-view-commit-details.md#how-to-view-commit-details-cli-commit)
+ [get\-differences](how-to-view-commit-details.md#how-to-view-commit-details-cli-differences)
+ [get\-merge\-conflicts](how-to-view-pull-request.md#get-merge-conflicts)
+ [get\-pull\-request](how-to-view-pull-request.md#get-pull-request)
+ [get\-repository](how-to-view-repository-details.md#how-to-view-repository-details-with-name-cli)
+ [get\-repository\-triggers](how-to-notify-edit.md#how-to-notify-edit-cli)
+ [list\-branches](how-to-view-branch-details.md#how-to-view-branch-details-cli)
+ [list\-pull\-requests](how-to-view-pull-request.md#list-pull-requests)
+ [list\-repositories](how-to-view-repository-details.md#how-to-view-repository-details-no-name-cli)
+ [list\-tags\-for\-resource](how-to-tag-repository.md#how-to-tag-repository-list)
+ [merge\-pull\-request\-by\-fast\-forward](how-to-merge-pull-request.md#merge-pull-request-by-fast-forward)
+ [post\-comment\-for\-compared\-commit](how-to-commit-comment.md#how-to-commit-comment-cli-comment)
+ [post\-comment\-for\-pull\-request](how-to-review-pull-request.md#post-comment-for-pull-request)
+ [post\-comment\-reply](how-to-commit-comment.md#how-to-commit-comment-cli-commit-reply)
+ [put\-file](how-to-create-file.md#how-to-create-file-cli)
+ [put\-repository\-triggers](how-to-notify-edit.md#how-to-notify-edit-cli)
+ [tag\-resource](how-to-tag-repository.md#how-to-tag-repository-add)
+ [test\-repository\-triggers](how-to-notify-test.md#how-to-notify-test-cli)
+ [untag\-resource](how-to-tag-repository.md#how-to-tag-repository-delete)
+ [update\-comment](how-to-commit-comment.md#how-to-commit-comment-cli-commit-update)
+ [update\-default\-branch](how-to-change-branch.md#how-to-change-branch-cli-default)
+ [update\-pull\-request\-description](how-to-update-pull-request.md#update-pull-request-description)
+ [update\-pull\-request\-status](how-to-close-pull-request.md#update-pull-request-status)
+ [update\-pull\-request\-title](how-to-update-pull-request.md#update-pull-request-title)
+ [update\-repository\-description](how-to-change-repository.md#how-to-change-repository-cli-description)
+ [update\-repository\-name](how-to-change-repository.md#how-to-change-repository-cli-name)