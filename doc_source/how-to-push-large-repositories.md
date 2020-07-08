# Migrate a repository incrementally<a name="how-to-push-large-repositories"></a>

When migrating to AWS CodeCommit, consider pushing your repository in increments or chunks to reduce the chances an intermittent network issue or degraded network performance causes the entire push to fail\. By using incremental pushes with a script like the one included here, you can restart the migration and push only those commits that did not succeed on the earlier attempt\.

The procedures in this topic show you how to create and run a script that migrates your repository in increments and repushes only those increments that did not succeed until the migration is complete\.

These instructions are written with the assumption that you have already completed the steps in [Setting up ](setting-up.md) and [Create a repository](how-to-create-repository.md)\. 

**Topics**
+ [Step 0: Determine whether to migrate incrementally](#how-to-push-large-repositories-determine)
+ [Step 1: Install prerequisites and add the CodeCommit repository as a remote](#how-to-push-large-repositories-prereq)
+ [Step 2: Create the script to use for migrating incrementally](#how-to-push-large-repositories-createscript)
+ [Step 3: Run the script and migrate incrementally to CodeCommit](#how-to-push-large-repositories-runscript)
+ [Appendix: Sample script `incremental-repo-migration.py`](#how-to-push-large-repositories-sample)

## Step 0: Determine whether to migrate incrementally<a name="how-to-push-large-repositories-determine"></a>

There are several factors to consider to determine the overall size of your repository and whether to migrate incrementally\. The most obvious is the overall size of the artifacts in the repository\. Factors such as the accumulated history of the repository can also contribute to size\. A repository with years of history and branches can be very large, even though the individual assets are not\. There are a number of strategies you can pursue to make migrating these repositories simpler and more efficient\. For example, you can use a shallow clone strategy when cloning a repository with a long history of development, or you can turn off delta compression for large binary files\. You can research options by consulting your Git documentation, or you can choose to set up and configure incremental pushes for migrating your repository using the sample script included in this topic, `incremental-repo-migration.py`\. 

You might want to configure incremental pushes if one or more of the following conditions is true:
+ The repository you want to migrate has more than five years of history\.
+ Your internet connection is subject to intermittent outages, dropped packets, slow response, or other interruptions in service\.
+ The overall size of the repository is larger than 2 GB and you intend to migrate the entire repository\.
+ The repository contains large artifacts or binaries that do not compress well, such as large image files with more than five tracked versions\.
+ You have previously attempted a migration to CodeCommit and received an "Internal Service Error" message\. 

Even if none of the above conditions are true, you can still choose to push incrementally\.

## Step 1: Install prerequisites and add the CodeCommit repository as a remote<a name="how-to-push-large-repositories-prereq"></a>

You can create your own custom script, which has its own prerequisites\. If you use the sample included in this topic, you must:
+ Install its prerequisites\.
+ Clone the repository to your local computer\.
+ Add the CodeCommit repository as a remote for the repository you want to migrate\.

**Set up to run incremental\-repo\-migration\.py**

1.  On your local computer, install Python 2\.6 or later\. For more information and the latest versions, see [the Python website](https://www.python.org/downloads/)\.

1. On the same computer, install GitPython, which is a Python library used to interact with Git repositories\. For more information, see [the GitPython documentation](http://gitpython.readthedocs.org/en/stable/)\.

1.  Use the git clone \-\-mirror command to clone the repository you want to migrate to your local computer\. From the terminal \(Linux, macOS, or Unix\) or the command prompt \(Windows\), use the git clone \-\-mirror command to create a local repo for the repository, including the directory where you want to create the local repo\. For example, to clone a Git repository named *MyMigrationRepo* with a URL of *https://example\.com/my\-repo/* to a directory named *my\-repo*:

   ```
   git clone --mirror https://example.com/my-repo/MyMigrationRepo.git my-repo
   ```

   You should see output similar to the following, which indicates the repository has been cloned into a bare local repo named my\-repo:

   ```
   Cloning into bare repository 'my-repo'...
   remote: Counting objects: 20, done.
   remote: Compressing objects: 100% (17/17), done.
   remote: Total 20 (delta 5), reused 15 (delta 3)
   Unpacking objects: 100% (20/20), done.
   Checking connectivity... done.
   ```

1. Change directories to the local repo for the repository you just cloned \(for example, *my\-repo*\)\. From that directory, use the git remote add *DefaultRemoteName* *RemoteRepositoryURL* command to add the CodeCommit repository as a remote repository for the local repo\.
**Note**  
When pushing large repositories, consider using SSH instead of HTTPS\. When you push a large change, a large number of changes, or a large repository, long\-running HTTPS connections are often terminated prematurely due to networking issues or firewall settings\. For more information about setting up CodeCommit for SSH, see [For SSH connections on Linux, macOS, or Unix](setting-up-ssh-unixes.md) or [For SSH connections on Windows](setting-up-ssh-windows.md)\.

    For example, use the following command to add the SSH endpoint for a CodeCommit repository named MyDestinationRepo as a remote repository for the remote named `codecommit`: 

   ```
   git remote add codecommit ssh://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDestinationRepo
   ```
**Tip**  
Because this is a clone, the default remote name \(`origin`\) is already in use\. You must use another remote name\. Although the example uses `codecommit`, you can use any name you want\. Use the git remote show command to review the list of remotes set for your local repo\.

1. Use the git remote \-v command to display the fetch and push settings for your local repo and confirm they are set correctly\. For example:

   ```
   codecommit  ssh://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDestinationRepo (fetch)
   codecommit  ssh://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDestinationRepo (push)
   ```
**Tip**  
If you still see fetch and push entries for a different remote repository \(for example, entries for origin\), use the git remote set\-url \-\-delete command to remove them\.

## Step 2: Create the script to use for migrating incrementally<a name="how-to-push-large-repositories-createscript"></a>

These steps are written with the assumption that you are using the `incremental-repo-migration.py` sample script\. 

1. Open a text editor and paste the contents of [the sample script](#how-to-push-large-repositories-sample) into an empty document\.

1. Save the document in a documents directory \(not the working directory of your local repo\) and name it `incremental-repo-migration.py`\. Make sure the directory you choose is one configured in your local environment or path variables, so you can run the Python script from a command line or terminal\.

## Step 3: Run the script and migrate incrementally to CodeCommit<a name="how-to-push-large-repositories-runscript"></a>

 Now that you have created your `incremental-repo-migration.py` script, you can use it to incrementally migrate a local repo to a CodeCommit repository\. By default, the script pushes commits in batches of 1,000 commits and attempts to use the Git settings for the directory from which it is run as the settings for the local repo and remote repository\. You can use the options included in `incremental-repo-migration.py` to configure other settings, if necessary\.

1. From the terminal or command prompt, change directories to the local repo you want to migrate\.

1. From that directory, run the following command:

   ```
   python incremental-repo-migration.py
   ```

1. The script runs and shows progress at the terminal or command prompt\. Some large repositories are slow to show progress\. The script stops if a single push fails three times\. You can then rerun the script, and it starts from the batch that failed\. You can rerun the script until all pushes succeed and the migration is complete\.

**Tip**  
You can run `incremental-repo-migration.py` from any directory as long as you use the `-l` and `-r` options to specify the local and remote settings to use\. For example, to use the script from any directory to migrate a local repo located at /tmp/*my\-repo* to a remote nicknamed *codecommit*:  

```
python incremental-repo-migration.py -l "/tmp/my-repo" -r "codecommit" 
```
 You might also want to use the `-b` option to change the default batch size used when pushing incrementally\. For example, if you are regularly pushing a repository with very large binary files that change often and are working from a location that has restricted network bandwidth, you might want to use the `-b` option to change the batch size to 500 instead of 1,000\. For example:  

```
python incremental-repo-migration.py -b 500
```
This pushes the local repo incrementally in batches of 500 commits\. If you decide to change the batch size again when you migrate the repository \(for example, if you decide to decrease the batch size after an unsuccessful attempt\), remember to use the `-c` option to remove the batch tags before resetting the batch size with `-b`:  

```
python incremental-repo-migration.py -c
python incremental-repo-migration.py -b 250
```

**Important**  
Do not use the `-c` option if you want to rerun the script after a failure\. The `-c` option removes the tags used to batch the commits\. Use the `-c` option only if you want to change the batch size and start again, or if you decide you no longer want to use the script\.

## Appendix: Sample script `incremental-repo-migration.py`<a name="how-to-push-large-repositories-sample"></a>

For your convenience, we have developed a sample Python script, `incremental-repo-migration.py`, for pushing a repository incrementally\. This script is an open source code sample and provided as\-is\.

```
# Copyright 2015 Amazon.com, Inc. or its affiliates. All Rights Reserved. Licensed under the Amazon Software License (the "License"). 
# You may not use this file except in compliance with the License. A copy of the License is located at 
#    http://aws.amazon.com/asl/ 
# This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, express or implied. See the License for
# the specific language governing permissions and limitations under the License.

#!/usr/bin/env python

import os
import sys
from optparse import OptionParser
from git import Repo, TagReference, RemoteProgress, GitCommandError

class PushProgressPrinter(RemoteProgress):
    def update(self, op_code, cur_count, max_count=None, message=''):
        op_id = op_code & self.OP_MASK
        stage_id = op_code & self.STAGE_MASK
        if op_id == self.WRITING and stage_id == self.BEGIN:
            print("\tObjects: %d" % max_count)

class RepositoryMigration:

    MAX_COMMITS_TOLERANCE_PERCENT = 0.05
    PUSH_RETRY_LIMIT = 3
    MIGRATION_TAG_PREFIX = "codecommit_migration_"

    def migrate_repository_in_parts(self, repo_dir, remote_name, commit_batch_size, clean):
        self.next_tag_number = 0
        self.migration_tags = []
        self.walked_commits = set()
        self.local_repo = Repo(repo_dir)
        self.remote_name = remote_name
        self.max_commits_per_push = commit_batch_size
        self.max_commits_tolerance = self.max_commits_per_push * self.MAX_COMMITS_TOLERANCE_PERCENT

        try:
            self.remote_repo = self.local_repo.remote(remote_name)
            self.get_remote_migration_tags()
        except (ValueError, GitCommandError):
            print("Could not contact the remote repository. The most common reasons for this error are that the name of the remote repository is incorrect, or that you do not have permissions to interact with that remote repository.")
            sys.exit(1)

        if clean:
            self.clean_up(clean_up_remote=True)
            return

        self.clean_up()

        print("Analyzing repository")
        head_commit = self.local_repo.head.commit
        sys.setrecursionlimit(max(sys.getrecursionlimit(), head_commit.count()))

        # tag commits on default branch
        leftover_commits = self.migrate_commit(head_commit)
        self.tag_commits([commit for (commit, commit_count) in leftover_commits])

        # tag commits on each branch
        for branch in self.local_repo.heads:
            leftover_commits = self.migrate_commit(branch.commit)
            self.tag_commits([commit for (commit, commit_count) in leftover_commits])

        # push the tags
        self.push_migration_tags()

        # push all branch references
        for branch in self.local_repo.heads:
            print("Pushing branch %s" % branch.name)
            self.do_push_with_retries(ref=branch.name)

        # push all tags
        print("Pushing tags")
        self.do_push_with_retries(push_tags=True)

        self.get_remote_migration_tags()
        self.clean_up(clean_up_remote=True)

        print("Migration to CodeCommit was successful")

    def migrate_commit(self, commit):
        if commit in self.walked_commits:
            return []

        pending_ancestor_pushes = []
        commit_count = 1

        if len(commit.parents) > 1:
            # This is a merge commit
            # Ensure that all parents are pushed first
            for parent_commit in commit.parents:
                pending_ancestor_pushes.extend(self.migrate_commit(parent_commit))
        elif len(commit.parents) == 1:
            # Split linear history into individual pushes
            next_ancestor, commits_to_next_ancestor = self.find_next_ancestor_for_push(commit.parents[0])
            commit_count += commits_to_next_ancestor
            pending_ancestor_pushes.extend(self.migrate_commit(next_ancestor))

        self.walked_commits.add(commit)

        return self.stage_push(commit, commit_count, pending_ancestor_pushes)

    def find_next_ancestor_for_push(self, commit):
        commit_count = 0

        # Traverse linear history until we reach our commit limit, a merge commit, or an initial commit
        while len(commit.parents) == 1 and commit_count < self.max_commits_per_push and commit not in self.walked_commits:
            commit_count += 1
            self.walked_commits.add(commit)
            commit = commit.parents[0]

        return commit, commit_count

    def stage_push(self, commit, commit_count, pending_ancestor_pushes):
        # Determine whether we can roll up pending ancestor pushes into this push
        combined_commit_count = commit_count + sum(ancestor_commit_count for (ancestor, ancestor_commit_count) in pending_ancestor_pushes)

        if combined_commit_count < self.max_commits_per_push:
            # don't push anything, roll up all pending ancestor pushes into this pending push
            return [(commit, combined_commit_count)]

        if combined_commit_count <= (self.max_commits_per_push + self.max_commits_tolerance):
            # roll up everything into this commit and push
            self.tag_commits([commit])
            return []

        if commit_count >= self.max_commits_per_push:
            # need to push each pending ancestor and this commit
            self.tag_commits([ancestor for (ancestor, ancestor_commit_count) in pending_ancestor_pushes])
            self.tag_commits([commit])
            return []

        # push each pending ancestor, but roll up this commit
        self.tag_commits([ancestor for (ancestor, ancestor_commit_count) in pending_ancestor_pushes])
        return [(commit, commit_count)]

    def tag_commits(self, commits):
        for commit in commits:
            self.next_tag_number += 1
            tag_name = self.MIGRATION_TAG_PREFIX + str(self.next_tag_number)

            if tag_name not in self.remote_migration_tags:
                tag = self.local_repo.create_tag(tag_name, ref=commit)
                self.migration_tags.append(tag)
            elif self.remote_migration_tags[tag_name] != str(commit):
                print("Migration tags on the remote do not match the local tags. Most likely your batch size has changed since the last time you ran this script. Please run this script with the --clean option, and try again.")
                sys.exit(1)

    def push_migration_tags(self):
        print("Will attempt to push %d tags" % len(self.migration_tags))
        self.migration_tags.sort(key=lambda tag: int(tag.name.replace(self.MIGRATION_TAG_PREFIX, "")))
        for tag in self.migration_tags:
            print("Pushing tag %s (out of %d tags), commit %s" % (tag.name, self.next_tag_number, str(tag.commit)))
            self.do_push_with_retries(ref=tag.name)

    def do_push_with_retries(self, ref=None, push_tags=False):
        for i in range(0, self.PUSH_RETRY_LIMIT):
            if i == 0:
                progress_printer = PushProgressPrinter()
            else:
                progress_printer = None

            try:
                if push_tags:
                    infos = self.remote_repo.push(tags=True, progress=progress_printer)
                elif ref is not None:
                    infos = self.remote_repo.push(refspec=ref, progress=progress_printer)
                else:
                    infos = self.remote_repo.push(progress=progress_printer)

                success = True
                if len(infos) == 0:
                    success = False
                else:
                    for info in infos:
                        if info.flags & info.UP_TO_DATE or info.flags & info.NEW_TAG or info.flags & info.NEW_HEAD:
                            continue
                        success = False
                        print(info.summary)

                if success:
                    return
            except GitCommandError as err:
                print(err)

        if push_tags:
            print("Pushing all tags failed after %d attempts" % (self.PUSH_RETRY_LIMIT))
        elif ref is not None:
            print("Pushing %s failed after %d attempts" % (ref, self.PUSH_RETRY_LIMIT))
            print("For more information about the cause of this error, run the following command from the local repo: 'git push %s %s'" % (self.remote_name, ref))
        else:
            print("Pushing all branches failed after %d attempts" % (self.PUSH_RETRY_LIMIT))
        sys.exit(1)

    def get_remote_migration_tags(self):
        remote_tags_output = self.local_repo.git.ls_remote(self.remote_name, tags=True).split('\n')
        self.remote_migration_tags = dict((tag.split()[1].replace("refs/tags/",""), tag.split()[0]) for tag in remote_tags_output if self.MIGRATION_TAG_PREFIX in tag)

    def clean_up(self, clean_up_remote=False):
        tags = [tag for tag in self.local_repo.tags if tag.name.startswith(self.MIGRATION_TAG_PREFIX)]

        # delete the local tags
        TagReference.delete(self.local_repo, *tags)

        # delete the remote tags
        if clean_up_remote:
            tags_to_delete = [":" + tag_name for tag_name in self.remote_migration_tags]
            self.remote_repo.push(refspec=tags_to_delete)

parser = OptionParser()
parser.add_option("-l", "--local",
                  action="store", dest="localrepo", default=os.getcwd(),
                  help="The path to the local repo. If this option is not specified, the script will attempt to use current directory by default. If it is not a local git repo, the script will fail.")
parser.add_option("-r", "--remote",
                  action="store", dest="remoterepo", default="codecommit",
                  help="The name of the remote repository to be used as the push or migration destination. The remote must already be set in the local repo ('git remote add ...'). If this option is not specified, the script will use 'codecommit' by default.")
parser.add_option("-b", "--batch",
                  action="store", dest="batchsize", default="1000",
                  help="Specifies the commit batch size for pushes. If not explicitly set, the default is 1,000 commits.")
parser.add_option("-c", "--clean",
                  action="store_true", dest="clean", default=False,
                  help="Remove the temporary tags created by migration from both the local repo and the remote repository. This option will not do any migration work, just cleanup. Cleanup is done automatically at the end of a successful migration, but not after a failure so that when you re-run the script, the tags from the prior run can be used to identify commit batches that were not pushed successfully.")

(options, args) = parser.parse_args()

migration = RepositoryMigration()
migration.migrate_repository_in_parts(options.localrepo, options.remoterepo, int(options.batchsize), options.clean)
```