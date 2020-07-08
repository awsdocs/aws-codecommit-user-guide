# Working with approval rule templates<a name="approval-rule-templates"></a>

You can create approval rules for pull requests\. However, if you want to have one or more approval rules automatically applied to some or all of the pull requests created in repositories, use approval rule templates\. Approval rule templates help you customize your development workflows across repositories so that different branches have appropriate levels of approvals and control\. You can define different rules for production and development branches\. Those rules are applied every time a pull request that matches the rule conditions is created\.

An approval rule template can be associated with one or more repositories in the AWS Region where they are created\. When a template is associated with a repository, it automatically creates approval rules for pull requests in that repository as part of creating the pull request\. Just like a single approval rule, an approval rule template defines an approval rule structure, including the number of required approvals and an optional pool of users from which approvals must come\. Unlike an approval rule, you can also define destination references \(the branch or branches\), also known as *branch filters*\. If you define destination references, then only pull requests whose destination branch names match the specified branch names \(destination references\) in the template have rules created for them\. So, for example, if you specify **refs/heads/master** as a destination reference, the approval rule defined in the template is only applied to pull requests if the destination branch is `master`\.

![\[An approval rule template that requires 1 approver from a defined approval rule if a pull request is created on the master branch, associated with two repositories\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-approval-rule-template.png)

**Topics**
+ [Create an approval rule template](how-to-create-template.md)
+ [Associate an approval rule template with a repository](how-to-associate-template.md)
+ [Manage approval rule templates](how-to-manage-templates.md)
+ [Disassociate an approval rule template](how-to-disassociate-template.md)
+ [Delete an approval rule template](how-to-delete-template.md)