# Tagging repositories in AWS CodeCommit<a name="how-to-tag-repository"></a>

A *tag* is a custom attribute label that you or AWS assigns to an AWS resource\. AWS tags are different from Git tags, which can be applied to commits\. Each AWS tag has two parts:
+ A *tag key* \(for example, `CostCenter`, `Environment`, `Project`, or `Secret`\)\. Tag keys are case sensitive\.
+ An optional field known as a *tag value* \(for example, `111122223333`, `Production`, or a team name\)\. Omitting the tag value is the same as using an empty string\. Like tag keys, tag values are case sensitive\.

Together these are known as key\-value pairs\. For limits on the number of tags you can have on a repository and restrictions on tag keys and values, see [Limits](limits.md#limits-tags)\.

Tags help you identify and organize your AWS resources\. Many AWS services support tagging, so you can assign the same tag to resources from different services to indicate that the resources are related\. For example, you can assign the same tag to a CodeCommit repository that you assign to an Amazon S3 bucket\. For more information about tagging strategies, see [Tagging AWS Resources](https://docs.aws.amazon.com/general/latest/gr/aws_tagging.html)\. 

In CodeCommit, the primary resource is a repository\. You can use the CodeCommit console, the AWS CLI, CodeCommit APIs, or AWS SDKs to add, manage, and remove tags for a repository\. In addition to identifying, organizing, and tracking your repository with tags, you can use tags in IAM policies to help control who can view and interact with your repository\. For examples of tag\-based access policies, see [Example 5: Deny or allow actions on repositories with tags](auth-and-access-control-iam-identity-based-access-control.md#identity-based-policies-example-5)\.

**Topics**
+ [Add a tag to a repository](how-to-tag-repository-add.md)
+ [View tags for a repository](how-to-tag-repository-list.md)
+ [Edit tags for a repository](how-to-tag-repository-update.md)
+ [Remove a tag from a repository](how-to-tag-repository-delete.md)