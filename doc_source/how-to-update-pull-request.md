# Update a Pull Request<a name="how-to-update-pull-request"></a>

You can use your local Git client to push commits to the source branch, which updates the pull request with code changes\. You might update the pull request with more commits because:
+ You want users to review code changes you made to the source branch code in response to comments in the pull request\.
+ One or more commits have been made to the destination branch since the pull request was created\. You want to incorporate those changes into the source branch as part of the review \(forward integration\)\. This changes the state of the pull request to **Mergeable** and enables the merging and closing of the pull request from the console\.

You can use the AWS CodeCommit console or the AWS CLI to update the title or description of a pull request\. You might want to update the pull request because:
+ Other users don't understand the description, or the original title is misleading\.
+ You want the title or description to reflect changes made to the source branch of an open pull request\.

**Topics**
+ [Use Git to Update a Pull Request](#how-to-update-pull-request-git)
+ [Use the AWS CodeCommit Console to Update a Pull Request](#how-to-update-pull-request-console)
+ [Use the AWS CLI to Update Pull Requests](#how-to-update-pull-request-cli)

## Use Git to Update a Pull Request<a name="how-to-update-pull-request-git"></a>

You can use Git to update the source branch of a pull request with changes to the code to:
+ Add more code to the review\.
+ Incorporate changes suggested in review comments\.
+ Forward\-integrate changes to the destination branch in the source branch\.
+ Make sure that all the changes to be merged into the destination branch have been reviewed in the pull request\.

You make the changes on your local computer, and then commit and push them to the source branch\. If [notifications are configured for the repository](how-to-repository-email.md), users subscribed to the topic receive emails when you push changes to the source branch of an open pull request\.

**To update the source branch with code changes**

1. From the local repo on your computer, at the terminal or command line, make sure you have pulled the latest changes to the repository, and then run the git checkout command, specifying the source branch of the pull request\. For example, to check out a source branch of a pull request named *pullrequestbranch*:

   ```
   git checkout pullrequestbranch
   ```

1. Make any changes you want reviewed\. For example, if you want to change the code in the source branch in responses to user comments, edit the files with those changes\. If you want to integrate changes that have been made to the destination branch into the source branch \(forward integration\), run the git merge command, specifying the destination branch, to merge those changes into the source branch\. 
**Tip**  
You might want to use diff tool or merge tool software to help view and choose the changes you want integrated into a source branch\.

1. After you have made your changes, run the git add and git commit commands to stage and commit them\. 
**Tip**  
You can run these commands separately, or you can use the `-a` option to add changed files to a commit automatically\. For example, you could run a command similar to the following:   

   ```
   git commit -am "This is an example commit message."
   ```

   For more information, see [Basic Git Commands](how-to-basic-git.md) or consult your Git documentation\.

1. Run the **git push **command to push your changes to AWS CodeCommit\. Your pull request is updated with the changes you made to the source branch\.

## Use the AWS CodeCommit Console to Update a Pull Request<a name="how-to-update-pull-request-console"></a>

You can use the AWS CodeCommit console to update the title and description of a pull request in an AWS CodeCommit repository\. 

1. Open the AWS CodeCommit console at [https://console\.aws\.amazon\.com/codecommit](https://console.aws.amazon.com/codecommit)\.

1. In the list of repositories, choose the name of the repository\. 

1. In the navigation pane, choose **Pull Requests**\.

1. By default, a list of all open pull requests is displayed\. Choose the open pull request you want to update\.  
![\[Open pull requests displayed in the AWS CodeCommit console.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-pull-request-view.png)

1. In the pull request, choose the option to edit the title or description\.

## Use the AWS CLI to Update Pull Requests<a name="how-to-update-pull-request-cli"></a>

To use AWS CLI commands with AWS CodeCommit, install the AWS CLI\. For more information, see [Command Line Reference](cmd-ref.md)\. 

To use the AWS CLI to update pull requests in an AWS CodeCommit repository:

1.  To update the title of a pull request in a repository, run the update\-pull\-request\-title command, specifying:
   + The ID of the pull request \(with the \-\-pull\-request\-id option\)\.
   + The title of the pull request \(with the \-\-title option\)\.

   For example, to update the title of a pull request with the ID of *47*:

   ```
   aws codecommit update-pull-request-title --pull-request-id 47 --title "Consolidation of global variables - updated review"  
   ```

   If successful, this command produces output similar to the following:

   ```
   {
      "pullRequest": { 
         "authorArn": "arn:aws:iam::111111111111:user/Li_Juan",
         "clientRequestToken": "",
         "creationDate": 1508530823.12,
         "description": "Review the latest changes and updates to the global variables. I have updated this request with some changes, including removing some unused variables.",
         "lastActivityDate": 1508372657.188,
         "pullRequestId": "47",
         "pullRequestStatus": "OPEN",
         "pullRequestTargets": [ 
            { 
               "destinationCommit": "9f31c968EXAMPLE",
               "destinationReference": "refs/heads/master",
               "mergeMetadata": { 
                  "isMerged": false,
               },
               "repositoryName": "MyDemoRepo",
               "sourceCommit": "99132ab0EXAMPLE",
               "sourceReference": "refs/heads/variables-branch"
            }
         ],
         "title": "Consolidation of global variables - updated review"
      }
   }
   ```

1. To update the description of a pull request, run the update\-pull\-request\-description command, specifying:
   + The ID of the pull request \(with the \-\-pull\-request\-id option\)\.
   + The description \(with the \-\-description option\)\. 

    For example, to update the description of a pull request with the ID of *47* :

   ```
   aws codecommit update-pull-request-description --pull-request-id 47 --description "Updated the pull request to remove unused global variable."
   ```

   If successful, this command produces output similar to the following:

   ```
   {
      "pullRequest": { 
         "authorArn": "arn:aws:iam::111111111111:user/Li_Juan",
         "clientRequestToken": "",
         "creationDate": 1508530823.155,
         "description": "Updated the pull request to remove unused global variable.",
         "lastActivityDate": 1508372423.204,
         "pullRequestId": "47",
         "pullRequestStatus": "OPEN",
         "pullRequestTargets": [ 
            { 
               "destinationCommit": "9f31c968EXAMPLE",
               "destinationReference": "refs/heads/master",
               "mergeMetadata": { 
                  "isMerged": false,
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