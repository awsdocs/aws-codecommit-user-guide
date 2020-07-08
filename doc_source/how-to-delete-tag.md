# Delete a Git tag in AWS CodeCommit<a name="how-to-delete-tag"></a>

To delete a Git tag in a CodeCommit repository, use Git from a local repo connected to the CodeCommit repository\. \.

## Use Git to delete a Git tag<a name="how-to-delete-tag-git"></a>

Follow these steps to use Git from a local repo to delete a Git tag in a CodeCommit repository\.

These steps are written with the assumption that you have already connected the local repo to the CodeCommit repository\. For instructions, see [Connect to a repository](how-to-connect.md)\.

1. To delete the Git tag from the local repo, run the git tag \-d *tag\-name* command where *tag\-name* is the name of the Git tag you want to delete\.
**Tip**  
To get a list of Git tag names, run git tag\.

   For example, to delete a Git tag in the local repo named `beta`:

   ```
   git tag -d beta
   ```

1. To delete the Git tag from the CodeCommit repository, run the git push *remote\-name* \-\-delete *tag\-name* command where *remote\-name* is the nickname the local repo uses for the CodeCommit repository and *tag\-name* is the name of the Git tag you want to delete from the CodeCommit repository\.
**Tip**  
To get a list of CodeCommit repository names and their URLs, run the git remote \-v command\.

   For example, to delete a Git tag named `beta` in the CodeCommit repository named `origin`:

   ```
   git push origin --delete beta
   ```