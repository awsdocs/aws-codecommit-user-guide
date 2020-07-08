# Add a tag to a repository<a name="how-to-tag-repository-add"></a>

Adding tags to a repository can help you identify and organize your AWS resources and manage access to them\. First, you add one or more tags \(key\-value pairs\) to a repository\. Keep in mind that there are limits on the number of tags you can have on a repository\. There are restrictions on the characters you can use in the key and value fields\. For more information, see [Limits](limits.md#limits-tags)\. After you have tags, you can create IAM policies to manage access to the repository based on these tags\. You can use the the CodeCommit console or the AWS CLI to add tags to a repository\. 

**Important**  
Adding tags to a repository can impact access to that repository\. Before you add a tag to a repository, make sure to review any IAM policies that might use tags to control access to resources such as repositories\. For examples of tag\-based access policies, see [Example 5: Deny or allow actions on repositories with tags](auth-and-access-control-iam-identity-based-access-control.md#identity-based-policies-example-5)\.

For more information about adding tags to a repository when you create it, see [Create a repository \(console\)](how-to-create-repository.md#how-to-create-repository-console)\.

**Topics**
+ [Add a tag to a repository \(console\)](#how-to-tag-repository-add-console)
+ [Add a tag to a repository \(AWS CLI\)](#how-to-tag-repository-add-cli)

## Add a tag to a repository \(console\)<a name="how-to-tag-repository-add-console"></a>

You can use the CodeCommit console to add one or more tags to a CodeCommit repository\. 

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In **Repositories**, choose the name of the repository where you want to add tags\.

1. In the navigation pane, choose **Settings**\. Choose **Repository tags**\.

1. If no tags have been added to the repository, choose **Add tag**\. Otherwise, choose **Edit**, and then choose **Add tag**\.

1. In **Key**, enter a name for the tag\. You can add an optional value for the tag in **Value**\.   
![\[Adding a tag to a repository\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-repository-tags-edit.png)

1. \(Optional\) To add another tag, choose **Add tag** again\.

1. When you have finished adding tags, choose **Submit**\.

## Add a tag to a repository \(AWS CLI\)<a name="how-to-tag-repository-add-cli"></a>

Follow these steps to use the AWS CLI to add a tag to a CodeCommit repository\. To add a tag to a repository when you create it, see [Create a repository \(AWS CLI\)](how-to-create-repository.md#how-to-create-repository-cli)\.

In these steps, we assume that you have already installed a recent version of the AWS CLI or updated to the current version\. For more information, see [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html)\.

At the terminal or command line, run the tag\-resource command, specifying the Amazon Resource Name \(ARN\) of the repository where you want to add tags and the key and value of the tag you want to add\. You can add more than one tag to a repository\. For example, to tag a repository named *MyDemoRepo* with two tags, a tag key named *Status* with the tag value of *Secret*, and a tag key named *Team* with the tag value of *Saanvi*:

```
aws codecommit tag-resource --resource-arn arn:aws:codecommit:us-west-2:111111111111:MyDemoRepo --tags Status=Secret,Team=Saanvi 
```

If successful, this command returns nothing\.