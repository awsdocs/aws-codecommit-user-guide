# Create a Git tag in AWS CodeCommit<a name="how-to-create-tag"></a>

You can use a Git tag to mark a commit with a label that helps other repository users understand its importance\. To create a Git tag in a CodeCommit repository, you can use Git from a local repo connected to the CodeCommit repository\. After you have created a Git tag in the local repo, you can use git push \-\-tags to push it to the CodeCommit repository\. 

For more information, see [View tag details](how-to-view-tag-details.md)\.

## Use Git to create a tag<a name="how-to-create-tag-git"></a>

Follow these steps to use Git from a local repo to create a Git tag in a CodeCommit repository\.

In these steps, we assume that you have already connected the local repo to the CodeCommit repository\. For instructions, see [Connect to a repository](how-to-connect.md)\.

1. Run the git tag *new\-tag\-name* *commit\-id* command, where *new\-tag\-name* is the new Git tag's name and *commit\-id* is the ID of the commit to associate with the Git tag\.

   For example, the following command creates a Git tag named `beta` and associates it with the commit ID `dc082f9a...af873b88`:

   ```
   git tag beta dc082f9a...af873b88
   ```

1. To push the new Git tag from the local repo to the CodeCommit repository, run the git push *remote\-name* *new\-tag\-name* command, where *remote\-name* is the name of the CodeCommit repository and *new\-tag\-name* is the name of the new Git tag\. 

   For example, to push a new Git tag named `beta` to a CodeCommit repository named `origin`:

   ```
   git push origin beta
   ```

**Note**  
To push all new Git tags from your local repo to the CodeCommit repository, run git push \-\-tags\.  
To ensure your local repo is updated with all of the Git tags in the CodeCommit repository, run git fetch followed by git fetch \-\-tags\.

For more options, see your Git documentation\.