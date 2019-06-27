# Tagging Repositories in AWS CodeCommit<a name="how-to-tag-repository"></a>

A *tag* is a custom attribute label that you or AWS assigns to an AWS resource\. AWS tags are different from Git tags, which can be applied to commits\. Each AWS tag has two parts:
+ A *tag key* \(for example, `CostCenter`, `Environment`, `Project`, or `Secret`\)\. Tag keys are case sensitive\.
+ An optional field known as a *tag value* \(for example, `111122223333`, `Production`, or a team name\)\. Omitting the tag value is the same as using an empty string\. Like tag keys, tag values are case sensitive\.

Together these are known as key\-value pairs\. 

Tags help you identify and organize your AWS resources\. Many AWS services support tagging, so you can assign the same tag to resources from different services to indicate that the resources are related\. For example, you can assign the same tag to a CodeCommit repository that you assign to an Amazon S3 bucket\. For tips on using tags, see the [AWS Tagging Strategies](https://aws.amazon.com/answers/account-management/aws-tagging-strategies/) post on the *AWS Answers* blog\. 

In CodeCommit, the primary resource is a repository\. You can use the AWS CLI, CodeCommit APIs, or AWS SDKs to add, manage, and remove tags for a repository\. In addition to identifying, organizing, and tracking your repository with tags, you can use tags in IAM policies to help control who can view and interact with your repository\. For examples of tag\-based access policies, see [Example 5: Deny or Allow Actions on Repositories with Tags](auth-and-access-control-iam-identity-based-access-control.md#identity-based-policies-example-5)\.

**Topics**
+ [Add a Tag to a Repository](#how-to-tag-repository-add)
+ [View Tags for a Repository](#how-to-tag-repository-list)
+ [Update Tags for a Repository](#how-to-tag-repository-update)
+ [Remove a Tag from a Repository](#how-to-tag-repository-delete)

## Add a Tag to a Repository<a name="how-to-tag-repository-add"></a>

Follow these steps to use the AWS CLI to add a tag to a CodeCommit repository\. To add a tag to a repository when you create it, see [Create a Repository \(AWS CLI\)](how-to-create-repository.md#how-to-create-repository-cli)\.

In these steps, we assume that you have already installed a recent version of the AWS CLI or updated to the current version\. For more information, see [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html)\.

At the terminal or command line, run the tag\-resource command, specifying the Amazon Resource Name \(ARN\) of the repository where you want to add tags and the key and value of the tag you want to add\. You can add more than one tag to a repository\. For example, to tag a repository named *MyDemoRepo* with two tags, a tag key named *Status* with the tag value of *Secret*, and a tag key named *Team* with the tag value of *Saanvi*:

```
aws codecommit tag-resource --resource-arn arn:aws:codecommit:us-west-2:111111111111:MyDemoRepo --tags Status=Secret,Team=Saanvi 
```

If successful, this command returns nothing\.

## View Tags for a Repository<a name="how-to-tag-repository-list"></a>

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

## Update Tags for a Repository<a name="how-to-tag-repository-update"></a>

Follow these steps to use the AWS CLI to update a tag for a CodeCommit repository\. You can change the value for an existing key, or add another key\. You can also remove tags from a repository, as shown in the next section\.

At the terminal or command line, run the tag\-resource command, specifying the Amazon Resource Name \(ARN\) of the repository where you want to update a tag and specify the tag key and tag value:

```
aws codecommit tag-resource --resource-arn arn:aws:codecommit:us-west-2:111111111111:MyDemoRepo --tags Team=Li
```

## Remove a Tag from a Repository<a name="how-to-tag-repository-delete"></a>

Follow these steps to use the AWS CLI to remove a tag from a CodeCommit repository\. Removing a tag does not delete it, but simply removes the association between the tag and the repository\. 

**Note**  
If you delete a CodeCommit repository, all tag associations are removed from the deleted repository\. You do not have to remove tags before deleting a repository\.

At the terminal or command line, run the untag\-resource command, specifying the Amazon Resource Name \(ARN\) of the repository where you want to remove tags and the tag key of the tag you want to remove\. For example, to remove a tag on a repository named *MyDemoRepo* with the tag key *Status*:

```
aws codecommit untag-resource --resource-arn arn:aws:codecommit:us-west-2:111111111111:MyDemoRepo --tag-keys Status
```

If successful, this command returns nothing\. To verify the tags associated with the repository, run the list\-tags\-for\-resource command\.
