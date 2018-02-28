# What Is AWS CodeCommit?<a name="welcome"></a>

AWS CodeCommit is a version control service hosted by Amazon Web Services that you can use to privately store and manage assets \(such as documents, source code, and binary files\) in the cloud\. For information about pricing for AWS CodeCommit, see [Pricing\.](http://aws.amazon.com/codecommit/pricing/)


+ [Introducing AWS CodeCommit](#welcome-introducing)
+ [How Does AWS CodeCommit Work?](#welcome-how-it-works)
+ [How Is AWS CodeCommit Different from File Versioning in Amazon S3?](#welcome-arc-vs-s3)
+ [How Do I Get Started with AWS CodeCommit?](#welcome-get-started)
+ [Where Can I Learn More About Git?](#welcome-get-started-with-git)

## Introducing AWS CodeCommit<a name="welcome-introducing"></a>

AWS CodeCommit is a secure, highly scalable, managed source control service that hosts private Git repositories\. AWS CodeCommit eliminates the need for you to manage your own source control system or worry about scaling its infrastructure\. You can use AWS CodeCommit to store anything from code to binaries\. It supports the standard functionality of Git, so it works seamlessly with your existing Git\-based tools\. 

With AWS CodeCommit, you can:

+ **Benefit from a fully managed service hosted by AWS**\. AWS CodeCommit provides high service availability and durability and eliminates the administrative overhead of managing your own hardware and software\. There is no hardware to provision and scale and no server software to install, configure, and update\.

+ **Store your code securely**\. AWS CodeCommit repositories are encrypted at rest as well as in transit\.

+ **Work collaboratively on code\.** AWS CodeCommit repositories support pull requests, where users can review and comment on each other's code changes before merging them to branches; notifications that automatically send emails to users about pull requests and comments; and more\.

+ **Easily scale your version control projects**\. AWS CodeCommit repositories can scale up to meet your development needs\. The service can handle repositories with large numbers of files or branches, large file sizes, and lengthy revision histories\.

+ **Store anything, anytime**\. AWS CodeCommit has no limit on the size of your repositories or on the file types you can store\.

+ **Integrate with other AWS and third\-party services**\. AWS CodeCommit keeps your repositories close to your other production resources in the AWS Cloud, which helps increase the speed and frequency of your development lifecycle\. It is integrated with IAM and can be used with other AWS services and in parallel with other repositories\. For more information, see [Product and Service Integrations with AWS CodeCommit](integrations.md)\.

+ **Easily migrate files from other remote repositories**\. You can migrate to AWS CodeCommit from any Git\-based repository\. 

+ **Use the Git tools you already know**\. AWS CodeCommit supports Git commands as well as its own AWS CLI commands and APIs\.

## How Does AWS CodeCommit Work?<a name="welcome-how-it-works"></a>

 AWS CodeCommit will seem familiar to users of Git\-based repositories, but even those unfamiliar should find the transition to AWS CodeCommit relatively simple\. AWS CodeCommit provides a console for the easy creation of repositories and the listing of existing repositories and branches\. In a few simple steps, users can find information about a repository and clone it to their computer, creating a local repo where they can make changes and then push them to the AWS CodeCommit repository\. Users can work from the command line on their local machines or use a GUI\-based editor\. 

The following figure shows how you use your development machine, the AWS CLI or AWS CodeCommit console, and the AWS CodeCommit service to create and manage repositories:

![\[Typical AWS CodeCommit workflow\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/arc-workflow.png)![\[Typical AWS CodeCommit workflow\]](http://docs.aws.amazon.com/codecommit/latest/userguide/)

1. Use the AWS CLI or the AWS CodeCommit console to create an AWS CodeCommit repository\.

1. From your development machine, use Git to run git clone, specifying the name of the AWS CodeCommit repository\. This creates a local repo that connects to the AWS CodeCommit repository\.

1. Use the local repo on your development machine to modify \(add, edit, and delete\) files, and then run git add to stage the modified files locally\. Run git commit to commit the files locally, and then run git push to send the files to the AWS CodeCommit repository\. 

1. Download changes from other users\. Run git pull to synchronize the files in the AWS CodeCommit repository with your local repo\. This ensures you're working with the latest version of the files\.

You can use the AWS CLI or the AWS CodeCommit console to track and manage your repositories\.

## How Is AWS CodeCommit Different from File Versioning in Amazon S3?<a name="welcome-arc-vs-s3"></a>

AWS CodeCommit is designed for team software development\. It manages batches of changes across multiple files, which can occur in parallel with changes made by other developers\. Amazon S3 versioning supports the recovery of past versions of files, but it's not focused on collaborative file tracking features that software development teams need\.

## How Do I Get Started with AWS CodeCommit?<a name="welcome-get-started"></a>

To get started with AWS CodeCommit:

1. Follow the steps in [Setting Up ](setting-up.md) to prepare your development machines\.

1. Follow the steps in one or more of the tutorials in [Getting Started](getting-started-topnode.md)\.

1. [Create](how-to-create-repository.md) version control projects in AWS CodeCommit or [migrate](how-to-migrate-repository.md) version control projects to AWS CodeCommit\.

## Where Can I Learn More About Git?<a name="welcome-get-started-with-git"></a>

If you don't know it already, you should [learn how to use Git](how-to-basic-git.md)\. Here are some helpful resources:

+ [Pro Git](http://git-scm.com/book), an online version of the *Pro Git* book\. Written by Scott Chacon\. Published by Apress\.

+ [Git Immersion](http://gitimmersion.com/), a try\-it\-yourself guided tour that walks you through the fundamentals of using Git\. Published by Neo Innovation, Inc\.

+ [Git Reference](http://gitref.org/index.html), an online quick reference that can also be used as a more in\-depth Git tutorial\. Published by the GitHub team\.

+ [Git Cheat Sheet](https://github.com/github/training-kit/blob/master/downloads/github-git-cheat-sheet.md) with basic Git command syntax\. Published by the GitHub team\.

+ [Git Pocket Guide](http://www.amazon.com/Git-Pocket-Guide-Richard-Silverman/dp/1449325866)\. Written by Richard E\. Silverman\. Published by O'Reilly Media, Inc\.