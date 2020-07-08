# View CodeCommit repository details<a name="how-to-view-repository-details"></a>

You can use the AWS CodeCommit console, AWS CLI, or Git from a local repo connected to the CodeCommit repository to view information about available repositories\.

Before you follow these instructions, complete the steps in [Setting up ](setting-up.md)\.

**Topics**
+ [View repository details \(console\)](#how-to-view-repository-details-console)
+ [View CodeCommit repository details \(Git\)](#how-to-view-repository-details-git)
+ [View CodeCommit repository details \(AWS CLI\)](#how-to-view-repository-details-cli)

## View repository details \(console\)<a name="how-to-view-repository-details-console"></a>

Use the AWS CodeCommit console to quickly view all repositories created with your AWS account\.

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In **Repositories**, choose the name of the repository\. 

1. Do one of the following:
   + To view the URL for cloning the repository, choose **Clone URL**, and then choose the protocol you want to use when cloning the repository\. This copies the clone URL\. To review it, paste it into a plain\-text editor\.
   + To view configurable details for the repository, in the navigation pane, choose **Settings**\. 

**Note**  
If you are signed in as an IAM user, you can configure and save your preferences for viewing code and other console settings\. For more information, see [Working with user preferences](user-preferences.md)\.

## View CodeCommit repository details \(Git\)<a name="how-to-view-repository-details-git"></a>

To use Git from a local repo to view details about CodeCommit repositories, run the git remote show command\.

Before you perform these steps, connect the local repo to the CodeCommit repository\. For instructions, see [Connect to a repository](how-to-connect.md)\.

1. Run the git remote show *remote\-name* command, where *remote\-name* is the alias of the CodeCommit repository \(by default, `origin`\)\.
**Tip**  
To get a list of CodeCommit repository names and their URLs, run the git remote \-v command\.

   For example, to view details about the CodeCommit repository with the alias `origin`:

   ```
   git remote show origin
   ```

1. For HTTPS:

   ```
   * remote origin
     Fetch URL: https://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo
     Push  URL: https://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo
     HEAD branch: (unknown)
     Remote branches:
       MyNewBranch tracked
       master tracked
     Local ref configured for 'git pull':
       MyNewBranch merges with remote MyNewBranch (up to date)
     Local refs configured for 'git push':
       MyNewBranch pushes to MyNewBranch (up to date)
       master pushes to master (up to date)
   ```

   For SSH:

   ```
   * remote origin
     Fetch URL: ssh://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo
     Push  URL: ssh://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo
     HEAD branch: (unknown)
     Remote branches:
       MyNewBranch tracked
       master tracked
     Local ref configured for 'git pull':
       MyNewBranch merges with remote MyNewBranch (up to date)
     Local refs configured for 'git push':
       MyNewBranch pushes to MyNewBranch (up to date)
       master pushes to master (up to date)
   ```
**Tip**  
To look up the SSH key ID for your IAM user, open the IAM console and expand **Security Credentials** on the IAM user details page\. The SSH key ID can be found in **SSH Keys for AWS CodeCommit**\. 

For more options, see your Git documentation\.

## View CodeCommit repository details \(AWS CLI\)<a name="how-to-view-repository-details-cli"></a>

To use AWS CLI commands with CodeCommit, install the AWS CLI\. For more information, see [Command line reference](cmd-ref.md)\. 

To use the AWS CLI to view repository details, run the following commands:
+ To view a list of CodeCommit repository names and their corresponding IDs, run [list\-repositories](#how-to-view-repository-details-no-name-cli)\.
+ To view information about a single CodeCommit repository, run [get\-repository](#how-to-view-repository-details-with-name-cli)\.
+ To view information about multiple repositories in CodeCommit, run [batch\-get\-repositories](#how-to-view-repository-details-with-names-cli)\.

### To view a list of CodeCommit repositories<a name="how-to-view-repository-details-no-name-cli"></a>

1. Run the list\-repositories command:

   ```
   aws codecommit list-repositories
   ```

   You can use the optional `--sort-by` or `--order` options to change the order of returned information\.

1. If successful, this command outputs a `repositories` object that contains the names and IDs of all repositories in CodeCommit associated with the AWS account\.

   Here is some example output based on the preceding command:

   ```
   {
       "repositories": [
           {
              "repositoryName": "MyDemoRepo"
              "repositoryId": "f7579e13-b83e-4027-aaef-650c0EXAMPLE",
           },
           {
              "repositoryName": "MyOtherDemoRepo"
              "repositoryId": "cfc29ac4-b0cb-44dc-9990-f6f51EXAMPLE"
           }
       ]
   }
   ```

### To view details about a single CodeCommit repository<a name="how-to-view-repository-details-with-name-cli"></a>

1. Run the get\-repository command, specifying the name of the CodeCommit repository with the `--repository-name` option\.
**Tip**  
To get the name of the CodeCommit repository, run the [list\-repositories](#how-to-view-repository-details-no-name-cli) command\.

   For example, to view details about a CodeCommit repository named `MyDemoRepo`:

   ```
   aws codecommit get-repository --repository-name MyDemoRepo
   ```

1. If successful, this command outputs a `repositoryMetadata` object with the following information:
   + The repository's name \(`repositoryName`\)\.
   + The repository's description \(`repositoryDescription`\)\.
   + The repository's unique, system\-generated ID \(`repositoryId`\)\.
   + The ID of the AWS account associated with the repository \(`accountId`\)\.

   Here is some example output, based on the preceding example command:

   ```
   {
           "repositoryMetadata": {
               "creationDate": 1429203623.625,
               "defaultBranch": "master",
               "repositoryName": "MyDemoRepo",
               "cloneUrlSsh": "ssh://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo",
               "lastModifiedDate": 1430783812.0869999,
               "repositoryDescription": "My demonstration repository",
               "cloneUrlHttp": "https://codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo",
               "repositoryId": "f7579e13-b83e-4027-aaef-650c0EXAMPLE",
               "Arn": "arn:aws:codecommit:us-east-2:111111111111:MyDemoRepo"
               "accountId": "111111111111"
           }
   }
   ```

### To view details about multiple CodeCommit repositories<a name="how-to-view-repository-details-with-names-cli"></a>

1. Run the batch\-get\-repositories command with the `--repository-names` option\. Add a space between each CodeCommit repository name\.
**Tip**  
To get the names of the repositories in CodeCommit, run the [list\-repositories](#how-to-view-repository-details-no-name-cli) command\.

   For example, to view details about two CodeCommit repositories named `MyDemoRepo` and `MyOtherDemoRepo`:

   ```
   aws codecommit batch-get-repositories --repository-names MyDemoRepo MyOtherDemoRepo
   ```

1. If successful, this command outputs an object with the following information:
   + A list of any CodeCommit repositories that could not be found \(`repositoriesNotFound`\)\.
   + A list of CodeCommit repositories \(`repositories`\)\. Each CodeCommit repository name is followed by:
     + The repository's description \(`repositoryDescription`\)\.
     + The repository's unique, system\-generated ID \(`repositoryId`\)\.
     + The ID of the AWS account associated with the repository \(`accountId`\)\.

   Here is some example output, based on the preceding example command:

   ```
   {
           "repositoriesNotFound": [],
           "repositories": [
                {
                   "creationDate": 1429203623.625,
                   "defaultBranch": "master",
                   "repositoryName": "MyDemoRepo",
                   "cloneUrlSsh": "ssh://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo",
                   "lastModifiedDate": 1430783812.0869999,
                   "repositoryDescription": "My demonstration repository",
                   "cloneUrlHttp": "https://codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo",
                   "repositoryId": "f7579e13-b83e-4027-aaef-650c0EXAMPLE",
                   "Arn": "arn:aws:codecommit:us-east-2:111111111111:MyDemoRepo"
                   "accountId": "111111111111"
               },
               {
                   "creationDate": 1429203623.627,
                   "defaultBranch": "master",
                   "repositoryName": "MyOtherDemoRepo",
                   "cloneUrlSsh": "ssh://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyOtherDemoRepo",
                   "lastModifiedDate": 1430783812.0889999,
                   "repositoryDescription": "My other demonstration repository",
                   "cloneUrlHttp": "https://codecommit.us-east-2.amazonaws.com/v1/repos/MyOtherDemoRepo",
                   "repositoryId": "cfc29ac4-b0cb-44dc-9990-f6f51EXAMPLE",
                   "Arn": "arn:aws:codecommit:us-east-2:111111111111:MyOtherDemoRepo"
                   "accountId": "111111111111"
               }
           ],
           "repositoriesNotFound": []
       }
   ```