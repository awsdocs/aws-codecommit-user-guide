# Create an AWS CodeCommit repository<a name="how-to-create-repository"></a>

Use the AWS CodeCommit console or the AWS Command Line Interface \(AWS CLI\) to create an empty CodeCommit repository\. To add tags to a respository after you create it, see [Add a tag to a repository](how-to-tag-repository-add.md)\.

These instructions assume that you have completed the steps in [Setting up ](setting-up.md)\. 

**Note**  
Depending on your usage, you might be charged for creating or accessing a repository\. For more information, see [Pricing](http://aws.amazon.com/codecommit/pricing) on the CodeCommit product information page\.

**Topics**
+ [Create a repository \(console\)](#how-to-create-repository-console)
+ [Create a repository \(AWS CLI\)](#how-to-create-repository-cli)

## Create a repository \(console\)<a name="how-to-create-repository-console"></a>

**To create a CodeCommit repository**

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In the region selector, choose the AWS Region where you want to create the repository\. For more information, see [Regions and Git connection endpoints](regions.md)\.

1. On the **Repositories** page, choose **Create repository**\. 

1. On the **Create repository** page, in **Repository name**, enter a name for the repository\.
**Note**  
Repository names are case sensitive\. The name must be unique in the AWS Region for your AWS account\.

1. \(Optional\) In **Description**, enter a description for the repository\. This can help you and other users identify the purpose of the repository\. 
**Note**  
The description field displays Markdown in the console and accepts all HTML characters and valid Unicode characters\. If you are an application developer who is using the `GetRepository` or `BatchGetRepositories` APIs and you plan to display the repository description field in a web browser, see the [CodeCommit API Reference](https://docs.aws.amazon.com/codecommit/latest/APIReference/)\.

1. \(Optional\) Choose **Add tag** to add one or more repository tags \(a custom attribute label that helps you organize and manage your AWS resources\) to your repository\. For more information, see [Tagging repositories in AWS CodeCommit](how-to-tag-repository.md)\.

1. \(Optional\) Select **Enable Amazon CodeGuru Reviewer for Java** if this repository contains Java code, and you want CodeGuru Reviewer to analyze it\. CodeGuru Reviewer uses multiple machine learning models to find Java code defects and to suggest improvements and fixes in pull requests\. For more information, see the [https://docs.aws.amazon.com/codeguru/latest/reviewer-ug/Welcome.html](https://docs.aws.amazon.com/codeguru/latest/reviewer-ug/Welcome.html)\.

1. Choose **Create**\. 

After you create a repository, you can connect to it and start adding code either through the CodeCommit console or a local Git client, or by integrating your CodeCommit repository with your favorite IDE\. For more information, see [Setting up for AWS CodeCommit ](setting-up.md)\. You can also add your repository to a continuous delivery pipeline\. For more information, see [Simple Pipeline Walkthrough](https://docs.aws.amazon.com/codepipeline/latest/userguide/getting-started-cc.html)\.

To get information about the new CodeCommit repository, such as the URLs to use when cloning the repository, choose the repository's name from the list, or just choose the connection protocol you want to use next to the repository's name\.

To share this repository with others, you must send them the HTTPS or SSH link to use to clone the repository\. Make sure they have the permissions required to access the repository\. For more information, see [Share a repository](how-to-share-repository.md) and [Authentication and access control for AWS CodeCommit](auth-and-access-control.md)\. 

## Create a repository \(AWS CLI\)<a name="how-to-create-repository-cli"></a>

You can use the AWS CLI to create a CodeCommit repository\. Unlike the console, you can add tags to a repository if you create it using the AWS CLI\.

1. Make sure that you have configured the AWS CLI with the AWS Region where the repository exists\. To verify the Region, run the following command at the command line or terminal and review the information for default region name\.

   ```
   aws configure
   ```

   The default region name must match the AWS Region for the repository in CodeCommit\. For more information, see [Regions and Git connection endpoints](regions.md)\.

1. Run the create\-repository command, specifying:
   + A name that uniquely identifies the CodeCommit repository \(with the `--repository-name` option\)\.
**Note**  
This name must be unique across an AWS account\.
   + An optional comment about the CodeCommit repository \(with the `--repository-description` option\)\.
   + An optional key\-value pair or pairs to use as tags for the CodeCommit repository \(with the `--tags` option\)\.

   For example, to create a CodeCommit repository named `MyDemoRepo` with the description `"My demonstration repository"` and a tag with a key named *Team* with the value of *Saanvi*, use this command\.

   ```
   aws codecommit create-repository --repository-name MyDemoRepo --repository-description "My demonstration repository" --tags Team=Saanvi
   ```
**Note**  
The description field displays Markdown in the console and accepts all HTML characters and valid Unicode characters\. If you are an application developer who is using the `GetRepository` or `BatchGetRepositories` APIs and you plan to display the repository description field in a web browser, see the [CodeCommit API Reference](https://docs.aws.amazon.com/codecommit/latest/APIReference/)\.

1. If successful, this command outputs a `repositoryMetadata` object with the following information:
   + The description \(`repositoryDescription`\)\.
   + The unique, system\-generated ID \(`repositoryId`\)\.
   + The name \(`repositoryName`\)\.
   + The ID of the AWS account associated with the CodeCommit repository \(`accountId`\)\.

   The following is example output, based on the preceding example command\.

   ```
   {
       "repositoryMetadata": {
           "repositoryName": "MyDemoRepo",
           "cloneUrlSsh": "ssh://ssh://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo",
           "lastModifiedDate": 1446071622.494,
           "repositoryDescription": "My demonstration repository",
           "cloneUrlHttp": "https://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo",
           "creationDate": 1446071622.494,
           "repositoryId": "f7579e13-b83e-4027-aaef-650c0EXAMPLE",
           "Arn": "arn:aws:codecommit:us-east-2:111111111111:MyDemoRepo",
           "accountId": "111111111111"
       }
   }
   ```
**Note**  
Tags that were added when the repository was created are not returned in the output\. To view a list of tags associated with a repository, run the [list\-tags\-for\-resource](how-to-tag-repository-list.md) command\.

1. Make a note of the name and ID of the CodeCommit repository\. You need them to monitor and change information about the CodeCommit repository, especially if you use AWS CLI\.

   If you forget the name or ID, follow the instructions in [View CodeCommit repository details \(AWS CLI\)](how-to-view-repository-details.md#how-to-view-repository-details-cli)\.

After you create a repository, you can connect to it and start adding code\. For more information, see [Connect to a repository](how-to-connect.md)\. You can also add your repository to a continuous delivery pipeline\. For more information, see [Simple Pipeline Walkthrough](https://docs.aws.amazon.com/codepipeline/latest/userguide/getting-started-cc.html)\.