# Delete an approval rule template<a name="how-to-delete-template"></a>

You can delete an approval rule template if you are not using it in any repositories\. Deleting unused approval rule templates helps keep your templates organized and makes it easier to find templates that make sense for your workflows\.

**Topics**
+ [Delete an approval rule template \(console\)](#how-to-delete-template-console)
+ [Delete an approval rule template \(AWS CLI\)](#how-to-delete-template-cli)

## Delete an approval rule template \(console\)<a name="how-to-delete-template-console"></a>

You can delete an approval rule template if it is no longer relevant to your development work\. When you use the console to delete an approval rule template, it is disassociated from any repositories during the deletion process\.<a name="delete-template-console"></a>

## To delete an approval rule template<a name="delete-template-console"></a>

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. Choose **Approval rule templates**\. Choose the template you want to delete, and then choose **Delete**\.

## Delete an approval rule template \(AWS CLI\)<a name="how-to-delete-template-cli"></a>

You can use the AWS CLI to delete an approval rule if it has been disassociated from all repositories\. For more information, see [Disassociate an approval rule template \(AWS CLI\)](how-to-disassociate-template.md#how-to-disassociate-template-cli)\.<a name="delete-template"></a>

## To delete an approval rule template<a name="delete-template"></a>

1. At a terminal or command line, run the delete\-approval\-rule\-template command, specifying the name of the approval rule template you want to delete:

   ```
   aws codecommit delete-approval-rule-template --approval-rule-template-name 1-approver-for-all-pull-requests
   ```

1. If successful, this command returns output similar to the following\. If the approval rule template has already been deleted, this command returns nothing\.

   ```
   {
       "approvalRuleTemplateId": "41de97b7-EXAMPLE"
   }
   ```