# Close a Pull Request in an AWS CodeCommit Repository<a name="how-to-close-pull-request"></a>

When you're satisfied that your code has been reviewed, you can close a pull request in one of several ways:

+ <a name="why-git-merge"></a>On your local computer, you can use the git merge command to merge the source branch into the destination branch, and then push your merged code to the destination branch\. This closes the pull request automatically\. The git merge command also allows you to choose the merge option or strategy you use for the merge\. This is the only option available to merge the branches if the pull request status shows **Resolve conflicts**\. To learn more about git merge and merge options, see [git\-merge](https://git-scm.com/docs/git-merge) or your Git documentation\.

+ In the console, you can close a pull request without merging the code\. You might want to do this if you want to use the git merge command to merge the branches manually, or if the code in the pull request source branch isn't code you want merged into the destination branch\. This is the only option available if the code cannot be merged automatically\. You see an advisory message to resolve conflicts, which you must do on your local computer with the git merge command or a diff or merge tool\. 

+ <a name="is-mergable"></a>In the console, you might be able to merge your source branch to the destination branch automatically, which closes the pull request automatically\. You see an advisory message that the pull request is mergeable, and the **Merge** button in the pull request is active\. When you choose **Merge**, the merge is performed using the fast\-forward merge option\. 
**Note**  
The fast\-forward option does not create a commit or commit message for the merge\. If you want a merge commit to appear in the history of the destination branch, you can choose not to automatically merge the code as part of closing the pull request\. Instead, you can manually merge the branches using the git merge command with a different merge option\. 

+ AWS CodeCommit closes a pull request automatically if either the source or destination branch of the pull request is deleted\.

+ In the AWS CLI, you can update the status of a pull request from `OPEN` to `CLOSED`\. This closes the pull request\. You can also use the AWS CLI to attempt to merge and close the pull request\.


+ [Use the AWS CodeCommit Console to Close a Pull Request](#how-to-close-pull-request-console)
+ [Use the AWS CLI to Close Pull Requests](#how-to-close-pull-request-cli)

## Use the AWS CodeCommit Console to Close a Pull Request<a name="how-to-close-pull-request-console"></a>

You can use the AWS CodeCommit console to close a pull request in an AWS CodeCommit repository\. After the status of a pull request is changed to **Closed**, it cannot be changed back to **Open**, but users can still comment on the changes and reply to comments\.

1. Open the AWS CodeCommit console at [https://console\.aws\.amazon\.com/codecommit](https://console.aws.amazon.com/codecommit)\.

1. In the list of repositories, choose the name of the repository\. 

1. In the navigation pane, choose **Pull Requests**\.

1. By default, a list of all open pull requests is displayed\. Choose the open pull request you would like to close\.  
![\[Open pull requests displayed in the AWS CodeCommit console.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-pull-request-view.png)

1. In the pull request, choose one of the following:

   + **Merge**: This option, if available, closes the pull request and attempts to merge the code into the destination branch using the fast\-forward merge option\. You can also optionally select the option to automatically delete the pull request source branch after the merge is successful\. If the merge attempt is not successful, the source branch is not deleted\.
**Note**  
The **Merge** option is available only if there are no merge conflicts detected between the source and destination branches\. 

   + **Close pull request**: This option closes the pull request without attempting to merge the source branch into the destination branch\. This option does not provide a way to delete the source branch as part of closing the pull request, but you can do it yourself after the request is closed\.

## Use the AWS CLI to Close Pull Requests<a name="how-to-close-pull-request-cli"></a>

To use AWS CLI commands with AWS CodeCommit, install the AWS CLI\. For more information, see [Command Line Reference](cmd-ref.md)\. 

To use the AWS CLI to close pull requests in an AWS CodeCommit repository:

1. To update the status of a pull request in a repository from `OPEN` to `CLOSED`, run the update\-pull\-request\-status command, specifying:

   + The ID of the pull request \(with the \-\-pull\-request\-id option\)\.

   + The status of the pull request \(with the \-\-pull\-request\-status option\)\.

   For example, to update the status of a pull request with the ID of *42* to a status of *CLOSED* in an AWS CodeCommit repository named `MyDemoRepo`:

   ```
   aws codecommit update-pull-request-status --pull-request-id 42 --pull-request-status CLOSED  
   ```

   If successful, this command produces output similar to the following:

   ```
   {
      "pullRequest": { 
         "authorArn": "arn:aws:iam::111111111111:user/Jane_Doe",
         "clientRequestToken": "123Example",
         "creationDate": 1508962823.165,
         "description": "A code review of the new feature I just added to the service.",
         "lastActivityDate": 1508442444.12,
         "pullRequestId": "42",
         "pullRequestStatus": "CLOSED",
         "pullRequestTargets": [ 
            { 
               "destinationCommit": "5d036259EXAMPLE",
               "destinationReference": "refs/heads/master",
               "mergeMetadata": { 
                  "isMerged": false,
               },
               "repositoryName": "MyDemoRepo",
               "sourceCommit": "317f8570EXAMPLE",
               "sourceReference": "refs/heads/jane-branch"
            }
         ],
         "title": "Pronunciation difficulty analyzer"
      }
   }
   ```

1. To merge and close a pull request, run the merge\-pull\-request\-by\-fast\-forward command, specifying:

   + The ID of the pull request \(with the \-\-pull\-request\-id option\)\.

   + The full commit ID of the tip of the source branch \(with the \-\-source\-commit\-id option\)\. 

   + The name of the repository \(with the \-\-repository\-name option\)\.

    For example, to merge and close a pull request with the ID of *47* and a source commit ID of *99132ab0EXAMPLE* in a repository named *MyDemoRepo*:

   ```
   aws codecommit merge-pull-request-by-fast-forward --pull-request-id 47 --source-commit-id 99132ab0EXAMPLE --repository-name MyDemoRepo
   ```

   If successful, this command produces output similar to the following:

   ```
   {
      "pullRequest": { 
         "authorArn": "arn:aws:iam::111111111111:user/Li_Juan",
         "clientRequestToken": "",
         "creationDate": 1508530823.142,
         "description": "Review the latest changes and updates to the global variables",
         "lastActivityDate": 1508887223.155,
         "pullRequestId": "47",
         "pullRequestStatus": "CLOSED",
         "pullRequestTargets": [ 
            { 
               "destinationCommit": "9f31c968EXAMPLE",
               "destinationReference": "refs/heads/master",
               "mergeMetadata": { 
                  "isMerged": true,
                  "mergedBy": "arn:aws:iam::111111111111:user/Mary_Major"
               },
               "repositoryName": "MyDemoRepo",
               "sourceCommit": "99132ab0EXAMPLE",
               "sourceReference": "refs/heads/variables-branch"
            }
         ],
         "title": "Consolidation of global variables"
      }
   }
   ```