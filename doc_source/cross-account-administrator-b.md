# Cross\-account repository access: Actions for the administrator in AccountB<a name="cross-account-administrator-b"></a>

To allow users or groups in AccountB to access a repository in AccountA, the AccountB administrator must create a group in AccountB\. This group must be configured with a policy that allows group members to assume the role created by the AccountA administrator\. 

The following sections provide steps and examples\.

**Topics**
+ [Step 1: Create an IAM group for repository access for AccountB users](#cross-account-create-group-b)
+ [Step 2: Create a policy and add users to the IAM group](#cross-account-create-policy-b)

## Step 1: Create an IAM group for repository access for AccountB users<a name="cross-account-create-group-b"></a>

The simplest way to manage which IAM users in AccountB can access the AccountA repository is to create an IAM group in AccountB that has permission to assume the role in AccountA, and then add the IAM users to that group\.<a name="cross-account-create-group-b-procedure"></a>

**To create a group for cross\-account repository access**

1. Sign in to the AWS Management Console as an IAM user with the permissions required to create IAM groups and policies and manage IAM users in AccountB\.

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the IAM console, choose **Groups**\.

1. Choose **Create New Group**\.

1. In **Group Name**, enter a name for the group \(for example, *DevelopersWithCrossAccountRepositoryAccess*\)\. Choose **Next Step**\.

1. In **Attach Policy**, choose **Next Step**\. You create the cross\-account policy in the next procedure\. Finish creating the group\.

## Step 2: Create a policy and add users to the IAM group<a name="cross-account-create-policy-b"></a>

Now that you have a group, create the policy that allows members of this group to assume the role that gives them access to the repository in AccountA\. Then add to the group the IAM users in AccountB that you want to allow access in AccountA\.<a name="cross-account-create-policy-for-group"></a>

**To create a policy for the group and add users to it**

1. In the IAM console, choose **Groups**, and then choose the name of the group you just created \(for example, *DevelopersWithCrossAccountRepositoryAccess*\)\.

1. Choose the **Permissions** tab\. Expand **Inline Policies**, and then choose the link to create an inline policy\. \(If you are configuring a group that already has an inline policy, choose **Create Group Policy**\.\)

1. Choose **Custom Policy**, and then choose **Select**\. 

1. In **Policy Name**, enter a name for the policy \(for example, *AccessPolicyForSharedRepository*\)\.

1. In **Policy Document**, paste the following policy\. In `Resource`, replace the ARN with the ARN of the policy created by the administrator in AccountA \(for example, arn:aws:iam::*111122223333*:role/*MyCrossAccountRepositoryContributorRole*\), and then choose **Apply Policy**\. For more information about the policy created by the administrator in AccountA, see [Step 1: Create a policy for repository access in AccountA](cross-account-administrator-a.md#cross-account-create-policy-a)\.

   ```
   {
     "Version": "2012-10-17",
     "Statement": {
       "Effect": "Allow",
       "Action": "sts:AssumeRole",
       "Resource": "arn:aws:iam::111122223333:role/MyCrossAccountRepositoryContributorRole"
     }
   }
   ```

1. Choose the **Users** tab\. Choose **Add Users to Group**, and then add the AccountB IAM users\. For example, you might add an IAM user with the user name *Saanvi\_Sarkar* to the group\.
**Note**  
Users in AccountB must have programmatic access, including an access key and secret key, to configure their local computers for access to the shared CodeCommit repository\. If you are creating IAM users, be sure to save the access key and secret key\. To ensure the security of your AWS account, the secret access key is accessible only at the time you create it\.