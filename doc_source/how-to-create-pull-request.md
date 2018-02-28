# Create a Pull Request<a name="how-to-create-pull-request"></a>

Creating pull requests helps other users see and review your code changes before you merge them into another branch\. First, you create a branch for your code changes\. This is referred to as the source branch for a pull request\. After you commit and push changes to the repository, you can create a pull request that compares the contents of that branch \(the source branch\) to the branch where you want to merge your changes after the pull request is closed \(the destination branch\)\. 

You can use the AWS CodeCommit console or the AWS CLI to create pull requests for your repository\. 


+ [Use the AWS CodeCommit Console to Create a Pull Request](#how-to-create-pull-request-console)
+ [Use the AWS CLI to Create a Pull Request](#how-to-create-pull-request-cli)

## Use the AWS CodeCommit Console to Create a Pull Request<a name="how-to-create-pull-request-console"></a>

You can use the AWS CodeCommit console to create a pull request in an AWS CodeCommit repository\. If your repository is [configured with notifications](how-to-repository-email.md), subscribed users receive an email when you create a pull request\.

1. Open the AWS CodeCommit console at [https://console\.aws\.amazon\.com/codecommit](https://console.aws.amazon.com/codecommit)\.

1. In the list of repositories, choose the name of the repository\. 

1. In the navigation pane, choose **Pull Requests**\.
**Tip**  
You can also create pull requests from **Branches**, **Code**, or **Compare**\.

1. Choose **Create pull request**\.   
![\[Creating a pull request from the Pull Requests view in the AWS CodeCommit console.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-pull-request-view.png)

1. In **Create pull request**, in **Source**, choose the branch that contains the changes you want reviewed\. 

1. In **Destination**, review the branch where you intend to merge your code changes when the pull request is closed\. By default, the destination branch is preconfigured with the default branch of the repository, but you can choose a different branch\.

1. Choose **Compare**\. A comparison runs on the two branches, and the differences between them are displayed\. An analysis is also performed to determine whether the two branches can be merged automatically when the pull request is closed\.

1. Review the comparison details and the changes to be sure that the pull request contains the changes and commits you want reviewed\. If not, adjust your choices for source and destination branches, and choose **Compare** again\.

1. When you are satisfied with the comparison results for the pull request, in **Title**, provide a short but descriptive title for this review\. This is the title that appears in the list of pull requests for the repository\. 

1. \(Optional\) In **Description**, provide details about this review and any other useful information for reviewers\.

1. Choose **Create**\.  
![\[Creating a pull request\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-pull-request-create.png)

Your pull request appears in the list of pull requests for the repository\. If you [configured notifications](how-to-repository-email.md), subscribers to the Amazon SNS topic receive an email to inform them of the newly created pull request\.

## Use the AWS CLI to Create a Pull Request<a name="how-to-create-pull-request-cli"></a>

To use AWS CLI commands with AWS CodeCommit, install the AWS CLI\. For more information, see [Command Line Reference](cmd-ref.md)\. 

To use the AWS CLI to create a pull request in an AWS CodeCommit repository:

1. Run the create\-pull\-request command, specifying:

   + The name of the pull request \(with the \-\-title option\)\.

   + The description of the pull request \(with the \-\-description option\)\.

   + A list of targets for the create\-pull\-request command, including:

     + The name of the AWS CodeCommit repository where the pull request is created \(with the repositoryName attribute\)\.

     + The name of the branch that contains the code changes you want reviewed, also known as the source branch \(with the sourceReference attribute\)\.

     + \(Optional\) The name of the branch where you intend to merge your code changes, also known as the destination branch, if you do not want to merge to the default branch \(with the destinationReference attribute\)\.

   + A unique, client\-generated idempotency token \(with the \-\-client\-request\-token option\)\. 

   For example, to create a pull request named *My Pull Request* with a description of *Please review these changes by Tuesday* that targets the *MyNewBranch* source branch and is to be merged to the default branch *master* in an AWS CodeCommit repository named `MyDemoRepo`:

   ```
   aws codecommit create-pull-request --title "My Pull Request" --description "Please review these changes by Tuesday" --client-request-token 123Example --targets repositoryName=MyDemoRepo,sourceReference=MyNewBranch 
   ```

1. If successful, this command produces output similar to the following:

   ```
   {
      "pullRequest": { 
         "authorArn": "arn:aws:iam::111111111111:user/Jane_Doe",
         "clientRequestToken": "123Example",
         "creationDate": 1508962823.285,
         "description": "Please review these changes by Tuesday",
         "lastActivityDate": 1508962823.285,
         "pullRequestId": "42",
         "pullRequestStatus": "OPEN",
         "pullRequestTargets": [ 
            { 
               "destinationCommit": "5d036259EXAMPLE",
               "destinationReference": "refs/heads/master",
               "mergeMetadata": { 
                  "isMerged": false,
               },
               "repositoryName": "MyDemoRepo",
               "sourceCommit": "317f8570EXAMPLE",
               "sourceReference": "refs/heads/MyNewBranch"
            }
         ],
         "title": "My Pull Request"
      }
   }
   ```