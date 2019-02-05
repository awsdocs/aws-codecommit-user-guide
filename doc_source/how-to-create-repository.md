--------

 The procedures in this guide support the new console design\. If you choose to use the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\.

--------

# Create an AWS CodeCommit Repository<a name="how-to-create-repository"></a>

Use AWS CLI or the AWS CodeCommit console to create an empty AWS CodeCommit repository\.

These instructions are written with the assumption that you have already completed the steps in [Setting Up ](setting-up.md)\. 

**Note**  
Depending on your usage, you might be charged for creating or accessing a repository\. For more information, see [Pricing](http://aws.amazon.com/codecommit/pricing) on the AWS CodeCommit product information page\.

**Topics**
+ [Create a Repository \(Console\)](#how-to-create-repository-console)
+ [Create an AWS CodeCommit Repository \(AWS CLI\)](#how-to-create-repository-cli)

## Create a Repository \(Console\)<a name="how-to-create-repository-console"></a>

To create a new AWS CodeCommit repository:

1. Open the AWS CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In the region selector, choose the region where you want to create the repository\. For more information, see [Regions and Git Connection Endpoints](regions.md)\.

1. On the **Repositories** page, choose **Create repository**\. 

1. On the **Create repository** page, in **Repository name**, enter a name for the repository\.
**Note**  
This name must be unique in the region for your AWS account\.

1. \(Optional\) In the **Description** box, enter a description for the repository\. This can help you and other users identify the purpose of the repository\. 
**Note**  
The description field displays Markdown in the console and accepts all HTML characters and valid Unicode characters\. If you are an application developer who is using the `GetRepository` or `BatchGetRepositories` APIs and you plan to display the repository description field in a web browser, see the [AWS CodeCommit API Reference](https://docs.aws.amazon.com/codecommit/latest/APIReference/)\.

1. Choose **Create**\. 

After you create a repository, you can connect to it and start adding code either through the AWS CodeCommit console or a local Git client, or by integrating your AWS CodeCommit repository with your favorite IDE\. For more information, see [Setting Up for AWS CodeCommit ](setting-up.md)\. You can also add your repository to a continuous delivery pipeline\. For more information, see [Simple Pipeline Walkthrough](https://docs.aws.amazon.com/codepipeline/latest/userguide/getting-started-cc.html)\.

To get information about the new AWS CodeCommit repository, such as the URLs to use when cloning the repository, choose the repository's name from the list, or just choose the connection protocol you want to use next to the repository's name\.

To share this repository with others, you must send them the HTTPS or SSH link to use to clone the repository\. Make sure they have the permissions required to access the repository\. For more information, see [Share a Repository](how-to-share-repository.md) and [Authentication and Access Control for AWS CodeCommit](auth-and-access-control.md)\. 

## Create an AWS CodeCommit Repository \(AWS CLI\)<a name="how-to-create-repository-cli"></a>

To create a new AWS CodeCommit repository:

1. Make sure that you have configured the AWS CLI with the region where the repository exists\. To verify the region, type the following command at the command line or terminal and review the information for default region name:

   ```
   aws configure
   ```

   The default region name must match the region for the repository in AWS CodeCommit\. For more information, see [Regions and Git Connection Endpoints](regions.md)\.

1. Run the create\-repository command, specifying:
   + A name that uniquely identifies the AWS CodeCommit repository \(with the `--repository-name` option\)\.
**Note**  
This name must be unique across an AWS account\.
   + Optionally, a comment about the AWS CodeCommit repository \(with the `--repository-description` option\)\.

   For example, to create an AWS CodeCommit repository named `MyDemoRepo` with the description `"My demonstration repository"`:

   ```
   aws codecommit create-repository --repository-name MyDemoRepo --repository-description "My demonstration repository" 
   ```
**Note**  
The description field displays Markdown in the console and accepts all HTML characters and valid Unicode characters\. If you are an application developer who is using the `GetRepository` or `BatchGetRepositories` APIs and you plan to display the repository description field in a web browser, see the [AWS CodeCommit API Reference](https://docs.aws.amazon.com/codecommit/latest/APIReference/)\.

1. If successful, this command outputs a `repositoryMetadata` object with the following information:
   + The description \(`repositoryDescription`\)\.
   + The unique, system\-generated ID \(`repositoryId`\)\.
   + The name \(`repositoryName`\)\.
   + The ID of the AWS account associated with the AWS CodeCommit repository \(`accountId`\)\.

   Here is some example output, based on the preceding example command:

   ```
   {
         "repositoryMetadata": {
             "repositoryName": "MyDemoRepo",
             "repositoryDescription": "My demonstration repository",
             "repositoryId": "f7579e13-b83e-4027-aaef-650c0EXAMPLE",
             "accountId": "creator-account-ID"
         }
     }
   ```

1. Make a note of the name and ID of the AWS CodeCommit repository\. You need them to monitor and change information about the AWS CodeCommit repository, especially if you use AWS CLI\.

   If you forget the name or ID, follow the instructions in [View AWS CodeCommit Repository Details \(AWS CLI\)](how-to-view-repository-details.md#how-to-view-repository-details-cli)\.

After you create a repository, you can connect to it and start adding code\. For more information, see [Connect to a Repository](how-to-connect.md)\. You can also add your repository to a continuous delivery pipeline\. For more information, see [Simple Pipeline Walkthrough](https://docs.aws.amazon.com/codepipeline/latest/userguide/getting-started-cc.html)\.