# Create an AWS CodeCommit Repository<a name="how-to-create-repository"></a>

Use AWS CLI or the AWS CodeCommit console to create a new, empty AWS CodeCommit repository\.

These instructions assume you have already completed the steps in [Setting Up ](setting-up.md)\. 

**Note**  
Depending on your usage, you might be charged for creating or accessing a repository\. For more information, see [Pricing](http://aws.amazon.com/codecommit/pricing) on the AWS CodeCommit product information page\.


+ [Use the AWS CodeCommit Console to Create a Repository](#how-to-create-repository-console)
+ [Use the AWS CLI to Create an AWS CodeCommit Repository](#how-to-create-repository-cli)

## Use the AWS CodeCommit Console to Create a Repository<a name="how-to-create-repository-console"></a>

To create a new AWS CodeCommit repository \(console\):

1. Open the AWS CodeCommit console at [https://console\.aws\.amazon\.com/codecommit](https://console.aws.amazon.com/codecommit)\.

1. In the region selector, choose the region where you will create the repository\. For more information, see [Regions and Git Connection Endpoints](regions.md)\.

1. On the **Dashboard** page, choose **Create repository**\. \(If a welcome page appears instead of the **Dashboard** page, choose **Get Started Now**\.\) 

1. On the **Create repository** page, in **Repository name**, type a name for the repository\.
**Note**  
This name must be unique in the region for your AWS account\.

1. \(Optional\) In the **Description** box, type a description for the repository\. This can help you and other users identify the purpose of the repository\. 
**Note**  
The description field accepts all HTML characters and all valid Unicode characters\. If you are an application developer using the `GetRepository` or `BatchGetRepositories` APIs and plan to display the repository description field in a web browser, see the [AWS CodeCommit API Reference](http://docs.aws.amazon.com/codecommit/latest/APIReference/)\.

1. Choose **Create repository**\. 

1. In **Configure email notifications**, configure notifications so that repository users receive emails about important repository events\. This step is optional, but recommended\. You can choose the event types \(for example, comments on code\) and whether to use an existing Amazon SNS topic or create one specifically for this purpose\. You can choose to skip this step and configure notifications at a later time\. For more information, see [Configuring Notifications for Events in an AWS CodeCommit Repository](how-to-repository-email.md)\. 

After you create a repository, you can connect to it and start adding code\. To learn more, see [Connect to a Repository](how-to-connect.md)\. You can also add your repository to a continuous delivery pipeline\. To learn more, see [Simple Pipeline Walkthrough](http://docs.aws.amazon.com/codepipeline/latest/userguide/getting-started-cc.html)\.

To get information about the new AWS CodeCommit repository, such as the URLs to use when cloning the repository, choose the repository's name from the list\.

To share this repository with others, you will need to send them the HTTPS or SSH link to use to clone the repository\. Make sure they have the permissions required to access the repository\. For more information, see [Share a Repository](how-to-share-repository.md) and [Authentication and Access Control for AWS CodeCommit](auth-and-access-control.md)\. 

## Use the AWS CLI to Create an AWS CodeCommit Repository<a name="how-to-create-repository-cli"></a>

To create a new AWS CodeCommit repository \(CLI\):

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
The description field accepts all HTML characters and all valid Unicode characters\. If you are an application developer using the `GetRepository` or `BatchGetRepositories` APIs and plan to display the repository description field in a web browser, see the [AWS CodeCommit API Reference](http://docs.aws.amazon.com/codecommit/latest/APIReference/)\.

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

1. Note the AWS CodeCommit repository's name and ID\. You will need them to monitor and change information about the AWS CodeCommit repository, especially if you use AWS CLI\.

   If you forget the AWS CodeCommit repository's name or ID, follow the instructions in [Use the AWS CLI to View AWS CodeCommit Repository Details](how-to-view-repository-details.md#how-to-view-repository-details-cli)\.

After you create a repository, you can connect to it and start adding code\. To learn more, see [Connect to a Repository](how-to-connect.md)\. You can also add your repository to a continuous delivery pipeline\. To learn more, see [Simple Pipeline Walkthrough](http://docs.aws.amazon.com/codepipeline/latest/userguide/getting-started-cc.html)\.