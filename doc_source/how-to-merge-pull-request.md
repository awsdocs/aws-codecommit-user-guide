# Merge a Pull Request in an AWS CodeCommit Repository<a name="how-to-merge-pull-request"></a>

When you're satisfied that your code has been reviewed, you can merge a pull request in one of several ways:
+ <a name="why-git-merge"></a>On your local computer, you can use the git merge command to merge the source branch into the destination branch, and then push your merged code to the destination branch\. This closes the pull request automatically if the pull request is merged using the fast\-forward merge strategy\. The git merge command also allows you to choose merge options or strategies that are not available in the CodeCommit console\.  For more information about git merge and merge options, see [git\-merge](https://git-scm.com/docs/git-merge) or your Git documentation\.
+ <a name="is-mergable"></a>In the console, you might be able to merge your source branch to the destination branch using one of the available merge strategies, which also closes the pull request\. You see an advisory message that the pull request is mergeable\. When you choose **Merge**, the merge is performed using the available merge strategy that you choose\. The default merge strategy is to use fast\-forward if it is applicable, which is the default option for Git\. Depending on the state of the code in the source and destination branches, that strategy might not be available, but other options might be, such as squash or 3\-way\.
+ CodeCommit closes a pull request automatically if either the source or destination branch of the pull request is deleted\.
+ In the AWS CLI, you can use the AWS CLI to attempt to merge and close the pull request using the fast\-forward, squash, or 3\-way merge strategy\. 

**Topics**
+ [Merge a Pull Request \(Console\)](#how-to-merge-pull-request-console)
+ [Merge a Pull Request \(AWS CLI\)](#how-to-merge-pull-request-cli)

## Merge a Pull Request \(Console\)<a name="how-to-merge-pull-request-console"></a>

You can use the CodeCommit console to merge a pull request in a CodeCommit repository\. After the status of a pull request is changed to **Merged**, it will no longer appear in the list of open pull requests\. A merged pull request is categorized as closed\. It cannot be changed back to **Open**, but users can still comment on the changes and reply to comments\.

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In **Repositories**, choose the name of the repository\. 

1. In the navigation pane, choose **Pull requests**\.

1. By default, a list of all open pull requests is displayed\. Choose the open pull request you want to merge\.

1. In the pull request, choose between the available merge strategies\. Merge strategies that cannot be applied will appear greyed out\. If no merge strategies are available, you can choose to manually resolve conflicts in the CodeCommit console, or you can resolve them locally using your Git client\. For more information, see [Resolve Conflicts in a Pull Request in an AWS CodeCommit Repository](how-to-resolve-conflict-pull-request.md)\.  
![\[A pull request showing the merge strategies available for the merge in the CodeCommit console.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-merge-squash.png)
   + A fast\-forward merge will move the reference for the destination branch forward to the most recent commit of the source branch\. This is the default behavior of Git when possible\. No merge commit will be created, but all commit history from the source branch is retained as if it had occurred in the destination branch\. Fast\-forward merges do not appear as a branch merge in the commit visualizer view of the destination branch's history, as no merge commit is created, and the tip of the source branch is fast\-forwarded to the tip of the destination branch\. 
   + A squash merge will create one commit that contains the changes in the source branch and apply that single squashed commit to the destination branch\. By default, the commit message for that squash commit contains all the commit messages of the changes in the source branch\. No individual commit history of the branch changes will be retained\. This can help simplify your repository history while still retaining a graphical representation of the merge in the commit visualizer view of the destination branch's history\. 
   + A three\-way merge will create a merge commit for the merge in the destination branch, but also retain the individual commits made in the source branch as part of the history of the destination branch\. This can help maintain a complete history of changes to your repository\.

1. If you choose the squash or 3\-way merge strategy, review the automatically\-generated commit message and modify it if you want to change the information\. Add your name and email for the commit history\.

1. If desired, deselect the option to delete the source branch as part of the merge\. The default option is to delete the source branch when merging a pull request\.

1. Choose **Merge pull request** to complete the merge\.

## Merge a Pull Request \(AWS CLI\)<a name="how-to-merge-pull-request-cli"></a>

To use AWS CLI commands with CodeCommit, install the AWS CLI\. For more information, see [Command Line Reference](cmd-ref.md)\. 

**To use the AWS CLI to merge pull requests in a CodeCommit repository**

1. To merge and close a pull request using the fast\-forward merge strategy, run the merge\-pull\-request\-by\-fast\-forward command, specifying:
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

1. To merge and close a pull request using the squash merge strategy, run the merge\-pull\-request\-by\-squash command, specifying:
   + The ID of the pull request \(with the \-\-pull\-request\-id option\)\.
   + The full commit ID of the tip of the source branch \(with the \-\-source\-commit\-id option\)\. 
   + The name of the repository \(with the \-\-repository\-name option\)\.
   + The level of conflict detail you want to use \(with the \-\-conflict\-detail\-level option\)\. If unspecified, the default `FILE_LEVEL` is used\.
   + The conflict resolution strategy you want to use \(with the \-\-conflict\-resolution\-strategy option\)\. If unspecified, this defaults to `NONE`, and conflicts must be resolved manually\.
   + The commit message to include \(with the \-\-commit\-message option\)\.
   + The name to use for the commit \(with the \-\-name option\)\.
   + The email address to use for the commit \(with the \-\-email option\)\.
   + Whether to keep any empty folders \(with the \-\-keep\-empty\-folders option\)\.

    For example, to merge and close a pull request with the ID of *47* and a source commit ID of *99132ab0EXAMPLE* in a repository named *MyDemoRepo* using the conflict detail of `LINE_LEVEL` and the conflict resolution strategy of `ACCEPT_SOURCE`:

   ```
   aws codecommit merge-pull-request-by-squash --pull-request-id 47 --source-commit-id 99132ab0EXAMPLE --repository-name MyDemoRepo --conflict-detail-level LINE_LEVEL --conflict-resolution-strategy ACCEPT_SOURCE --name "Jorge Souza" --email "jorge_souza@example.com" --commit-message "Merging pull request 47 by squash and accepting source in merge conflicts"
   ```

   If successful, this command produces the same kind of output as merging by fast\-forward, output similar to the following:

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
                  "mergedBy": "arn:aws:iam::111111111111:user/Jorge_Souza"
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

1. To merge and close a pull request using the three\-way merge strategy, run the merge\-pull\-request\-by\-three\-way command, specifying:
   + The ID of the pull request \(with the \-\-pull\-request\-id option\)\.
   + The full commit ID of the tip of the source branch \(with the \-\-source\-commit\-id option\)\. 
   + The name of the repository \(with the \-\-repository\-name option\)\.
   + The level of conflict detail you want to use \(with the \-\-conflict\-detail\-level option\)\. If unspecified, the default `FILE_LEVEL` is used\.
   + The conflict resolution strategy you want to use \(with the \-\-conflict\-resolution\-strategy option\)\. If unspecified, this defaults to `NONE`, and conflicts must be resolved manually\.
   + The commit message to include \(with the \-\-commit\-message option\)\.
   + The name to use for the commit \(with the \-\-name option\)\.
   + The email address to use for the commit \(with the \-\-email option\)\.
   + Whether to keep any empty folders \(with the \-\-keep\-empty\-folders option\)\.

    For example, to merge and close a pull request with the ID of *47* and a source commit ID of *99132ab0EXAMPLE* in a repository named *MyDemoRepo* using the default options for conflict detail and conflict resolution strategy:

   ```
   aws codecommit merge-pull-request-by-fast-forward --pull-request-id 47 --source-commit-id 99132ab0EXAMPLE --repository-name MyDemoRepo --name "Maria Garcia" --email "maria_garcia@example.com" --commit-message "Merging pull request 47 by three-way with default options"
   ```

   If successful, this command produces the same kind of output as merging by fast\-forward, similar to the following:

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
                  "mergedBy": "arn:aws:iam::111111111111:user/Maria_Garcia"
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