# Update a pull request<a name="how-to-update-pull-request"></a>

You can use the AWS CodeCommit console or the AWS CLI to update the title or description of a pull request\. You might want to update the pull request because:
+ Other users don't understand the description, or the original title is misleading\.
+ You want the title or description to reflect changes made to the source branch of an open pull request\.

## Update a pull request \(console\)<a name="how-to-update-pull-request-console"></a>

You can use the CodeCommit console to update the title and description of a pull request in an CodeCommit repository\. 

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In **Repositories**, choose the name of the repository where you want to update a pull request\. 

1. In the navigation pane, choose **Pull requests**\.

1. By default, a list of all open pull requests is displayed\. Choose the open pull request you want to update\.

1. In the pull request, choose **Details**, and then choose **Edit details** to edit the title or description\.
**Note**  
You cannot update the title or description of a closed or merged pull request\.

## Update pull requests \(AWS CLI\)<a name="how-to-update-pull-request-cli"></a>

To use AWS CLI commands with CodeCommit, install the AWS CLI\. For more information, see [Command line reference](cmd-ref.md)\. 

You might also be interested in the following commands:
+ [update\-pull\-request\-approval\-state](how-to-review-pull-request.md#update-pull-request-approval-state), to approve or revoke approval on a pull request\.
+ [create\-pull\-request\-approval\-rule](how-to-create-pull-request-approval-rule.md#how-to-create-pull-request-approval-rule-cli), to create an approval rule for a pull request\.
+ [delete\-pull\-request\-approval\-rule](how-to-edit-delete-pull-request-approval-rule.md#delete-pull-request-approval-rule), to delete an approval rule for a pull request\.

**To use the AWS CLI to update pull requests in a CodeCommit repository**

1.  To update the title of a pull request in a repository, run the update\-pull\-request\-title command, specifying:
   + The ID of the pull request \(with the \-\-pull\-request\-id option\)\.
   + The title of the pull request \(with the \-\-title option\)\.

   For example, to update the title of a pull request with the ID of *47*:

   ```
   aws codecommit update-pull-request-title --pull-request-id 47 --title "Consolidation of global variables - updated review"  
   ```

   If successful, this command produces output similar to the following:

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
                       "approvalRuleTemplateId": "dd8b26gr-EXAMPLE",
                       "approvalRuleTemplateName": "2-approver-rule-for-master"
                   },
                   "ruleContentSha256": "4711b576EXAMPLE"
               }
           ],
           "authorArn": "arn:aws:iam::123456789012:user/Li_Juan",
           "clientRequestToken": "",
           "creationDate": 1508530823.12,
           "description": "Review the latest changes and updates to the global variables. I have updated this request with some changes, including removing some unused variables.",
           "lastActivityDate": 1508372657.188,
           "pullRequestId": "47",
           "pullRequestStatus": "OPEN",
           "pullRequestTargets": [
               {
                   "destinationCommit": "9f31c968EXAMPLE",
                   "destinationReference": "refs/heads/master",
                   "mergeMetadata": {
                       "isMerged": false,
                   },
                   "repositoryName": "MyDemoRepo",
                   "sourceCommit": "99132ab0EXAMPLE",
                   "sourceReference": "refs/heads/variables-branch"
               }
           ],
           "title": "Consolidation of global variables - updated review"
       }
   }
   ```

1. To update the description of a pull request, run the update\-pull\-request\-description command, specifying:
   + The ID of the pull request \(with the \-\-pull\-request\-id option\)\.
   + The description \(with the \-\-description option\)\. 

    For example, to update the description of a pull request with the ID of *47* :

   ```
   aws codecommit update-pull-request-description --pull-request-id 47 --description "Updated the pull request to remove unused global variable."
   ```

   If successful, this command produces output similar to the following:

   ```
   {
      "pullRequest": { 
         "authorArn": "arn:aws:iam::111111111111:user/Li_Juan",
         "clientRequestToken": "",
         "creationDate": 1508530823.155,
         "description": "Updated the pull request to remove unused global variable.",
         "lastActivityDate": 1508372423.204,
         "pullRequestId": "47",
         "pullRequestStatus": "OPEN",
         "pullRequestTargets": [ 
            { 
               "destinationCommit": "9f31c968EXAMPLE",
               "destinationReference": "refs/heads/master",
               "mergeMetadata": { 
                  "isMerged": false,
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