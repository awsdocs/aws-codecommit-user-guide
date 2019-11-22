# Review a Pull Request<a name="how-to-review-pull-request"></a>

You can use the AWS CodeCommit console to review the changes included in a pull request\. You can add comments to the request, files, and individual lines of code\. You can also reply to comments made by other users\. If your repository is [configured with notifications](how-to-repository-email.md), you receive emails when users reply to your comments or when users comment on a pull request\.

You can use the AWS CLI to comment on a pull request and reply to comments\. To review the changes, you must use the CodeCommit console, the git diff command, or a diff tool\.

**Topics**
+ [Review a Pull Request \(Console\)](#how-to-review-pull-request-console)
+ [Review Pull Requests \(AWS CLI\)](#how-to-review-pull-request-cli)

## Review a Pull Request \(Console\)<a name="how-to-review-pull-request-console"></a>

You can use the CodeCommit console to review a pull request in a CodeCommit repository\. 

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In **Repositories**, choose the name of the repository\. 

1. In the navigation pane, choose **Pull requests**\.

1. By default, a list of all open pull requests is displayed\. Choose the open pull request you want to review\.   
![\[Open pull requests displayed in the CodeCommit console.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-pull-request-view.png)
**Note**  
You can comment on a closed or merged pull request, but you cannot merge or reopen it\.

1. In the pull request, choose **Changes**\.

1. Do one of the following:
   + To add a general comment for the entire pull request, in **Comments on changes**, in **New comment**, enter a comment and then choose **Save**\. You can use [Markdown](https://docs.aws.amazon.com/general/latest/gr/aws-markdown.html), or you can enter your comment in plaintext\.  
![\[A general comment on the changes in a pull request.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-commenting-changecomment.png)
   + To add a comment to a file in the commit, in **Changes**, find the name of the file\. Choose the comment bubble that appears next to the file name ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-commentbubble.png), enter a comment, and then choose **Save**\.   
![\[Adding a comment on a file in a pull request.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-commenting-addfilecomment.png)
   + To add a comment to a changed line in the pull request, in **Changes**, go to the line you want to comment on\. Choose the comment bubble ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-commentbubble.png) that appears for that line, enter a comment, and then choose **Save**\.   
![\[Adding a comment on a line in a pull request.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-pull-request-comment.png)

1. To reply to comments on a commit, in **Changes** or **Activity**, choose **Reply**\.   
![\[Adding a reply to a comment in a pull request.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-pull-request-reply-activity.png)

1. To approve the changes made in a pull request, choose **Approve**\. You can view approvals, approval rules for a pull request, and approval rules created by approval rule templates in **Approvals**\. If you decide you do not want to approve the pull request after all, you can choose **Revoke approval**\.
**Note**  
You can only approve or revoke approval on an open pull request\. You cannot approve or revoke approval on a pull request whose status is Merged or Closed\.  
![\[Approvals and approval rules in a pull request.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-approval-rule-met.png)

## Review Pull Requests \(AWS CLI\)<a name="how-to-review-pull-request-cli"></a>

To use AWS CLI commands with CodeCommit, install the AWS CLI\. For more information, see [Command Line Reference](cmd-ref.md)\. 

**To use the AWS CLI to review pull requests in an CodeCommit repository**

1. To add a comment to a pull request in a repository, run the post\-comment\-for\-pull\-request command, specifying:
   + The ID of the pull request \(with the \-\-pull\-request\-id option\)\.
   + The name of the repository that contains the pull request \(with the \-\-repository\-name option\)\.
   + The full commit ID of the commit in the destination branch where the pull request is merged \(with the \-\-before\-commit\-id option\)\.
   + The full commit ID of the commit in the source branch that is the current tip of the branch for the pull request when you post the comment \(with the \-\-after\-commit\-id option\)\.
   + A unique, client\-generated idempotency token \(with the \-\-client\-request\-token option\)\.
   + The content of your comment \(with the \-\-content option\)\.
   + A list of location information about where to place the comment, including:
     + The name of the file being compared, including its extension and subdirectory, if any \(with the filePath attribute\)\.
     + The line number of the change in a compared file \(with the filePosition attribute\)\.
     + Whether the comment on the change is "before" or "after" in the comparison between the source and destination branches \(with the relativeFileVersion attribute\)\.

   For example, to add the comment *"These don't appear to be used anywhere\. Can we remove them?"* on the change to the *ahs\_count\.py* file in a pull request with the ID of *47* in a repository named *MyDemoRepo*:

   ```
   aws codecommit post-comment-for-pull-request --pull-request-id "47" --repository-name MyDemoRepo --before-commit-id 317f8570EXAMPLE --after-commit-id 5d036259EXAMPLE --client-request-token 123Example --content "These don't appear to be used anywhere. Can we remove them?" --location filePath=ahs_count.py,filePosition=367,relativeFileVersion=AFTER   
   ```

   If successful, this command produces output similar to the following:

   ```
   { 
            "afterBlobId": "1f330709EXAMPLE",
            "afterCommitId": "5d036259EXAMPLE",
            "beforeBlobId": "80906a4cEXAMPLE",
            "beforeCommitId": "317f8570EXAMPLE",
            "comment": {
                  "authorArn": "arn:aws:iam::111111111111:user/Saanvi_Sarkar",
                  "clientRequestToken": "123Example",
                  "commentId": "abcd1234EXAMPLEb5678efgh",
                  "content": "These don't appear to be used anywhere. Can we remove them?",
                  "creationDate": 1508369622.123,
                  "deleted": false,
                  "lastModifiedDate": 1508369622.123
               }
                "location": { 
                  "filePath": "ahs_count.py",
                  "filePosition": 367,
                  "relativeFileVersion": "AFTER"
                },
            "repositoryName": "MyDemoRepo",
            "pullRequestId": "47"
    }
   ```

1. To view comments for a pull request, run the get\-comments\-for\-pull\-request command, specifying:
   + The name of the CodeCommit repository \(with the `--repository-name` option\)\.
   + The system\-generated ID of the pull request \(with the `--pull-request-id` option\)\.
   + \(Optional\) An enumeration token to return the next batch of the results \(with the `--next-token` option\)\.
   + \(Optional\) A non\-negative integer to limit the number of returned results \(with the `--max-results` option\)\.

   For example, to view comments for a pull request with an ID of 42:

   ```
   aws codecommit get-comments-for-pull-request --pull-request-id 42
   ```

   If successful, this command produces output similar to the following:

   ```
   {
      "commentsForPullRequestData": [ 
         { 
            "afterBlobId": "1f330709EXAMPLE",
            "afterCommitId": "5d036259EXAMPLE",
            "beforeBlobId": "80906a4cEXAMPLE",
            "beforeCommitId": "317f8570EXAMPLE",
            "comments": [ 
               { 
                  "authorArn": "arn:aws:iam::111111111111:user/Saanvi_Sarkar",
                  "clientRequestToken": "",
                  "commentId": "abcd1234EXAMPLEb5678efgh",
                  "content": "These don't appear to be used anywhere. Can we remove them?",
                  "creationDate": 1508369622.123,
                  "deleted": false,
                  "lastModifiedDate": 1508369622.123
               },
               {
                  "authorArn": "arn:aws:iam::111111111111:user/Li_Juan",
                  "clientRequestToken": "",
                  "commentId": "442b498bEXAMPLE5756813",
                  "content": "Good catch. I'll remove them.",
                  "creationDate": 1508369829.104,
                  "deleted": false,
                  "lastModifiedDate": 150836912.273
                }
            ],
            "location": { 
               "filePath": "ahs_count.py",
               "filePosition": 367,
               "relativeFileVersion": "AFTER"
            },
            "repositoryName": "MyDemoRepo",
            "pullRequestId": "42"
         }
      ],
      "nextToken": "exampleToken"
   }
   ```

1. <a name="update-pull-request-approval-state"></a>To approve or revoke approval for a pull request, run the update\-pull\-request\-approval\-state command, specifying:
   + The ID of the pull request \(using the \-\-pull\-request\-id option\)\.
   + The revision ID of the pull request \(using the \-\-revision\-id option\)\. You can get the current revision ID for a pull request by using the [get\-pull\-request](how-to-view-pull-request.md#get-pull-request) command\.
   + The approval state you want to apply \(using the \-\-approval\-state\) option\. Valid approval states include `APPROVE` and `REVOKE`\.

   For example, to approve a pull request with the ID of *27* and a revision ID of *9f29d167EXAMPLE*:

   ```
   aws codecommit update-pull-request-approval-state --pull-request-id 27 --revision-id 9f29d167EXAMPLE --approval-state "APPROVE"
   ```

   If successful, this command returns nothing\.

1. To post a reply to a comment in a pull request, run the post\-comment\-reply command, specifying:
   + The system\-generated ID of the comment to which you want to reply \(with the \-\-in\-reply\-to option\)\.
   + A unique, client\-generated idempotency token \(with the \-\-client\-request\-token option\)\.
   + The content of your reply \(with the \-\-content option\)\. 

    For example, to add the reply *"Good catch\. I'll remove them\."* to the comment with the system\-generated ID of *abcd1234EXAMPLEb5678efgh*: 

   ```
   aws codecommit post-comment-reply --in-reply-to abcd1234EXAMPLEb5678efgh --content "Good catch. I'll remove them." --client-request-token 123Example
   ```

   If successful, this command produces output similar to the following:

   ```
   { 
       "comment": {
           "authorArn": "arn:aws:iam::111111111111:user/Li_Juan",
           "clientRequestToken": "123Example",
           "commentId": "442b498bEXAMPLE5756813",
           "content": "Good catch. I'll remove them.",
           "creationDate": 1508369829.136,
           "deleted": false,
           "lastModifiedDate": 150836912.221
       }
    }
   ```