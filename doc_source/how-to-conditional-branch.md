# Limit pushes and merges to branches in AWS CodeCommit<a name="how-to-conditional-branch"></a>

By default, any CodeCommit repository user who has sufficient permissions to push code to the repository can contribute to any branch in that repository\. This is true no matter how you add a branch to the repository: by using the console, the command line, or Git\. However, you might want to configure a branch so that only some repository users can push or merge code to that branch\. For example, you might want to configure a branch used for production code so that only a subset of senior developers can push or merge changes to that branch\. Other developers can still pull from the branch, make their own branches, and create pull requests, but they cannot push or merge changes to that branch\. You can configure this access by creating a conditional policy that uses a context key for one or more branches in IAM\. 

**Note**  
To complete some of the procedures in this topic, you must sign in with an adminstrative user that has sufficient permissions to configure and apply IAM policies\. For more information, see [Creating an IAM Admin User and Group](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html)\. 

**Topics**
+ [Configure an IAM policy to limit pushes and merges to a branch](#how-to-conditional-branch-create-policy)
+ [Apply the IAM policy to an IAM group or role](#how-to-conditional-branch-apply-policy)
+ [Test the policy](#how-to-conditional-branch-test)

## Configure an IAM policy to limit pushes and merges to a branch<a name="how-to-conditional-branch-create-policy"></a>

You can create a policy in IAM that prevents users from updating a branch, including pushing commits to a branch and merging pull requests to a branch\. To do this, your policy uses a conditional statement, so that the effect of the `Deny` statement applies only if the condition is met\. The APIs you include in the `Deny` statement determine which actions are not allowed\. You can configure this policy to apply to only one branch in a repository, a number of branches in a repository, or to all branches that match the criteria across all repositories in an AWS account\. <a name="how-to-conditional-branch-create-policy-procedure"></a>

**To create a conditional policy for branches**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**\. 

1. Choose **Create policy**\.

1. Choose **JSON**, and then paste the following example policy\. Replace the value of `Resource` with the ARN of the repository that contains the branch for which you want to restrict access\. Replace the value of `codecommit:References` with a reference to the branch or branches to which you want to restrict access\. For example, this policy denies pushing commits, merging branches, merging pull requests, and adding files to a branch named *`master`* and a branch named `prod` in a repository named `MyDemoRepo`:

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Deny",
               "Action": [
                   "codecommit:GitPush",
                   "codecommit:DeleteBranch",
                   "codecommit:PutFile",
                   "codecommit:MergeBranchesByFastForward",
                   "codecommit:MergeBranchesBySquash",
                   "codecommit:MergeBranchesByThreeWay",
                   "codecommit:MergePullRequestByFastForward",
                   "codecommit:MergePullRequestBySquash",
                   "codecommit:MergePullRequestByThreeWay"
               ],
               "Resource": "arn:aws:codecommit:us-east-2:111111111111:MyDemoRepo",
               "Condition": {
                   "StringEqualsIfExists": {
                       "codecommit:References": [
                           "refs/heads/master", 
                           "refs/heads/prod"
                        ]
                   },
                   "Null": {
                       "codecommit:References": false
                   }
               }
           }
       ]
   }
   ```

   Branches in Git are simply pointers \(references\) to the SHA\-1 value of the head commit, which is why the condition uses `References`\. The `Null` statement is required in any policy whose effect is `Deny` and where `GitPush` is one of the actions\. This is required because of the way Git and `git-receive-pack` work when pushing changes from a local repo to CodeCommit\.
**Tip**  
To create a policy that applies to all branches named master in all repositories in an AWS account, change the value of `Resource` from a repository ARN to an asterisk \(`*`\)\. 

1. Choose **Review policy**\. Correct any errors in your policy statement, and then continue to **Create policy**\.

1. When the JSON is validated, the **Create policy** page is displayed\. A warning appears in the **Summary** section, advising you that this policy does not grant permissions\. This is expected\. 
   + In **Name**, enter a name for this policy, such as **DenyChangesToMaster**\.
   + In **Description**, enter a description of the policy's purpose\. This is optional, but recommended\.
   + Choose **Create policy**\.

## Apply the IAM policy to an IAM group or role<a name="how-to-conditional-branch-apply-policy"></a>

You've created a policy that limits pushes and merges to a branch, but the policy has no effect until you apply it to an IAM user, group, or role\. As a best practice, consider applying the policy to an IAM group or role\. Applying policies to individual IAM users does not scale well\.<a name="how-to-conditional-branch-apply-policy-procedure"></a>

**To apply the conditional policy to a group or role**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, if you want to apply the policy to an IAM group, choose **Groups**\.If you want to apply the policy to a role that users assume, choose **Role**\. Choose the name of the group or role\.

1. On the **Permissions** tab, choose **Attach Policy**\.

1. Select the conditional policy you created from the list of policies, and then choose **Attach policy**\.

For more information, see [Attaching and Detatching IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html)\.

## Test the policy<a name="how-to-conditional-branch-test"></a>

You should test the effects of the policy you've applied on the group or role to ensure that it acts as expected\. There are many ways you can do this\. For example, to test a policy similar to the one shown above, you can:
+ Sign in to the CodeCommit console with an IAM user who is either a member of an IAM group that has the policy applied, or assumes a role that has the policy applied\. In the console, add a file on the branch where the restrictions apply\. You should see an error message when you attempt to save or upload a file to that branch\. Add a file to a different branch\. The operation should succeed\.
+ Sign in to the CodeCommit console with an IAM user who is either a member of an IAM group that has the policy applied, or assumes a role that has the policy applied\. Create a pull request that merges to the branch where the restrictions apply\. You should be able to create the pull request, but get an error if you try to merge it\. 
+ From the terminal or command line, create a commit on the branch where the restrictions apply, and then push that commit to the CodeCommit repository\. You should see an error message\. Commits and pushes made from other branches should work as usual\.