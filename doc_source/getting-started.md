# Getting started with Git and AWS CodeCommit<a name="getting-started"></a>

If you are new to Git and CodeCommit, this tutorial helps you learn some simple commands to get you started\. If you are already familiar with Git, you can skip this tutorial and go to [Getting started with CodeCommit ](getting-started-cc.md)\.

In this tutorial, you create a repository that represents a local copy of the CodeCommit repository, which we refer to as a local repo\. 

After you create the local repo, you make some changes to it\. Then you send \(push\) your changes to the CodeCommit repository\.

You also simulate a team environment where two users independently commit changes to their local repo and push those changes to the CodeCommit repository\. The users then pull the changes from the CodeCommit repository to their own local repo to see the changes the other user made\.

You also create branches and tags and manage some access permissions in the CodeCommit repository\. 

After you complete this tutorial, you should have enough practice with the core Git and CodeCommit concepts to use them for your own projects\. 

Complete the [prerequisites and setup](setting-up.md), including:
+ Assign permissions to the IAM user\.
+ Set up for connecting using [HTTPS](setting-up-gc.md), SSH, or [git\-remote\-codecommit](setting-up-git-remote-codecommit.md)\. For more information about these choices, see [Setting up for AWS CodeCommit ](setting-up.md)\.
+ Configure the AWS CLI if you want to use the command line or terminal for all operations, including creating the repository\.

