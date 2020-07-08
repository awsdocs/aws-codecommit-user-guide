# Resolve conflicts in a pull request in an AWS CodeCommit repository<a name="how-to-resolve-conflict-pull-request"></a>

If your pull request has conflicts and cannot be merged, you can try to resolve the conflicts in one of several ways:
+ On your local computer, you can use the git diff command to find the conflicts between the two branches and make changes to resolve them\. You can also use a difference tool or other software to help you find and resolve differences\. Once you have resolved them to your satisfaction, you can push your source branch with the changes that contain the resolved conflicts, which will update the pull request\. For more information about git diff and git difftool, see your Git documentation\.
+ In the console, you can choose **Resolve conflicts**\. This opens a plain\-text editor that shows conflicts in a similar way as the git diff command\. You can manually review the conflicts in each file that contain them, make changes, and then update the pull request with your changes\.
+ In the AWS CLI, you can use the AWS CLI to get information about merge conflicts and create an unreferenced merge commit to test a merge\. 

**Topics**
+ [Resolve conflicts in a pull request \(console\)](#how-to-resolve-conflict-pull-request-console)
+ [Resolve conflicts in a pull request \(AWS CLI\)](#how-to-resolve-conflict-pull-request-cli)

## Resolve conflicts in a pull request \(console\)<a name="how-to-resolve-conflict-pull-request-console"></a>

You can use the CodeCommit console to resolve conflicts in a pull request in a CodeCommit repository\. 

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In **Repositories**, choose the name of the repository\. 

1. In the navigation pane, choose **Pull requests**\.

1. By default, a list of all open pull requests is displayed\. Choose the open pull request that you want to merge but it contains conflicts\.

1. In the pull request, choose **Resolve conflicts**\. This option only appears if there are conflicts that must be resolved before the pull request can be merged\.  
![\[A pull request showing that it has conflicts that must be resolved before it can be merged.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-pull-request-resolve-conflicts.png)

1. A conflict resolution window opens listing each file that has conflicts that must be resolved\. Choose each file in the list to review the conflicts, and make any necessary changes until all conflicts have been resolved\.  
![\[The conflict resolution editor showing a file with conflicts that have not yet been resolved.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-pull-request-resolve.png)
   + You can choose to use the source file contents, the destination file contents, or if the file is not a binary file, to manually edit the contents of a file so it contains only the changes you want\. Standard git diff markers are used to show the conflicts between the destination \(HEAD\) and source branches in the file\.
   + If a file is a binary file, a Git submodule, or if there is a file/folder name conflict, you must choose to use the source file or the destination file to resolve the conflicts\. You cannot view or edit binary files in the CodeCommit console\.
   + If there are file mode conflicts, you will see the option to resolve that conflict by choosing between the file mode of the source file and the file mode of the destination file\. 
   + If you decide you want to discard your changes for a file and restore it to its conflicted state, choose **Reset file**\. This allows you to resolve the conflicts in a different way\.

1. When you are satisfied with your changes, choose **Update pull request**\.
**Note**  
You must resolve all conflicts in all files before you can successfully update the pull request with your changes\. 

1. The pull request is updated with your changes and mergeable\. You will see the merge page\. You can choose to merge the pull request at this time, or you can return to the list of pull requests\. 

## Resolve conflicts in a pull request \(AWS CLI\)<a name="how-to-resolve-conflict-pull-request-cli"></a>

To use AWS CLI commands with CodeCommit, install the AWS CLI\. For more information, see [Command line reference](cmd-ref.md)\. 

No single AWS CLI command will enable you to resolve conflicts in a pull request and merge that request\. However, you can use individual commands to discover conflicts, attempt to resolve them, and test whether a pull request is mergeable\. You can use:
+ get\-merge\-options, to find out what merge options are available for a merge between two commit specifiers\.
+ get\-merge\-conflicts, to return a list of files with merge conflicts in a merge between two commit specifiers\.
+ batch\-describe\-merge\-conflicts, to get information about all merge conflicts in files in a merge between two commits using a specified merge strategy\. 
+ describe\-merge\-conflicts, to get detailed information about merge conflicts for a specific file between two commits using a specified merge strategy\.
+ create\-unreferenced\-merge\-commit, to test the result of merging two commit specifiers using a specified merge strategy\.

1. <a name="get-merge-options"></a>To discover what merge options are available for a merge between two commit specifiers, run the get\-merge\-options command, specifying:
   + A commit specifier for the source of the merge \(with the \-\-source\-commit\-specifier option\)\.
   + A commit specifier for the destination for the merge \(with the \-\-destination\-commit\-specifier option\)\. 
   + The name of the repository \(with the \-\-repository\-name option\)\.
   + \(Optional\) A conflict resolution strategy to use \(with the \-\-conflict\-resolution\-strategy option\)\.
   + \(Optional\) The level of detail you want about any conflicts \(with the \-\-conflict\-detail\-level option\)\.

    For example, to determine the merge strategies available for merging a source branch named *bugfix\-1234* with a destination branch named *master* in a repository named *MyDemoRepo*:

   ```
   aws codecommit get-merge-options --source-commit-specifier bugfix-1234 --destination-commit-specifier master --repository-name MyDemoRepo
   ```

   If successful, this command produces output similar to the following:

   ```
   {
       "mergeOptions": [
           "FAST_FORWARD_MERGE",
           "SQUASH_MERGE",
           "THREE_WAY_MERGE"
       ],
       "sourceCommitId": "d49940adEXAMPLE",
       "destinationCommitId": "86958e0aEXAMPLE",
       "baseCommitId": "86958e0aEXAMPLE"
   }
   ```

1. <a name="get-merge-conflict"></a>To get a list of files that contain merge conflicts in a merge between two commit specifiers, run the get\-merge\-conflicts command, specifying:
   + A commit specifier for the source of the merge \(with the \-\-source\-commit\-specifier option\)\.
   + A commit specifier for the destination for the merge \(with the \-\-destination\-commit\-specifier option\)\. 
   + The name of the repository \(with the \-\-repository\-name option\)\.
   + The merge option you want to use \(with the \-\-merge\-option option\)\.
   + \(Optional\) The level of detail you want about any conflicts \(with the \-\-conflict\-detail\-level option\)\.
   + \(Optional\) A conflict resolution strategy to use \(with the \-\-conflict\-resolution\-strategy option\)\.
   + \(Optional\) The maximum number of files with conflicts to return \(with the \-\-max\-conflict\-files option\)\.

   For example, to get a list of files that contain conflicts in a merge between a source branch named feature\-randomizationfeature and a destination branch named master using the three\-way merge strategy in a repository named MyDemoRepo:

   ```
   aws codecommit get-merge-conflicts --source-commit-specifier feature-randomizationfeature --destination-commit-specifier master --merge-option THREE_WAY_MERGE --repository-name MyDemoRepo
   ```

   If successful, this command produces output similar to the following:

   ```
   {
       "mergeable": false,
       "destinationCommitId": "86958e0aEXAMPLE",
       "sourceCommitId": "6ccd57fdEXAMPLE",
       "baseCommitId": "767b6958EXAMPLE",
       "conflictMetadataList": [
           {
               "filePath": "readme.md",
               "fileSizes": {
                   "source": 139,
                   "destination": 230,
                   "base": 85
               },
               "fileModes": {
                   "source": "NORMAL",
                   "destination": "NORMAL",
                   "base": "NORMAL"
               },
               "objectTypes": {
                   "source": "FILE",
                   "destination": "FILE",
                   "base": "FILE"
               },
               "numberOfConflicts": 1,
               "isBinaryFile": {
                   "source": false,
                   "destination": false,
                   "base": false
               },
               "contentConflict": true,
               "fileModeConflict": false,
               "objectTypeConflict": false,
               "mergeOperations": {
                   "source": "M",
                   "destination": "M"
               }
           }
       ]
   }
   ```

1. <a name="batch-describe-merge-conflicts"></a>To get information about merge conflicts in all files or a subset of files in a merge between two commit specifiers, run the batch\-describe\-merge\-conflicts command, specifying:
   + A commit specifier for the source of the merge \(with the \-\-source\-commit\-specifier option\)\.
   + A commit specifier for the destination for the merge \(with the \-\-destination\-commit\-specifier option\)\. 
   + The merge option you want to use \(with the \-\-merge\-option option\)\.
   + The name of the repository \(with the \-\-repository\-name option\)\.
   + \(Optional\) A conflict resolution strategy to use \(with the \-\-conflict\-resolution\-strategy option\)\.
   + \(Optional\) The level of detail you want about any conflicts \(with the \-\-conflict\-detail\-level option\)\.
   + \(Optional\) The maximum number of merge hunks to return \(with the \-\-max\-merge\-hunks option\)\.
   + \(Optional\) The maximum number of files with conflicts to return \(with the \-\-max\-conflict\-files option\)\.
   + \(Optional\) The path of target files to use to describe the conflicts \(with the \-\-file\-paths option\)\.

    For example, to determine the merge conflicts for merging a source branch named *feature\-randomizationfeature* with a destination branch named *master* using the *THREE\_WAY\_MERGE* strategy in a repository named *MyDemoRepo*:

   ```
   aws codecommit batch-describe-merge-conflicts --source-commit-specifier feature-randomizationfeature --destination-commit-specifier master --merge-option THREE_WAY_MERGE --repository-name MyDemoRepo
   ```

   If successful, this command produces output similar to the following:

   ```
   {
       "conflicts": [
           {
               "conflictMetadata": {
                   "filePath": "readme.md",
                   "fileSizes": {
                       "source": 139,
                       "destination": 230,
                       "base": 85
                   },
                   "fileModes": {
                       "source": "NORMAL",
                       "destination": "NORMAL",
                       "base": "NORMAL"
                   },
                   "objectTypes": {
                       "source": "FILE",
                       "destination": "FILE",
                       "base": "FILE"
                   },
                   "numberOfConflicts": 1,
                   "isBinaryFile": {
                       "source": false,
                       "destination": false,
                       "base": false
                   },
                   "contentConflict": true,
                   "fileModeConflict": false,
                   "objectTypeConflict": false,
                   "mergeOperations": {
                       "source": "M",
                       "destination": "M"
                   }
               },
               "mergeHunks": [
                   {
                       "isConflict": true,
                       "source": {
                           "startLine": 0,
                           "endLine": 3,
                           "hunkContent": "VGhpcyBpEXAMPLE=="
                       },
                       "destination": {
                           "startLine": 0,
                           "endLine": 1,
                           "hunkContent": "VXNlIHRoEXAMPLE="
                       }
                   }
               ]
           }
       ],
       "errors": [],
       "destinationCommitId": "86958e0aEXAMPLE",
       "sourceCommitId": "6ccd57fdEXAMPLE",
       "baseCommitId": "767b6958EXAMPLE"
   }
   ```

1. <a name="describe-merge-conflicts"></a>To get detailed information about any merge conflicts for a specific file in a merge between two commit specifiers, run the describe\-merge\-conflicts command, specifying:
   + A commit specifier for the source of the merge \(with the \-\-source\-commit\-specifier option\)\.
   + A commit specifier for the destination for the merge \(with the \-\-destination\-commit\-specifier option\)\. 
   + The merge option you want to use \(with the \-\-merge\-option option\)\.
   + The path of target file to use to describe the conflicts \(with the \-\-file\-path option\)\.
   + The name of the repository \(with the \-\-repository\-name option\)\.
   + \(Optional\) A conflict resolution strategy to use \(with the \-\-conflict\-resolution\-strategy option\)\.
   + \(Optional\) The level of detail you want about any conflicts \(with the \-\-conflict\-detail\-level option\)\.
   + \(Optional\) The maximum number of merge hunks to return \(with the \-\-max\-merge\-hunks option\)\.
   + \(Optional\) The maximum number of files with conflicts to return \(with the \-\-max\-conflict\-files option\)\.

   For example, to determine the merge conflicts for a file named *readme\.md* in a source branch named *feature\-randomizationfeature* with a destination branch named *master* using the *THREE\_WAY\_MERGE* strategy in a repository named *MyDemoRepo*:

   ```
   aws codecommit describe-merge-conflicts --source-commit-specifier feature-randomizationfeature --destination-commit-specifier master --merge-option THREE_WAY_MERGE --file-path readme.md --repository-name MyDemoRepo
   ```

   If successful, this command produces output similar to the following:

   ```
   {
       "conflictMetadata": {
           "filePath": "readme.md",
           "fileSizes": {
               "source": 139,
               "destination": 230,
               "base": 85
           },
           "fileModes": {
               "source": "NORMAL",
               "destination": "NORMAL",
               "base": "NORMAL"
           },
           "objectTypes": {
               "source": "FILE",
               "destination": "FILE",
               "base": "FILE"
           },
           "numberOfConflicts": 1,
           "isBinaryFile": {
               "source": false,
               "destination": false,
               "base": false
           },
           "contentConflict": true,
           "fileModeConflict": false,
           "objectTypeConflict": false,
           "mergeOperations": {
               "source": "M",
               "destination": "M"
           }
       },
       "mergeHunks": [
           {
               "isConflict": true,
               "source": {
                   "startLine": 0,
                   "endLine": 3,
                   "hunkContent": "VGhpcyBpEXAMPLE=="
               },
               "destination": {
                   "startLine": 0,
                   "endLine": 1,
                   "hunkContent": "VXNlIHRoEXAMPLE="
               }
           }
       ],
       "destinationCommitId": "86958e0aEXAMPLE",
       "sourceCommitId": "6ccd57fdEXAMPLE",
       "baseCommitId": "767b69580EXAMPLE"
   }
   ```

1. <a name="create-unreferenced-merge-commit"></a>To create an unreferenced commit that represents the result of merging two commit specifiers, run the create\-unreferenced\-merge\-commit command, specifying:
   + A commit specifier for the source of the merge \(with the \-\-source\-commit\-specifier option\)\.
   + A commit specifier for the destination for the merge \(with the \-\-destination\-commit\-specifier option\)\. 
   + The merge option you want to use \(with the \-\-merge\-option option\)\.
   + The name of the repository \(with the \-\-repository\-name option\)\.
   + \(Optional\) A conflict resolution strategy to use \(with the \-\-conflict\-resolution\-strategy option\)\.
   + \(Optional\) The level of detail you want about any conflicts \(with the \-\-conflict\-detail\-level option\)\.
   + \(Optional\) The commit message to include \(with the \-\-commit\-message option\)\.
   + \(Optional\) The name to use for the commit \(with the \-\-name option\)\.
   + \(Optional\) The email address to use for the commit \(with the \-\-email option\)\.
   + \(Optional\) Whether to keep any empty folders \(with the \-\-keep\-empty\-folders option\)\.

    For example, to determine the merge conflicts for merging a source branch named *bugfix\-1234* with a destination branch named *master* using the ACCEPT\_SOURCE strategy in a repository named *MyDemoRepo*:

   ```
   aws codecommit create-unreferenced-merge-commit --source-commit-specifier bugfix-1234 --destination-commit-specifier master --merge-option THREE_WAY_MERGE --repository-name MyDemoRepo --name "Maria Garcia" --email "maria_garcia@example.com" --commit-message "Testing the results of this merge."
   ```

   If successful, this command produces output similar to the following:

   ```
   {
       "commitId": "4f178133EXAMPLE",
       "treeId": "389765daEXAMPLE"
   }
   ```