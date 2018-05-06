# Overview of Managing Access Permissions to Your AWS CodeCommit Resources<a name="auth-and-access-control-iam-access-control-identity-based"></a>

Every AWS resource is owned by an AWS account\. Permissions to create or access a resource are governed by permissions policies\. An account administrator can attach permissions policies to IAM identities \(that is, users, groups, and roles\)\. Some services, such as AWS Lambda, also support attaching permissions policies to resources\. 

**Note**  
An *account administrator* \(or administrator user\) is a user with administrator privileges\. For more information, see [IAM Best Practices](http://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) in the *IAM User Guide*\.

When granting permissions, you decide who gets the permissions, the resources they get permissions for, and the specific actions that you want to allow on those resources\.

**Topics**
+ [AWS CodeCommit Resources and Operations](#arn-formats)
+ [Understanding Resource Ownership](#understanding-resource-ownership)
+ [Managing Access to Resources](#managing-access-resources)
+ [Resource Scoping in AWS CodeCommit](#resource-scoping)
+ [Specifying Policy Elements: Resources, Actions, Effects, and Principals](#actions-effects-principals)
+ [Specifying Conditions in a Policy](#policy-conditions)

## AWS CodeCommit Resources and Operations<a name="arn-formats"></a>

In AWS CodeCommit, the primary resource is a repository\. Each resource has a unique Amazon Resource Names \(ARN\) associated with it\. In a policy, you use an Amazon Resource Name \(ARN\) to identify the resource that the policy applies to\. For more information about ARNs, see [Amazon Resource Names \(ARN\) and AWS Service Namespaces](http://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html) in the *Amazon Web Services General Reference*\. AWS CodeCommit does not currently support other resource types, which are referred to as subresources\.

The following table describes how to specify AWS CodeCommit resources\.


| Resource Type | ARN Format | 
| --- | --- | 
| Repository |  arn:aws:codecommit:*region*:*account\-id*:*repository\-name*  | 
|  All AWS CodeCommit repositories  |  arn:aws:codecommit:\*  | 
|  All AWS CodeCommit repositories owned by the specified account in the specified region  |  arn:aws:codecommit:*region*:*account\-id*:\*  | 

**Note**  
Most AWS services treat a colon \(:\) or a forward slash \(/\) in ARNs as the same character\. However, AWS CodeCommit requires an exact match in resource patterns and rules\. When creating event patterns, be sure to use the correct ARN characters so that they match the ARN syntax in the resource\.

For example, you can indicate a specific repository \(*MyDemoRepo*\) in your statement using its ARN as follows:

```
"Resource": "arn:aws:codecommit:us-west-2:111111111111:MyDemoRepo"
```

To specify all repositories that belong to a specific account, use the wildcard character \(\*\) as follows:

```
"Resource": "arn:aws:codecommit:us-west-2:111111111111:*"
```

To specify all resources, or if a specific API action does not support ARNs, use the wildcard character \(\*\) in the `Resource` element as follows:

```
"Resource": "*"
```

You can also use the wildcard character\(\*\) to specify all resources that match part of a repository name\. For example, the following ARN specifies any AWS CodeCommit repository that begins with the name `MyDemo` and that is registered to the AWS account `111111111111` in the `us-east-2` AWS Region:

```
arn:aws:codecommit:us-east-2:111111111111:MyDemo*
```

 For a list of available operations that work with the AWS CodeCommit resources, see [AWS CodeCommit Permissions Reference](auth-and-access-control-permissions-reference.md)\.

## Understanding Resource Ownership<a name="understanding-resource-ownership"></a>

The AWS account owns the resources that are created in the account, regardless of who created them\. Specifically, the resource owner is the AWS account of the [principal entity](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html) \(that is, the root account, an IAM user, or an IAM role\) that authenticates the resource creation request\. The following examples illustrate how this works:
+ If you create an IAM user in your AWS account and grant permissions to create AWS CodeCommit resources to that user, the user can create AWS CodeCommit resources\. However, your AWS account, to which the user belongs, owns the AWS CodeCommit resources\.
+ If you use the root account credentials of your AWS account to create a rule, your AWS account is the owner of the AWS CodeCommit resource\.
+ If you create an IAM role in your AWS account with permissions to create AWS CodeCommit resources, anyone who can assume the role can create AWS CodeCommit resources\. Your AWS account, to which the role belongs, owns the AWS CodeCommit resources\.

## Managing Access to Resources<a name="managing-access-resources"></a>

To manage access to AWS resources, you use permissions policies\. A *permissions policy* describes who has access to what\. The following section explains the options for creating permissions policies\.

**Note**  
This section discusses using IAM in the context of AWS CodeCommit\. It doesn't provide detailed information about the IAM service\. For more information about IAM, see [What Is IAM?](http://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) in the *IAM User Guide*\. For information about IAM policy syntax and descriptions, see [AWS IAM Policy Reference](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies.html) in the *IAM User Guide*\.

Permissions policies that are attached to an IAM identity are referred to as identity\-based policies \(IAM polices\)\. Permissions policies that are attached to a resource are referred to as resource\-based policies\. Currently, AWS CodeCommit supports only identity\-based policies \(IAM policies\)\.

**Topics**
+ [Identity\-Based Policies \(IAM Policies\)](#identity-based-policies)
+ [Resource\-Based Policies](#resource-based-policies-overview)

### Identity\-Based Policies \(IAM Policies\)<a name="identity-based-policies"></a>

To manage access to AWS resources, you attach permissions policies to IAM identities\. In AWS CodeCommit, you use identity\-based policies to control access to repositories\. For example, you can do the following: 
+ **Attach a permissions policy to a user or a group in your account** – To grant a user permissions to view AWS CodeCommit resources in the AWS CodeCommit console, attach an identity\-based permissions policy to a user or group that the user belongs to\.
+ **Attach a permissions policy to a role \(to grant cross\-account permissions\)** – Delegation, such as when you want to grant cross\-account access, involves setting up a trust between the account that owns the resource \(the trusting account\), and the account that contains the users who need to access the resource \(the trusted account\)\. A permissions policy grants the user of a role the needed permissions to carry out the intended tasks on the resource\. A trust policy specifies which trusted accounts are allowed to grant its users permissions to assume the role\. For more information, see [IAM Terms and Concepts](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html)\. 

  To grant cross\-account permissions, attach an identity\-based permissions policy to an IAM role\. For example, the administrator in Account A can create a role to grant cross\-account permissions to another AWS account \(for example, Account B\) or an AWS service as follows:

  1. Account A administrator creates an IAM role and attaches a permissions policy to the role that grants permissions on resources in Account A\.

  1. Account A administrator attaches a trust policy to the role identifying Account B as the principal who can assume the role\.

  1. Account B administrator can then delegate permissions to assume the role to any users in Account B\. Doing this allows users in Account B to create or access resources in Account A\. If you want to grant an AWS service permission to assume the role, the principal in the trust policy can also be an AWS service principal\. For more information, see Delegation in [IAM Terms and Concepts](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html)\.

  For more information about using IAM to delegate permissions, see [Access Management](http://docs.aws.amazon.com/IAM/latest/UserGuide/access.html) in the *IAM User Guide*\.

The following example policy allows a user to create a branch in a repository named *MyDemoRepo*:

```
{
  "Version": "2012-10-17",
  "Statement" : [
    {
      "Effect" : "Allow",
      "Action" : [
        "codecommit:CreateBranch"
      ],
      "Resource" : "arn:aws:codecommit:us-east-2:111111111111:MyDemoRepo"
    }
  ]
}
```

To restrict the calls and resources that users in your account have access to, create specific IAM policies, and then attach those policies to IAM users\. For more information about how to create IAM roles and to explore example IAM policy statements for AWS CodeCommit, see [Overview of Managing Access Permissions to Your AWS CodeCommit Resources](#auth-and-access-control-iam-access-control-identity-based)\. 

### Resource\-Based Policies<a name="resource-based-policies-overview"></a>

Some services, such as Amazon S3, also support resource\-based permissions policies\. For example, you can attach a resource\-based policy to an S3 bucket to manage access permissions to that bucket\. AWS CodeCommit doesn't support resource\-based policies\. 

## Resource Scoping in AWS CodeCommit<a name="resource-scoping"></a>

In AWS CodeCommit, you can scope identity\-based policies and permissions to resources, as described in [AWS CodeCommit Resources and Operations](#arn-formats)\. However, you cannot scope the `ListRepositories` permission to a resource\. Instead, you must scope it to all resources \(using the wildcard `*`\)\. Otherwise, the action fails\. 

All other AWS CodeCommit permissions can be scoped to resources\.

## Specifying Policy Elements: Resources, Actions, Effects, and Principals<a name="actions-effects-principals"></a>

You can create policies to allow or deny users access to resources, or allow or deny users to take specific actions on those resources\. AWS CodeCommit defines a set of public API operations that define how users work with the service, whether that is through the AWS CodeCommit console, the SDKs, the AWS CLI, or by directly calling those APIs\. To grant permissions for these API operations, AWS CodeCommit defines a set of actions that you can specify in a policy\. 

Some API operations can require permissions for more than one action\. For more information about resources and API operations, see [AWS CodeCommit Resources and Operations](#arn-formats) and [AWS CodeCommit Permissions Reference](auth-and-access-control-permissions-reference.md)\.

The following are the basic elements of a policy:
+ **Resource** – To identify the resource that the policy applies to, you use an Amazon Resource Name \(ARN\)\. For more information, see [AWS CodeCommit Resources and Operations](#arn-formats)\.
+ **Action** – To identify resource operations that you want to allow or deny, you use action keywords\. For example, depending on the specified `Effect`, the `codecommit:GetBranch` permission either allows or denies the user to perform the `GetBranch` operation, which gets details about a branch in an AWS CodeCommit repository\.
+ **Effect** – You specify the effect, either allow or deny, that takes place when the user requests the specific action\. If you don't explicitly grant access to \(allow\) a resource, access is implicitly denied\. You can also explicitly deny access to a resource to make sure that a user cannot access it, even if a different policy grants access\.
+ **Principal** – In identity\-based policies \(IAM policies\), the only type of policies that AWS CodeCommit supports, the user that the policy is attached to is the implicit principal\. 

To learn more about IAM policy syntax, see [AWS IAM Policy Reference](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies.html) in the *IAM User Guide*\.

For a table showing all of the AWS CodeCommit API actions and the resources that they apply to, see [AWS CodeCommit Permissions Reference](auth-and-access-control-permissions-reference.md)\.

## Specifying Conditions in a Policy<a name="policy-conditions"></a>

When you grant permissions, you use the access policy language for IAM to specify the conditions under which a policy should take effect\. For example, you might want a policy to be applied only after a specific date\. For more information about specifying conditions in a policy language, see [Condition](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html#Condition) and [Policy Grammar](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_grammar.html) in the *IAM User Guide*\.

To express conditions, you use predefined condition keys\. There are no condition keys specific to AWS CodeCommit\. However, there are AWS\-wide condition keys that you can use as appropriate\. For a complete list of AWS\-wide keys, see [Available Keys for Conditions](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html#AvailableKeys) in the *IAM User Guide*\. 