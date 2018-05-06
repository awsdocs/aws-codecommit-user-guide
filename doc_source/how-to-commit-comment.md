# Comment on a Commit in AWS CodeCommit<a name="how-to-commit-comment"></a>

You can use the AWS CodeCommit console to comment on commits in a repository, and view and reply to other users' comments on commits\. This can help you discuss changes made in a repository, including:
+ Why specific changes were made\.
+ Whether more changes or fixes are required\.
+ Whether changes should be merged into another branch\.

You can comment on an overall commit, on a file within a commit, or on a specific line or change within a file\.

![\[A commit with three comments.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-commenting-commitwithcomments.png)

**Note**  
For best results, use commenting when you are signed in as an IAM user\. The commenting functionality is not optimized for users who sign in with root account credentials, federated access, or temporary credentials\.

**Topics**
+ [View Comments on a Commit in a Repository](#how-to-commit-comment-view-console)
+ [Add and Reply to Comments on a Commit in a Repository](#how-to-commit-comment-add-console)
+ [Use the AWS CLI to View, Add, Update, and Reply to Commments](#how-to-commit-comment-cli)

## View Comments on a Commit in a Repository<a name="how-to-commit-comment-view-console"></a>

You can use the AWS CodeCommit console to view comments on a commit\. 

**To view comments on a commit \(console\)**

1. Open the AWS CodeCommit console at [https://console\.aws\.amazon\.com/codecommit](https://console.aws.amazon.com/codecommit)\.

1. On the **Dashboard** page, from the list of repositories, choose the repository for which you want to review comments on commits\. 

1. In the navigation pane, choose **Commits**\. In the commit history view, a history of commits to the repository's default branch is displayed\. Choose the title of the commit where you want to view any comments\.

   The page for that specific commit is displayed, along with any comments already present on that commit, in **Changes**\.  
![\[A commit showing a comment on a changed line of code.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-commenting-linecomment.png)

1. To only view the comments for a commit, choose **Comments**\. All file and line comments are shown, along with any comments on the changes in the commit itself, in reverse chronological order\.  
![\[The comments view for a commit, with three comments shown: one file comment, one line comment, and one change comment.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-commenting-commenttab.png)

   To view the context of a comment, choose **Show context**\. The view changes to where the comment was made in the **Changes** view\.

## Add and Reply to Comments on a Commit in a Repository<a name="how-to-commit-comment-add-console"></a>

You can use the AWS CodeCommit console to add comments to the comparison of a commit and a parent, or to the comparison between two specified commits\. You can also reply to existing comments\. 

### Add and Reply to Comments on a Commit<a name="how-to-commit-comment-add-cpage"></a>

You can add and reply to comments to a commit\. Your comments are marked as those belonging to the IAM user or role you used to sign in to the console\.

**To add and reply to comments on a commit \(console\)**

1. Open the AWS CodeCommit console at [https://console\.aws\.amazon\.com/codecommit](https://console.aws.amazon.com/codecommit)\.

1. On the **Dashboard** page, from the list of repositories, choose the repository where you want to comment on commits\. 

1. In the navigation pane, choose **Commits**\. In the commit history view, choose the title of the commit where you want to add or reply to comments\.

   The page for that commit is displayed, along with any comments\.

1. To add a comment, do one of the following:
   + To add a general comment, in **Comments on changes**, type your comment, and then choose **Save**\. You can use [Markdown](https://en.wikipedia.org/wiki/Markdown), or you can type your comment in plaintext\.  
![\[A general comment on the changes in a commit.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-commenting-changecomment.png)
   + To add a comment to a file in the commit, in **Changes**, find the name of the file\. Choose the comment bubble ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-commentbubble.png), type your comment, and then choose **Save**\.   
![\[Adding a comment on a file in a commit.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-commenting-addfilecomment.png)
   + To add a comment to a changed line in the commit, in **Changes**, go to the line where the change appears\. Choose the comment bubble ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-commentbubble.png), type your comment, and then choose **Save**\.   
![\[Adding a comment on a line in a commit.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-commenting-addlinecomment.png)
**Note**  
You can edit your comment after you have saved it, but you cannot delete it from the AWS CodeCommit console\. Consider using the **Preview** mode for your comment before you save your comment\.

1. To reply to comments on a commit, in either **Changes** or **Comments**, choose **Reply**\.   
![\[Choose Reply to add a comment to an existing comment on a commit.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-commenting-commenttab.png)

### Add and Reply to Comments When Comparing Two Commit Specifiers<a name="how-to-commit-comment-console-compare"></a>

You can add comments to a comparison between branches, tags, or commits\.

**To add or reply to comments when comparing commit specifiers \(console\)**

1. Open the AWS CodeCommit console at [https://console\.aws\.amazon\.com/codecommit](https://console.aws.amazon.com/codecommit)\.

1. On the **Dashboard** page, from the list of repositories, choose the repository where you want to compare commits, branches, or tagged commits\. 

1. In the navigation pane, choose **Compare**\.  
![\[Compare any two commit specifiers\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-compare-1.png)

1. Use the **Choose** buttons to compare two commit specifiers\. Use the drop\-down lists or paste in commit IDs\. 

1. Do one or more of the following:
   + To add comments to files or lines, choose the comment bubble ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-commentbubble.png)\.
   + To add general comments on the compared changes, go to **Comments on changes**\.

## Use the AWS CLI to View, Add, Update, and Reply to Commments<a name="how-to-commit-comment-cli"></a>

You can view, add, reply, update, and delete the contents of a comment by running the following commands:
+ [get\-comments\-for\-compared\-commit](#how-to-commit-comment-cli-get-comments), to view the comments on the comparison between two commits\.
+ [get\-comment](#how-to-commit-comment-cli-get-comment-info), to view details on a specific comment\.
+ [delete\-comment\-content](#how-to-commit-comment-cli-commit-delete), to delete the contents of a comment that you created\.
+ [post\-comment\-for\-compared\-commit](#how-to-commit-comment-cli-comment), to create a comment on the comparison between two commits\.
+ [update\-comment](#how-to-commit-comment-cli-commit-update), to update a comment\.
+ [post\-comment\-reply](#how-to-commit-comment-cli-commit-reply), to reply to a comment\.

### To view comments on a commit<a name="how-to-commit-comment-cli-get-comments"></a>

1. Run the get\-comments\-for\-compared\-commit command, specifying:
   + The AWS CodeCommit repository's name \(with the `--repository-name` option\)\.
   + The full commit ID of the 'after' commit, to establish the directionality of the comparison \(with the `--after-commit-id option`\)\.
   + The full commit ID of the 'before' commit, to establish the directionality of the comparison \(with the `--before-commit-id` option\)\. 
   + \(Optional\) An enumeration token to return the next batch of the results \(with the `--next-token` option\)\.
   + \(Optional\) A non\-negative integer to limit the number of returned results \(with the `--max-results` option\)\.

   For example, to view comments made on the comparison between two commits in a repository named *MyDemoRepo*:

   ```
   aws codecommit get-comments-for-compared-commit --repository-name MyDemoRepo --before-commit-ID 6e147360EXAMPLE --after-commit-id 317f8570EXAMPLE
   ```

1. If successful, this command produces output similar to the following:

   ```
   {
      "commentsForComparedCommitData": [ 
         { 
            "afterBlobId": "1f330709EXAMPLE",
            "afterCommitId": "317f8570EXAMPLE",
            "beforeBlobId": "80906a4cEXAMPLE",
            "beforeCommitId": "6e147360EXAMPLE",
            "comments": [ 
               { 
                  "authorArn": "arn:aws:iam::111111111111:user/Li_Juan",
                  "clientRequestToken": "123Example",
                  "commentId": "ff30b348EXAMPLEb9aa670f",
                  "content": "Whoops - I meant to add this comment to the line, not the file, but I don't see how to delete it.",
                  "creationDate": 1508369768.142,
                  "deleted": false,
                  "CommentId": "123abc-EXAMPLE",
                  "lastModifiedDate": 1508369842.278
               },
               {
                  "authorArn": "arn:aws:iam::111111111111:user/Li_Juan",
                  "clientRequestToken": "123Example",
                  "commentId": "553b509bEXAMPLE56198325",
                  "content": "Can you add a test case for this?",
                  "creationDate": 1508369612.240,
                  "deleted": false,
                  "commentId": "456def-EXAMPLE",
                  "lastModifiedDate": 1508369612.240
                }
            ],
            "location": { 
               "filePath": "cl_sample.js",
               "filePosition": 1232,
               "relativeFileVersion": "after"
            },
            "repositoryName": "MyDemoRepo"
         }
      ],
      "nextToken": "exampleToken"
   }
   ```

### To view details of a comment on a commit<a name="how-to-commit-comment-cli-get-comment-info"></a>

1. Run the get\-comment command, specifying the system\-generated comment ID\. For example:

   ```
   aws codecommit get-comment --comment-id ff30b348EXAMPLEb9aa670f
   ```

1. If successful, this command returns output similar to the following:

   ```
   {
      "comment": { 
         "authorArn": "arn:aws:iam::111111111111:user/Li_Juan",
         "clientRequestToken": "123Example",
         "commentId": "ff30b348EXAMPLEb9aa670f",
         "content": "Whoops - I meant to add this comment to the line, but I don't see how to delete it.",
         "creationDate": 1508369768.142,
         "deleted": false,
         "commentId": "",
         "lastModifiedDate": 1508369842.278
      }
   }
   ```

### To delete the contents of a comment on a commit<a name="how-to-commit-comment-cli-commit-delete"></a>

1. Run the delete\-comment\-content command, specifying the system\-generated comment ID\. For example:

   ```
   aws codecommit delete-comment-content --comment-id ff30b348EXAMPLEb9aa670f
   ```
**Note**  
You can only delete the content of a comment if you created the comment\.

1. If successful, this command produces output similar to the following:

   ```
   {
      "comment": { 
         "creationDate": 1508369768.142,  
         "deleted": true,
         "lastModifiedDate": 1508369842.278,
         "clientRequestToken": "123Example",
         "commentId": "ff30b348EXAMPLEb9aa670f",
         "authorArn": "arn:aws:iam::111111111111:user/Li_Juan"
      }
   }
   ```

### To create a comment on a commit<a name="how-to-commit-comment-cli-comment"></a>

1. Run the `post-comment-for-compared-commit` command, specifying:
   + The AWS CodeCommit repository's name \(with the `--repository-name` option\)\.
   + The full commit ID of the 'after' commit, to establish the directionality of the comparison \(with the after\-commit\-id option\)\.
   + The full commit ID of the 'before' commit, to establish the directionality of the comparison \(with the before\-commit\-id option\)\. 
   + A unique, client\-generated idempotency token \(with the \-\-client\-request\-token option\)\.
   + The content of your comment \(with the \-\-content option\)\.
   + A list of location information about where to place the comment, including:
     + The name of the file being compared, including its extension and subdirectory, if any \(with the filePath attribute\)\.
     + The line number of the change within a compared file \(with the filePosition attribute\)\.
     + Whether the comment on the change is "before" or "after" in the comparison between the source and destination branches \(with the relativeFileVersion attribute\)\.

   For example, to add the comment *"Can you add a test case for this?"* on the change to the *cl\_sample\.js* file in the comparison between two commits in a repository named *MyDemoRepo*:

   ```
   aws codecommit post-comment-for-compared-commit --repository-name MyDemoRepo --before-commit-id 317f8570EXAMPLE --after-commit-id 5d036259EXAMPLE --client-request-token 123Example --content "Can you add a test case for this?" --location filePath=cl_sample.js,filePosition=1232,relativeFileVersion=AFTER   
   ```

1. If successful, this command produces output similar to the following:

   ```
   { 
            "afterBlobId": "1f330709EXAMPLE",
            "afterCommitId": "317f8570EXAMPLE",
            "beforeBlobId": "80906a4cEXAMPLE",
            "beforeCommitId": "6e147360EXAMPLE",
            "comment": {
                  "authorArn": "arn:aws:iam::111111111111:user/Li_Juan",
                  "clientRequestToken": "",
                  "commentId": "553b509bEXAMPLE56198325",
                  "content": "Can you add a test case for this?",
                  "creationDate": 1508369612.203,
                  "deleted": false,
                  "commentId": "abc123-EXAMPLE",
                  "lastModifiedDate": 1508369612.203
                },
                "location": { 
                  "filePath": "cl_sample.js",
                  "filePosition": 1232,
                  "relativeFileVersion": "AFTER"
                },
            "repositoryName": "MyDemoRepo"
    }
   ```

### To update a comment on a commit<a name="how-to-commit-comment-cli-commit-update"></a>

1. Run the `update-comment` command, specifying the system\-generated comment ID and the content with which you want to replace any existing content\. 
**Note**  
You can only update the content of a comment if you created the comment\.

   For example, to add the content *"Fixed as requested\. I'll update the pull request\."* to a comment with an ID of *442b498bEXAMPLE5756813* :

   ```
   aws codecommit update-comment --comment-id 442b498bEXAMPLE5756813 --content "Fixed as requested. I'll update the pull request."  
   ```

1. If successful, this command produces output similar to the following:

   ```
   { 
       "comment": {
           "authorArn": "arn:aws:iam::111111111111:user/Li_Juan",
           "clientRequestToken": "",
           "commentId": "442b498bEXAMPLE5756813",
           "content": "Fixed as requested. I'll update the pull request.",
           "creationDate": 1508369929.783,
           "deleted": false,
           "lastModifiedDate": 1508369929.287
       }
    }
   ```

### To reply to a comment on a commit<a name="how-to-commit-comment-cli-commit-reply"></a>

1. To post a reply to a comment in a pull request, run the post\-comment\-reply command, specifying:
   + The system\-generated ID of the comment to which you want to reply \(with the \-\-in\-reply\-to option\)\.
   + A unique, client\-generated idempotency token \(with the \-\-client\-request\-token option\)\.
   + The content of your reply \(with the \-\-content option\)\. 

    For example, to add the reply *"Good catch\. I'll remove them\."* to the comment with the system\-generated ID of *abcd1234EXAMPLEb5678efgh*: 

   ```
   aws codecommit post-comment-reply --in-reply-to abcd1234EXAMPLEb5678efgh --content "Good catch. I'll remove them." --client-request-token 123Example
   ```

1. If successful, this command produces output similar to the following:

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