# Remove a tag from a repository<a name="how-to-tag-repository-delete"></a>

You can remove one or more tags associated with a repository\. Removing a tag does not delete the tag from other AWS resources that are associated with that tag\.

**Important**  
Removing tags for a repository can impact access to that repository\. Before you remove a tag from a repository, make sure to review any IAM policies that might use the key or value for a tag to control access to resources such as repositories\. For examples of tag\-based access policies, see [Example 5: Deny or allow actions on repositories with tags](auth-and-access-control-iam-identity-based-access-control.md#identity-based-policies-example-5)\.

## Remove a tag from a repository \(console\)<a name="how-to-tag-repository-delete-console"></a>

You can use the CodeCommit console to remove the association between a tag and a CodeCommit repository\. 

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In **Repositories**, choose the name of the repository where you want to remove tags\.

1. In the navigation pane, choose **Settings**\. Choose **Repository tags**\. 

1. Choose **Edit**\.

1. Find the tag you want to remove, and then choose **Remove tag**\.

1. When you have finished removing tags, choose **Submit**\.

## Remove a tag from a repository \(AWS CLI\)<a name="how-to-tag-repository-delete-cli"></a>

Follow these steps to use the AWS CLI to remove a tag from a CodeCommit repository\. Removing a tag does not delete it, but simply removes the association between the tag and the repository\. 

**Note**  
If you delete a CodeCommit repository, all tag associations are removed from the deleted repository\. You do not have to remove tags before you delete a repository\.

At the terminal or command line, run the untag\-resource command, specifying the Amazon Resource Name \(ARN\) of the repository where you want to remove tags and the tag key of the tag you want to remove\. For example, to remove a tag on a repository named *MyDemoRepo* with the tag key *Status*:

```
aws codecommit untag-resource --resource-arn arn:aws:codecommit:us-west-2:111111111111:MyDemoRepo --tag-keys Status
```

If successful, this command returns nothing\. To verify the tags associated with the repository, run the list\-tags\-for\-resource command\.