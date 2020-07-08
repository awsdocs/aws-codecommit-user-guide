# Push commits to an additional Git repository<a name="how-to-mirror-repo-pushes"></a>

You can configure your local repo to push changes to two remote repositories\. For example, you might want to continue using your existing Git repository solution while you try out AWS CodeCommit\. Follow these basic steps to push changes in your local repo to CodeCommit and a separate Git repository\.

**Tip**  
If you do not have a Git repository, you can create an empty one on a service other than CodeCommit and then migrate your CodeCommit repository to it\. You should follow steps similar to the ones in [Migrate to CodeCommit](how-to-migrate-repository.md)\.

1. From the command prompt or terminal, switch to your local repo directory and run the git remote \-v command\. You should see output similar to the following:

   For HTTPS:

   ```
   origin  https://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo (fetch)
   origin  https://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo (push)
   ```

   For SSH:

   ```
   origin  ssh://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo (fetch)
   origin  ssh://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo (push)
   ```

1. Run the git remote set\-url \-\-add \-\-push origin *git\-repository\-name* command where *git\-repository\-name* is the URL and name of the Git repository where you want to host your code\. This changes the push destination of `origin` to that Git repository\.
**Note**  
git remote set\-url \-\-add \-\-push overrides the default URL for pushes, so you must run this command twice, as demonstrated in later steps\.

   For example, the following command changes the push of origin to *some\-URL*/MyDestinationRepo:

   ```
   git remote set-url --add --push origin some-URL/MyDestinationRepo
   ```

   This command returns nothing\.
**Tip**  
If you are pushing to a Git repository that requires credentials, make sure you configure those credentials in a credential helper or in the configuration of the *some\-URL* string\. Otherwise, your pushes to that repository fail\.

1. Run the git remote \-v command again, which should create output similar to the following:

   For HTTPS:

   ```
   origin  https://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo (fetch)
   origin  some-URL/MyDestinationRepo (push)
   ```

   For SSH:

   ```
   origin  ssh://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo (fetch)
   origin  some-URL/MyDestinationRepo (push)
   ```

1. Now add the CodeCommit repository\. Run git remote set\-url \-\-add \-\-push origin again, this time with the URL and repository name of your CodeCommit repository\.

   For example, the following command adds the push of **origin** to https://git\-codecommit\.us\-east\-2\.amazonaws\.com/v1/repos/MyDemoRepo:

   For HTTPS:

   ```
   git remote set-url --add --push origin https://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo
   ```

   For SSH:

   ```
   git remote set-url --add --push origin ssh://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo
   ```

   This command returns nothing\.

1. Run the git remote \-v command again, which should create output similar to the following:

   For HTTPS:

   ```
   origin  https://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo (fetch)
   origin  some-URL/MyDestinationRepo (push)        
   origin  https://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo (push)
   ```

   For SSH:

   ```
   origin  ssh://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo (fetch)
   origin  some-URL/MyDestinationRepo (push)        
   origin  ssh://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo (push)
   ```

   You now have two Git repositories as the destination for your pushes, but your pushes go to *some\-URL*/MyDestinationRepo first\. If the push to that repository fails, your commits are not pushed to either repository\.
**Tip**  
If the other repository requires credentials you want to enter manually, consider changing the order of the pushes so that you push to CodeCommit first\. Run git remote set\-url \-\-delete to delete the repository that is pushed to first, and then run git remote set\-url \-\-add to add it again so that it becomes the second push destination in the list\.   
For more options, see your Git documentation\.

1. To verify you are now pushing to both remote repositories, use a text editor to create the following text file in your local repo:

   ```
   bees.txt
   -------
   Bees are flying insects closely related to wasps and ants, and are known for their role in pollination and for producing honey and beeswax.
   ```

1. Run git add to stage the change in your local repo:

   ```
   git add bees.txt
   ```

1. Run git commit to commit the change in your local repo:

   ```
   git commit -m "Added bees.txt"
   ```

1. To push the commit from the local repo to your remote repositories, run git push \-u *remote\-name* *branch\-name* where *remote\-name* is the nickname the local repo uses for the remote repositories and *branch\-name* is the name of the branch to push to the repository\.
**Tip**  
You only have to use the `-u` option the first time you push\. Then the upstream tracking information is set\.

   For example, running git push \-u origin master would show the push went to both remote repositories in the expected branches, with output similar to the following:

   For HTTPS:

   ```
   Counting objects: 5, done.
   Delta compression using up to 4 threads.
   Compressing objects: 100% (3/3), done.
   Writing objects: 100% (3/3), 5.61 KiB | 0 bytes/s, done.
   Total 3 (delta 1), reused 0 (delta 0)
   To some-URL/MyDestinationRepo
      a5ba4ed..250f6c3  master -> master
   Counting objects: 5, done.
   Delta compression using up to 4 threads.
   Compressing objects: 100% (3/3), done.
   Writing objects: 100% (3/3), 5.61 KiB | 0 bytes/s, done.
   Total 3 (delta 1), reused 0 (delta 0)
   remote:
   To https://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo
      a5ba4ed..250f6c3  master -> master
   ```

   For SSH:

   ```
   Counting objects: 5, done.
   Delta compression using up to 4 threads.
   Compressing objects: 100% (3/3), done.
   Writing objects: 100% (3/3), 5.61 KiB | 0 bytes/s, done.
   Total 3 (delta 1), reused 0 (delta 0)
   To some-URL/MyDestinationRepo
      a5ba4ed..250f6c3  master -> master
   Counting objects: 5, done.
   Delta compression using up to 4 threads.
   Compressing objects: 100% (3/3), done.
   Writing objects: 100% (3/3), 5.61 KiB | 0 bytes/s, done.
   Total 3 (delta 1), reused 0 (delta 0)
   remote:
   To ssh://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo
      a5ba4ed..250f6c3  master -> master
   ```

For more options, see your Git documentation\.