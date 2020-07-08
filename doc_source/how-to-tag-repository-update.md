# Edit tags for a repository<a name="how-to-tag-repository-update"></a>

You can change the value for a tag associated with a repository\. You can also change the name of the key, which is equivalent to removing the current tag and adding a different one with the new name and the same value as the other key\. Keep in mind that there are limits on the characters you can use in the key and value fields\. For more information, see [Limits](limits.md#limits-tags)\.

**Important**  
Editing tags for a repository can impact access to that repository\. Before you edit the name \(key\) or value of a tag for a repository, make sure to review any IAM policies that might use the key or value for a tag to control access to resources such as repositories\. For examples of tag\-based access policies, see [Example 5: Deny or allow actions on repositories with tags](auth-and-access-control-iam-identity-based-access-control.md#identity-based-policies-example-5)\.

## Edit a tag for a repository \(console\)<a name="how-to-tag-repository-update-console"></a>

You can use the CodeCommit console to edit the tags associated with a CodeCommit repository\. 

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In **Repositories**, choose the name of the repository where you want to edit tags\.

1. In the navigation pane, choose **Settings**\. Choose **Repository tags**\. 

1. Choose **Edit**\.

1.   
![\[Editing the value of a tag for a repository\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-repository-tags-edit2.png)

   Do one of the following:
   + To change the tag, enter a new name in **Key**\. Changing the name of the tag is the equivalent of removing a tag and adding a new tag with the new key name\.
   + To change the value of a tag, enter a new value\. If you want to change the value to nothing, delete the current value and leave the field blank\.

1. When you have finished editing tags, choose **Submit**\.

## Edit tags for a repository \(AWS CLI\)<a name="how-to-tag-repository-update-cli"></a>

Follow these steps to use the AWS CLI to update a tag for a CodeCommit repository\. You can change the value for an existing key, or add another key\. 

At the terminal or command line, run the tag\-resource command, specifying the Amazon Resource Name \(ARN\) of the repository where you want to update a tag and specify the tag key and tag value:

```
aws codecommit tag-resource --resource-arn arn:aws:codecommit:us-west-2:111111111111:MyDemoRepo --tags Team=Li
```