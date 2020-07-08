# View commit details in AWS CodeCommit<a name="how-to-view-commit-details"></a>

You can use the AWS CodeCommit console to browse the history of commits in a repository\. This can help you identify changes made in a repository, including:
+ When and by whom the changes were made\.
+ When specific commits were merged into a branch\.

Viewing the history of commits for a branch might also help you understand the difference between branches\. If you use tagging, you can also quickly view the commit that was labeled with a tag and the parents of that tagged commit\. At the command line, you can use Git to view details about the commits in a local repo or a CodeCommit repository\. 

## Browse commits in a repository<a name="how-to-view-commit-details-console"></a>

You can use the AWS CodeCommit console to browse the history of commits to a repository\. You can also view a graph of the commits in the repository and its branches over time\. This can help you understand the history of the repository, including when changes were made\.

**Note**  
Using the git rebase command to rebase a repository changes the history of a repository, which might cause commits to appear out of order\. For more information, see [Git Branching\-Rebasing](https://git-scm.com/book/en/v2/Git-Branching-Rebasing) or your Git documentation\.

**Topics**
+ [Browse the commit history of a repository](#how-to-view-commit-details-console-history)
+ [View a graph of the commit history of a repository](#how-to-view-commit-details-console-visualizer)

### Browse the commit history of a repository<a name="how-to-view-commit-details-console-history"></a>

You can browse the commit history for a specific branch or tag of the repository, including information about the committer and the commit message\. You can also view the code for a commit\.

**To browse the history of commits**

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In **Repositories**, choose the repository for which you want to review the commit history\. 

1. In the navigation pane, choose **Commits**\. In the commit history view, a history of commits for the repository in the default branch is displayed, in reverse chronological order of the commit date\. Date and time are in coordinated universal time \(UTC\)\. You can view the commit history of a different branch by choosing the view selector button and then choosing a branch from the list\. If you are using tags in your repository, you can view a commit with a specific tag and its parents by choosing that tag in the view selector button\.  
![\[The commits view in the console\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-commit-list.png)

1. To view the difference between a commit and its parent, and to see any comments on the changes, choose the abbreviated commit ID\. For more information, see [Compare a commit to its parent](how-to-compare-commits.md#how-to-compare-commits-parent) and [Comment on a commit](how-to-commit-comment.md)\. To view the difference between a commit and any other commit specifier, including a branch, tag, or commit ID, see [Compare any two commit specifiers](how-to-compare-commits.md#how-to-compare-commits-compare)\.

1. Do one or more of the following:
   + To view the date and time a change was made, hover over the commit date\.
   + To view the full commit ID, copy and then paste it into a text editor or other location\. To copy it, choose **Copy ID**\.
   + To view the code as it was at the time of a commit, choose **Browse**\. The contents of the repository as they were at the time of that commit is displayed in the **Code** view\. The view selector button displays the abbreviated commit ID instead of a branch or tag\.

### View a graph of the commit history of a repository<a name="how-to-view-commit-details-console-visualizer"></a>

You can view a graph of the commits made to a repository\. The **Commit Visualizer** view is a directed acyclic graph \(DAG\) representation of all the commits made to a branch of the repository\. This graphical representation can help you understand when commits and associated features were added or merged\. It can also help you pinpoint when a change was made in relation to other changes\.

**Note**  
Commits that are merged using the fast\-forward method do not appear as separate lines in the graph of commits\.

**To view a graph of commits**

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In **Repositories**, choose the repository for which you want to view a commit graph\. 

1. In the navigation pane, choose **Commits**, and then choose the **Commit visualizer** tab\.  
![\[A graphical view of a repository in the console\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-cv-complex1.png)

   In the commit graph, the abbreviated commit ID and the subject for each commit message appears next to that point in the graph\. 
**Note**  
The graph can display up to 35 branches on a page\. If there are more than 35 branches, the graph is too complex to display\. You can simplify the view in two ways:   
By using the view selector button to show the graph for a specific branch\.
By pasting a full commit ID into the search box to render the graph from that commit\.

1. To render a new graph from a commit, choose the point in the graph that corresponds to that commit\. The view selector button changes to the abbreviated commit ID\.  
![\[A new graph rendered from a specific commit\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-cv-commit.png)

## View commit details \(AWS CLI\)<a name="how-to-view-commit-details-cli"></a>

Git lets you view details about commits\. You can also use the AWS CLI to view details about the commits in a local repo or in a CodeCommit repository by running the following commands:
+ To view information about a commit, run [aws codecommit get\-commit](#how-to-view-commit-details-cli-commit)\.
+ To view information about multiple commits, run [aws codecommit batch\-get\-commits](#how-to-view-commit-details-cli-batch-get-commits)\.
+ To view information about a merge commit, run [aws codecommit get\-merge\-commit](#how-to-view-commit-details-cli-merge-commit)\.
+ To view information about changes for a commit specifier \(branch, tag, HEAD, or other fully qualified references, such as commit IDs\), run [aws codecommit get\-differences](#how-to-view-commit-details-cli-differences)\.
+ To view the base64\-encoded content of a Git blob object in a repository, run [aws codecommit get\-blob](#how-to-view-commit-details-cli-blob)\.

### To view information about a commit<a name="how-to-view-commit-details-cli-commit"></a>

1. Run the aws codecommit get\-commit command, specifying:
   + The name of the CodeCommit repository \(with the `--repository-name` option\)\.
   + The full commit ID\. 

   For example, to view information about a commit with the ID `317f8570EXAMPLE` in a CodeCommit repository named `MyDemoRepo`:

   ```
   aws codecommit get-commit  --repository-name MyDemoRepo  --commit-id 317f8570EXAMPLE 
   ```

1. If successful, the output of this command includes the following:
   + Information about the author of the commit \(as configured in Git\), including the date in timestamp format and the coordinated universal time \(UTC\) offset\.
   + Information about the committer \(as configured in Git\) including the date in timestamp format and the UTC offset\.
   + The ID of the Git tree where the commit exists\.
   + The commit ID of the parent commit\.
   + The commit message\.

   Here is some example output, based on the preceding example command:

   ```
   {
       "commit": {
           "additionalData": "",
           "committer": {
               "date": "1484167798 -0800",
               "name": "Mary Major",
               "email": "mary_major@example.com"
           },
           "author": {
               "date": "1484167798 -0800",
               "name": "Mary Major",
               "email": "mary_major@example.com"
           },
           "treeId": "347a3408EXAMPLE",
           "parents": [
               "4c925148EXAMPLE"
           ],
           "message": "Fix incorrect variable name"
       }
   }
   ```

### To view information about a merge commit<a name="how-to-view-commit-details-cli-merge-commit"></a>

1. Run the get\-merge\-commit command, specifying:
   + A commit specifier for the source of the merge \(with the \-\-source\-commit\-specifier option\)\.
   + A commit specifier for the destination for the merge \(with the \-\-destination\-commit\-specifier option\)\. 
   + The merge option you want to use \(with the \-\-merge\-option option\)\.
   + The name of the repository \(with the \-\-repository\-name option\)\.

   For example, to view information about a merge commit for the source branch named *bugfix\-bug1234* with a destination branch named *master* using the *THREE\_WAY\_MERGE* strategy in a repository named *MyDemoRepo*:

   ```
   aws codecommit get-merge-commit --source-commit-specifier bugfix-bug1234 --destination-commit-specifier master --merge-option THREE_WAY_MERGE --repository-name MyDemoRepo
   ```

1. If successful, the output of this command returns information similar to the following:

   ```
   {
       "sourceCommitId": "c5709475EXAMPLE", 
       "destinationCommitId": "317f8570EXAMPLE", 
       "baseCommitId": "fb12a539EXAMPLE",
       "mergeCommitId": "ffc4d608eEXAMPLE"
   }
   ```

### To view information about multiple commits<a name="how-to-view-commit-details-cli-batch-get-commits"></a>

1. Run the batch\-get\-commits command, specifying:
   + The name of the CodeCommit repository \(with the `--repository-name` option\)\.
   + A list of full commit IDs for every commit about which you want to view information\. 

   For example, to view information about commits with the IDs `317f8570EXAMPLE` and `4c925148EXAMPLE` in a CodeCommit repository named `MyDemoRepo`:

   ```
   aws codecommit batch-get-commits  --repository-name MyDemoRepo  --commit-ids 317f8570EXAMPLE 4c925148EXAMPLE
   ```

1. If successful, the output of this command includes the following:
   + Information about the authors of the commits \(as configured in Git\), including the date in timestamp format and the coordinated universal time \(UTC\) offset\.
   + Information about the committers \(as configured in Git\) including the date in timestamp format and the UTC offset\.
   + The IDs of the Git tree where the commit exists\.
   + The commit IDs of the parent commit\.
   + The commit messages\.

   Here is some example output, based on the preceding example command:

   ```
   {
       "commits": [
         {
           "additionalData": "",
           "committer": {
               "date": "1508280564 -0800",
               "name": "Mary Major",
               "email": "mary_major@example.com"
           },
           "author": {
               "date": "1508280564 -0800",
               "name": "Mary Major",
               "email": "mary_major@example.com"
           },
           "commitId": "317f8570EXAMPLE",
           "treeId": "1f330709EXAMPLE",
           "parents": [
               "6e147360EXAMPLE"
           ],
           "message": "Change variable name and add new response element"
       },
       {
           "additionalData": "",
           "committer": {
               "date": "1508280542 -0800",
               "name": "Li Juan",
               "email": "li_juan@example.com"
           },
           "author": {
               "date": "1508280542 -0800",
               "name": "Li Juan",
               "email": "li_juan@example.com"
           },
           "commitId": "4c925148EXAMPLE",
           "treeId": "1f330709EXAMPLE",
           "parents": [
               "317f8570EXAMPLE"
           ],
           "message": "Added new class"
       }   
   }
   ```

### To view information about the changes for a commit specifier<a name="how-to-view-commit-details-cli-differences"></a>

1. Run the aws codecommit get\-differences command, specifying:
   + The name of the CodeCommit repository \(with the `--repository-name` option\)\.
   + The commit specifiers you want to get information about\. Only `--after-commit-specifier` is required\. If you do not specify `--before-commit-specifier`, all files current as of the `--after-commit-specifier` are shown\. 

   For example, to view information about the differences between commits with the IDs `317f8570EXAMPLE` and `4c925148EXAMPLE` in a CodeCommit repository named `MyDemoRepo`:

   ```
   aws codecommit get-differences  --repository-name MyDemoRepo  --before-commit-specifier 317f8570EXAMPLE --after-commit-specifier 4c925148EXAMPLE
   ```

1. If successful, the output of this command includes the following:
   + A list of differences, including the change type \(A for added, D for deleted, or M for modified\)\.
   + The mode of the file change type\.
   + The ID of the Git blob object that contains the change\.

   Here is some example output, based on the preceding example command:

   ```
   {
       "differences": [
           {
               "afterBlob": {
                   "path": "blob.txt",
                   "blobId": "2eb4af3bEXAMPLE",
                   "mode": "100644"
               },
               "changeType": "M",
               "beforeBlob": {
                   "path": "blob.txt",
                   "blobId": "bf7fcf28fEXAMPLE",
                   "mode": "100644"
               }
           }
       ]
   }
   ```

### To view information about a Git blob object<a name="how-to-view-commit-details-cli-blob"></a>

1. Run the aws codecommit get\-blob command, specifying:
   + The name of the CodeCommit repository \(with the `--repository-name` option\)\.
   + The ID of the Git blob \(with the `--blob-id `option\)\. 

   For example, to view information about a Git blob with the ID of `2eb4af3bEXAMPLE` in a CodeCommit repository named `MyDemoRepo`:

   ```
   aws codecommit get-blob  --repository-name MyDemoRepo  --blob-id 2eb4af3bEXAMPLE
   ```

1. If successful, the output of this command includes the following:
   + The base64\-encoded content of the blob, usually a file\.

   For example, the output of the previous command might be similar to the following:

   ```
   {
       "content": "QSBCaW5hcnkgTGFyToEXAMPLE="
   }
   ```

## View commit details \(Git\)<a name="how-to-view-commit-details-git"></a>

Before you follow these steps, you should have already connected the local repo to the CodeCommit repository and committed changes\. For instructions, see [Connect to a repository](how-to-connect.md)\.

To show the changes for the most recent commit to a repository, run the git show command\.

```
git show
```

The command produces output similar to the following:

```
commit 4f8c6f9d
Author: Mary Major <mary.major@example.com>
Date:   Mon May 23 15:56:48 2016 -0700

    Added bumblebee.txt

diff --git a/bumblebee.txt b/bumblebee.txt
new file mode 100644
index 0000000..443b974
--- /dev/null
+++ b/bumblebee.txt
@@ -0,0 +1 @@
+A bumblebee, also written bumble bee, is a member of the bee genus Bombus, in the family Apidae.
\ No newline at end of file
```

**Note**  
In this and the following examples, commit IDs have been abbreviated\. The full commit IDs are not shown\.

To view the changes that occurred, use the git show command with the commit ID:

```
git show 94ba1e60

commit 94ba1e60
Author: John Doe <johndoe@example.com>
Date:   Mon May 23 15:39:14 2016 -0700

    Added horse.txt

diff --git a/horse.txt b/horse.txt
new file mode 100644
index 0000000..080f68f
--- /dev/null
+++ b/horse.txt
@@ -0,0 +1 @@
+The horse (Equus ferus caballus) is one of two extant subspecies of Equus ferus.
```

To see the differences between two commits, run the git diff command and include the two commit IDs\.

```
git diff ce22850d 4f8c6f9d
```

In this example, the difference between the two commits is that two files were added\. The command produces output similar to the following:

```
diff --git a/bees.txt b/bees.txt
new file mode 100644
index 0000000..cf57550
--- /dev/null
+++ b/bees.txt
@@ -0,0 +1 @@
+Bees are flying insects closely related to wasps and ants, and are known for their role in pollination and for producing honey and beeswax.
diff --git a/bumblebee.txt b/bumblebee.txt
new file mode 100644
index 0000000..443b974
--- /dev/null
+++ b/bumblebee.txt
@@ -0,0 +1 @@
+A bumblebee, also written bumble bee, is a member of the bee genus Bombus, in the family Apidae.
\ No newline at end of file
```

To use Git to view details about the commits in a local repo, run the git log command:

```
git log
```

If successful, this command produces output similar to the following:

```
commit 94ba1e60
Author: John Doe <johndoe@example.com>
Date:   Mon May 23 15:39:14 2016 -0700

    Added horse.txt

commit 4c925148
Author: Jane Doe <janedoe@example.com>
Date:   Mon May 22 14:54:55 2014 -0700

    Added cat.txt and dog.txt
```

To show only commit IDs and messages, run the git log \-\-pretty=oneline command:

```
git log --pretty=oneline
```

If successful, this command produces output similar to the following:

```
94ba1e60 Added horse.txt
4c925148 Added cat.txt and dog.txt
```

For more options, see your Git documentation\.