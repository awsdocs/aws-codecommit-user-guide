# Synchronize changes between a local repo and an AWS CodeCommit repository<a name="how-to-sync-changes"></a>

You use Git to synchronize changes between a local repo and the CodeCommit repository connected to the local repo\.

To push changes from the local repo to the CodeCommit repository, run git push *remote\-name* *branch\-name*\.

To pull changes to the local repo from the CodeCommit repository, run git pull *remote\-name* *branch\-name*\.

For both pushing and pulling, *remote\-name* is the nickname the local repo uses for the CodeCommit repository\. *branch\-name* is the name of the branch on the CodeCommit repository to push to or pull from\.

**Tip**  
To get the nickname the local repo uses for the CodeCommit repository, run git remote\. To get a list of branch names, run git branch\. An asterisk \(`*`\) appears next to the name of the current branch\. \(You can also run git status to show the current branch name\.\)

**Note**  
If you cloned the repository, from the perspective of the local repo, *remote\-name* is not the name of the CodeCommit repository\. When you clone a repository, *remote\-name* is set automatically to `origin`\. 

For example, to push changes from the local repo to the `master` branch in the CodeCommit repository with the nickname `origin`:

```
git push origin master
```

Similarly, to pull changes to the local repo from the `master` branch in the CodeCommit repository with the nickname `origin`:

```
git pull origin master
```

**Tip**  
If you add the `-u` option to git push, you set upstream tracking information\. For example, if you run git push \-u origin master\), in the future you can run git push and git pull without *remote\-name* *branch\-name*\. To get upstream tracking information, run git remote show *remote\-name* \(for example, git remote show origin\)\.

For more options, see your Git documentation\.