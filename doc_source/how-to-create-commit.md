# Create a commit in AWS CodeCommit<a name="how-to-create-commit"></a>

You can use Git or the AWS CLI to create a commit in a CodeCommit repository\. If the local repo is connected to a CodeCommit repository, you use Git to push the commit from the local repo to the CodeCommit repository\. To create a commit directly in the CodeCommit console, see [Create or add a file to an AWS CodeCommit repository](how-to-create-file.md) and [Edit the contents of a file in an AWS CodeCommit repository](how-to-edit-file.md)\. 

**Note**  
If you use the AWS CLI, make sure that you have a recent version installed to ensure that you are using a version that contains the `create-commit` command\.

**Topics**
+ [Create the first commit for a repository using the AWS CLI](#how-to-create-first-commit)
+ [Create a commit using a Git client](#how-to-create-commit-git)
+ [Create a commit using the AWS CLI](#how-to-create-commit-cli)

## Create the first commit for a repository using the AWS CLI<a name="how-to-create-first-commit"></a>

You can use the AWS CLI and the `put-file` command to create your first commit for a repository\. Using put\-file creates a first commit that adds a file to your empty repository, and it creates a branch with the name you specify\. It designates the new branch as the default branch for your repository\. 

**Note**  
To use AWS CLI commands with CodeCommit, install the AWS CLI\. For more information, see [Command line reference](cmd-ref.md)\. <a name="create-first-commit"></a>

## To create the first commit for a repository using the AWS CLI

1. On your local computer, create the file you want to add as the first file to the CodeCommit repository\. A common practice is to create a `README.md` markdown file that explains the purpose of this repository to other repository users\. If you include a `README.md` file, the content of the file will be displayed automatically at the bottom of the **Code** page for your repository in the CodeCommit console\.

1. At the terminal or command line, run the put\-file command, specifying:
   + The name of the repository where you want to add the first file\.
   + The name of the branch you want to create as the default branch\.
   + The local location of the file\. The syntax used for this location varies, depending on your local operating system\.
   + The name of the file you want to add, including the path where the updated file is stored in the repository\.
   + The user name and email you want to associate with this file\.
   + A commit message that explains why you added this file\.

   The user name, email address, and commit message are optional, but can help other users know who made the change and why\. If you do not supply a user name, CodeCommit defaults to using your IAM user name or a derivation of your console login as the author name\.

   For example, to add a file named *README\.md* with the content of "Welcome to our team repository\!" to a repository named *MyDemoRepo* to a branch named *development*:

   ```
   aws codecommit put-file --repository-name MyDemoRepo --branch-name development --file-path README.md --file-content "Welcome to our team repository!" --name "Mary Major" --email "mary_major@example.com" --commit-message "I added a quick readme for our new team repository."
   ```

   If successful, this command returns output similar to the following:

   ```
   {
       "commitId": "724caa36EXAMPLE",
       "blobId": "a8a94062EXAMPLE",
       "treeId": "08b2fc73EXAMPLE"
   }
   ```

## Create a commit using a Git client<a name="how-to-create-commit-git"></a>

You can create commits using a Git client installed on your local computer, and then push those commits to your CodeCommit repository\.

1. Complete the prerequisites, including [Setting up ](setting-up.md)\.
**Important**  
If you have not completed setup, you cannot connect or commit to the repository using Git\.

1. Make sure you are creating a commit in the correct branch\. To see a list of available branches and find out which branch you are currently set to use, run git branch\. All branches are displayed\. An asterisk \(`*`\) appears next to your current branch\. To switch to a different branch, run git checkout *branch\-name*\.

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

1. If you run git status again, you should see output similar to the following\. It indicates that the commit is ready to be pushed from the local repo to the CodeCommit repository:

   ```
   ...    
   nothing to commit, working directory clean
   ```

1. Before you push the finalized commit from the local repo to the CodeCommit repository, you can see what you are pushing by running git diff \-\-stat *remote\-name*/*branch\-name*, where *remote\-name* is the nickname the local repo uses for the CodeCommit repository and *branch\-name* is the name of the branch to compare\.
**Tip**  
To get the nickname, run git remote\. To get a list of branch names, run git branch\. An asterisk \(`*`\) appears next to the current branch\. You can also run git status to get the current branch name\.
**Note**  
If you cloned the repository, from the perspective of the local repo, *remote\-name* is not the name of the CodeCommit repository\. When you clone a repository, *remote\-name* is set automatically to `origin`\. 

   For example, git diff \-\-stat origin/master would show output similar to the following:

   ```
   bird.txt | 1 +
   1 file changed, 1 insertion(+)
   ```

   The output assumes you have already connected the local repo to the CodeCommit repository\. \(For instructions, see [Connect to a repository](how-to-connect.md)\.\)

1. When you're ready to push the commit from the local repo to the CodeCommit repository, run git push *remote\-name* *branch\-name*, where *remote\-name* is the nickname the local repo uses for the CodeCommit repository and *branch\-name* is the name of the branch to push to the CodeCommit repository\.

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

## Create a commit using the AWS CLI<a name="how-to-create-commit-cli"></a>

You can use the AWS CLI and the `create-commit` command to create a commit for a repository on the tip of a specified branch\. You can also create an unreferenced merge commit to represent the results of merging two commit specifiers\. For more information, see [Create an unreferenced commit](how-to-resolve-conflict-pull-request.md#create-unreferenced-merge-commit)\.

**Note**  
To use AWS CLI commands with CodeCommit, install the AWS CLI\. For more information, see [Command line reference](cmd-ref.md)\. <a name="create-commit"></a>

**To create a commit**

1. On your local computer, make the changes you want committed to the CodeCommit repository\.

1. At the terminal or command line, run the create\-commit command, specifying:
   + The repository where you want to commit the changes\.
   + The branch where you want to commit the changes\.
   + The full commit ID of the most recent commit made to that branch, also known as the tip or head commit or the parent commit ID\.
   + Whether to keep any empty folders if the changes you made delete the content of those folders\. By default, this value is false\.
   + The information about the files you want added, changed, or deleted\.
   + The user name and email you want associated with these changes\.
   + A commit message that explains why you made these changes\.

   The user name, email address, and commit message are optional, but help other users know who made the changes and why\. If you do not supply a user name, CodeCommit defaults to using your IAM user name or a derivation of your console login as the author name\.

   For example, to create a commit for a repository that adds a `meeting.md` file to a repository named *MyDemoRepo* in the *master* branch:

   ```
   aws codecommit create-commit --repository-name MyDemoRepo --branch-name master --parent-commit-id a4d4d5da-EXAMPLE --put-files "filePath=meeting.md,fileContent='We will use this file for meeting notes.'"
   ```

   If successful, this command returns output similar to the following:

   ```
   {
       "commitId": "4df8b524-EXAMPLE",
       "treeId": "55b57003-EXAMPLE",
       "filesAdded": [
           {
               "blobId": "5e1c309d-EXAMPLE",
               "absolutePath": "meeting.md",
               "fileMode": "NORMAL"
           }
       ],
       "filesDeleted": [],
       "filesUpdated": []
   }
   ```

   To create a commit that makes changes to files named *file1\.py* and *file2\.py*, renames a file from *picture\.png* to *image1\.png* and moves it from a directory named *pictures* to a directory named, *images*, and deletes a file named *ExampleSolution\.py* in a repository named *MyDemoRepo* on a branch named *MyFeatureBranch* whose most recent commit has an ID of *4c925148EXAMPLE*:

   ```
   aws codecommit create-commit --repository-name MyDemoRepo --branch-name MyFeatureBranch --parent-commit-id 4c925148EXAMPLE --name "Saanvi Sarkar"
    --email "saanvi_sarkar@example.com" --commit-message "I'm creating this commit to update a variable name in a number of files."
    --keep-empty-folders false  --put-files '{"filePath": "file1.py", "fileMode": "EXECUTABLE", "fileContent": "bucket_name = sys.argv[1] region = sys.argv[2]"}'
   '{"filePath": "file2.txt", "fileMode": "NORMAL", "fileContent": "//Adding a comment to explain the variable changes in file1.py"}' '{"filePath": "images/image1.png",
   "fileMode": "NORMAL", "sourceFile": {"filePath": "pictures/picture.png", "isMove": true}}' --delete-files filePath="ExampleSolution.py"
   ```
**Note**  
The syntax for the \-\-put\-files segment will vary slightly depending on your operating system\. The above example is optimized for Linux, macOS, or Unix users and Windows users with a Bash emulator\. Windows users at the command line or in Powershell should use syntax appropriate for those systems\.

   If successful, this command returns output similar to the following:

   ```
   {
      "commitId": "317f8570EXAMPLE",
      "treeId": "347a3408EXAMPLE",
      "filesAdded": [
           {
           "absolutePath": "images/image1.png",
           "blobId": "d68ba6ccEXAMPLE",
           "fileMode": "NORMAL"
           }
       ],
       "filesUpdated": [
           {
           "absolutePath": "file1.py",
           "blobId": "0a4d55a8EXAMPLE",
           "fileMode": "EXECUTABLE"
           },
           {
           "absolutePath": "file2.txt",
           "blobId": "915766bbEXAMPLE",
           "fileMode": "NORMAL"
           }
       ],
       "filesDeleted": [
           {
           "absolutePath": "ExampleSolution.py",
           "blobId": "4f9cebe6aEXAMPLE",
           "fileMode": "EXECUTABLE"
           },
           {
           "absolutePath": "pictures/picture.png",
           "blobId": "fb12a539EXAMPLE",
           "fileMode": "NORMAL"
           }
       ]
   }
   ```