# AWS CodeCommit command line reference<a name="cmd-ref"></a>

This reference helps you learn how to use the AWS CLI\.

**To install and configure the AWS CLI**

1. On your local machine, download and install the AWS CLI\. This is a prerequisite for interacting with CodeCommit from the command line\. For more information, see [Getting Set Up with the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-set-up.html)\.
**Note**  
CodeCommit works only with AWS CLI versions 1\.7\.38 and later\. As a best practice, install or upgrade the AWS CLI to the latest version available\. To determine which version of the AWS CLI you have installed, run the aws \-\-version command\.  
To upgrade an older version of the AWS CLI to the latest version, see [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html)\.

1.  Run this command to verify the CodeCommit commands for the AWS CLI are installed:

   ```
   aws codecommit help
   ```

   This command should return a list of CodeCommit commands\.

1. Configure the AWS CLI with a profile by using the configure command, as follows:

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

   For more information about creating and configuring profiles to use with the AWS CLI, see the following:
   + [Named Profiles](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-profiles.html)
   + [Using an IAM Role in the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-role.html)
   + [Set command](https://docs.aws.amazon.com/cli/latest/reference/set.html)
   + [Connecting to AWS CodeCommit repositories with rotating credentials](temporary-access.md)

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
   + eu\-north\-1
   + ap\-east\-1
   + me\-south\-1
   + cn\-north\-1
   + cn\-northwest\-1

   For more information about CodeCommit and AWS Regions, see [Regions and Git connection endpoints](regions.md)\. For more information about IAM, access keys, and secret keys, see [How Do I Get Credentials?](https://docs.aws.amazon.com/IAM/latest/UserGuide/IAM_Introduction.html#IAM_SecurityCredentials) and [Managing Access Keys for IAM Users](https://docs.aws.amazon.com/IAM/latest/UserGuide/ManagingCredentials.html)\. For more information about the AWS CLI and profiles, see [Named Profiles](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-profiles.html)\.

To view a list of all available CodeCommit commands, run the following command:

```
aws codecommit help
```

To view information about a CodeCommit command, run the following command, where *command\-name* is the name of the command \(for example, create\-repository\):

```
aws codecommit command-name help
```

See the following for descriptions and example usage of the commands in the AWS CLI:
+ [associate\-approval\-rule\-template\-with\-repository](how-to-associate-template.md#associate-template-repository)
+ [batch\-associate\-approval\-rule\-template\-with\-repositories](how-to-associate-template.md#batch-associate-template-repositories)
+ [batch\-disassociate\-approval\-rule\-template\-from\-repositories](how-to-disassociate-template.md#batch-disassociate-template)
+ [batch\-describe\-merge\-conflicts](how-to-resolve-conflict-pull-request.md#batch-describe-merge-conflicts)
+ [batch\-get\-commits](how-to-view-commit-details.md#how-to-view-commit-details-cli-batch-get-commits)
+ [batch\-get\-repositories](how-to-view-repository-details.md#how-to-view-repository-details-with-names-cli)
+ [create\-approval\-rule\-template](how-to-create-template.md#create-template-cli)
+ [create\-branch](how-to-create-branch.md#how-to-create-branch-cli)
+ [create\-commit](how-to-create-commit.md#how-to-create-commit-cli)
+ [create\-pull\-request](how-to-create-pull-request.md#how-to-create-pull-request-cli)
+ [create\-pull\-request\-approval\-rule](how-to-create-pull-request-approval-rule.md#how-to-create-pull-request-approval-rule-cli)
+ [create\-repository](how-to-create-repository.md#how-to-create-repository-cli)
+ [create\-unreferenced\-merge\-commit](how-to-resolve-conflict-pull-request.md#create-unreferenced-merge-commit)
+ [delete\-approval\-rule\-template](how-to-delete-template.md#delete-template)
+ [delete\-branch](how-to-delete-branch.md#how-to-delete-branch-cli)
+ [delete\-comment\-content](how-to-commit-comment.md#how-to-commit-comment-cli-commit-delete)
+ [delete\-file](how-to-edit-file.md#how-to-edit-file-cli)
+ [delete\-repository](how-to-delete-repository.md#how-to-delete-repository-cli)
+ [describe\-merge\-conflicts](how-to-resolve-conflict-pull-request.md#describe-merge-conflicts)
+ [delete\-pull\-request\-approval\-rule](how-to-edit-delete-pull-request-approval-rule.md#delete-pull-request-approval-rule)
+ [describe\-pull\-request\-events](how-to-view-pull-request.md#describe-pull-request-events)
+ [disassociate\-pull\-request\-approval\-rule\-template\-from\-repository](how-to-disassociate-template.md#disassociate-template)
+ [evaluate\-pull\-request\-approval\-rules](how-to-merge-pull-request.md#evaluate-pull-request-approval-rules)
+ [get\-approval\-rule\-template](how-to-manage-templates.md#get-template)
+ [get\-blob](how-to-view-commit-details.md#how-to-view-commit-details-cli-blob)
+ [get\-branch](how-to-view-branch-details.md#how-to-view-branch-details-cli-details)
+ [get\-comment](how-to-commit-comment.md#how-to-commit-comment-cli-get-comment-info)
+ [get\-comment\-reactions](how-to-commit-comment.md#how-to-commit-comment-cli-commit-emoji-view)
+ [get\-comments\-for\-compared\-commit](how-to-commit-comment.md#how-to-commit-comment-cli-get-comments)
+ [get\-comments\-for\-pull\-request](how-to-review-pull-request.md#get-comments-for-pull-request)
+ [get\-commit](how-to-view-commit-details.md#how-to-view-commit-details-cli-commit)
+ [get\-differences](how-to-view-commit-details.md#how-to-view-commit-details-cli-differences)
+ [get\-merge\-commit](how-to-view-commit-details.md#how-to-view-commit-details-cli-merge-commit)
+ [get\-merge\-conflicts](how-to-view-pull-request.md#get-merge-conflicts)
+ [get\-merge\-options](how-to-resolve-conflict-pull-request.md#get-merge-options)
+ [get\-pull\-request](how-to-view-pull-request.md#get-pull-request)
+ [get\-pull\-request\-approval\-states](how-to-view-pull-request.md#get-pull-request-approval-state)
+ [get\-pull\-request\-override\-state](how-to-override-approval-rules.md#get-override-status)
+ [get\-repository](how-to-view-repository-details.md#how-to-view-repository-details-with-name-cli)
+ [get\-repository\-triggers](how-to-notify-edit.md#how-to-notify-edit-cli)
+ [list\-approval\-rule\-templates](how-to-manage-templates.md#list-templates)
+ [list\-associated\-approval\-rule\-templates\-for\-repository](how-to-manage-templates.md#list-associated-templates)
+ [list\-branches](how-to-view-branch-details.md#how-to-view-branch-details-cli)
+ [list\-pull\-requests](how-to-view-pull-request.md#list-pull-requests)
+ [list\-repositories](how-to-view-repository-details.md#how-to-view-repository-details-no-name-cli)
+ [list\-repositories\-for\-approval\-rule\-template](how-to-manage-templates.md#list-associated-repositories)
+ [list\-tags\-for\-resource](how-to-tag-repository-list.md)
+ [merge\-branches\-by\-fast\-forward](how-to-compare-branches.md#merge-branches-by-fast-forward)
+ [merge\-branches\-by\-squash](how-to-compare-branches.md#merge-branches-by-squash)
+ [merge\-branches\-by\-three\-way](how-to-compare-branches.md#merge-branches-by-three-way)
+ [merge\-pull\-request\-by\-fast\-forward](how-to-merge-pull-request.md#merge-pull-request-by-fast-forward)
+ [merge\-pull\-request\-by\-squash](how-to-merge-pull-request.md#merge-pull-request-by-squash)
+ [merge\-pull\-request\-by\-three\-way](how-to-merge-pull-request.md#merge-pull-request-by-three-way)
+ [override\-pull\-request\-approval\-rules](how-to-override-approval-rules.md#override-approval-rules)
+ [post\-comment\-for\-compared\-commit](how-to-commit-comment.md#how-to-commit-comment-cli-comment)
+ [post\-comment\-for\-pull\-request](how-to-review-pull-request.md#post-comment-for-pull-request)
+ [post\-comment\-reply](how-to-commit-comment.md#how-to-commit-comment-cli-commit-reply)
+ [put\-comment\-reaction](how-to-commit-comment.md#how-to-commit-comment-cli-commit-reply-emoji)
+ [put\-file](how-to-create-file.md#how-to-create-file-cli)
+ [put\-repository\-triggers](how-to-notify-edit.md#how-to-notify-edit-cli)
+ [tag\-resource](how-to-tag-repository-add.md)
+ [test\-repository\-triggers](how-to-notify-test.md#how-to-notify-test-cli)
+ [untag\-resource](how-to-tag-repository-delete.md)
+ [update\-approval\-rule\-template\-content](how-to-manage-templates.md#update-template-content)
+ [update\-approval\-rule\-template\-description](how-to-manage-templates.md#update-template-description)
+ [update\-approval\-rule\-template\-name](how-to-manage-templates.md#update-template-name)
+ [update\-comment](how-to-commit-comment.md#how-to-commit-comment-cli-commit-update)
+ [update\-default\-branch](how-to-change-branch.md#how-to-change-branch-cli-default)
+ [update\-pull\-request\-approval\-rule\-content](how-to-edit-delete-pull-request-approval-rule.md#update-pull-request-approval-rule-content)
+ [update\-pull\-request\-approval\-state](how-to-review-pull-request.md#update-pull-request-approval-state)
+ [update\-pull\-request\-description](how-to-update-pull-request.md#update-pull-request-description)
+ [update\-pull\-request\-status](how-to-close-pull-request.md#update-pull-request-status)
+ [update\-pull\-request\-title](how-to-update-pull-request.md#update-pull-request-title)
+ [update\-repository\-description](how-to-change-repository.md#how-to-change-repository-cli-description)
+ [update\-repository\-name](how-to-change-repository.md#how-to-change-repository-cli-name)