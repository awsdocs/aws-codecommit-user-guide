# Manage approval rule templates<a name="how-to-manage-templates"></a>

You can manage the approval rule templates in an AWS Region to help understand how they are being used and what they are for\. For example, you can edit the names and descriptions of approval rule templates to help others understand their purpose\. You can list all the approval rule templates in an AWS Region, and get information about the content and structure of a template\. You can review which templates are associated with a repository, and which repositories are associated with a template\.

## Manage approval rule templates \(console\)<a name="how-to-manage-template-console"></a>

You can view and manage your approval rule templates in the CodeCommit console\.<a name="manage-template-console"></a>

## To manage approval rule templates<a name="manage-template-console"></a>

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. Choose **Approval rule templates** to view the list of approval rule templates in the AWS Region where you are signed in\. 
**Note**  
Approval rule templates are only available in the AWS Region where they were created\.

1. If you want to make changes to a template, choose it from the list, and then choose **Edit**\.

1. Make your changes, and then choose **Save**\.

## Manage approval rule templates \(AWS CLI\)<a name="how-to-manage-template-cli"></a>

You can manage your approval rule templates with the following AWS CLI commands:
+ [list\-approval\-rule\-templates](#list-templates), to view a list of all approval rule templates in an AWS Region
+ [get\-approval\-rule\-template](#get-template), to view the content of an approval rule template
+ [update\-approval\-rule\-template\-content](#update-template-content), to change the content of an approval rule template
+ [update\-approval\-rule\-template\-name](#update-template-name), to change the name of an approval rule template
+ [update\-approval\-rule\-template\-description](#update-template-description), to change the description of an approval rule template
+ [list\-repositories\-for\-approval\-rule\-template](#list-associated-repositories), to view all repositories associated with an approval rule template
+ [list\-associated\-approval\-rule\-templates\-for\-repository](#list-associated-templates), to view all approval rule templates associated with a repository<a name="list-templates"></a>

## To list all approval rule templates in an AWS Region<a name="list-templates"></a>

1. At the terminal or command line, run the list\-approval\-rule\-templates command\. For example, to list all approval rule templates in the US East \(Ohio\) Region:

   ```
   aws codecommit list-approval-rule-templates --region us-east-2
   ```

1. If successful, this command returns output similar to the following:

   ```
   {
       "approvalRuleTemplateNames": [
           "2-approver-rule-for-master",
           "1-approver-rule-for-all-pull-requests"
       ]
   }
   ```<a name="get-template"></a>

## To get the content of an approval rule template<a name="get-template"></a>

1. At the terminal or command line, run the get\-approval\-rule\-template command, specifying the name of the approval rule template:

   ```
   aws codecommit get-approval-rule-template --approval-rule-template-name 1-approver-rule-for-all-pull-requests
   ```

1. If successful, this command returns output similar to the following:

   ```
   {
       "approvalRuleTemplate": {
           "approvalRuleTemplateContent": "{\"Version\": \"2018-11-08\",\"Statements\": [{\"Type\": \"Approvers\",\"NumberOfApprovalsNeeded\": 1,\"ApprovalPoolMembers\": [\"arn:aws:sts::123456789012:assumed-role/CodeCommitReview/*\"]}]}",
           "ruleContentSha256": "621181bbEXAMPLE",
           "lastModifiedDate": 1571356106.936,
           "creationDate": 1571356106.936,
           "approvalRuleTemplateName": "1-approver-rule-for-all-pull-requests",
           "lastModifiedUser": "arn:aws:iam::123456789012:user/Li_Juan",
           "approvalRuleTemplateId": "a29abb15-EXAMPLE",
           "approvalRuleTemplateDescription": "All pull requests must be approved by one developer on the team."
       }
   }
   ```<a name="update-template-content"></a>

## To update the content of an approval rule template<a name="update-template-content"></a>

1. At the terminal or command prompt, run the update\-approval\-rule\-template\-content command, specifying the name of the template and the changed content\. For example, to change the content of an approval rule template named **1\-approver\-rule** to redefine the approval pool to users who assume the role of **CodeCommitReview**:

   ```
   aws codecommit update-approval-rule-template-content --approval-rule-template-name 1-approver-rule --new-rule-content "{\"Version\": \"2018-11-08\",\"DestinationReferences\": [\"refs/heads/master\"],\"Statements\": [{\"Type\": \"Approvers\",\"NumberOfApprovalsNeeded\": 2,\"ApprovalPoolMembers\": [\"arn:aws:sts::123456789012:assumed-role/CodeCommitReview/*\"]}]}"
   ```

1. If successful, this command returns output similar to the following:

   ```
   {
       "approvalRuleTemplate": {
           "creationDate": 1571352720.773,
           "approvalRuleTemplateDescription": "Requires 1 approval for all pull requests from the CodeCommitReview pool",
           "lastModifiedDate": 1571358728.41,
           "approvalRuleTemplateId": "41de97b7-EXAMPLE",
           "approvalRuleTemplateContent": "{\"Version\": \"2018-11-08\",\"Statements\": [{\"Type\": \"Approvers\",\"NumberOfApprovalsNeeded\": 1,\"ApprovalPoolMembers\": [\"arn:aws:sts::123456789012:assumed-role/CodeCommitReview/*\"]}]}",
           "approvalRuleTemplateName": "1-approver-rule-for-all-pull-requests",
           "ruleContentSha256": "2f6c21a5EXAMPLE",
           "lastModifiedUser": "arn:aws:iam::123456789012:user/Li_Juan"
       }
   }
   ```<a name="update-template-name"></a>

## To update the name of an approval rule template<a name="update-template-name"></a>

1. At the terminal or command prompt, run the update\-approval\-rule\-template\-name command, specifying the current name and the name you want to change it to\. For example, to change the name of an approval rule template from **1\-approver\-rule** to **1\-approver\-rule\-for\-all\-pull\-requests**:

   ```
   aws codecommit update-approval-rule-template-name --old-approval-rule-template-name "1-approver-rule" --new-approval-rule-template-name "1-approver-rule-for-all-pull-requests"
   ```

1. If successful, this command returns output similar to the following:

   ```
   {
       "approvalRuleTemplate": {
           "approvalRuleTemplateName": "1-approver-rule-for-all-pull-requests",
           "lastModifiedDate": 1571358241.619,
           "approvalRuleTemplateId": "41de97b7-EXAMPLE",
           "approvalRuleTemplateContent": "{\"Version\": \"2018-11-08\",\"Statements\": [{\"Type\": \"Approvers\",\"NumberOfApprovalsNeeded\": 1,\"ApprovalPoolMembers\": [\"arn:aws:sts::123456789012:assumed-role/CodeCommitReview/*\"]}]}",
           "creationDate": 1571352720.773,
           "lastModifiedUser": "arn:aws:iam::123456789012:user/Mary_Major",
           "approvalRuleTemplateDescription": "All pull requests must be approved by one developer on the team.",
           "ruleContentSha256": "2f6c21a5cEXAMPLE"
       }
   }
   ```<a name="update-template-description"></a>

## To update the description of an approval rule template<a name="update-template-description"></a>

1. At the terminal or command line, run the update\-approval\-rule\-template\-description command, specifying the name of the approval rule template and the new description:

   ```
   aws codecommit update-approval-rule-template-description --approval-rule-template-name "1-approver-rule-for-all-pull-requests" --approval-rule-template-description "Requires 1 approval for all pull requests from the CodeCommitReview pool" 
   ```

1. If successful, this command produces output similar to the following:

   ```
   {
       "approvalRuleTemplate": {
           "creationDate": 1571352720.773,
           "approvalRuleTemplateDescription": "Requires 1 approval for all pull requests from the CodeCommitReview pool",
           "lastModifiedDate": 1571358728.41,
           "approvalRuleTemplateId": "41de97b7-EXAMPLE",
           "approvalRuleTemplateContent": "{\"Version\": \"2018-11-08\",\"Statements\": [{\"Type\": \"Approvers\",\"NumberOfApprovalsNeeded\": 1,\"ApprovalPoolMembers\": [\"arn:aws:sts::123456789012:assumed-role/CodeCommitReview/*\"]}]}",
           "approvalRuleTemplateName": "1-approver-rule-for-all-pull-requests",
           "ruleContentSha256": "2f6c21a5EXAMPLE",
           "lastModifiedUser": "arn:aws:iam::123456789012:user/Li_Juan"
       }
   }
   ```<a name="list-associated-repositories"></a>

## To list all repositories associated with a template<a name="list-associated-repositories"></a>

1. At the command line or terminal, run the list\-repositories\-for\-approval\-rule\-template command, specifying the name of the template:

   ```
   aws codecommit list-repositories-for-approval-rule-template --approval-rule-template-name 2-approver-rule-for-master
   ```

1. If successful, this command returns output similar to the following:

   ```
   {
       "repositoryNames": [
           "MyDemoRepo",
           "MyClonedRepo"
       ]
   }
   ```<a name="list-associated-templates"></a>

## To list all templates associated with a repository<a name="list-associated-templates"></a>

1. At the command line or terminal, run the list\-associated\-approval\-rule\-templates\-for\-repository command, specifying the name of the repository:

   ```
   aws codecommit list-associated-approval-rule-templates-for-repository --repository-name MyDemoRepo
   ```

1. If successful, this command returns output similar to the following:

   ```
   {
       "approvalRuleTemplateNames": [
           "2-approver-rule-for-master",
           "1-approver-rule-for-all-pull-requests"
       ]
   }
   ```