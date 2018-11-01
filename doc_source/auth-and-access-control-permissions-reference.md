--------

 The procedures in this guide support the new console design\. If you choose to use the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\.

--------

# AWS CodeCommit Permissions Reference<a name="auth-and-access-control-permissions-reference"></a>

The following tables list each AWS CodeCommit API operation, the corresponding actions for which you can grant permissions, and the format of the resource ARN to use for granting permissions\. The AWS CodeCommit APIs are grouped into tables based on the scope of the actions allowed by that API\. Refer to it when setting up [Access Control](auth-and-access-control.md#access-control) and writing permissions policies that you can attach to an IAM identity \(identity\-based policies\)\. 

When you create a permissions policy, you specify the actions in the policy's `Action` field\. You specify the resource value in the policy's `Resource` field as an ARN, with or without a wildcard character \(\*\)\. 

To express conditions in your AWS CodeCommit policies, use AWS\-wide condition keys\. For a complete list of AWS\-wide keys, see [Available Keys](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html#AvailableKeys) in the *IAM User Guide*\.

**Note**  
To specify an action, use the `codecommit:` prefix followed by the API operation name \(for example, `codecommit:GetRepository` or `codecommit:CreateRepository`\. 

**Using Wildcards **

To specify multiple actions or resources, use a wildcard character \(\*\) in your ARN\. For example, `codecommit:*` specifies all AWS CodeCommit actions and `codecommit:Get*` specifies all AWS CodeCommit actions that begin with the word `Get`\. The following example grants access to all repositories with names that begin with `MyDemo`\. 

```
arn:aws:codecommit:us-west-2:111111111111:MyDemo*
```

You can use wildcards only with the *repository\-name* resources listed in the following table\. You can't use wildcards with *region* or *account\-id* resources\. For more information about wildcards, see [IAM Identifiers](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_identifiers.html) in *IAM User Guide*\. 

**Topics**
+ [Required Permissions for Git Client Commands](#aa-git)
+ [Permissions for Actions on Branches](#aa-branches)
+ [Permissions for Actions on Pull Requests](#aa-pr)
+ [Permissions for Actions on Individual Files](#aa-files)
+ [Permissions for Actions on Comments](#aa-comments)
+ [Permissions for Actions on Committed Code](#aa-code)
+ [Permissions for Actions on Repositories](#aa-repositories)
+ [Permissions for Actions on Triggers](#aa-triggers)
+ [Permissions for Actions on AWS CodePipeline Integration](#aa-acp)

## Required Permissions for Git Client Commands<a name="aa-git"></a>

In AWS CodeCommit, the `GitPull` IAM policy permissions apply to any Git client command where data is retrieved from AWS CodeCommit, including git fetch, git clone, and so on\. Similarly, the `GitPush` IAM policy permissions apply to any Git client command where data is sent to AWS CodeCommit\. For example, if the `GitPush` IAM policy permission is set to `Allow`, a user can push the deletion of a branch using the Git protocol\. That push is unaffected by any permissions applied to the `DeleteBranch` operation for that IAM user\. The `DeleteBranch` permission applies to actions performed with the console, the AWS CLI, the SDKs, and the API, but not the Git protocol\. 

`GitPull` and `GitPush` are IAM policy permissions\. They are not API actions\.

If you see an expand arrow \(**↗**\) in the upper\-right corner of the table, you can open the table in a new window\. To close the window, choose the close button \(**X**\) in the lower\-right corner\.


**AWS CodeCommit Required Permissions for Actions for Git Client Commands**  

| AWS CodeCommit Permissions for Git | Required Permissions  | Resources | 
| --- | --- | --- | 
|  GitPull  |  `codecommit:GitPull` Required to pull information from an AWS CodeCommit repository to a local repo\. This is an IAM policy permission only, not an API action\.  |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 
|  GitPush  |  `codecommit:GitPush` Required to push information from a local repo to an AWS CodeCommit repository\. This is an IAM policy permission only, not an API action\.  If you create a policy that includes a context key and a `Deny` statement that includes this permission, you must also include a `Null` context\. For more information, see [Limit Pushes and Merges to Branches in AWS CodeCommit](how-to-conditional-branch.md)\.   |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 

## Permissions for Actions on Branches<a name="aa-branches"></a>

The following permissions allow or deny actions on branches in AWS CodeCommit repositories\. These permissions pertain only to actions performed in the AWS CodeCommit console and with the AWS CodeCommit API, and to commands performed using the AWS CLI\. They do not pertain to similar actions that can be performed using the Git protocol\. For example, the **git show\-branch \-r** command displays a list of remote branches for a repository and its commits using the Git protocol\. It's not affected by any permissions for the AWS CodeCommit `ListBranches` operation\. 

If you see an expand arrow \(**↗**\) in the upper\-right corner of the table, you can open the table in a new window\. To close the window, choose the close button \(**X**\) in the lower\-right corner\.


**AWS CodeCommit API Operations and Required Permissions for Actions on Branches**  

| AWS CodeCommit API Operations for Branches | Required Permissions \(API Actions\) | Resources | 
| --- | --- | --- | 
|  [CreateBranch](http://docs.aws.amazon.com/codecommit/latest/APIReference/API_CreateBranch.html)  |  `codecommit:CreateBranch` Required to create a branch in an AWS CodeCommit repository\.  |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 
|  [DeleteBranch](http://docs.aws.amazon.com/codecommit/latest/APIReference/API_DeleteBranch.html)  |  `codecommit:DeleteBranch` Required to delete a branch from an AWS CodeCommit repository\.  |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 
|  [GetBranch](http://docs.aws.amazon.com/codecommit/latest/APIReference/API_GetBranch.html)  |  `codecommit:GetBranch` Required to get details about a branch in an AWS CodeCommit repository\.  |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 
| [ListBranches](http://docs.aws.amazon.com/codecommit/latest/APIReference/API_ListBranches.html) |  `codecommit:ListBranches` Required to get a list of branches in an AWS CodeCommit repository\.  |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 
| [UpdateDefaultBranch](http://docs.aws.amazon.com/codecommit/latest/APIReference/API_UpdateDefaultBranch.html) | codecommit:UpdateDefaultBranchRequired to change the default branch in an AWS CodeCommit repository\. |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 

## Permissions for Actions on Pull Requests<a name="aa-pr"></a>

The following permissions allow or deny actions on pull requests in AWS CodeCommit repositories\. These permissions pertain to actions performed with the AWS CodeCommit console and the AWS CodeCommit API, and commands performed using the AWS CLI\. They do not pertain to similar actions that can be performed using the Git protocol\. For related permissions on comments, see [Permissions for Actions on Comments](#aa-comments)\.

If you see an expand arrow \(**↗**\) in the upper\-right corner of the table, you can open the table in a new window\. To close the window, choose the close button \(**X**\) in the lower\-right corner\.


**AWS CodeCommit API Operations and Required Permissions for Actions on Pull Requests**  

| AWS CodeCommit API Operations | Required Permissions \(API Actions\) | Resources | 
| --- | --- | --- | 
|  BatchGetPullRequests  |  `codecommit:BatchGetPullRequests` Required to return information about one or more pull requests in an AWS CodeCommit repository\. This is an IAM policy permission only, not an API action that you can call\.  |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 
|  [CreatePullRequest](http://docs.aws.amazon.com/codecommit/latest/APIReference/API_CreatePullRequest.html)  |  `codecommit:CreatePullRequest` Required to create a pull request in an AWS CodeCommit repository\.  |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 
| [DescribePullRequestEvents](http://docs.aws.amazon.com/codecommit/latest/APIReference/API_DescribePullRequestEvents.html) | Required to return information about one or more pull request events\. | arn:aws:codecommit:region:account\-id:repository\-name | 
|  [GetCommentsForPullRequest](http://docs.aws.amazon.com/codecommit/latest/APIReference/API_GetCommentsForPullRequest.html)  |  `codecommit:GetCommentsForPullRequest` Required to return comments made on a pull request\.  |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 
| GetCommitsFromMergeBase |  `codecommit:GetCommitsFromMergeBase` Required to return information about the difference between commits in the context of a potential merge\. This is an IAM policy permission only, not an API action that you can call\.  |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 
| [GetMergeConflicts](http://docs.aws.amazon.com/codecommit/latest/APIReference/API_GetMergeConflicts.html) |  `codecommit:GetMergeConflicts` Required to return information information about merge conflicts between the source and destination branch in a pull request\.  | arn:aws:codecommit:region:account\-id:repository\-name | 
|  [GetPullRequest](http://docs.aws.amazon.com/codecommit/latest/APIReference/API_GetPullRequest.html)  |  `codecommit:DescribePullRequest` Required to return information about a pull request\.  |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 
| [ListPullRequests](http://docs.aws.amazon.com/codecommit/latest/APIReference/API_ListPullRequests.html) |  `codecommit:ListPullRequests` Required to return information about the pull requests for a repository\.  |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 
| [MergePullRequestByFastForward](http://docs.aws.amazon.com/codecommit/latest/APIReference/API_MergePullRequestByFastForward.html) | codecommit:MergePullRequestByFastForwardRequired to close a pull request and attempt to merge the source branch into the destination branch of a pull request using the fast\-forward merge option\. |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 
| [PostCommentForPullRequest](http://docs.aws.amazon.com/codecommit/latest/APIReference/API_PostCommentForPullRequest.html) | codecommit:PostCommentForPullRequest Required to post a comment on a pull request in an AWS CodeCommit repository\. |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 
| [UpdatePullRequestDescription](http://docs.aws.amazon.com/codecommit/latest/APIReference/API_UpdatePullRequestDescription.html) | codecommit:UpdatePullRequestDescription Required to change the description of a pull request in an AWS CodeCommit repository\. |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 
| [UpdatePullRequestStatus](http://docs.aws.amazon.com/codecommit/latest/APIReference/API_UpdatePullRequestStatus.html) | codecommit:UpdatePullRequestStatus Required to change the status of a pull request in an AWS CodeCommit repository\. |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 
| [UpdatePullRequestTitle](http://docs.aws.amazon.com/codecommit/latest/APIReference/API_UpdatePullRequestTitle.html) | codecommit:UpdatePullRequestTitle Required to change the title of a pull request in an AWS CodeCommit repository\. |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 

## Permissions for Actions on Individual Files<a name="aa-files"></a>

The following permissions allow or deny actions on individual files in AWS CodeCommit repositories\. These permissions pertain only to actions performed in the AWS CodeCommit console, the AWS CodeCommit API, and to commands performed using the AWS CLI\. They do not pertain to similar actions that can be performed using the Git protocol\. For example, the `git push` command pushes new and changed files to an AWS CodeCommit repository by using the Git protocol\. It's not affected by any permissions for the AWS CodeCommit `PutFile` operation\.

If you see an expand arrow \(**↗**\) in the upper\-right corner of the table, you can open the table in a new window\. To close the window, choose the close button \(**X**\) in the lower\-right corner\.


**AWS CodeCommit API Operations and Required Permissions for Actions on Individual Files**  

| AWS CodeCommit API Operations for Individual Files | Required Permissions  | Resources | 
| --- | --- | --- | 
|  [DeleteFile](http://docs.aws.amazon.com/codecommit/latest/APIReference/API_DeleteFile.html)  |  `codecommit:DeleteFile` Required to delete a specified file from a specified branch in an AWS CodeCommit repository from the AWS CodeCommit console\.  |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 
|  [GetBlob](http://docs.aws.amazon.com/codecommit/latest/APIReference/API_GetBlob.html)  |  `codecommit:GetBlob` Required to view the encoded content of an individual file in an AWS CodeCommit repository from the AWS CodeCommit console\.  |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 
|  [GetFile](http://docs.aws.amazon.com/codecommit/latest/APIReference/API_GetFile.html)  |  `codecommit:GetFile` Required to view the encoded content of an individual file and its metadata an AWS CodeCommit repository from the AWS CodeCommit console\.  |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 
|  [GetFolder](http://docs.aws.amazon.com/codecommit/latest/APIReference/API_GetFolder.html)  |  `codecommit:GetFolder` Required to view the contents of a specified folder in an AWS CodeCommit repository from the AWS CodeCommit console\.  |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 
|  [PutFile](http://docs.aws.amazon.com/codecommit/latest/APIReference/API_PutFile.html)  |  `codecommit:PutFile` Required to add a new or modified file to an AWS CodeCommit repository from the AWS CodeCommit console, AWS CodeCommit API, or the AWS CLI\.  |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 

## Permissions for Actions on Comments<a name="aa-comments"></a>

The following permissions allow or deny actions on comments in AWS CodeCommit repositories\. These permissions pertain to actions performed with the AWS CodeCommit console and the AWS CodeCommit API, and to commands performed using the AWS CLI\. For related permissions on comments in pull requests, see [Permissions for Actions on Pull Requests](#aa-pr)\.

If you see an expand arrow \(**↗**\) in the upper\-right corner of the table, you can open the table in a new window\. To close the window, choose the close button \(**X**\) in the lower\-right corner\.


**AWS CodeCommit API Operations and Required Permissions for Comments in Repositories**  

| AWS CodeCommit API Operations | Required Permissions \(API Actions\) | Resources | 
| --- | --- | --- | 
|  [DeleteCommentContent](http://docs.aws.amazon.com/codecommit/latest/APIReference/API_DeleteCommentContent.html)  |  `codecommit:DeleteCommentContent` Required to delete the content of a comment made on a change, file, or commit in a repository\. Comments cannot be deleted, but the content of a comment can be removed if the user has this permission\.   |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 
|  [GetComment](http://docs.aws.amazon.com/codecommit/latest/APIReference/API_GetComment.html)  |  `codecommit:GetComment` Required to return information about a comment made on a change, file, or commit in an AWS CodeCommit repository\.  |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 
|  [GetCommentsForComparedCommit](http://docs.aws.amazon.com/codecommit/latest/APIReference/API_GetCommentsForComparedCommit.html)  |  `codecommit:GetCommentsForComparedCommit` Required to return information about comments made on the comparison between two commits in an AWS CodeCommit repository\.  |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 
|  [PostCommentForComparedCommit](http://docs.aws.amazon.com/codecommit/latest/APIReference/API_PostCommentForComparedCommit.html)  |  `codecommit:PostCommentForComparedCommit` Required to create a comment on the comparison between two commits in an AWS CodeCommit repository\.  |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 
| [PostCommentReply](http://docs.aws.amazon.com/codecommit/latest/APIReference/API_PostCommentReply.html) |  `codecommit:PostCommentReply` Required to create a reply to a comment on a comparison between commits or on a pull request\.  |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 
| [UpdateComment](http://docs.aws.amazon.com/codecommit/latest/APIReference/API_UpdateComment.html) |  `codecommit:UpdateComment` Required to edit a comment on a comparison between commits or on a pull request\. Comments can only be edited by the comment author\.  |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 

## Permissions for Actions on Committed Code<a name="aa-code"></a>

The following permissions allow or deny actions on code committed to AWS CodeCommit repositories\. These permissions pertain to actions performed with the AWS CodeCommit console and the AWS CodeCommit API, and commands performed using the AWS CLI\. They do not pertain to similar actions that can be performed using the Git protocol\. For related permissions on comments on committed code, see [Permissions for Actions on Comments](#aa-comments)\.

Explicitly denying some of these permissions might result in unexpected consequences in the AWS CodeCommit console\. For example, setting `GetTree` to `Deny` prevents users from navigating the contents of a repository in the console, but does not block users from viewing the contents of a file in the repository \(if they are sent a link to the file in email, for example\)\. Setting `GetBlob` to `Deny` prevents users from viewing the contents of files, but does not block users from browsing the structure of a repository\. Setting `GetCommit` to `Deny` prevents users from retrieving details about commits\. Setting `GetObjectIdentifier` to `Deny` blocks most of the functionality of code browsing\. If you set all three of these actions to `Deny` in a policy, a user with that policy cannot browse code in the AWS CodeCommit console\.

If you see an expand arrow \(**↗**\) in the upper\-right corner of the table, you can open the table in a new window\. To close the window, choose the close button \(**X**\) in the lower\-right corner\.


**AWS CodeCommit API Operations and Required Permissions for Actions on Committed Code**  

| AWS CodeCommit API Operations | Required Permissions \(API Actions\) | Resources | 
| --- | --- | --- | 
|  BatchGetCommits  |  `codecommit:BatchGetCommits` Required to return information about one or more commits in an AWS CodeCommit repository\. This is an IAM policy permission only, not an API action that you can call\.  |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 
|  [GetCommit](http://docs.aws.amazon.com/codecommit/latest/APIReference/GetCommit.html)  |  `codecommit:GetCommit` Required to return information about a commit\.  |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 
|  GetCommitHistory  |  `codecommit:GetCommitHistory` Required to return information about the history of commits in a repository\. This is an IAM policy permission only, not an API action that you can call\.  |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 
| [GetDifferences](http://docs.aws.amazon.com/codecommit/latest/APIReference/API_GetDifferences.html) |  `codecommit:GetDifferences` Required to return information about the differences between commit specifiers \(such as a branch, tag, HEAD, commit ID, or other fully qualified reference\)\.  |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 
| GetObjectIdentifier | codecommit:GetObjectIdentifierRequired to resolve blobs, trees, and commits to their identifier\. This is an IAM policy permission only, not an API action that you can call\. |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 
| GetReferences | codecommit:GetReferencesRequired to return all references, such as branches and tags\. This is an IAM policy permission only, not an API action that you can call\. |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 
| GetTree | codecommit:GetTreeRequired to view the contents of a specified tree in an AWS CodeCommit repository from the AWS CodeCommit console\. This is an IAM policy permission only, not an API action that you can call\. |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 

## Permissions for Actions on Repositories<a name="aa-repositories"></a>

The following permissions allow or deny actions on AWS CodeCommit repositories\. These permissions pertain to actions performed with the AWS CodeCommit console and the AWS CodeCommit API, and to commands performed using the AWS CLI\. They do not pertain to similar actions that can be performed using the Git protocol\. 

If you see an expand arrow \(**↗**\) in the upper\-right corner of the table, you can open the table in a new window\. To close the window, choose the close button \(**X**\) in the lower\-right corner\.


**AWS CodeCommit API Operations and Required Permissions for Actions on Repositories**  

| AWS CodeCommit API Operations | Required Permissions \(API Actions\) | Resources | 
| --- | --- | --- | 
|  [BatchGetRepositories](http://docs.aws.amazon.com/codecommit/latest/APIReference/API_BatchGetRepositories.html)  |  `codecommit:BatchGetRepositories` Required to get information about multiple AWS CodeCommit repositories in an AWS account\. In `Resource`, you must specify the names of all of the AWS CodeCommit repositories for which a user is allowed \(or denied\) information\.  |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 
|  [CreateRepository](http://docs.aws.amazon.com/codecommit/latest/APIReference/CreateRepository.html)  |  `codecommit:CreateRepository` Required to create an AWS CodeCommit repository\.  |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 
|  [DeleteRepository](http://docs.aws.amazon.com/codecommit/latest/APIReference/API_DeleteRepository.html)  |  `codecommit:DeleteRepository` Required to delete an AWS CodeCommit repository\.  |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 
| [GetRepository](http://docs.aws.amazon.com/codecommit/latest/APIReference/API_GetRepository.html) |  `codecommit:GetRepository` Required to get information about a single AWS CodeCommit repository\.  |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 
| [ListRepositories](http://docs.aws.amazon.com/codecommit/latest/APIReference/API_ListRepositories.html) | codecommit:ListRepositoriesRequired to get a list of the names and system IDs of multiple AWS CodeCommit repositories for an AWS account\. The only allowed value for `Resource` for this action is all repositories \(`*`\)\. |  \*  | 
| [UpdateRepositoryDescription](http://docs.aws.amazon.com/codecommit/latest/APIReference/API_UpdateRepositoryDescription.html) | codecommit:UpdateRepositoryDescriptionRequired to change the description of an AWS CodeCommit repository\. |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 
| [UpdateRepositoryName](http://docs.aws.amazon.com/codecommit/latest/APIReference/API_UpdateRepositoryName.html) | codecommit:UpdateRepositoryNameRequired to change the name of an AWS CodeCommit repository\. In `Resource`, you must specify both the AWS CodeCommit repositories that are allowed to be changed and the new repository names\. |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 

## Permissions for Actions on Triggers<a name="aa-triggers"></a>

The following permissions allow or deny actions on triggers for AWS CodeCommit repositories\. 

If you see an expand arrow \(**↗**\) in the upper\-right corner of the table, you can open the table in a new window\. To close the window, choose the close button \(**X**\) in the lower\-right corner\.


**AWS CodeCommit API Operations and Required Permissions for Actions on Triggers**  

| AWS CodeCommit API Operations | Required Permissions \(API Actions\) | Resources | 
| --- | --- | --- | 
|  [GetRepositoryTriggers](http://docs.aws.amazon.com/codecommit/latest/APIReference/API_GetRepositoryTriggers.html)  |  `codecommit:GetRepositoryTriggers` Required to return information about triggers configured for a repository\.  |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 
|  [PutRepositoryTriggers](http://docs.aws.amazon.com/codecommit/latest/APIReference/API_PutRepositoryTriggers.html)  |  `codecommit:PutRepositoryTriggers` Required to create, edit, or delete triggers for a repository\.  |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 
|  [TestRepositoryTriggers](http://docs.aws.amazon.com/codecommit/latest/APIReference/API_TestRepositoryTriggers.html)  |  `codecommit:TestRepositoryTriggers` Required to test the functionality of a repository trigger by sending data to the topic or function configured for the trigger\.  |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 

## Permissions for Actions on AWS CodePipeline Integration<a name="aa-acp"></a>

In order for AWS CodePipeline to use an AWS CodeCommit repository in a source action for a pipeline, you must grant all of the permissions listed in the following table to the service role for AWS CodePipeline\. If these permissions are not set in the service role or are set to **Deny**, the pipeline does not run automatically when a change is made to the repository, and changes cannot be released manually\. 

If you see an expand arrow \(**↗**\) in the upper\-right corner of the table, you can open the table in a new window\. To close the window, choose the close button \(**X**\) in the lower\-right corner\.


**AWS CodeCommit API Operations and Required Permissions for Actions on AWS CodePipeline Integration**  

| AWS CodeCommit API Operations | Required Permissions \(API Actions\) | Resources | 
| --- | --- | --- | 
|  [GetBranch](http://docs.aws.amazon.com/codecommit/latest/APIReference/API_GetBranch.html)  |  `codecommit:GetBranch` Required to get details about a branch in an AWS CodeCommit repository\.  |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 
|  [GetCommit](http://docs.aws.amazon.com/codecommit/latest/APIReference/GetCommit.html)  |  `codecommit:GetCommit` Required to return information about a commit to the service role for AWS CodePipeline\.   |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 
|  UploadArchive  |  `codecommit:UploadArchive` Required to allow the service role for AWS CodePipeline to upload repository changes into a pipeline\. This is an IAM policy permission only, not an API action that you can call\.  |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 
|  GetUploadArchiveStatus  |  `codecommit:GetUploadArchiveStatus` Required to determine the status of an archive upload: whether it is in progress, complete, cancelled, or if an error occurred\. This is an IAM policy permission only, not an API action that you can call\.  |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 
| CancelUploadArchive | codecommit:CancelUploadArchiveRequired to cancel the uploading of an archive to a pipeline\. This is an IAM policy permission only, not an API action that can be called\. |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 