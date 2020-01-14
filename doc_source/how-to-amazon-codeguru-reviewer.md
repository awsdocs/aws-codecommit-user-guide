# Associate or Disassociate an AWS CodeCommit Repository with Amazon CodeGuru Reviewer<a name="how-to-amazon-codeguru-reviewer"></a>

Amazon CodeGuru Reviewer is an automated code review service that uses program analysis and machine learning to detect common issues and recommend fixes in your Java code\. You can associate repositories in your AWS account with CodeGuru Reviewer\. When you do, CodeGuru Reviewer creates a service\-linked role that allows CodeGuru Reviewer to analyze code in all pull requests created after the association is made\. 

After you associate a repository, CodeGuru Reviewer analyzes and comments on any issues it finds when you create pull requests\. Each comment is clearly marked as having come from CodeGuru Reviewer with the designation **Amazon CodeGuru Reviewer**\. You can reply to these comments just as you would to any other comment in a pull request, and you can also provide feedback on the quality of the suggestion\. This feedback is shared with CodeGuru Reviewer and can help improve the service and its suggestions\. 

**Note**  
You will not see comments from CodeGuru Reviewer in pull requests that were created before the repository was associated with it\. You might not see comments in pull requests created after the association for the following reasons:  
The pull request does not contain Java code\.
CodeGuru Reviewer has not had enough time to run and review the code in the pull request\. This process can take up to 30 minutes\.
CodeGuru Reviewer did not find any issues in the Java code in the pull request\.
The code review job failed to run\. Currently there is no way to determine if CodeGuru Reviewer did not successfully complete an analysis\.

For more information, see [Working with Pull Requests in AWS CodeCommit Repositories](pull-requests.md), [Review a Pull Request](how-to-review-pull-request.md), and the [https://docs.aws.amazon.com/codeguru/latest/reviewer-ug/welcome.html](https://docs.aws.amazon.com/codeguru/latest/reviewer-ug/welcome.html)\.

**Note**  
You must be signed in with an IAM user or role that has sufficient permissions to associate or disassociate a repository with CodeGuru Reviewer\. For information about the managed policies for CodeCommit that include these permissions, see [AWS Managed \(Predefined\) Policies for CodeCommit](auth-and-access-control-iam-identity-based-access-control.md#managed-policies) and [AWS CodeCommit Managed Policies and Amazon CodeGuru Reviewer](auth-and-access-control-iam-identity-based-access-control.md#codeguru-permissions)\. For information about CodeGuru Reviewer permissions and security, see the *Amazon CodeGuru Reviewer User Guide*\.

**Topics**
+ [Associate a Repository with CodeGuru Reviewer](#how-to-amazon-codeguru-reviewer-associate)
+ [Disassociate a Repository from CodeGuru Reviewer](#how-to-amazon-codeguru-reviewer-disassociate)

## Associate a Repository with CodeGuru Reviewer<a name="how-to-amazon-codeguru-reviewer-associate"></a>

Use the AWS CodeCommit console to quickly associate a repository with CodeGuru Reviewer\. For other methods, see the *Amazon CodeGuru Reviewer User Guide*\.

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In **Repositories**, choose the name of the repository to associate with CodeGuru Reviewer\. 

1. Choose **Settings**, and then choose **Amazon CodeGuru Reviewer**\.

1. Choose **Associate repository**\. 
**Note**  
It can take up to 10 minutes to fully associate a repository with CodeGuru Reviewer\. The status will not update automatically\. To view the current status, choose the refresh button\.  
![\[An CodeCommit repository that has been associated with Amazon CodeGuru Reviewer.\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-settings-associate-disassociate.png)

## Disassociate a Repository from CodeGuru Reviewer<a name="how-to-amazon-codeguru-reviewer-disassociate"></a>

Use the AWS CodeCommit console to quickly disassociate a repository from CodeGuru Reviewer\. For other methods, see the *Amazon CodeGuru Reviewer User Guide*\.

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In **Repositories**, choose the name of the repository you want to disassociate from CodeGuru Reviewer\. 

1. Choose **Settings**, and then choose **Amazon CodeGuru Reviewer**\.

1. Choose **Disassociate repository**\. 