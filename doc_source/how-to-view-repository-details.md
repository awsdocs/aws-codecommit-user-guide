# View AWS CodeCommit Repository Details<a name="how-to-view-repository-details"></a>

To view information about available repositories, you can use:

+ Git from a local repo connected to the AWS CodeCommit repository\.

+ The AWS CLI\.

+ The AWS CodeCommit console\.

Before you follow these instructions, complete the steps in [Setting Up ](setting-up.md)\.


+ [Use the AWS CodeCommit Console to View Repository Details](#how-to-view-repository-details-console)
+ [Use Git to View AWS CodeCommit Repository Details](#how-to-view-repository-details-git)
+ [Use the AWS CLI to View AWS CodeCommit Repository Details](#how-to-view-repository-details-cli)

## Use the AWS CodeCommit Console to View Repository Details<a name="how-to-view-repository-details-console"></a>

Use the AWS CodeCommit console to quickly view all repositories created with your AWS account\.

1. Open the AWS CodeCommit console at [https://console\.aws\.amazon\.com/codecommit](https://console.aws.amazon.com/codecommit)\.

1. Choose the name of the repository from the list\. 

1. Do one of the following:

   + To view the URL for cloning the repository, in the navigation pane, choose **Code**, choose **Clone URL**, and then choose the protocol you want to use when cloning the repository\.

   + To view configurable details for the repository, in the navigation pane, choose **Settings**\. 

**Note**  
If you are signed in as an IAM user, you can configure and save your preferences for viewing code and other console settings\. For more information, see [Working with User Preferences](user-preferences.md)\.

## Use Git to View AWS CodeCommit Repository Details<a name="how-to-view-repository-details-git"></a>

To use Git from a local repo to view details about AWS CodeCommit repositories, run the git remote show command\.

Before you perform these steps, connect the local repo to the AWS CodeCommit repository\. For instructions, see [Connect to a Repository](how-to-connect.md)\.

1. Run the git remote show *remote\-name* command, where *remote\-name* is the alias of the AWS CodeCommit repository \(by default, `origin`\)\.
**Tip**  
To get a list of AWS CodeCommit repository names along with their URLs, run the git remote \-v command\.

   For example, to view details about the AWS CodeCommit repository with the alias `origin`:

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

## Use the AWS CLI to View AWS CodeCommit Repository Details<a name="how-to-view-repository-details-cli"></a>

To use AWS CLI commands with AWS CodeCommit, install the AWS CLI\. For more information, see [Command Line Reference](cmd-ref.md)\. 

To use the AWS CLI to view repository details, run the following commands:

+ [list\-repositories](#how-to-view-repository-details-no-name-cli) to view a list of AWS CodeCommit repository names and their corresponding IDs\.

+ [get\-repository](#how-to-view-repository-details-with-name-cli) to view information about a single AWS CodeCommit repository\.

+ [batch\-get\-repositories](#how-to-view-repository-details-with-names-cli) to view information about multiple repositories in AWS CodeCommit\.

### To view a list of AWS CodeCommit repositories<a name="how-to-view-repository-details-no-name-cli"></a>

1. Run the list\-repositories command:

   ```
   aws codecommit list-repositories
   ```

   You can use the optional `--sort-by` or `--order` options to change the order of returned information\.

1. If successful, this command outputs a `repositories` object that contains the names and IDs of all repositories in AWS CodeCommit associated with the AWS account\.

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

### To view details about a single AWS CodeCommit repository<a name="how-to-view-repository-details-with-name-cli"></a>

1. Run the get\-repository command, specifying the name of the AWS CodeCommit repository with the `--repository-name` option\.
**Tip**  
To get the AWS CodeCommit repository's name, run the [list\-repositories](#how-to-view-repository-details-no-name-cli) command\.

   For example, to view details about an AWS CodeCommit repository named `MyDemoRepo`:

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
               "Arn": "arn:aws:codecommit:us-east-2:80398EXAMPLE:MyDemoRepo
               "accountId": "111111111111"
           }
   }
   ```

### To view details about multiple AWS CodeCommit repositories<a name="how-to-view-repository-details-with-names-cli"></a>

1. Run the batch\-get\-repositories command with the `--repository-names` option\. Add a space between each AWS CodeCommit repository name\.
**Tip**  
To get the names of the repositories in AWS CodeCommit, run the [list\-repositories](#how-to-view-repository-details-no-name-cli) command\.

   For example, to view details about two AWS CodeCommit repositories named `MyDemoRepo` and `MyOtherDemoRepo`:

   ```
   aws codecommit batch-get-repositories --repository-names MyDemoRepo MyOtherDemoRepo
   ```

1. If successful, this command outputs an object with the following information:

   + A list of any AWS CodeCommit repositories that could not be found \(`repositoriesNotFound`\)\.

   + A list of AWS CodeCommit repositories \(`repositories`\)\. Each AWS CodeCommit repository's name is followed by:

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
                   "Arn": "arn:aws:codecommit:us-east-2:80398EXAMPLE:MyDemoRepo
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
                   "Arn": "arn:aws:codecommit:us-east-2:80398EXAMPLE:MyOtherDemoRepo
                   "accountId": "111111111111"
               }
           ],
           "repositoriesNotFound": []
       }
   ```