# Review a Pull Request<a name="how-to-review-pull-request"></a>

You can use the AWS CodeCommit console to review the changes included in a pull request\. You can add comments to the request, to the files, and to specific lines of code\. You can also reply to comments made by other users\. If your repository is [configured with notifications](how-to-repository-email.md), you receive emails when users reply to your comments or when users comment on a pull request\.

You can use the AWS CLI to comment on a pull request and reply to comments\. To review the changes, you must use the git diff command or a diff tool\.


+ [Use the AWS CodeCommit Console to Review a Pull Request](#how-to-review-pull-request-console)
+ [Use the AWS CLI to Review Pull Requests](#how-to-review-pull-request-cli)

## Use the AWS CodeCommit Console to Review a Pull Request<a name="how-to-review-pull-request-console"></a>

You can use the AWS CodeCommit console to review a pull request in an AWS CodeCommit repository\. 

1. Open the AWS CodeCommit console at [https://console\.aws\.amazon\.com/codecommit](https://console.aws.amazon.com/codecommit)\.

1. In the list of repositories, choose the name of the repository\. 

1. In the navigation pane, choose **Pull Requests**\.

1. By default, a list of all open pull requests is displayed\. Choose the open pull request you want to review\. You can also comment on a closed pull request\.  
![\[Open pull requests displayed in the AWS CodeCommit console.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-pull-request-view.png)

1. In the pull request, choose **Changes**\.

1. Do one of the following:

   + To add a general comment, in **Comments on changes**, type a comment and then choose **Save**\. You can use [Markdown](https://docs.aws.amazon.com/general/latest/gr/aws-markdown.html), or you can type your comment in plaintext\.  
![\[A general comment on the changes in a pull request.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-commenting-changecomment.png)

   + To add a comment to a file in the commit, in **Changes**, find the name of the file\. Choose the comment bubble that appears next to the file name ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-commentbubble.png), type a comment, and then choose **Save**\.   
![\[Adding a comment on a file in a pull request.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-commenting-addfilecomment.png)

   + To add a comment to a changed line in the pull request, in **Changes**, go to the line you want to comment on\. Choose the comment bubble ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-commentbubble.png), type a comment, and then choose **Save**\.   
![\[Adding a comment on a line in a pull request.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-pull-request-comment.png)

1. To reply to comments on a commit, in **Changes** or **Activity**, choose **Reply**\.   
![\[Adding a reply to a comment in a pull request.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-pull-request-reply-activity.png)

If [notifications](how-to-repository-email.md) are configured, the user who created the pull request receives email about your comments\. You receive email if a user replies to your comments or if the pull request is updated\.

## Use the AWS CLI to Review Pull Requests<a name="how-to-review-pull-request-cli"></a>

To use AWS CLI commands with AWS CodeCommit, install the AWS CLI\. For more information, see [Command Line Reference](cmd-ref.md)\. 

To use the AWS CLI to review pull requests in an AWS CodeCommit repository:

1. To add a comment to a pull request in a repository, run the post\-comment\-for\-pull\-request command, specifying:

   + The ID of the pull request \(with the \-\-pull\-request\-id option\)\.

   + The name of the repository that contains the pull request \(with the \-\-repository\-name option\)\.

   + The full commit ID of the commit in the destination branch where the pull request will be merged \(with the \-\-before\-commit\-id option\)\.

   + The full commit ID of the commit in the source branch that is the current tip of the branch for the pull request when you post the comment \(with the \-\-after\-commit\-id option\)\.

   + A unique, client\-generated idempotency token \(with the \-\-client\-request\-token option\)\.

   + The content of your comment \(with the \-\-content option\)\.

   + A list of location information about where to place the comment, including:

     + The name of the file being compared, including its extension and subdirectory, if any \(with the filePath attribute\)\.

     + The line number of the change within a compared file \(with the filePosition attribute\)\.

     + Whether the comment on the change is "before" or "after" in the comparison between the source and destination branches \(with the relativeFileVersion attribute\)\.

   For example, to add the comment *"These don't appear to be used anywhere\. Can we remove them?"* on the change to the *ahs\_count\.py* file in a pull request with the ID of *47* in a repository named *MyDemoRepo*:

   ```
   aws codecommit post-comment-for-pull-request --pull-request-id "47" --repository-name MyDemoRepo --before-commit-id 317f8570EXAMPLE --after-commit-id 5d036259EXAMPLE --client-request-token 123Example --content ""These don't appear to be used anywhere. Can we remove them?"" --location filePath=ahs_count.py,filePosition=367,relativeFileVersion=AFTER   
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
                  "CommentId": "",
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

   + The AWS CodeCommit repository's name \(with the `--repository-name` option\)\.

   + The full commit ID of the commit in the source branch that was the tip of the branch at the time the comment was made \(with the `--after-commit-id option`\)\.

   + The full commit ID of the commit in the destination branch that was the tip of the branch at the time the pull request was created \(with the `--before-commit-id` option\)\. 

   + \(Optional\) An enumeration token to return the next batch of the results \(with the `--next-token` option\)\.

   + \(Optional\) A non\-negative integer to limit the number of returned results \(with the `--max-results` option\)\.

   For example, to view comments for a pull request in a repository named *MyDemoRepo*:

   ```
   aws codecommit get-comments-for-pull-request --repository-name MyDemoRepo --before-commit-ID 317f8570EXAMPLE --after-commit-id 5d036259EXAMPLE
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
                  "commentId": "abcd1234EXAMPLEb5678efgh",
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
           "CommentId": "abcd1234EXAMPLEb5678efgh",
           "lastModifiedDate": 150836912.221
       }
    }
   ```