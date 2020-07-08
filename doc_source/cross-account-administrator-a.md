# Cross\-account repository access: Actions for the administrator in AccountA<a name="cross-account-administrator-a"></a>

To allow users or groups in AccountB to access a repository in AccountA, an AccountA administrator must:
+ Create a policy in AccountA that grants access to the repository\.
+ Create a role in AccountA that can be assumed by IAM users and groups in AccountB\.
+ Attach the policy to the role\.

The following sections provide steps and examples\.

**Topics**
+ [Step 1: Create a policy for repository access in AccountA](#cross-account-create-policy-a)
+ [Step 2: Create a role for repository access in AccountA](#cross-account-create-role-a)

## Step 1: Create a policy for repository access in AccountA<a name="cross-account-create-policy-a"></a>

You can create a policy in AccountA that grants access to the repository in AccountB\. Depending on the level of access you want to allow, do one of the following:
+ Configure the policy to allow AccountB users access to a specific repository, but do not allow them to view a list of all repositories in AccountA\.
+ Configure additional access to allow AccountB users to choose the repository from a list of all repositories in AccountA\.<a name="cross-account-create-policy-a-procedure"></a>

**To create a policy for repository access**

1. Sign in to the AWS Management Console as an IAM user with permissions to create policies in AccountA\.

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**\.

1. Choose **Create policy**\.

1. Choose the **JSON** tab, and paste the following JSON policy document into the JSON text box\. Replace *us\-east\-2* with the AWS Region for the repository, *111122223333* with the account ID for AccountA, and *MySharedDemoRepo* with the name for your CodeCommit repository in AccountA:

   ```
   {
   "Version": "2012-10-17",
   "Statement": [
       {
           "Effect": "Allow",
           "Action": [
               "codecommit:BatchGet*",
               "codecommit:Create*",
               "codecommit:DeleteBranch",
               "codecommit:Get*",
               "codecommit:List*",
               "codecommit:Describe*",
               "codecommit:Put*",
               "codecommit:Post*",
               "codecommit:Merge*",
               "codecommit:Test*",
               "codecommit:Update*",
               "codecommit:GitPull",
               "codecommit:GitPush"
           ],
           "Resource": [
               "arn:aws:codecommit:us-east-2:111122223333:MySharedDemoRepo"
           ]
       }
   ]
   }
   ```

   If you want users who assume this role to be able to view a list of repositories on the CodeCommit console home page, add an additional statement to the policy, as follows:

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                   "codecommit:BatchGet*",
                   "codecommit:Create*",
                   "codecommit:DeleteBranch",
                   "codecommit:Get*",
                   "codecommit:List*",
                   "codecommit:Describe*",
                   "codecommit:Put*",
                   "codecommit:Post*",
                   "codecommit:Merge*",
                   "codecommit:Test*",
                   "codecommit:Update*",
                   "codecommit:GitPull",
                   "codecommit:GitPush"
               ],
               "Resource": [
                   "arn:aws:codecommit:us-east-2:111122223333:MySharedDemoRepo"
               ]
           },
           {
               "Effect": "Allow",
               "Action": "codecommit:ListRepositories",
               "Resource": "*"
           }
       ]
   }
   ```

   This access makes it easier for users who assume this role with this policy to find the repository to which they have access\. They can choose the name of the repository from the list and be directed to the home page of the shared repository \(`Code`\)\. Users cannot access any of the other repositories they see in the list, but they can view the repositories in AccountA on the **Dashboard** page\.

   If you do not want to allow users who assume the role to be able to view a list of all repositories in AccountA, use the first policy example, but make sure that you send those users a direct link to the home page of the shared repository in the CodeCommit console\.

1. Choose **Review policy**\. The policy validator reports syntax errors \(for example, if you forget to replace the example AWS account ID and repository name with your AWS account ID and repository name\)\.

1. On the **Review policy** page, enter a name for the policy \(for example, *CrossAccountAccessForMySharedDemoRepo*\)\. You can also provide an optional description for this policy\. Choose **Create policy**\. 

## Step 2: Create a role for repository access in AccountA<a name="cross-account-create-role-a"></a>

After you have configured a policy, create a role that IAM users and groups in AccountB can assume, and attach the policy to that role\.<a name="cross-account-create-role-a-procedure"></a>

**To create a role for repository access**

1. In the IAM console, choose **Roles**\.

1. Choose **Create role**\.

1. Choose **Another AWS account**\.

1. In **Account ID**, enter the AWS account ID for AccountB \(for example, *888888888888*\)\. Choose **Next: Permissions**\.

1. In **Attach permissions policies**, select the policy you created in the previous procedure \(*CrossAccountAccessForMySharedDemoRepo*\)\. Choose **Next: Review**\.

1. In **Role name**, enter a name for the role \(for example, *MyCrossAccountRepositoryContributorRole*\)\. You can also enter an optional description to help others understand the purpose of the role\.

1. Choose **Create role**\.

1. Open the role you just created, and copy the role ARN \(for example, `arn:aws:iam::111122223333:role/MyCrossAccountRepositoryContributorRole`\)\. You need to provide this ARN to the AccountB administrator\.