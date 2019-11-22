# View Tags for a Repository<a name="how-to-tag-repository-list"></a>

Tags can help you identify and organize your AWS resources and manage access to them\. For tips on using tags, see the [AWS Tagging Strategies](https://aws.amazon.com/answers/account-management/aws-tagging-strategies/) post on the *AWS Answers* blog\. For examples of tag\-based access policies, see [Example 5: Deny or Allow Actions on Repositories with Tags](auth-and-access-control-iam-identity-based-access-control.md#identity-based-policies-example-5)\.

## View Tags for a Repository \(Console\)<a name="how-to-tag-repository-list-console"></a>

You can use the CodeCommit console to view the tags associated with a CodeCommit repository\. 

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In **Repositories**, choose the name of the repository where you want to view tags\.

1. In the navigation pane, choose **Settings**\. Choose **Repository tags**\.   
![\[Viewing tags for a repository\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-repository-tags-view.png)

## View Tags for a Repository \(AWS CLI\)<a name="how-to-tag-repository-list-cli"></a>

Follow these steps to use the AWS CLI to view the AWS tags for a CodeCommit repository\. If no tags have been added, the returned list is empty\.

At the terminal or command line, run the list\-tags\-for\-resource command\. For example, to view a list of tag keys and tag values for a repository named *MyDemoRepo* with the ARN *arn:aws:codecommit:us\-east\-2:111111111111:MyDemoRepo*:

```
aws codecommit list-tags-for-resource --resource-arn arn:aws:codecommit:us-west-2:111111111111:MyDemoRepo
```

If successful, this command returns information similar to the following:

```
{
    "tags": {
        "Status": "Secret",
        "Team": "Saanvi"
    }
}
```