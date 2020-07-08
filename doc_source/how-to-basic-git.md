# Basic Git commands<a name="how-to-basic-git"></a>

You can use Git to work with a local repo and the CodeCommit repository to which you've connected the local repo\.

The following are some basic examples of frequently used Git commands\.

For more options, see your Git documentation\.

**Topics**
+ [Configuration variables](#how-to-basic-git-configuration-variables)
+ [Remote repositories](#how-to-basic-git-remotes)
+ [Commits](#how-to-basic-git-commits)
+ [Branches](#how-to-basic-git-branches)
+ [Tags](#how-to-basic-git-tags)

## Configuration variables<a name="how-to-basic-git-configuration-variables"></a>


|  |  | 
| --- |--- |
|  Lists all configuration variables\.  |  `git config --list`  | 
|  Lists only local configuration variables\.  |  `git config --local -l`  | 
|  Lists only system configuration variables\.  |  `git config --system -l`  | 
|  Lists only global configuration variables\.  |  `git config --global -l`  | 
|  Sets a configuration variable in the specified configuration file\.  |  `git config [--local \| --global \| --system] variable-name variable-value`  | 
| Edits a configuration file directly\. Can also be used to discover the location of a specific configuration file\. To exit edit mode, typically you type `:q` \(to exit without saving changes\) or `:wq` \(to save changes and then exit\), and then press Enter\. | `git config [--local \| --global \| --system] --edit` | 

## Remote repositories<a name="how-to-basic-git-remotes"></a>


|  |  | 
| --- |--- |
|  Initializes a local repo in preparation for connecting it to an CodeCommit repository\.   |  `git init`  | 
|  Can be used to set up a connection between a local repo and a remote repository \(such as a CodeCommit repository\) using the specified nickname the local repo has for the CodeCommit repository and the specified URL to the CodeCommit repository\.  |  `git remote add remote-name remote-url`  | 
|  Creates a local repo by making a copy of a CodeCommit repository at the specified URL, in the specified subfolder of the current folder on the local machine\. This command also creates a remote tracking branch for each branch in the cloned CodeCommit repository and creates and checks out an initial branch that is forked from the current default branch in the cloned CodeCommit repository\.  |  `git clone remote-url local-subfolder-name`  | 
|  Shows the nickname the local repo uses for the CodeCommit repository\.  |  `git remote`  | 
|  Shows the nickname and the URL the local repo uses for fetches and pushes to the CodeCommit repository\.  |  `git remote -v`  | 
|  Pushes finalized commits from the local repo to the CodeCommit repository, using the specified nickname the local repo has for the CodeCommit repository and the specified branch\. Also sets up upstream tracking information for the local repo during the push\.  |  `git push -u remote-name branch-name`  | 
| Pushes finalized commits from the local repo to the CodeCommit repository after upstream tracking information is set\. | `git push` | 
|  Pulls finalized commits to the local repo from the CodeCommit repository, using the specified nickname the local repo has for the CodeCommit repository and the specified branch  |  `git pull remote-name branch-name`  | 
| Pulls finalized commits to the local repo from the CodeCommit repository after upstream tracking information is set\. | `git pull` | 
|  Disconnects the local repo from the CodeCommit repository, using the specified nickname the local repo has for the CodeCommit repository\.  |  `git remote rm remote-name`  | 

## Commits<a name="how-to-basic-git-commits"></a>


|  |  | 
| --- |--- |
|  Shows what has or hasn't been added to the pending commit in the local repo\.  |  `git status`  | 
|  Shows what has or hasn't been added to the pending commit in the local repo in a concise format\. \(`M` = modified, `A` = added, `D` = deleted, and so on\)  |  `git status -sb`  | 
|  Shows changes between the pending commit and the latest commit in the local repo\.  |  `git diff HEAD`  | 
|  Adds specific files to the pending commit in the local repo\.  |  `git add [file-name-1 file-name-2 file-name-N \| file-pattern]`  | 
|  Adds all new, modified, and deleted files to the pending commit in the local repo\.  |  `git add `  | 
|  Begins finalizing the pending commit in the local repo, which displays an editor to provide a commit message\. After the message is entered, the pending commit is finalized\.  |  `git commit`  | 
|  Finalizes the pending commit in the local repo, including specifying a commit message at the same time\.  |  `git commit -m "Some meaningful commit comment"`  | 
|  Lists recent commits in the local repo\.  |  `git log`   | 
|  Lists recent commits in the local repo in a graph format\.  |  `git log --graph`  | 
|  Lists recent commits in the local repo in a predefined condensed format\.  |  `git log --pretty=oneline`  | 
|  Lists recent commits in the local repo in a predefined condensed format, with a graph\.  |  `git log --graph --pretty=oneline`  | 
|  Lists recent commits in the local repo in a custom format, with a graph\. \(For more options, see [Git Basics \- Viewing the Commit History](http://git-scm.com/book/en/Git-Basics-Viewing-the-Commit-History)\)  |  `git log --graph --pretty=format:"%H (%h) : %cn : %ar : %s"`  | 

## Branches<a name="how-to-basic-git-branches"></a>


|  |  | 
| --- |--- |
|  Lists all branches in the local repo with an asterisk \(`*`\) displayed next to your current branch\.  |  `git branch`  | 
|  Pulls information about all existing branches in the CodeCommit repository to the local repo\.  |  `git fetch`  | 
|  Lists all branches in the local repo and remote tracking branches in the local repo\.  |  `git branch -a`  | 
|  Lists only remote tracking branches in the local repo\.  |  `git branch -r`  | 
|  Creates a new branch in the local repo using the specified branch name\.  |  `git branch new-branch-name`  | 
|  Switches to another branch in the local repo using the specified branch name\.  |  `git checkout other-branch-name`  | 
|  Creates a new branch in the local repo using the specified branch name, and then switches to it\.  |  `git checkout -b new-branch-name`  | 
|  Pushes a new branch from the local repo to the CodeCommit repository using the specified nickname the local repo has for the CodeCommit repository and the specified branch name\. Also sets up upstream tracking information for the branch in the local repo during the push\.  |  `git push -u remote-name new-branch-name`  | 
|  Creates a new branch in the local repo using the specified branch name\. Then connects the new branch in the local repo to an existing branch in the CodeCommit repository, using the specified nickname the local repo has for the CodeCommit repository and the specified branch name\.  |  `git branch --track new-branch-name remote-name/remote-branch-name`  | 
|  Merges changes from another branch in the local repo to the current branch in the local repo\.  |  `git merge from-other-branch-name`  | 
|  Deletes a branch in the local repo unless it contains work that has not been merged\.   |  `git branch -d branch-name`  | 
|  Deletes a branch in the CodeCommit repository using the specified nickname the local repo has for the CodeCommit repository and the specified branch name\. \(Note the use of the colon \(`:`\)\.\)  |  `git push remote-name :branch-name`  | 

## Tags<a name="how-to-basic-git-tags"></a>


|  |  | 
| --- |--- |
|  Lists all tags in the local repo\.  |  `git tag`  | 
|  Pulls all tags from the CodeCommit repository to the local repo\.  |  `git fetch --tags`  | 
|  Shows information about a specific tag in the local repo\.  |  `git show tag-name`  | 
|  Creates a "lightweight" tag in the local repo\.  |  `git tag tag-name commit-id-to-point-tag-at`  | 
|  Pushes a specific tag from the local repo to the CodeCommit repository using the specified nickname the local repo has for the CodeCommit repository and the specified tag name\.  |  `git push remote-name tag-name`  | 
|  Pushes all tags from the local repo to the CodeCommit repository using the specified nickname the local repo has for the CodeCommit repository\.  |  `git push remote-name --tags`  | 
|  Deletes a tag in the local repo\.  |  `git tag -d tag-name`  | 
|  Deletes a tag in the CodeCommit repository using the specified nickname the local repo has for the CodeCommit repository and the specified tag name\. \(Note the use of the colon \(`:`\)\.\)  |  `git push remote-name :tag-name`  | 