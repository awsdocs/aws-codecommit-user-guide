# View pull requests in an AWS CodeCommit repository<a name="how-to-view-pull-request"></a>

You can use the AWS CodeCommit console or the AWS CLI to view pull requests for your repository\. By default, you see only open pull requests, but you can change the filter to view all pull requests, only closed requests, only pull requests that you created, and more\. 

**Topics**
+ [View pull requests \(console\)](#how-to-view-pull-request-console)
+ [View pull requests \(AWS CLI\)](#how-to-view-pull-request-cli)

## View pull requests \(console\)<a name="how-to-view-pull-request-console"></a>

You can use the AWS CodeCommit console to view a list of pull requests in a CodeCommit repository\. By changing the filter, you can change the list display to only show you a certain set of pull requests\. For example, you can view a list of pull requests you created with a status of **Open**, or you can choose a different filter and view pull requests you created with a status of **Closed**\.

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In **Repositories**, choose the name of the repository where you want to view pull requests\. 

1. In the navigation pane, choose **Pull Requests**\.

1. By default, a list of all open pull requests is displayed\.   
![\[Pull requests displayed in the AWS CodeCommit console.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-pull-request-view-list.png)

1. To change the display filter, choose from the list of available filters:
   + **Open pull requests** \(default\): Displays all pull requests with a status of **Open**\.
   + **All pull requests**: Displays all pull requests\.
   + **Closed pull requests**: Displays all pull requests with a status of **Closed**\.
   + **My pull requests**: Displays all pull requests that you created, regardless of the status\. It does not display reviews that you have commented on or otherwise participated in\.
   + **My open pull requests**: Displays all pull requests that you created with a status of **Open**\.
   + **My closed pull requests**: Displays all pull requests that you created with a status of **Closed**\.

1. When you find a pull request in the displayed list that you would like to view, choose it\.

## View pull requests \(AWS CLI\)<a name="how-to-view-pull-request-cli"></a>

To use AWS CLI commands with CodeCommit, install the AWS CLI\. For more information, see [Command line reference](cmd-ref.md)\. 

Follow these steps to use the AWS CLI to view pull requests in an CodeCommit repository\.

1. To view a list of pull requests in a repository, run the list\-pull\-requests command, specifying:
   + The name of the CodeCommit repository where you want to view pull requests \(with the \-\-repository\-name option\)\.
   + \(Optional\) The status of the pull request \(with the \-\-pull\-request\-status option\)\.
   + \(Optional\) The Amazon Resource Name \(ARN\) of the IAM user who created the pull request \(with the \-\-author\-arn option\)\.
   + \(Optional\) An enumeration token that can be used to return batches of results \(with the \-\-next\-token option\) 
   + \(Optional\) A limit on the number of returned results per request \(with the \-\-max\-results option\)\.

   For example, to list pull requests created by an IAM user with the ARN *arn:aws:iam::111111111111:user/Li\_Juan* and the status of *CLOSED* in a CodeCommit repository named `MyDemoRepo`:

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

1. To view details of a pull request, run the get\-pull\-request command with the \-\-pull\-request\-id option, specifying the ID of the pull request\. For example, to view information about a pull request with the ID of *27*:

   ```
   aws codecommit get-pull-request --pull-request-id 27
   ```

   If successful, this command produces output similar to the following:

   ```
   {
       "pullRequest": {
           "approvalRules": [
               {
                   "approvalRuleContent": "{\"Version\": \"2018-11-08\",\"Statements\": [{\"Type\": \"Approvers\",\"NumberOfApprovalsNeeded\": 2,\"ApprovalPoolMembers\": [\"arn:aws:sts::123456789012:assumed-role/CodeCommitReview/*\"]}]}",
                   "approvalRuleId": "dd8b17fe-EXAMPLE",
                   "approvalRuleName": "2-approver-rule-for-master",
                   "creationDate": 1571356106.936,
                   "lastModifiedDate": 571356106.936,
                   "lastModifiedUser": "arn:aws:iam::123456789012:user/Mary_Major",
                   "ruleContentSha256": "4711b576EXAMPLE"
               }
           ],
           "lastActivityDate": 1562619583.565,
           "pullRequestTargets": [
               {
                   "sourceCommit": "ca45e279EXAMPLE",
                   "sourceReference": "refs/heads/bugfix-1234",
                   "mergeBase": "a99f5ddbEXAMPLE",
                   "destinationReference": "refs/heads/master",
                   "mergeMetadata": {
                       "isMerged": false
                   },
                   "destinationCommit": "2abfc6beEXAMPLE",
                   "repositoryName": "MyDemoRepo"
               }
           ],
           "revisionId": "e47def21EXAMPLE",
           "title": "Quick fix for bug 1234",
           "authorArn": "arn:aws:iam::123456789012:user/Nikhil_Jayashankar",
           "clientRequestToken": "d8d7612e-EXAMPLE",
           "creationDate": 1562619583.565,
           "pullRequestId": "27",
           "pullRequestStatus": "OPEN"
       }
   }
   ```

1. <a name="get-pull-request-approval-state"></a>To view approvals on a pull request, run the get\-pull\-request\-approval\-state command, specifying:
   + The ID of the pull request \(using the \-\-pull\-request\-id option\)\.
   + The revision ID of the pull request \(using the \-\-revision\-id option\)\. You can get the current revision ID for a pull request by using the [get\-pull\-request](#get-pull-request) command\.

   For example, to view approvals on a pull request with an ID of *8* and a revision ID of *9f29d167EXAMPLE*:

   ```
   aws codecommit get-pull-request-approval-state --pull-request-id 8 --revision-id 9f29d167EXAMPLE
   ```

   If successful, this command produces output similar to the following:

   ```
   {
       "approvals": [
           {
               "userArn": "arn:aws:iam::123456789012:user/Mary_Major",
               "approvalState": "APPROVE"
           }
       ]
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
   + The name of the CodeCommit repository \(with the \-\-repository\-name option\)\.
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