**Topics**
+ [Step 1: Create a CodeCommit repository](#getting-started-create-repo)
+ [Step 2: Create a local repo](#getting-started-set-up-folders)
+ [Step 3: Create your first commit](#getting-started-create-commit)
+ [Step 4: Push your first commit](#getting-started-init-repo)
+ [Step 5: Share the CodeCommit repository and push and pull another commit](#getting-started-pull-commits)
+ [Step 6: Create and share a branch](#getting-started-branching)
+ [Step 7: Create and share a tag](#getting-started-tags)
+ [Step 8: Set up access permissions](#getting-started-permissions)
+ [Step 9: Clean up](#getting-started-clean-up)

## Step 1: Create a CodeCommit repository<a name="getting-started-create-repo"></a>

In this step, you use the CodeCommit console to create the repository\. 

You can skip this step if you already have a CodeCommit repository you want to use\. 

**Note**  
Depending on your usage, you might be charged for creating or accessing a repository\. For more information, see [Pricing](http://aws.amazon.com/codecommit/pricing) on the CodeCommit product information page\.

**To create the CodeCommit repository**

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. Use the region selector to choose the AWS Region where you want to create the repository\. For more information, see [Regions and Git connection endpoints](regions.md)\.

1. On the **Repositories** page, choose **Create repository**\. 

1. On the **Create repository** page, in **Repository name**, enter a name for your repository \(for example, **MyDemoRepo**\)\.
**Note**  
Repository names are case sensitive and can be no longer than 100 characters\. For more information, see [Limits](limits.md#limits-repository-names)\.

1. \(Optional\) In **Description**, enter a description \(for example, **My demonstration repository**\)\. This can help you and other users identify the purpose of the repository\.

1. \(Optional\) Choose **Add tag** to add one or more repository tags \(a custom attribute label that helps you organize and manage your AWS resources\) to your repository\. For more information, see [Tagging repositories in AWS CodeCommit](how-to-tag-repository.md)\.

1. \(Optional\) Select **Enable Amazon CodeGuru Reviewer for Java** if this repository will contain Java code, and you want to have CodeGuru Reviewer analyze that Java code\. CodeGuru Reviewer uses multiple machine learning models to find Java code defects and to automatically suggest improvements and fixes in pull requests\. For more information, see the Amazon CodeGuru Reviewer User Guide\.

1. Choose **Create**\. 

**Note**  
The remaining steps in this tutorial use `MyDemoRepo` for the name of the CodeCommit repository\. If you choose a different name, be sure to use it throughout this tutorial\.

For more information about creating repositories, including how to create a repository from the terminal or command line, see [Create a repository](how-to-create-repository.md)\.

## Step 2: Create a local repo<a name="getting-started-set-up-folders"></a>

In this step, you set up a local repo on your local machine to connect to your repository\. To do this, you select a directory on your local machine that represents the local repo\. You use Git to clone and initialize a copy of your empty CodeCommit repository inside of that directory\. Then you specify the user name and email address used to annotate your commits\. 

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In the region selector, choose the AWS Region where the repository was created\. Repositories are specific to an AWS Region\. For more information, see [Regions and Git connection endpoints](regions.md)\.

1. Find the repository you want to connect to from the list and choose it\. Choose **Clone URL**, and then choose the protocol you want to use when cloning or connecting to the repository\. This copies the clone URL\.
   + Copy the HTTPS URL if you are using either Git credentials with your IAM user or the credential helper included with the AWS CLI\.
   + Copy the HTTPS \(GRC\) URL if you are using the git\-remote\-codecommit command on your local computer\.
   + Copy the SSH URL if you are using an SSH public/private key pair with your IAM user\.
**Note**  
 If you see a **Welcome** page instead of a list of repositories, there are no repositories associated with your AWS account in the AWS Region where you are signed in\. To create a repository, see [Create an AWS CodeCommit repository](how-to-create-repository.md) or follow the steps in the [Getting started with Git and CodeCommit](#getting-started) tutorial\.

1. At the terminal or command prompt, clone the repository with the git clone command, and providing the clone URL you copied in the previous step\. Your clone URL will differ depending on which protocol and configuration you use\. For example, if you are using HTTPS with Git credentials to clone a repository named *MyDemoRepo* in the US East \(Ohio\) Region:

   ```
   git clone https://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo my-demo-repo
   ```

   If you are using HTTPS with git\-remote\-codecommit:

   ```
   git clone codecommit://MyDemoRepo my-demo-repo
   ```

   If you are using SSH:

   ```
   git clone ssh://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo my-demo-repo
   ```
**Note**  
If you see an error when attempting to clone a repository, you might not have completed the setup necessary for your local computer\. For more information, see [Setting up for AWS CodeCommit ](setting-up.md)\.

## Step 3: Create your first commit<a name="getting-started-create-commit"></a>

In this step, you create your first commit in your local repo\. To do this, you create two example files in your local repo\. You use Git to stage the change to, and then commit the change to, your local repo\.

1. Use a text editor to create the following two example text files in your directory\. Name the files `cat.txt` and `dog.txt`:

   ```
   cat.txt
   -------
   The domestic cat (Felis catus or Felis silvestris catus) is a small, usually furry, domesticated, and carnivorous mammal.
   ```

   ```
   dog.txt
   -------
   The domestic dog (Canis lupus familiaris) is a canid that is known as man's best friend.
   ```

1. Run git config to add your user name and email address represented by placeholders *your\-user\-name* and *your\-email\-address* to your local repo\. This makes it easier to identify the commits you make: 

   ```
   git config --local user.name "your-user-name"
   git config --local user.email your-email-address
   ```

1. Run git add to stage the change:

   ```
   git add cat.txt dog.txt
   ```

1. Run git commit to commit the change:

   ```
   git commit -m "Added cat.txt and dog.txt"
   ```
**Tip**  
To see details about the commit you just made, run git log\.

## Step 4: Push your first commit<a name="getting-started-init-repo"></a>

In this step, you push the commit from your local repo to your CodeCommit repository\. 

Run git push to push your commit through the default remote name Git uses for your CodeCommit repository \(`origin`\), from the default branch in your local repo \(`master`\):

```
git push -u origin master
```

**Tip**  
After you have pushed files to your CodeCommit repository, you can use the CodeCommit console to view the contents\. For more information, see [Browse files in a repository](how-to-browse.md)\.

## Step 5: Share the CodeCommit repository and push and pull another commit<a name="getting-started-pull-commits"></a>

In this step, you share information about the CodeCommit repository with a fellow team member\. The team member uses this information to get a local copy, make some changes to it, and then push the modified local copy to your CodeCommit repository\. You then pull the changes from the CodeCommit repository to your local repo\. 

In this tutorial, you simulate the fellow user by having Git create a directory separate from the one you created in [step 2](#getting-started-set-up-folders)\. \(Typically, this directory is on a different machine\.\) This new directory is a copy of your CodeCommit repository\. Any changes you make to the existing directory or this new directory are made independently\. The only way to identify changes to these directories is to pull from the CodeCommit repository\. 

Even though they're on the same local machine, we call the existing directory your *local repo* and the new directory the *shared repo*\.

From the new directory, you get a separate copy of the CodeCommit repository\. You then add a new example file, commit the changes to the shared repo, and then push the commit from the shared repo to your CodeCommit repository\.

Lastly, you pull the changes from your repository to your local repo and then browse it to see the changes committed by the other user\.

1. Switch to the `/tmp` directory or the `c:\temp` directory\.

1. Run git clone to pull down a copy of the repository into the shared repo:

   For HTTPS:

   ```
   git clone https://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo shared-demo-repo
   ```

   For HTTPS with git\-remote\-codecommit:

   ```
   git clone codecommit://MyDemoRepo shared-demo-repo
   ```

   For SSH:

   ```
   git clone ssh://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo shared-demo-repo
   ```
**Note**  
When you clone a repository using SSH on Windows operating systems, you might need to add the SSH key ID to the connection string as follows:  

   ```
   git clone ssh://Your-SSH-Key-ID@git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo my-demo-repo
   ```
For more information, see [For SSH connections on Windows](setting-up-ssh-windows.md)\.

   In this command, `MyDemoRepo` is the name of your CodeCommit repository\. `shared-demo-repo` is the name of the directory Git creates in the `/tmp` directory or the `c:\temp` directory\. After Git creates the directory, Git pulls down a copy of your repository into the `shared-demo-repo` directory\.

1. Switch to the `shared-demo-repo` directory:

   ```
   (For Linux, macOS, or Unix) cd /tmp/shared-demo-repo
   (For Windows) cd c:\temp\shared-demo-repo
   ```

1. Run git config to add another user name and email address represented by placeholders *other\-user\-name* and *other\-email\-address*\. This makes it easier to identify the commits the other user makes: 

   ```
   git config --local user.name "other-user-name"
   git config --local user.email other-email-address
   ```

1. Use a text editor to create the following example text file in the `shared-demo-repo` directory\. Name the file `horse.txt`:

   ```
   horse.txt
   -------
   The horse (Equus ferus caballus) is one of two extant subspecies of Equus ferus.
   ```

1. Run git add to stage the change to the shared repo:

   ```
   git add horse.txt
   ```

1. Run git commit to commit the change to the shared repo:

   ```
   git commit -m "Added horse.txt"
   ```

1. Run git push to push your initial commit through the default remote name Git uses for your CodeCommit repository \(`origin`\), from the default branch in your local repo \(`master`\):

   ```
   git push -u origin master
   ```

1. Switch to your local repo and run git pull to pull into your local repo the commit the shared repo made to the CodeCommit repository\. Then run git log to see the commit that was initiated from the shared repo\.

## Step 6: Create and share a branch<a name="getting-started-branching"></a>

In this step, you create a branch in your local repo, make a few changes, and then push the branch to your CodeCommit repository\. You then pull the branch to the shared repo from your CodeCommit repository\. 

A branch allows you to independently develop a different version of the repository's contents \(for example, to work on a new software feature without affecting the work of your team members\)\. When that feature is stable, you merge the branch into a more stable branch of the software\.

You use Git to create the branch and then point it to the first commit you made\. You use Git to push the branch to the CodeCommit repository\. You then switch to your shared repo and use Git to pull the new branch into your shared local repo and explore the branch\.

1. From your local repo, run git checkout, specifying the name of the branch \(for example, `MyNewBranch`\) and the ID of the first commit you made in the local repo\. 

   If you don't know the commit ID, run git log to get it\. Make sure the commit has your user name and email address, not the user name and email address of the other user\. This is to simulate that `master` is a stable version of the CodeCommit repository and the `MyNewBranch` branch is for some new, relatively unstable feature:

   ```
   git checkout -b MyNewBranch commit-ID
   ```

1. Run git push to send the new branch from the local repo to the CodeCommit repository:

   ```
   git push origin MyNewBranch
   ```

1. Now, pull the branch into the shared repo and check your results:

   1. Switch to the shared repo directory \(shared\-demo\-repo\)\.

   1. Pull in the new branch \(git fetch origin\)\.

   1. Confirm that the branch has been pulled in \(git branch \-\-all displays a list of all branches for the repository\)\.

   1. Switch to the new branch \(git checkout MyNewBranch\)\.

   1. Confirm that you have switched to the `MyNewBranch` branch by running git status or git branch\. The output shows which branch you are on\. In this case, it should be `MyNewBranch`\. 

   1. View the list of commits in the branch \(git log\)\.

   Here's the list of Git commands to call:

   ```
   git fetch origin
   git branch --all
   git checkout MyNewBranch
   git branch or git status
   git log
   ```

1. Switch back to the `master` branch and view its list of commits\. The Git commands should look like this:

   ```
   git checkout master
   git log
   ```

1. Switch to the `master` branch in your local repo\. You can run git status or git branch\. The output shows which branch you are on\. In this case, it should be `master`\. The Git commands should look like this:

   ```
   git checkout master
   git branch or git status
   ```

## Step 7: Create and share a tag<a name="getting-started-tags"></a>

In this step, you create two tags in your local repo, associate the tags with commits, and then push the tags to your CodeCommit repository\. You then pull the changes from the CodeCommit repository to the shared repo\. 

A tag is used to give a human\-readable name to a commit \(or branch or even another tag\)\. You would do this, for example, if you want to tag a commit as `v2.1`\. A commit, branch, or tag can have any number of tags associated with it, but an individual tag can be associated with only one commit, branch, or tag\. In this tutorial, you tag one commit as `release` and one as `beta`\.

You use Git to create the tags, pointing the `release` tag to the first commit you made and the `beta` tag to the commit made by the other user\. You then use Git to push the tags to the CodeCommit repository\. Then you switch to your shared repo and use Git to pull the tags into your shared local repo and explore the tags\.

1. From your local repo, run git tag, specifying the name of the new tag \(`release`\) and the ID of the first commit you made in the local repo\. 

   If you don't know the commit ID, run git log to get it\. Make sure the commit has your user name and email address, not the user name and email address of the other user\. This is to simulate that your commit is a stable version of the CodeCommit repository:

   ```
   git tag release commit-ID
   ```

   Run git tag again to tag the commit from the other user with the `beta` tag\. This is to simulate that the commit is for some new, relatively unstable feature:

   ```
   git tag beta commit-ID
   ```

1. Run git push \-\-tags to send the tags to the CodeCommit repository\.

1. Now pull the tags into the shared repo and check your results:

   1. Switch to the shared repo directory \(shared\-demo\-repo\)\.

   1. Pull in the new tags \(git fetch origin\)\.

   1. Confirm that the tags have been pulled in \(git tag displays a list of tags for the repository\)\.

   1. View information about each tag \(git log release and git log beta\)\.

   Here's the list of Git commands to call:

   ```
   git fetch origin
   git tag
   git log release
   git log beta
   ```

1. Try this out in the local repo, too:

   ```
   git log release
   git log beta
   ```

## Step 8: Set up access permissions<a name="getting-started-permissions"></a>

In this step, you give a user permission to synchronize the shared repo with the CodeCommit repository\. This is an optional step\. It's recommended for users who are interested in learning about how to control access to CodeCommit repositories\.

To do this, you use the IAM console to create an IAM user, who, by default, does not have permissions to synchronize the shared repo with the CodeCommit repository\. You can run git pull to verify this\. If the new user doesn't have permission to synchronize, the command doesn't work\. Then you go back to the IAM console and apply a policy that allows the user to use git pull\. Again, you can run git pull to verify this\. 

This step is written with the assumption you have permissions to create IAM users in your AWS account\. If you don't have these permissions, you can't perform the procedures in this step\. Skip ahead to [Step 9: Clean up](#getting-started-clean-up) to clean up the resources you used for your tutorial\.

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

   Be sure to sign in with the same user name and password you used in [Setting up ](setting-up.md)\.

1. In the navigation pane, choose **Users**, and then choose **Create New Users**\.

1. In the first **Enter User Names** box, enter an example user name \(for example, **JaneDoe\-CodeCommit**\)\. Select the **Generate an access key for each user** box, and then choose **Create**\.

1. Choose **Show User Security Credentials**\. Make a note of the access key ID and secret access key or choose **Download Credentials**\. 

1. Follow the instructions in [For HTTPS users using Git credentials](setting-up-gc.md)  to generate and supply the credentials of the IAM user\.

   If you want to use SSH, follow the instructions in [SSH and Linux, macOS, or Unix: Set up the public and private keys for Git and CodeCommit](setting-up-ssh-unixes.md#setting-up-ssh-unixes-keys-unixes) or [SSH and Windows: Set up the public and private keys for Git and CodeCommit](setting-up-ssh-windows.md#setting-up-ssh-windows-keys-windows) to set up the user with public and private keys\.

1. Run git pull\. The following error should appear:

   For HTTPS:

    `fatal: unable to access 'https://git-codecommit.us-east-2.amazonaws.com/v1/repos/repository-name/': The requested URL returned error: 403`\. 

   For SSH:

   `fatal: unable to access 'ssh://git-codecommit.us-east-2.amazonaws.com/v1/repos/repository-name/': The requested URL returned error: 403`\. 

   The error appears because the new user doesn't have permission to synchronize the shared repo with the CodeCommit repository\.

1. Return to the IAM console\. In the navigation pane, choose **Policies**, and then choose **Create Policy**\. \(If a **Get Started** button appears, choose it, and then choose **Create Policy**\.\)

1. Next to **Create Your Own Policy**, choose **Select**\.

1. In the **Policy Name** box, enter a name \(for example, **CodeCommitAccess\-GettingStarted**\)\.

1. In the **Policy Document** box, enter the following, which allows an IAM user to pull from any repository associated with the IAM user:

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Action": [
           "codecommit:GitPull"
         ],
         "Resource": "*"
       }
     ]
   }
   ```
**Tip**  
If you want the IAM user to be able to push commits to any repository associated with the IAM user, enter this instead:  

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Action": [
           "codecommit:GitPull",
           "codecommit:GitPush"
         ],
         "Resource": "*"
       }
     ]
   }
   ```
For information about other CodeCommit action and resource permissions you can give to users, see [Authentication and access control for AWS CodeCommit](auth-and-access-control.md)\.

1. In the navigation pane, choose **Users**\.

1. Choose the example user name \(for example, **JaneDoe\-CodeCommit**\) to which you want to attach the policy\.

1. Choose the **Permissions** tab\.

1. In **Managed Policies**, choose **Attach Policy**\.

1. Select the **CodeCommitAccess\-GettingStarted** policy you just created, and then choose **Attach Policy**\.

1. Run git pull\. This time the command should work and an `Already up-to-date` message should appear\. 

1. If you are using HTTPS, switch to your original Git credentials or, if using git\-remote\-codecommit, your usual profile\. For more information, see the instructions in [Setup for HTTPS users using Git credentials](setting-up-gc.md) or [Setup steps for HTTPS connections to AWS CodeCommit with git\-remote\-codecommit](setting-up-git-remote-codecommit.md)\.

   If you are using SSH, switch to your original keys\. For more information, see [SSH and Linux, macOS, or Unix: Set up the public and private keys for Git and CodeCommit](setting-up-ssh-unixes.md#setting-up-ssh-unixes-keys-unixes) or [SSH and Windows: Set up the public and private keys for Git and CodeCommit](setting-up-ssh-windows.md#setting-up-ssh-windows-keys-windows)\.

You've reached the end of this tutorial\. 

## Step 9: Clean up<a name="getting-started-clean-up"></a>

In this step, you delete the CodeCommit repository you used in this tutorial, so you won't continue to be charged for the storage space\. 

You also remove the local repo and shared repo on your local machine because they won't be needed after you delete the CodeCommit repository\.

**Important**  
After you delete this repository, you won't be able to clone it to any local repo or shared repo\. You also won't be able to pull data from it, or push data to it, from any local repo or shared repo\. This action cannot be undone\.

### To delete the CodeCommit repository \(console\)<a name="getting-started-clean-up-console"></a>

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. On the **Dashboard** page, in the list of repositories, choose **MyDemoRepo**\. 

1. In the navigation pane, choose **Settings**\. 

1. On the **Settings** page, in **Delete repository**, choose **Delete repository**\.

1. In the box next to **Type the name of the repository to confirm deletion**, enter **MyDemoRepo**, and then choose **Delete**\. 

### To delete the CodeCommit repository \(AWS CLI\)<a name="getting-started-clean-up-cli"></a>

Run the [delete\-repository](how-to-delete-repository.md#how-to-delete-repository-cli) command:

```
aws codecommit delete-repository --repository-name MyDemoRepo
```

### To delete the local repo and shared repo<a name="getting-started-delete-repos"></a>

For Linux, macOS, or Unix: 

```
cd /tmp
rm -rf /tmp/my-demo-repo
rm -rf /tmp/shared-demo-repo
```

For Windows: 

```
cd c:\temp
rd /s /q c:\temp\my-demo-repo
rd /s /q c:\temp\shared-demo-repo
```