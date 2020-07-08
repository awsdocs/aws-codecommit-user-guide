# Edit or delete an approval rule for a pull request<a name="how-to-edit-delete-pull-request-approval-rule"></a>

When you have an approval rule on a pull request, you cannot merge that pull request until its conditions have been met\. You can change the approval rules for pull requests to make it easier to satisfy their conditions, or to increase the rigor of reviews\. You can change the number of users who must approve a pull request\. You can also add, remove, or change the membership in an approval pool of users for the rule\. Lastly, if you no longer want to use an approval rule for a pull request, you can delete it\.

**Note**  
You can also override approval rules for a pull request\. For more information, see [Override approval rules on a pull request](how-to-override-approval-rules.md)\.

You can use the AWS CodeCommit console or the AWS CLI to edit and delete approval rules for your repository\. 

**Topics**
+ [Edit or delete an approval rule for a pull request \(console\)](#how-to-edit-delete-pull-request-approval-rule-console)
+ [Edit or delete an approval rule for a pull request \(AWS CLI\)](#how-to-edit-delete-pull-request-approval-rule-cli)

## Edit or delete an approval rule for a pull request \(console\)<a name="how-to-edit-delete-pull-request-approval-rule-console"></a>

You can use the CodeCommit console to edit or delete an approval rule for a pull request in a CodeCommit repository\. 

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In **Repositories**, choose the name of the repository where you want to edit or delete an approval rule for a pull request\. 

1. In the navigation pane, choose **Pull Requests**\.

1. Choose the pull request where you want to edit or delete an approval rule\. You can only edit and delete approval rules for open pull requests\.  
![\[A list of pull requests for a repository in the CodeCommit console.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-pull-request-view.png)

1. In the pull request, choose **Approvals**, and then choose the rule you want to edit or delete from the list\. Do one of the following:
   + If you want to edit the rule, choose **Edit**\. 
   + If you want to delete the rule, choose **Delete**, and then follow the instructions for verifying the deletion of the rule\.

1. In **Edit approval rule**, make the changes you want to the rule, and then choose **Submit**\.  
![\[Editing an approval rule\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-pull-request-edit-rule.png)

1. When you have finished configuring the approval rule, choose **Submit**\.

## Edit or delete an approval rule for a pull request \(AWS CLI\)<a name="how-to-edit-delete-pull-request-approval-rule-cli"></a>

To use AWS CLI commands with CodeCommit, install the AWS CLI\. For more information, see [Command line reference](cmd-ref.md)\. 

You can use the AWS CLI to edit the content of an approval rule and to delete an approval rule\. 

**Note**  
You might also be interested in the following commands:  
[update\-pull\-request\-approval\-state](how-to-review-pull-request.md#update-pull-request-approval-state), to approve or revoke approval on a pull request\.
[get\-pull\-request\-approval\-states](how-to-view-pull-request.md#get-pull-request-approval-state), to view the approvals on the pull request\.
[evaluate\-pull\-request\-approval\-rules](how-to-merge-pull-request.md#evaluate-pull-request-approval-rules), to determine whether approval rules for a pull request have had their conditions satisifed\.

**To use the AWS CLI to edit or delete an approval rule for a pull request in a CodeCommit repository**<a name="update-pull-request-approval-rule-content"></a>

1. To edit an approval rule, run the update\-pull\-request\-approval\-rule\-content command, specifying:
   + The ID of the pull request \(with the \-\-id option\)\.
   + The name of the approval rule \(with the \-\-approval\-rule\-name option\)\.
   + The content of the approval rule \(with the \-\-approval\-rule\-content option\)\.

   This example updates an approval rule named *Require two approved approvers* for a pull request with the ID of *27*\. The rule requires one user approval from an approval pool that includes any IAM user in the *123456789012* AWS account:

   ```
   aws codecommit update-pull-request-approval-rule-content --pull-request-id 27 --approval-rule-name "Require two approved approvers" --approval-rule-content "{Version: 2018-11-08, Statements: [{Type: \"Approvers\", NumberOfApprovalsNeeded: 1, ApprovalPoolMembers:[\"CodeCommitApprovers:123456789012:user/*\"]}]}}"
   ```

1. If successful, this command produces output similar to the following:

   ```
   {
       "approvalRule": {
           "approvalRuleContent": "{Version: 2018-11-08, Statements: [{Type: \"Approvers\", NumberOfApprovalsNeeded: 1, ApprovalPoolMembers:[\"CodeCommitApprovers:123456789012:user/*\"]}]}}",
           "approvalRuleId": "aac33506-EXAMPLE",
           "originApprovalRuleTemplate": {},
           "creationDate": 1570752871.932,
           "lastModifiedDate": 1570754058.333,
           "approvalRuleName": Require two approved approvers",
           "lastModifiedUser": "arn:aws:iam::123456789012:user/Mary_Major",
           "ruleContentSha256": "cd93921cEXAMPLE",
       }
   }
   ```

1. <a name="delete-pull-request-approval-rule"></a>To delete an approval rule, run the delete\-pull\-request\-approval\-rule command, specifying:
   + The ID of the pull request \(with the \-\-id option\)\.
   + The name of the approval rule \(with the \-\-approval\-rule\-name option\)\.

   For example, to delete an approval rule with the name *My Approval Rule* for a pull request with the ID of *15*:

   ```
   aws codecommit delete-pull-request-approval-rule --pull-request-id 15 --approval-rule-name "My Approval Rule"
   ```

   If successful, this command returns output similar to the following:

   ```
   {
       "approvalRuleId": "077d8e8a8-EXAMPLE"
   }
   ```