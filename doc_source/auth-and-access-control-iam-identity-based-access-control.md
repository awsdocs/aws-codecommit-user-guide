# Using identity\-based policies \(IAM Policies\) for CodeCommit<a name="auth-and-access-control-iam-identity-based-access-control"></a>

The following examples of identity\-based policies demonstrate how an account administrator can attach permissions policies to IAM identities \(users, groups, and roles\) to grant permissions to perform operations on CodeCommit resources\.

**Important**  
We recommend that you first review the introductory topics that explain the basic concepts and options available to manage access to your CodeCommit resources\. For more information, see [Overview of managing access permissions to your CodeCommit resources](auth-and-access-control.md#auth-and-access-control-iam-access-control-identity-based)\.

**Topics**
+ [Permissions required to use the CodeCommit console](#console-permissions)
+ [Viewing resources in the console](#console-resources)
+ [AWS managed policies for CodeCommit](security-iam-awsmanpol.md)
+ [Customer managed policy examples](customer-managed-policies.md)

The following is an example of an identity\-based permissions policy: 

```
{
  "Version": "2012-10-17",
  "Statement" : [
    {
      "Effect" : "Allow",
      "Action" : [
        "codecommit:BatchGetRepositories"
      ],
      "Resource" : [
        "arn:aws:codecommit:us-east-2:111111111111:MyDestinationRepo",
        "arn:aws:codecommit:us-east-2:111111111111:MyDemo*"
      ]
    }
  ]
}
```

This policy has one statement that allows a user to get information about the CodeCommit repository named `MyDestinationRepo` and all CodeCommit repositories that start with the name `MyDemo` in the **us\-east\-2** Region\. 

## Permissions required to use the CodeCommit console<a name="console-permissions"></a>

To see the required permissions for each CodeCommit API operation, and for more information about CodeCommit operations, see [CodeCommit permissions reference](auth-and-access-control-permissions-reference.md)\.

To allow users to use the CodeCommit console, the administrator must grant them permissions for CodeCommit actions\. For example, you could attach the [AWSCodeCommitPowerUser](security-iam-awsmanpol.md#managed-policies-poweruser) managed policy or its equivalent to a user or group\.

In addition to permissions granted to users by identity\-based policies, CodeCommit requires permissions for AWS Key Management Service \(AWS KMS\) actions\. An IAM user does not need explicit `Allow` permissions for these actions, but the user must not have any policies attached that set the following permissions to `Deny`:

```
        "kms:Encrypt",
        "kms:Decrypt",
        "kms:ReEncrypt",
        "kms:GenerateDataKey",
        "kms:GenerateDataKeyWithoutPlaintext",
        "kms:DescribeKey"
```

For more information about encryption and CodeCommit, see [AWS KMS and encryption](encryption.md)\.

## Viewing resources in the console<a name="console-resources"></a>

The CodeCommit console requires the `ListRepositories` permission to display a list of repositories for your Amazon Web Services account in the AWS Region where you are signed in\. The console also includes a **Go to resource** function to quickly perform a case insensitive search for resources\. This search is performed in your Amazon Web Services account in the AWS Region where you are signed in\. The following resources are displayed across the following services:
+ AWS CodeBuild: Build projects
+ AWS CodeCommit: Repositories
+ AWS CodeDeploy: Applications
+ AWS CodePipeline: Pipelines

To perform this search across resources in all services, you must have the following permissions:
+ CodeBuild: `ListProjects`
+ CodeCommit: `ListRepositories`
+ CodeDeploy: `ListApplications`
+ CodePipeline: `ListPipelines`

Results are not returned for a service's resources if you do not have permissions for that service\. Even if you have permissions for viewing resources, specific resources will not be returned if there is an explicit `Deny` to view those resources\.