# Merge a pull request in an AWS CodeCommit repository<a name="how-to-merge-pull-request"></a>

After your code has been reviewed and all approval rules \(if any\) on the pull request have been satisfied, you can merge a pull request in one of several ways:
+ <a name="is-mergable"></a>You can use the console to merge your source branch to the destination branch using one of the available merge strategies, which also closes the pull request\. You can also resolve any merge conflicts in the console\. The console displays a message that indicates if the pull request is mergeable or if conflicts must be resolved\. When all conflicts are resolved and you choose **Merge**, the merge is performed using the merge strategy that you choose\. Fast\-forward is the default merge strategy, which is the default option for Git\. Depending on the state of the code in the source and destination branches, that strategy might not be available, but other options might be, such as squash or 3\-way\.
+ You can use the AWS CLI to merge and close the pull request using the fast\-forward, squash, or 3\-way merge strategy\. 
+ <a name="why-git-merge"></a>On your local computer, you can use the git merge command to merge the source branch into the destination branch, and then push your merged code to the destination branch\. This approach has drawbacks that you should carefully consider\. It merges the pull request regardless if the requirements for approval rules on the pull request have been satisfied, circumventing those controls\. Merging and pushing the destination branch also closes the pull request automatically if the pull request is merged using the fast\-forward merge strategy\. One advantage of this approach is that the git merge command allows you to choose merge options or strategies that are not available in the CodeCommit console\. For more information about git merge and merge options, see [git\-merge](https://git-scm.com/docs/git-merge) or your Git documentation\.

CodeCommit closes a pull request automatically if either the source or destination branch of the pull request is deleted\.

**Topics**
+ [Merge a pull request \(console\)](#how-to-merge-pull-request-console)
+ [Merge a pull request \(AWS CLI\)](#how-to-merge-pull-request-cli)

## Merge a pull request \(console\)<a name="how-to-merge-pull-request-console"></a>

You can use the CodeCommit console to merge a pull request in a CodeCommit repository\. After the status of a pull request is changed to **Merged**, it no longer appears in the list of open pull requests\. A merged pull request is categorized as closed\. It cannot be changed back to **Open**, but users can still comment on the changes and reply to comments\. After a pull request is merged or closed, you cannot approve it, revoke approval for it, or override the approval rules applied to the pull request\.

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In **Repositories**, choose the name of the repository\. 

1. In the navigation pane, choose **Pull requests**\.

1. By default, a list of all open pull requests is displayed\. Choose the open pull request you want to merge\.

1. In the pull request, choose **Approvals**\. Review the list of approvers, and verify that all approval rules \(if any\) have had their conditions satisfied\. You cannot merge a pull request if one or more approval rules have the status of **Rule not satisfied**\. If no one has approved the pull request, consider whether you want to merge it, or whether you want to wait for approvals\.
**Note**  
If an approval rule was created for a pull request, you can edit it or delete it to unblock the merge\. If the approval rule was created with an approval rule template, you cannot edit or delete it\. You can only choose to override the requirements\. For more information, see [Override approval rules on a pull request](how-to-override-approval-rules.md)\.  
![\[A pull request showing an approval rule whose condititions have not been satisfied and an empty list of approvers.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-approval-rule-not-met.png)

1. Choose **Merge**\.

1. In the pull request, choose between the available merge strategies\. Merge strategies that cannot be applied appear greyed out\. If no merge strategies are available, you can choose to manually resolve conflicts in the CodeCommit console, or you can resolve them locally using your Git client\. For more information, see [Resolve conflicts in a pull request in an AWS CodeCommit repository](how-to-resolve-conflict-pull-request.md)\.  
![\[A pull request showing the merge strategies available for the merge in the CodeCommit console.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-merge-squash.png)
   + A fast\-forward merge moves the reference for the destination branch forward to the most recent commit of the source branch\. This is the default behavior of Git when possible\. No merge commit is created, but all commit history from the source branch is retained as if it had occurred in the destination branch\. Fast\-forward merges do not appear as a branch merge in the commit visualizer view of the destination branch's history because no merge commit is created\. The tip of the source branch is fast\-forwarded to the tip of the destination branch\. 
   + A squash merge creates one commit that contains the changes in the source branch and applies that single squashed commit to the destination branch\. By default, the commit message for that squash commit contains all the commit messages of the changes in the source branch\. No individual commit history of the branch changes is retained\. This can help simplify your repository history while still retaining a graphical representation of the merge in the commit visualizer view of the destination branch's history\. 
   + A three\-way merge creates a merge commit for the merge in the destination branch, but also retains the individual commits made in the source branch as part of the history of the destination branch\. This can help maintain a complete history of changes to your repository\.

1. If you choose the squash or 3\-way merge strategy, review the automatically generated commit message and modify it if you want to change the information\. Add your name and email address for the commit history\.

1. \(Optional\) Clear the option to delete the source branch as part of the merge\. The default is to delete the source branch when a pull request is merged\.

1. Choose **Merge pull request** to complete the merge\.

## Merge a pull request \(AWS CLI\)<a name="how-to-merge-pull-request-cli"></a>

To use AWS CLI commands with CodeCommit, install the AWS CLI\. For more information, see [Command line reference](cmd-ref.md)\. 

**To use the AWS CLI to merge pull requests in a CodeCommit repository**

1. <a name="evaluate-pull-request-approval-rules"></a>To evaluate whether a pull request has had all of its approval rules satisfied and is ready to be merged, run the evaluate\-pull\-request\-approval\-rules command, specifying:
   + The ID of the pull request \(using the \-\-pull\-request\-id option\)\.
   + The revision ID of the pull request \(using the \-\-revision\-id option\)\. You can get the current revision ID for a pull request by using the [get\-pull\-request](how-to-view-pull-request.md#get-pull-request) command\.

   For example, to evaluate the state of approval rules on a pull request with an ID of *27* and a revision ID of *9f29d167EXAMPLE*:

   ```
   aws codecommit evaluate-pull-request-approval-rules --pull-request-id 27 --revision-id 9f29d167EXAMPLE
   ```

   If successful, this command produces output similar to the following:

   ```
   {
       "evaluation": {
           "approved": false,
           "approvalRulesNotSatisfied": [
               "Require two approved approvers"
           ],
           "overridden": false,
           "approvalRulesSatisfied": []
       }
   }
   ```
**Note**  
This output indicates that a pull request is not mergable because the requirements of an approval rule have not been satisfied\. To merge this pull request, you can have reviewers approve it to meet the conditions of the rule\. Depending on your permissions and how the rule was created, you might also be able to edit, override, or delete the rule\. For more information, see [Review a pull request](how-to-review-pull-request.md), [Override approval rules on a pull request](how-to-override-approval-rules.md), and [Edit or delete an approval rule for a pull request](how-to-edit-delete-pull-request-approval-rule.md)\. 

1. To merge and close a pull request using the fast\-forward merge strategy, run the merge\-pull\-request\-by\-fast\-forward command, specifying:
   + The ID of the pull request \(with the \-\-pull\-request\-id option\)\.
   + The full commit ID of the tip of the source branch \(with the \-\-source\-commit\-id option\)\. 
   + The name of the repository \(with the \-\-repository\-name option\)\.

    For example, to merge and close a pull request with the ID of *47* and a source commit ID of *99132ab0EXAMPLE* in a repository named *MyDemoRepo*:

   ```
   aws codecommit merge-pull-request-by-fast-forward --pull-request-id 47 --source-commit-id 99132ab0EXAMPLE --repository-name MyDemoRepo
   ```

   If successful, this command produces output similar to the following:

   ```
   {
       "pullRequest": {
           "approvalRules": [
               {
                   "approvalRuleContent": "{\"Version\": \"2018-11-08\",\"Statements\": [{\"Type\": \"Approvers\",\"NumberOfApprovalsNeeded\": 1,\"ApprovalPoolMembers\": [\"arn:aws:sts::123456789012:assumed-role/CodeCommitReview/*\"]}]}",
                   "approvalRuleId": "dd8b17fe-EXAMPLE",
                   "approvalRuleName": "I want one approver for this pull request",
                   "creationDate": 1571356106.936,
                   "lastModifiedDate": 571356106.936,
                   "lastModifiedUser": "arn:aws:iam::123456789012:user/Mary_Major",
                   "ruleContentSha256": "4711b576EXAMPLE"
               }
           ],
           "authorArn": "arn:aws:iam::123456789012:user/Li_Juan",
           "clientRequestToken": "",
           "creationDate": 1508530823.142,
           "description": "Review the latest changes and updates to the global variables",
           "lastActivityDate": 1508887223.155,
           "pullRequestId": "47",
           "pullRequestStatus": "CLOSED",
           "pullRequestTargets": [
               {
                   "destinationCommit": "9f31c968EXAMPLE",
                   "destinationReference": "refs/heads/master",
                   "mergeMetadata": {
                       "isMerged": true,
                       "mergedBy": "arn:aws:iam::123456789012:user/Mary_Major"
                   },
                   "repositoryName": "MyDemoRepo",
                   "sourceCommit": "99132ab0EXAMPLE",
                   "sourceReference": "refs/heads/variables-branch"
               }
           ],
           "title": "Consolidation of global variables"
       }
   }
   ```

1. To merge and close a pull request using the squash merge strategy, run the merge\-pull\-request\-by\-squash command, specifying:
   + The ID of the pull request \(with the \-\-pull\-request\-id option\)\.
   + The full commit ID of the tip of the source branch \(with the \-\-source\-commit\-id option\)\. 
   + The name of the repository \(with the \-\-repository\-name option\)\.
   + The level of conflict detail you want to use \(with the \-\-conflict\-detail\-level option\)\. If unspecified, the default `FILE_LEVEL` is used\.
   + The conflict resolution strategy you want to use \(with the \-\-conflict\-resolution\-strategy option\)\. If unspecified, this defaults to `NONE`, and conflicts must be resolved manually\.
   + The commit message to include \(with the \-\-commit\-message option\)\.
   + The name to use for the commit \(with the \-\-name option\)\.
   + The email address to use for the commit \(with the \-\-email option\)\.
   + Whether to keep any empty folders \(with the \-\-keep\-empty\-folders option\)\.

   The following example merges and closes a pull request with the ID of *47* and a source commit ID of *99132ab0EXAMPLE* in a repository named *MyDemoRepo*\. It uses the conflict detail of `LINE_LEVEL` and the conflict resolution strategy of `ACCEPT_SOURCE`:

   ```
   aws codecommit merge-pull-request-by-squash --pull-request-id 47 --source-commit-id 99132ab0EXAMPLE --repository-name MyDemoRepo --conflict-detail-level LINE_LEVEL --conflict-resolution-strategy ACCEPT_SOURCE --name "Jorge Souza" --email "jorge_souza@example.com" --commit-message "Merging pull request 47 by squash and accepting source in merge conflicts"
   ```

   If successful, this command produces the same kind of output as merging by fast\-forward, output similar to the following:

   ```
   {
       "pullRequest": {
           "approvalRules": [
               {
                   "approvalRuleContent": "{\"Version\": \"2018-11-08\",\"DestinationReferences\": [\"refs/heads/master\"],\"Statements\": [{\"Type\": \"Approvers\",\"NumberOfApprovalsNeeded\": 2,\"ApprovalPoolMembers\": [\"arn:aws:sts::123456789012:assumed-role/CodeCommitReview/*\"]}]}",
                   "approvalRuleId": "dd8b17fe-EXAMPLE",
                   "approvalRuleName": "2-approver-rule-for-master",
                   "creationDate": 1571356106.936,
                   "lastModifiedDate": 571356106.936,
                   "lastModifiedUser": "arn:aws:iam::123456789012:user/Mary_Major",
                   "originApprovalRuleTemplate": {
                       "approvalRuleTemplateId": "dd8b17fe-EXAMPLE",
                       "approvalRuleTemplateName": "2-approver-rule-for-master"
                   },
                   "ruleContentSha256": "4711b576EXAMPLE"
               }
           ],
           "authorArn": "arn:aws:iam::123456789012:user/Li_Juan",
           "clientRequestToken": "",
           "creationDate": 1508530823.142,
           "description": "Review the latest changes and updates to the global variables",
           "lastActivityDate": 1508887223.155,
           "pullRequestId": "47",
           "pullRequestStatus": "CLOSED",
           "pullRequestTargets": [
               {
                   "destinationCommit": "9f31c968EXAMPLE",
                   "destinationReference": "refs/heads/master",
                   "mergeMetadata": {
                       "isMerged": true,
                       "mergedBy": "arn:aws:iam::123456789012:user/Mary_Major"
                   },
                   "repositoryName": "MyDemoRepo",
                   "sourceCommit": "99132ab0EXAMPLE",
                   "sourceReference": "refs/heads/variables-branch"
               }
           ],
           "title": "Consolidation of global variables"
       }
   }
   ```

1. To merge and close a pull request using the three\-way merge strategy, run the merge\-pull\-request\-by\-three\-way command, specifying:
   + The ID of the pull request \(with the \-\-pull\-request\-id option\)\.
   + The full commit ID of the tip of the source branch \(with the \-\-source\-commit\-id option\)\. 
   + The name of the repository \(with the \-\-repository\-name option\)\.
   + The level of conflict detail you want to use \(with the \-\-conflict\-detail\-level option\)\. If unspecified, the default `FILE_LEVEL` is used\.
   + The conflict resolution strategy you want to use \(with the \-\-conflict\-resolution\-strategy option\)\. If unspecified, this defaults to `NONE`, and conflicts must be resolved manually\.
   + The commit message to include \(with the \-\-commit\-message option\)\.
   + The name to use for the commit \(with the \-\-name option\)\.
   + The email address to use for the commit \(with the \-\-email option\)\.
   + Whether to keep any empty folders \(with the \-\-keep\-empty\-folders option\)\.

   The following example merges and closes a pull request with the ID of *47* and a source commit ID of *99132ab0EXAMPLE* in a repository named *MyDemoRepo*\. It uses the default options for conflict detail and conflict resolution strategy:

   ```
   aws codecommit merge-pull-request-by-fast-forward --pull-request-id 47 --source-commit-id 99132ab0EXAMPLE --repository-name MyDemoRepo --name "Maria Garcia" --email "maria_garcia@example.com" --commit-message "Merging pull request 47 by three-way with default options"
   ```

   If successful, this command produces the same kind of output as merging by fast\-forward, similar to the following:

   ```
   {
       "pullRequest": {
           "approvalRules": [
               {
                   "approvalRuleContent": "{\"Version\": \"2018-11-08\",\"DestinationReferences\": [\"refs/heads/master\"],\"Statements\": [{\"Type\": \"Approvers\",\"NumberOfApprovalsNeeded\": 2,\"ApprovalPoolMembers\": [\"arn:aws:sts::123456789012:assumed-role/CodeCommitReview/*\"]}]}",
                   "approvalRuleId": "dd8b17fe-EXAMPLE",
                   "approvalRuleName": "2-approver-rule-for-master",
                   "creationDate": 1571356106.936,
                   "lastModifiedDate": 571356106.936,
                   "lastModifiedUser": "arn:aws:iam::123456789012:user/Mary_Major",
                   "originApprovalRuleTemplate": {
                       "approvalRuleTemplateId": "dd8b17fe-EXAMPLE",
                       "approvalRuleTemplateName": "2-approver-rule-for-master"
                   },
                   "ruleContentSha256": "4711b576EXAMPLE"
               }
           ],
           "authorArn": "arn:aws:iam::123456789012:user/Li_Juan",
           "clientRequestToken": "",
           "creationDate": 1508530823.142,
           "description": "Review the latest changes and updates to the global variables",
           "lastActivityDate": 1508887223.155,
           "pullRequestId": "47",
           "pullRequestStatus": "CLOSED",
           "pullRequestTargets": [
               {
                   "destinationCommit": "9f31c968EXAMPLE",
                   "destinationReference": "refs/heads/master",
                   "mergeMetadata": {
                       "isMerged": true,
                       "mergedBy": "arn:aws:iam::123456789012:user/Mary_Major"
                   },
                   "repositoryName": "MyDemoRepo",
                   "sourceCommit": "99132ab0EXAMPLE",
                   "sourceReference": "refs/heads/variables-branch"
               }
           ],
           "title": "Consolidation of global variables"
       }
   }
   ```