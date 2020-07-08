# Create an approval rule template<a name="how-to-create-template"></a>

You can create one or more approval rule templates to help you customize your development workflows across repositories\. By creating multiple templates, you can configure the automatic creation of approval rules so that different branches have appropriate levels of approvals and control\. For example, you can create different templates for production and development branches and apply these templates to one or more repositories\. When users create pull requests in those repositories, the request is evaluated against those templates\. If the request matches the conditions in the applied templates, approval rules are created for the pull request\.

You can use the console or AWS CLI to create approval rule templates\.

**Topics**
+ [Create an approval rule template \(console\)](#how-to-create-template-console)
+ [Create an approval rule template \(AWS CLI\)](#how-to-create-template-cli)

## Create an approval rule template \(console\)<a name="how-to-create-template-console"></a>

Approval rule templates are not associated with any repository by default\. You can make an association between a template and one or more repositories when you create the template, or you can add the associations at a later time\.<a name="create-template-console"></a>

## To create an approval rule template<a name="create-template-console"></a>

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. Choose **Approval rule templates**, and then choose **Create template**\.

1. In **Approval rule template name**, give the template a descriptive name so you know what it is for\. For example, if you want to require one person from a set of senior developers to approve a pull request before it can be merged, you could name the rule **Require 1 approver from a senior developer for master branch**\. 

1. \(Optional\) In **Description**, provide a description of the purpose of this template\. This can help others decide whether this template is appropriate for their repositories\.

1. In **Number of approvals needed**, enter the number you want\. The default is 1\. 

1. \(Optional\) If you want to require that the approvals for a pull request come from a specific group of users, in **Approval rule members**, choose **Add**\. In **Approver type**, choose one of the following: 
   + **IAM user name or assumed role**: This option prepopulates the AWS account ID for the account you used to sign in, and only requires a name\. It can be used for both IAM users and federated access users whose name matches the provided name\. This is a very powerful option that offers a great deal of flexibility\. For example, if you choose this option and are signed in with the AWS account 123456789012, and you specify **Mary\_Major**, all of the following are counted as approvals coming from that user:
     + An IAM user in the account \(`arn:aws:iam::123456789012:user/Mary_Major`\)
     + A federated user identified in IAM as Mary\_Major \(`arn:aws:sts::123456789012:federated-user/Mary_Major`\)

     This option does not recognize an active session of someone assuming the role of **CodeCommitReview** with a role session name of Mary\_Major \(`arn:aws:sts::123456789012:assumed-role/CodeCommitReview/Mary_Major`\) unless you include a wildcard \(`*Mary_Major`\)\. You can also specify the role name explicitly \(`CodeCommitReview/Mary_Major`\)\.
   + **Fully qualified ARN**: This option allows you to specify the fully qualified Amazon Resource Name \(ARN\) of the IAM user or role\. This option also supports assumed roles used by other AWS services, such as AWS Lambda and AWS CodeBuild\. For assumed roles, the ARN format should be `arn:aws:sts::AccountID:assumed-role/RoleName` for roles and `arn:aws:sts::AccountID:assumed-role/FunctionName` for functions\.

   If you chose **IAM user name or assumed role** as the approver type, in **Value**, enter the name of the IAM user or role or the fully qualified ARN of the user or role\. Choose **Add** again to add more users or roles, until you have added all the users or roles whose approvals count toward the number of required approvals\. 

   Both approver types allow you to use wildcards \(\*\) in their values\. For example, if you choose the **IAM user name or assumed role** option, and you specify **CodeCommitReview/\***, all users who assume the role of **CodeCommitReview** are counted in the approval pool\. Their individual role session names count toward the required number of approvers\. In this way, both Mary\_Major and Li\_Juan count as approvals when signed in and assuming the role of `CodeCommitReview`\. For more information about IAM ARNs, wildcards, and formats, see [IAM Identifiers](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_identifiers.html#identifiers-arns)\.
**Note**  
Approval rules do not support cross\-account approvals\.

1. \(Optional\) In **Branch filters**, enter destination branch names to use to filter the creation of approval rules\. For example, if you specify *master*, an approval rule is created for pull requests in associated repositories only if the destination branch for the pull request is a branch named *master*\. You can use wildcards \(\*\) in branch names to apply approval rules to all branch names that match the wildcard cases\. However, you cannot use a wildcard at the beginning of a branch name\. You can specify up to 100 branch names\. If you do not specify any filters, the template applies to all branches in an associated repository\.

1. \(Optional\) In **Associated repositories**, in the **Repositories** list, choose the repositories in this AWS Region that you want to associate with this approval rule\. 
**Note**  
You can choose to associate repositories after creating the template\. For more information, see [Associate an approval rule template with a repository](how-to-associate-template.md)\.

1. Choose **Create**\.

![\[An approval rule template that requires 1 approver from a defined approval rule if a pull request is created on the master branch, associated with two repositories\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-approval-rule-template.png)

## Create an approval rule template \(AWS CLI\)<a name="how-to-create-template-cli"></a>

You can use the AWS CLI to create approval rule templates\. When you use the AWS CLI, you can specify destination references for the template, so that it applies only to pull requests whose destination branches match those in the template\.<a name="create-template-cli"></a>

## To create an approval rule template<a name="create-template-cli"></a>

1. At a terminal or command line, run the create\-approval\-rule\-template command, specifying:
   + The name for the approval rule template\. Consider using a name that describes its purpose\.
   + A description of the approval rule template\. As with the name, consider providing a detailed description\.
   + The JSON structure of the approval rule template\. This structure can include requirements for destination references, which are the destination branches for pull requests for which the approval rule is applied, and approval pool members, who are users whose approvals count toward the number of required approvals\.

   When creating the content of the approval rule, you can specify approvers in an approval pool in one of two ways:
   + **CodeCommitApprovers**: This option only requires an AWS account and a resource\. It can be used for both IAM users and federated access users whose name matches the provided resource name\. This is a very powerful option that offers a great deal of flexibility\. For example, if you specify the AWS account 123456789012 and **Mary\_Major**, all of the following are counted as approvals coming from that user:
     + An IAM user in the account \(`arn:aws:iam::123456789012:user/Mary_Major`\)
     + A federated user identified in IAM as Mary\_Major \(`arn:aws:sts::123456789012:federated-user/Mary_Major`\)

     This option does not recognize an active session of someone assuming the role of *SeniorDevelopers* with a role session name of *Mary\_Major* \(`arn:aws:sts::123456789012:assumed-role/SeniorDevelopers/Mary_Major`\) unless you include a wildcard \(`*Mary_Major`\)\.
   + **Fully qualified ARN**: This option allows you to specify the fully qualified Amazon Resource Name \(ARN\) of the IAM user or role\. 

   For more information about IAM ARNs, wildcards, and formats, see [IAM Identifiers](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_identifiers.html#identifiers-arns)\.

   The following example creates an approval rule template named **2\-approver\-rule\-for\-master** and a description of **Requires two developers from the team to approve the pull request if the destination branch is master**\. The template requires two users who assume the role of **CodeCommitReview** to approve any pull request before it can be merged to the **master** branch:

   ```
   aws codecommit create-approval-rule-template --approval-rule-template-name 2-approver-rule-for-master --approval-rule-template-description "Requires two developers from the team to approve the pull request if the destination branch is master" --approval-rule-template-content "{\"Version\": \"2018-11-08\",\"DestinationReferences\": [\"refs/heads/master\"],\"Statements\": [{\"Type\": \"Approvers\",\"NumberOfApprovalsNeeded\": 2,\"ApprovalPoolMembers\": [\"arn:aws:sts::123456789012:assumed-role/CodeCommitReview/*\"]}]}"
   ```

1. If successful, this command returns output similar to the following:

   ```
   {
       "approvalRuleTemplate": {
           "approvalRuleTemplateName": "2-approver-rule-for-master",
           "creationDate": 1571356106.936,
           "approvalRuleTemplateId": "dd8b17fe-EXAMPLE",
           "approvalRuleTemplateContent": "{\"Version\": \"2018-11-08\",\"DestinationReferences\": [\"refs/heads/master\"],\"Statements\": [{\"Type\": \"Approvers\",\"NumberOfApprovalsNeeded\": 2,\"ApprovalPoolMembers\": [\"arn:aws:sts::123456789012:assumed-role/CodeCommitReview/*\"]}]}",
           "lastModifiedUser": "arn:aws:iam::123456789012:user/Mary_Major",
           "approvalRuleTemplateDescription": "Requires two developers from the team to approve the pull request if the destination branch is master",
           "lastModifiedDate": 1571356106.936,
           "ruleContentSha256": "4711b576EXAMPLE"
       }
   }
   ```