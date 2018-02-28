# Create a Commit in AWS CodeCommit<a name="how-to-create-commit"></a>

Follow these steps to use Git to create a commit in a local repo\. If the local repo is connected to an AWS CodeCommit repository, you use Git to push the commit from the local repo to the AWS CodeCommit repository\.

1. Complete the prerequisites, including [Setting Up ](setting-up.md)\.
**Important**  
If you have not completed setup, you will not be able to connect or commit to the repository\.

1. Make sure you are creating a commit in the desired branch\. To see a list of available branches and find out which branch you are currently set to use, run git branch\. All branches will be displayed\. An asterisk \(`*`\) will appear next to your current branch\. To switch to a different branch, run git checkout *branch\-name*\.

1. Make a change to the branch \(such as adding, modifying, or deleting a file\)\.

   For example, in the local repo, create a file named `bird.txt` with the following text:

   ```
   bird.txt
   --------
   Birds (class Aves or clade Avialae) are feathered, winged, two-legged, warm-blooded, egg-laying vertebrates.
   ```

1. Run git status, which should indicate that `bird.txt` has not yet been included in any pending commit:

   ```
   ...        
   Untracked files:
     (use "git add <file>..." to include in what will be committed)
           
           bird.txt
   ```

1. Run git add bird\.txt to include the new file in the pending commit\.

1. If you run git status again, you should see output similar to the following\. It indicates that `bird.txt` is now part of the pending commit or staged for commit:

   ```
   ...
   Changes to be committed:
     (use "git reset HEAD <file>..." to unstage)
       
           new file:   bird.txt
   ```

1. To finalize the commit, run git commit with the `-m` option \(for example,  git commit \-m "*Adding bird\.txt to the repository\.*"\) The `-m` option creates the commit message\. 

1. If you run git status again, you should see output similar to the following\. It indicates that the commit is ready to be pushed from the local repo to the AWS CodeCommit repository:

   ```
   ...    
   nothing to commit, working directory clean
   ```

1. Before you push the finalized commit from the local repo to the AWS CodeCommit repository, you can see what will be pushed by running git diff \-\-stat *remote\-name*/*branch\-name*, where *remote\-name* is the nickname the local repo uses for the AWS CodeCommit repository and *branch\-name* is the name of the branch to compare\.
**Tip**  
To get the nickname, run git remote\. To get a list of branch names, run git branch\. An asterisk \(`*`\) will appear next to the current branch\. You can also run git status to get the current branch name\.
**Note**  
If you cloned the repository, from the local repo's perspective, *remote\-name* is not the name of the AWS CodeCommit repository\. When you clone a repository, *remote\-name* is set automatically to `origin`\. 

   For example, git diff \-\-stat origin/master would show output similar to the following:

   ```
   bird.txt | 1 +
   1 file changed, 1 insertion(+)
   ```

   Of course, the output assumes you have already connected the local repo to the AWS CodeCommit repository\. \(For instructions, see [Connect to a Repository](how-to-connect.md)\.\)

1. When you're ready to push the commit from the local repo to the AWS CodeCommit repository, run git push *remote\-name* *branch\-name*, where *remote\-name* is the nickname the local repo uses for the AWS CodeCommit repository and *branch\-name* is the name of the branch to push to the AWS CodeCommit repository\.

   For example, running git push origin master would show output similar to the following:

   For HTTPS:

   ```
   Counting objects: 7, done.
   Delta compression using up to 4 threads.
   Compressing objects: 100% (4/4), done.
   Writing objects: 100% (5/5), 516 bytes | 0 bytes/s, done.
   Total 5 (delta 2), reused 0 (delta 0)
   remote:
   To https://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo
       b9e7aa6..3dbf4dd master -> master
   ```

   For SSH:

   ```
   Counting objects: 7, done.
   Delta compression using up to 4 threads.
   Compressing objects: 100% (4/4), done.
   Writing objects: 100% (5/5), 516 bytes | 0 bytes/s, done.
   Total 5 (delta 2), reused 0 (delta 0)
   remote:
   To ssh://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo
       b9e7aa6..3dbf4dd master -> master
   ```
**Tip**  
If you add the `-u` option to git push \(for example, git push \-u origin master\), then you only need to run git push in the future because upstream tracking information has been set\. To get upstream tracking information, run git remote show *remote\-name* \(for example, git remote show origin\)\.

For more options, see your Git documentation\.