# Associate an approval rule template with a repository<a name="how-to-associate-template"></a>

Approval rule templates are created in a specific AWS Region, but they do not affect any repositores in that AWS Region until they are associated\. To apply a template to one or more repositories, you must associate the template with the repository or repositories\. You can apply a single template to multiple repositories in an AWS Region\. This helps you automate and standardize the development workflow in your repositories by creating consistent conditions for approving and merging pull requests\.

You can only associate an approval rule template with repositories in the AWS Region where the approval rule template was created\.

**Topics**
+ [Associate an approval rule template \(console\)](#how-to-associate-template-console)
+ [Associate an approval rule template \(AWS CLI\)](#how-to-associate-template-cli)

## Associate an approval rule template \(console\)<a name="how-to-associate-template-console"></a>

You might have associated repositories with an approval rule template when you created it\. \(That step is optional\.\) You can add or remove associations by editing the template\.<a name="associate-template-console"></a>

## To associate an approval rule template with repositories<a name="associate-template-console"></a>

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. Choose **Approval rule templates**\. Choose the template, and then choose **Edit**\.

1. In **Associated Repositories**, choose the repositories from the **Repositories** list\. Each associated repository appears under the list box\.

1. Choose **Save**\. Approval rules are now applied to any pull requests created in those associated repositories\.

## Associate an approval rule template \(AWS CLI\)<a name="how-to-associate-template-cli"></a>

You can use the AWS CLI to associate an approval rule template with one or more repositories\. <a name="associate-template-repository"></a>

## To associate a template with a single repository<a name="associate-template-repository"></a>

1. At the terminal or command line, run the associate\-approval\-rule\-template\-with\-repository command, specifying:
   + The name of the approval rule template you want to associate with a repository\.
   + The name of the repository to be associated with the approval rule template\.

   For example, to associate an approval rule template named *2\-approver\-rule\-for\-master* with a repository named *MyDemoRepo*:

   ```
   aws codecommit associate-approval-rule-template-with-repository --repository-name MyDemoRepo --approval-rule-template-name 2-approver-rule-for-master
   ```

1. If successful, this command returns nothing\.<a name="batch-associate-template-repositories"></a>

## To associate a template with multiple repositories<a name="batch-associate-template-repositories"></a>

1. At the terminal or command line, run the batch\-associate\-approval\-rule\-template\-with\-repositories command, specifying:
   + The name of the approval rule template you want to associate with a repository\.
   + The names of the repositories to be associated with the approval rule template\.

   For example, to associate an approval rule template named **2\-approver\-rule\-for\-master** with a repository named **MyDemoRepo** and **MyOtherDemoRepo**:

   ```
   aws codecommit batch-associate-approval-rule-template-with-repositories --repository-names "MyDemoRepo", "MyOtherDemoRepo" --approval-rule-template-name 2-approver-rule-for-master
   ```

1. If successful, this command returns output similar to the following:

   ```
   {
       "associatedRepositoryNames": [
           "MyDemoRepo",
           "MyOtherDemoRepo"
       ],
       "errors": []
   }
   ```