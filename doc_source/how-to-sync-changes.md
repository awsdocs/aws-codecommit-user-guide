# Synchronize Changes Between a Local Repo and an AWS CodeCommit Repository<a name="how-to-sync-changes"></a>

You use Git to synchronize changes between a local repo and the AWS CodeCommit repository connected to the local repo\.

To push changes from the local repo to the AWS CodeCommit repository, run git push *remote\-name* *branch\-name*\.

To pull changes to the local repo from the AWS CodeCommit repository, run git pull *remote\-name* *branch\-name*\.

For both pushing and pulling, *remote\-name* is the nickname the local repo uses for the AWS CodeCommit repository; *branch\-name* is the name of the branch on the AWS CodeCommit repository to push to or pull from\.

**Tip**  
To get the nickname the local repo uses for the AWS CodeCommit repository, run git remote\. To get a list of branch names, run git branch\. An asterisk \(`*`\) appears next to the name of the current branch\. \(Alternatively, run git status to show the current branch name\.\)

**Note**  
If you cloned the repository, from the local repo's perspective, *remote\-name* is not the name of the AWS CodeCommit repository\. When you clone a repository, *remote\-name* is set automatically to `origin`\. 

For example, to push changes from the local repo to the `master` branch in the AWS CodeCommit repository with the nickname `origin`:

```
git push origin master
```

Similarly, to pull changes to the local repo from the `master` branch in the AWS CodeCommit repository with the nickname `origin`:

```
git pull origin master
```

**Tip**  
If you add the `-u` option to git push, you will set upstream tracking information\. For example, if you run git push \-u origin master\), in the future you can run git push and git pull without *remote\-name* *branch\-name*\. To get upstream tracking information, run git remote show *remote\-name* \(for example, git remote show origin\)\.

For more options, see your Git documentation\.