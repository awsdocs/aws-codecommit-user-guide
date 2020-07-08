# Override approval rules on a pull request<a name="how-to-override-approval-rules"></a>

In the normal course of development, you want users to meet the conditions of approval rules before you merge pull requests\. However, there might be times when you need to expedite merging a pull request\. For example, you might want to put a bug fix into production, but no one in the approval pool is available to approve the pull request\. In cases like these, you can choose to override the approval rules on a pull request\. You can override all approval rules for a pull request, including those created specifically for the pull request and generated from an approval rule template\. You cannot selectively override a specific approval rule, just all rules\. After you have set aside the approval rule requirements by overriding the rules, you can merge the pull request to its destination branch\.

When you override approval rules on a pull request, information about the user who overrode the rules is recorded in the activity for the pull request\. This way you can go back into the history of a pull request and review who overrode the rules\. You can also choose to revoke the override if the pull request is still open\. After the pull request has been merged, you can no longer revoke the override\.

**Topics**
+ [Override approval rules \(console\)](#how-to-override-approval-rules-console)
+ [Override approval rules \(AWS CLI\)](#how-to-override-approval-rules-cli)

## Override approval rules \(console\)<a name="how-to-override-approval-rules-console"></a>

You can override the requirements of approval rules on a pull request in the console, as part of reviewing a pull request\. If you change your mind, you can revoke your override, and the approval rule requirements are reapplied\. You can only override approval rules or revoke an override if the pull request is still open\. If it is merged or closed, you cannot change its override state\.

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In **Repositories**, choose the name of the repository\. 

1. In the navigation pane, choose **Pull requests**\. Choose the pull request where you want to override approval rule requirements, or revoke an override\.

1. On the **Approvals** tab, choose **Override approval rules**\. The requirements are set aside, and the button text changes to **Revoke override**\. To reapply the approval rule requirements, choose **Revoke override**\.

## Override approval rules \(AWS CLI\)<a name="how-to-override-approval-rules-cli"></a>

You can use the AWS CLI to override approval rule requirements\. You can also use it to view the override status for a pull request\.<a name="override-approval-rules"></a>

## To override approval rule requirements on a pull request<a name="override-approval-rules"></a>

1. At a terminal or command line, run the override\-pull\-request\-approval\-rules command, specifying:
   + The system\-generated ID of the pull request\.
   + The latest revision ID of the pull request\. To view this information, use get\-pull\-request\.
   + The status you want for the override, `OVERRIDE` or `REVOKE`\. The `REVOKE` status removes the `OVERRIDE` status but is not saved\.

   For example, to override approval rules on a pull request with an ID of **34** and a revision ID of **927df8d8EXAMPLE**:

   ```
   aws codecommit override-pull-request-approval-rules --pull-request-id 34 --revision-id 927df8d8dEXAMPLE --override-status OVERRIDE
   ```

1. If successful, this command returns nothing\.

1. To revoke the override on a pull request with an ID of **34** and a revision ID of **927df8d8EXAMPLE**:

   ```
   aws codecommit override-pull-request-approval-rules --pull-request-id 34 --revision-id 927df8d8dEXAMPLE --override-status REVOKE
   ```<a name="get-override-status"></a>

## To get information about the override status of a pull request<a name="get-override-status"></a>

1. At a terminal or command line, run the get\-pull\-request\-override\-state command, specifying:
   + The system\-generated ID of the pull request\.
   + The latest revision ID of the pull request\. To view this information, use get\-pull\-request\.

   For example, to view the override state for a pull request with an ID of **34** and a revision ID of **927df8d8EXAMPLE**:

   ```
   aws codecommit get-pull-request-override-state --pull-request-id 34 --revision-id 927df8d8dEXAMPLE
   ```

1. If successful, this command produces output similar to the following:

   ```
   {
       "overridden": true,
       "overrider": "arn:aws:iam::123456789012:user/Mary_Major"
   }
   ```