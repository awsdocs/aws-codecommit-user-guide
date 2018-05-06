# View Pull Requests in an AWS CodeCommit Repository<a name="how-to-view-pull-request"></a>

You can use the AWS CodeCommit console or the the AWS CLI to view pull requests for your repository\. By default, you see only open pull requests, but you can change the filter to view all pull requests, only closed requests, only pull requests that you created, and more\. 

**Topics**
+ [Use the AWS CodeCommit Console to View Pull Requests](#how-to-view-pull-request-console)
+ [Use the AWS CLI to View Pull Requests](#how-to-view-pull-request-cli)

## Use the AWS CodeCommit Console to View Pull Requests<a name="how-to-view-pull-request-console"></a>

You can use the AWS CodeCommit console to view a list of pull requests in an AWS CodeCommit repository\. By changing the filter, you can change the list display to only show you a certain set of pull requests\. For example, you can view a list of pull requests you created with a status of **Open**, or you can choose a different filter and view pull requests you created with a status of **Closed**\.

1. Open the AWS CodeCommit console at [https://console\.aws\.amazon\.com/codecommit](https://console.aws.amazon.com/codecommit)\.

1. In the list of repositories, choose the name of the repository\. 

1. In the navigation pane, choose **Pull Requests**\.

1. By default, a list of all open pull requests is displayed\. Pull requests are displayed in the order of most recent activity\.  
![\[Open pull requests displayed in the AWS CodeCommit console.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-pull-request-view.png)

1. To display other pull requests, in **View**, choose the display filter that meets your needs:
   + **All open requests** \(default\): Displays all pull requests with a status of **Open**\.
   + **All requests**: Displays all pull requests\.
   + **All closed requests**: Displays all pull requests with a status of **Closed**\.
   + **All my requests**: Displays all pull requests that you created, regardless of the status\. It does not display reviews that you have commented on or otherwise participated in\.
   + **All my open requests**: Displays all pull requests that you created with a status of **Open**\.
   + **All my closed requests**: Displays all pull requests that you created with a status of **Closed**\.

1. When you find a pull request in the displayed list that you would like to view, choose it\.

## Use the AWS CLI to View Pull Requests<a name="how-to-view-pull-request-cli"></a>

To use AWS CLI commands with AWS CodeCommit, install the AWS CLI\. For more information, see [Command Line Reference](cmd-ref.md)\. 

To use the AWS CLI to view pull requests in an AWS CodeCommit repository, follow these steps\.

1. To view a list of pull requests in a repository, run the list\-pull\-requests command, specifying:
   + The name of the AWS CodeCommit repository where you want to view pull requests \(with the \-\-repository\-name option\)\.
   + \(Optional\) The status of the pull request \(with the \-\-pull\-request\-status option\)\.
   + \(Optional\) The Amazon Resource Name \(ARN\) of the IAM user who created the pull request \(with the \-\-author\-arn option\)\.
   + \(Optional\) An enumeration token that can be used to return batches of results \(with the \-\-next\-token option\) 
   + \(Optional\) A limit on the number of returned results per request \(with the \-\-max\-results option\)\.

   For example, to list pull requests created by an IAM user with the ARN *arn:aws:iam::111111111111:user/Li\_Juan* and the status of *CLOSED* in an AWS CodeCommit repository named `MyDemoRepo`:

   ```
   aws codecommit list-pull-requests --author-arn arn:aws:iam::111111111111:user/Li_Juan --pull-request-status CLOSED --repository-name MyDemoRepo 
   ```

   If successful, this command produces output similar to the following:

   ```
   {
      "nextToken": "",
      "pullRequestIds": ["2","12","16","22","23","35","30","39","47"]
   }
   ```

   Pull request IDs are displayed in the order of most recent activity\.

1. To view details of a pull request, run the get\-pull\-request command with the \-\-pull\-request\-id option, specifying the ID of the pull request\. For example, to view information about a pull request with the ID of *42*:

   ```
   aws codecommit get-pull-request --pull-request-id 42
   ```

   If successful, this command produces output similar to the following:

   ```
   {
      "pullRequest": { 
         "authorArn": "arn:aws:iam::111111111111:user/Jane_Doe",
         "title": "Pronunciation difficulty analyzer"
         "pullRequestTargets": [ 
            { 
               "destinationReference": "refs/heads/master",
               "destinationCommit": "5d036259EXAMPLE",
               "sourceReference": "refs/heads/jane-branch"
               "sourceCommit": "317f8570EXAMPLE",
               "repositoryName": "MyDemoRepo",
               "mergeMetadata": { 
                  "isMerged": false,
               }, 
            }
         ],
         "lastActivityDate": 1508442444,
         "pullRequestId": "42", 
         "clientRequestToken": "123Example",
         "pullRequestStatus": "OPEN",
         "creationDate": 1508962823,
         "description": "A code review of the new feature I just added to the service.",
      }
   }
   ```

1. To view events in a pull request, run the describe\-pull\-request\-events command with the \-\-pull\-request\-id option, specifying the ID of the pull request\. For example, to view the events for a pull request with the ID of *8*:

   ```
   aws codecommit describe-pull-request-events --pull-request-id 8
   ```

   If successful, this command produces output similar to the following:

   ```
   {
       "pullRequestEvents": [
           {
               "pullRequestId": "8",
               "pullRequestEventType": "PULL_REQUEST_CREATED",
               "eventDate": 1510341779.53,
               "actor": "arn:aws:iam::111111111111:user/Zhang_Wei"
           },
           {
               "pullRequestStatusChangedEventMetadata": {
                   "pullRequestStatus": "CLOSED"
               },
               "pullRequestId": "8",
               "pullRequestEventType": "PULL_REQUEST_STATUS_CHANGED",
               "eventDate": 1510341930.72,
               "actor": "arn:aws:iam::111111111111:user/Jane_Doe"
           }
       ]
   }
   ```

1. To view whether there are any merge conflicts for a pull request, run the get\-merge\-conflicts command, specifying:
   + The name of the AWS CodeCommit repository \(with the \-\-repository\-name option\)\.
   + The branch, tag, HEAD, or other fully qualified reference for the source of the changes to use in the merge evaluation \(with the \-\-source\-commit\-specifier option\)\.
   + The branch, tag, HEAD, or other fully qualified reference for the destination of the changes to use in the merge evaluation \(with the \-\-destination\-commit\-specifier option\)\.
   + The merge option to use \(with the \-\-merge\-option option\) 

   For example, to view whether there are any merge conflicts between the tip of a source branch named *my\-feature\-branch* and a destination branch named *master* in a repository named `MyDemoRepo`:

   ```
   aws codecommit get-merge-conflicts --repository-name MyDemoRepo --source-commit-specifier my-feature-branch --destination-commit-specifier master --merge-option FAST_FORWARD_MERGE
   ```

   If successful, this command returns output similar to the following:

   ```
   {
       "destinationCommitId": "fac04518EXAMPLE",
       "mergeable": false,
       "sourceCommitId": "16d097f03EXAMPLE"
   }
   ```