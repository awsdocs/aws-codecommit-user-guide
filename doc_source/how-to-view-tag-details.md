# View Tag Details in AWS CodeCommit<a name="how-to-view-tag-details"></a>

In Git, a tag is a label you can apply to a reference like a commit to mark it with information that might be important to other repository users\. For example, you might tag the commit that was the beta release point for a project with the tag **beta**\. For more information, see [Use Git to Create a Tag](how-to-create-tag.md#how-to-create-tag-git)\.

You can use the AWS CodeCommit console to view information about tags in your repository, including the date and commit message of the commit referenced by each tag\. From the console, you can compare the commit referenced by the tag with the head of the default branch of your repository\. Like any other commit, you can also view the code at the point of that tag\.

You can also use Git from your terminal or command line to view details about tags in a local repo\. 

**Topics**
+ [Use the AWS CodeCommit Console to View Tag Details](#how-to-view-tag-details-console)
+ [Use Git to View Tag Details](#how-to-view-tag-details-git)

## Use the AWS CodeCommit Console to View Tag Details<a name="how-to-view-tag-details-console"></a>

Use the AWS CodeCommit console to quickly view a list of tags for your repository and details about the commits referenced by the tags\.

1. Open the AWS CodeCommit console at [https://console\.aws\.amazon\.com/codecommit](https://console.aws.amazon.com/codecommit)\.

1. In the list of repositories, choose the name of the repository\. 

1. In the navigation pane, choose **Tags**\.  
![\[A view of tags in a repository.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-tags-view.png)
**Note**  
You can adjust the number of tags displayed on the **Tags** page by changing the number of tags per page\. 

1. Do one of the following:
   + To view the code as it was at that tagged commit, choose the tag name\.
   + To view a graph of the repository from that tagged commit, choose the abbreviated commit ID\.
   + To view details of the commit, including the full commit message, committer, and author, choose the commit message\.
   + To compare the tagged commit with the head of the default branch in your repository, choose **Compare**\.

## Use Git to View Tag Details<a name="how-to-view-tag-details-git"></a>

To use Git to view details about tags in a local repo, run one of the following commands:
+ [git tag](#how-to-view-tag-details-git-tag) to view a list of tag names\.
+ [git show](#how-to-view-tag-details-git-show) to view information about a specific tag\.
+ [git ls\-remote](#how-to-view-tag-details-git-remote) to view information about tags in an AWS CodeCommit repository\.

**Note**  
To ensure that your local repo is updated with all of the tags in the AWS CodeCommit repository, run git fetch followed by git fetch \-\-tags\.

In the following steps, we assume that you have already connected the local repo to AWS CodeCommit repository\. For instructions, see [Connect to a Repository](how-to-connect.md)\.

### To view a list of tags in a local repo<a name="how-to-view-tag-details-git-tag"></a>

1. Run the git tag command:

   ```
   git tag
   ```

1. If successful, this command produces output similar to the following:

   ```
   beta
   release
   ```
**Note**  
If no tags have been defined, git tag returns nothing\.

For more options, see your Git documentation\.

### To view information about a tag in a local repo<a name="how-to-view-tag-details-git-show"></a>

1. Run the git show *tag\-name* command\. For example, to view information about a tag named `beta`, run:

   ```
   git show beta
   ```

1. If successful, this command produces output similar to the following:

   ```
   commit 317f8570...ad9e3c09
   Author: John Doe <johndoe@example.com>
   Date:   Tue Sep 23 13:49:51 2014 -0700
   
       Added horse.txt
   
   diff --git a/horse.txt b/horse.txt
   new file mode 100644
   index 0000000..df42ff1
   --- /dev/null
   +++ b/horse.txt
   @@ -0,0 +1 @@
   +The horse (Equus ferus caballus) is one of two extant subspecies of Equus ferus
   \ No newline at end of file
   ```
**Note**  
To exit the output of the tag information, type :q\.

For more options, see your Git documentation\.

### To view information about tags in an AWS CodeCommit repository<a name="how-to-view-tag-details-git-remote"></a>

1. Run the git ls\-remote \-\-tags command\.

   ```
   git ls-remote --tags
   ```

1. If successful, this command produces as output a list of the tags in the AWS CodeCommit repository: 

   ```
   129ce87a...70fbffba    refs/tags/beta
   785de9bd...59b402d8    refs/tags/release
   ```

   If no tags have been defined, git ls\-remote \-\-tags returns a blank line\.

For more options, see your Git documentation\.