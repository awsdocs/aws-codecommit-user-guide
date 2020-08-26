# Identity and Access Management for AWS CodeCommit<a name="security-iam"></a>

AWS Identity and Access Management \(IAM\) is an AWS service that helps an administrator securely control access to AWS resources\. IAM administrators control who can be *authenticated* \(signed in\) and *authorized* \(have permissions\) to use CodeCommit resources\. IAM is an AWS service that you can use with no additional charge\.

**Topics**
+ [Audience](#security_iam_audience)
+ [Authenticating with identities](#security_iam_authentication)
+ [Managing access using policies](#security_iam_access-manage)
+ [Authentication and access control for AWS CodeCommit](auth-and-access-control.md)
+ [How AWS CodeCommit works with IAM](security_iam_service-with-iam.md)
+ [CodeCommit resource\-based policies](#security_iam_service-with-iam-resource-based-policies)
+ [Authorization based on CodeCommit tags](#security_iam_service-with-iam-tags)
+ [CodeCommit IAM roles](#security_iam_service-with-iam-roles)
+ [AWS CodeCommit identity\-based policy examples](#security_iam_id-based-policy-examples)
+ [Troubleshooting AWS CodeCommit identity and access](#security_iam_troubleshoot)

## Audience<a name="security_iam_audience"></a>

How you use AWS Identity and Access Management \(IAM\) differs, depending on the work you do in CodeCommit\.

**Service user** – If you use the CodeCommit service to do your job, then your administrator provides you with the credentials and permissions that you need\. As you use more CodeCommit features to do your work, you might need additional permissions\. Understanding how access is managed can help you request the right permissions from your administrator\. If you cannot access a feature in CodeCommit, see [Troubleshooting AWS CodeCommit identity and access](#security_iam_troubleshoot)\.

**Service administrator** – If you're in charge of CodeCommit resources at your company, you probably have full access to CodeCommit\. It's your job to determine which CodeCommit features and resources your employees should access\. You must then submit requests to your IAM administrator to change the permissions of your service users\. Review the information on this page to understand the basic concepts of IAM\. To learn more about how your company can use IAM with CodeCommit, see [How AWS CodeCommit works with IAM](security_iam_service-with-iam.md)\.

**IAM administrator** – If you're an IAM administrator, you might want to learn details about how you can write policies to manage access to CodeCommit\. To view example CodeCommit identity\-based policies that you can use in IAM, see [AWS CodeCommit identity\-based policy examples](#security_iam_id-based-policy-examples)\.

## Authenticating with identities<a name="security_iam_authentication"></a>

Authentication is how you sign in to AWS using your identity credentials\. For more information about signing in using the AWS Management Console, see [The IAM Console and Sign\-in Page](https://docs.aws.amazon.com/IAM/latest/UserGuide/console.html) in the *IAM User Guide*\.

You must be *authenticated* \(signed in to AWS\) as the AWS account root user, an IAM user, or by assuming an IAM role\. You can also use your company's single sign\-on authentication, or even sign in using Google or Facebook\. In these cases, your administrator previously set up identity federation using IAM roles\. When you access AWS using credentials from another company, you are assuming a role indirectly\. 

To sign in directly to the [AWS Management Console](https://console.aws.amazon.com/), use your password with your root user email or your IAM user name\. You can access AWS programmatically using your root user or IAM user access keys\. AWS provides SDK and command line tools to cryptographically sign your request using your credentials\. If you don’t use AWS tools, you must sign the request yourself\. Do this using *Signature Version 4*, a protocol for authenticating inbound API requests\. For more information about authenticating requests, see [Signature Version 4 Signing Process](https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html) in the *AWS General Reference*\.

Regardless of the authentication method that you use, you might also be required to provide additional security information\. For example, AWS recommends that you use multi\-factor authentication \(MFA\) to increase the security of your account\. To learn more, see [Using Multi\-Factor Authentication \(MFA\) in AWS](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa.html) in the *IAM User Guide*\.

### AWS Account Root User<a name="security_iam_authentication-rootuser"></a>

  When you first create an AWS account, you begin with a single sign\-in identity that has complete access to all AWS services and resources in the account\. This identity is called the AWS account *root user* and is accessed by signing in with the email address and password that you used to create the account\. We strongly recommend that you do not use the root user for your everyday tasks, even the administrative ones\. Instead, adhere to the [best practice of using the root user only to create your first IAM user](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#create-iam-users)\. Then securely lock away the root user credentials and use them to perform only a few account and service management tasks\. 

### IAM users and groups<a name="security_iam_authentication-iamuser"></a>

An *[IAM user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users.html)* is an identity within your AWS account that has specific permissions for a single person or application\. An IAM user can have long\-term credentials such as a user name and password or a set of access keys\. To learn how to generate access keys, see [Managing Access Keys for IAM Users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html) in the *IAM User Guide*\. When you generate access keys for an IAM user, make sure you view and securely save the key pair\. You cannot recover the secret access key in the future\. Instead, you must generate a new access key pair\.

An [https://docs.aws.amazon.com/IAM/latest/UserGuide/id_groups.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_groups.html) is an identity that specifies a collection of IAM users\. You can't sign in as a group\. You can use groups to specify permissions for multiple users at a time\. Groups make permissions easier to manage for large sets of users\. For example, you could have a group named *IAMAdmins* and give that group permissions to administer IAM resources\.

Users are different from roles\. A user is uniquely associated with one person or application, but a role is intended to be assumable by anyone who needs it\. Users have permanent long\-term credentials, but roles provide temporary credentials\. To learn more, see [When to Create an IAM User \(Instead of a Role\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html#id_which-to-choose) in the *IAM User Guide*\.

### IAM roles<a name="security_iam_authentication-iamrole"></a>

An *[IAM role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html)* is an identity within your AWS account that has specific permissions\. It is similar to an IAM user, but is not associated with a specific person\. You can temporarily assume an IAM role in the AWS Management Console by [switching roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-console.html)\. You can assume a role by calling an AWS CLI or AWS API operation or by using a custom URL\. For more information about methods for using roles, see [Using IAM Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use.html) in the *IAM User Guide*\.

IAM roles with temporary credentials are useful in the following situations:
+ **Temporary IAM user permissions** – An IAM user can assume an IAM role to temporarily take on different permissions for a specific task\. 
+ **Federated user access** –  Instead of creating an IAM user, you can use existing identities from AWS Directory Service, your enterprise user directory, or a web identity provider\. These are known as *federated users*\. AWS assigns a role to a federated user when access is requested through an [identity provider](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers.html)\. For more information about federated users, see [Federated Users and Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction_access-management.html#intro-access-roles) in the *IAM User Guide*\. 
+ **Cross\-account access** – You can use an IAM role to allow someone \(a trusted principal\) in a different account to access resources in your account\. Roles are the primary way to grant cross\-account access\. However, with some AWS services, you can attach a policy directly to a resource \(instead of using a role as a proxy\)\. To learn the difference between roles and resource\-based policies for cross\-account access, see [How IAM Roles Differ from Resource\-based Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_compare-resource-policies.html) in the *IAM User Guide*\.
+ **AWS service access** –  A service role is an IAM role that a service assumes to perform actions in your account on your behalf\. When you set up some AWS service environments, you must define a role for the service to assume\. This service role must include all the permissions that are required for the service to access the AWS resources that it needs\. Service roles vary from service to service, but many allow you to choose your permissions as long as you meet the documented requirements for that service\. Service roles provide access only within your account and cannot be used to grant access to services in other accounts\. You can create, modify, and delete a service role from within IAM\. For example, you can create a role that allows Amazon Redshift to access an Amazon S3 bucket on your behalf and then load data from that bucket into an Amazon Redshift cluster\. For more information, see [Creating a Role to Delegate Permissions to an AWS Service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) in the *IAM User Guide*\. 
+ **Applications running on Amazon EC2** –  You can use an IAM role to manage temporary credentials for applications that are running on an EC2 instance and making AWS CLI or AWS API requests\. This is preferable to storing access keys within the EC2 instance\. To assign an AWS role to an EC2 instance and make it available to all of its applications, you create an instance profile that is attached to the instance\. An instance profile contains the role and enables programs that are running on the EC2 instance to get temporary credentials\. For more information, see [Using an IAM Role to Grant Permissions to Applications Running on Amazon EC2 Instances](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2.html) in the *IAM User Guide*\. 

To learn whether to use IAM roles, see [When to Create an IAM Role \(Instead of a User\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html#id_which-to-choose_role) in the *IAM User Guide*\.

## Managing access using policies<a name="security_iam_access-manage"></a>

You control access in AWS by creating policies and attaching them to IAM identities or AWS resources\. A policy is an object in AWS that, when associated with an identity or resource, defines their permissions\. AWS evaluates these policies when an entity \(root user, IAM user, or IAM role\) makes a request\. Permissions in the policies determine whether the request is allowed or denied\. Most policies are stored in AWS as JSON documents\. For more information about the structure and contents of JSON policy documents, see [Overview of JSON Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html#access_policies-json) in the *IAM User Guide*\.

An IAM administrator can use policies to specify who has access to AWS resources, and what actions they can perform on those resources\. Every IAM entity \(user or role\) starts with no permissions\. In other words, by default, users can do nothing, not even change their own password\. To give a user permission to do something, an administrator must attach a permissions policy to a user\. Or the administrator can add the user to a group that has the intended permissions\. When an administrator gives permissions to a group, all users in that group are granted those permissions\.

IAM policies define permissions for an action regardless of the method that you use to perform the operation\. For example, suppose that you have a policy that allows the `iam:GetRole` action\. A user with that policy can get role information from the AWS Management Console, the AWS CLI, or the AWS API\.

### Identity\-based policies<a name="security_iam_access-manage-id-based-policies"></a>

Identity\-based policies are JSON permissions policy documents that you can attach to an identity, such as an IAM user, role, or group\. These policies control what actions that identity can perform, on which resources, and under what conditions\. To learn how to create an identity\-based policy, see [Creating IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html) in the *IAM User Guide*\.

Identity\-based policies can be further categorized as *inline policies* or *managed policies*\. Inline policies are embedded directly into a single user, group, or role\. Managed policies are standalone policies that you can attach to multiple users, groups, and roles in your AWS account\. Managed policies include AWS managed policies and customer managed policies\. To learn how to choose between a managed policy or an inline policy, see [Choosing Between Managed Policies and Inline Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#choosing-managed-or-inline) in the *IAM User Guide*\.

### Resource\-based policies<a name="security_iam_access-manage-resource-based-policies"></a>

Resource\-based policies are JSON policy documents that you attach to a resource such as an Amazon S3 bucket\. Service administrators can use these policies to define what actions a specified principal \(account member, user, or role\) can perform on that resource and under what conditions\. Resource\-based policies are inline policies\. There are no managed resource\-based policies\.

### Access control lists \(ACLs\)<a name="security_iam_access-manage-acl"></a>

Access control lists \(ACLs\) are a type of policy that controls which principals \(account members, users, or roles\) have permissions to access a resource\. ACLs are similar to resource\-based policies, although they do not use the JSON policy document format\. Amazon S3, AWS WAF, and Amazon VPC are examples of services that support ACLs\. To learn more about ACLs, see [Access Control List \(ACL\) Overview](https://docs.aws.amazon.com/AmazonS3/latest/dev/acl-overview.html) in the *Amazon Simple Storage Service Developer Guide*\.

### Other policy types<a name="security_iam_access-manage-other-policies"></a>

AWS supports additional, less\-common policy types\. These policy types can set the maximum permissions granted to you by the more common policy types\. 
+ **Permissions boundaries** – A permissions boundary is an advanced feature in which you set the maximum permissions that an identity\-based policy can grant to an IAM entity \(IAM user or role\)\. You can set a permissions boundary for an entity\. The resulting permissions are the intersection of entity's identity\-based policies and its permissions boundaries\. Resource\-based policies that specify the user or role in the `Principal` field are not limited by the permissions boundary\. An explicit deny in any of these policies overrides the allow\. For more information about permissions boundaries, see [Permissions Boundaries for IAM Entities](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_boundaries.html) in the *IAM User Guide*\.
+ **Service control policies \(SCPs\)** – SCPs are JSON policies that specify the maximum permissions for an organization or organizational unit \(OU\) in AWS Organizations\. AWS Organizations is a service for grouping and centrally managing multiple AWS accounts that your business owns\. If you enable all features in an organization, then you can apply service control policies \(SCPs\) to any or all of your accounts\. The SCP limits permissions for entities in member accounts, including each AWS account root user\. For more information about Organizations and SCPs, see [How SCPs Work](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_about-scps.html) in the *AWS Organizations User Guide*\.
+ **Session policies** – Session policies are advanced policies that you pass as a parameter when you programmatically create a temporary session for a role or federated user\. The resulting session's permissions are the intersection of the user or role's identity\-based policies and the session policies\. Permissions can also come from a resource\-based policy\. An explicit deny in any of these policies overrides the allow\. For more information, see [Session Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html#policies_session) in the *IAM User Guide*\. 

### Multiple policy types<a name="security_iam_access-manage-multiple-policies"></a>

When multiple types of policies apply to a request, the resulting permissions are more complicated to understand\. To learn how AWS determines whether to allow a request when multiple policy types are involved, see [Policy Evaluation Logic](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html) in the *IAM User Guide*\.

## CodeCommit resource\-based policies<a name="security_iam_service-with-iam-resource-based-policies"></a>

CodeCommit does not support resource\-based policies\. 

## Authorization based on CodeCommit tags<a name="security_iam_service-with-iam-tags"></a>

You can attach tags to CodeCommit resources or pass tags in a request to CodeCommit\. To control access based on tags, you provide tag information in the [condition element](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) of a policy using the `codecommit:ResourceTag/key-name`, `aws:RequestTag/key-name`, or `aws:TagKeys` condition keys\. For more information about tagging CodeCommit resources, see [Example 5: Deny or allow actions on repositories with tags](auth-and-access-control-iam-identity-based-access-control.md#identity-based-policies-example-5)\. For more information about tagging strategies, see [Tagging AWS Resources](https://docs.aws.amazon.com/general/latest/gr/aws_tagging.html)\.

CodeCommit also supports policies based on session tags\. For more information, see [Session Tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_session-tags.html)\. 

### Using tags to provide identity information in CodeCommit<a name="security-iam_service-with-iam-tags-identity"></a>

CodeCommit supports the use of session tags, which are key\-value pair attributes that you pass when you assume an IAM role, use temporary credentials, or federate a user in AWS Security Token Service \(AWS STS\)\. You can also associate tags with an IAM user\. You can use the information provided in these tags to make it easier to identify who made a change or caused an event\. CodeCommit includes the values for tags with the following key names in CodeCommit events:


****  

| Key name | Value | 
| --- | --- | 
| displayName | The human\-readable name to display and associate with the user \(for example, Mary Major or Saanvi Sarkar\)\. | 
| emailAddress | The email address you want displayed for and associated with the user \(for example, mary\_major@example\.com or saanvi\_sarkar@example\.com\)\. | 

If this information is provided, CodeCommit includes it in events sent to Amazon EventBridge and Amazon CloudWatch Events\. For more information, see [Monitoring CodeCommit events in Amazon EventBridge and Amazon CloudWatch Events](monitoring-events.md)\.

To use session tagging, roles must have policies that include the `sts:TagSession` permission set to `Allow`\. If you are using federated access, you can configure display name and email tag information as part of your setup\. For example, if you're using Azure Active Directory, you might provide the following claim information:


****  

| Claim name | Value | 
| --- | --- | 
| https://aws\.amazon\.com/SAML/Attributes/PrincipalTag:displayName | user\.displayname | 
| https://aws\.amazon\.com/SAML/Attributes/PrincipalTag:emailAddress | user\.mail | 

You can use the AWS CLI to pass session tags for `displayName` and `emailAddress` using AssumeRole\. For example, a user who wants to assume a role named *Developer* who wants to associate her name *Mary Major* might use the assume\-role command similar to the following:

```
aws sts assume-role \
--role-arn arn:aws:iam::123456789012:role/Developer \
--role-session-name Mary-Major \
–-tags Key=displayName,Value="Mary Major" Key=emailAddress,Value="mary_major@example.com" \
--external-id Example987
```

For more information, see [AssumeRole](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_session-tags.html#id_session-tags_adding-assume-role)\.

You can use the `AssumeRoleWithSAML` operation to return a set of temporary credentials that include `displayName` and `emailAddress` tags\. You can use these tags when you access CodeCommit repositories\. This requires that your company or group has already integrated your third\-party SAML solution with AWS\. If so, you can pass SAML attributes as session tags\. For example, if you wanted to pass identity attributes for a display name and email address for a user named *Saanvi Sarkar* as session tags:

```
<Attribute Name="https://aws.amazon.com/SAML/Attributes/PrincipalTag:displayName">
  <AttributeValue>Saanvi Sarkar</AttributeValue>
</Attribute>
<Attribute Name="https://aws.amazon.com/SAML/Attributes/PrincipalTag:emailAddress">
  <AttributeValue>saanvi_sarkar@example.com</AttributeValue>
</Attribute>
```

For more information, see [Passing Session Tags using AssumeRoleWithSAML](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_session-tags.html#id_session-tags_adding-assume-role-saml)\.

You can use the `AssumeRoleWithIdentity` operation to return a set of temporary credentials that include `displayName` and `emailAddress` tags\. You can use these tags when you access CodeCommit repositories\. To pass session tags from OpenID Connect \(OIDC\), you must include the session tags in the JSON Web Token \(JWT\)\. For example, the decoded JWP token used to call `AssumeRoleWithWebIdentity` that includes the `displayName` and `emailAddress` session tags for a user named *Li Juan*:

```
{
    "sub": "lijuan",
    "aud": "ac_oic_client",
    "jti": "ZYUCeREXAMPLE",
    "iss": "https://xyz.com",
    "iat": 1566583294,
    "exp": 1566583354,
    "auth_time": 1566583292,
    "https://aws.amazon.com/tags": {
        "principal_tags": {
            "displayName": ["Li Juan"],
            "emailAddress": ["li_juan@example.com"],
        },
        "transitive_tag_keys": [
            "displayName",
            "emailAddress"
        ]
    }
}
```

For more information, see [Passing Session Tags using AssumeRoleWithWebIdentity](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_session-tags.html#id_session-tags_adding-assume-role-idp)\.

You can use the `GetFederationToken` operation to return a set of temporary credentials that include `displayName` and `emailAddress` tags\. You can use these tags when you access CodeCommit repositories\. For example, to use the AWS CLI to get a federation token that includes the `displayName` and `emailAddress` tags:

```
aws sts get-federation-token \
--name my-federated-user \
–-tags key=displayName,value="Nikhil Jayashankar" key=emailAddress,value=nikhil_jayashankar@example.com
```

For more information, see [Passing Session Tags using GetFederationToken](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_session-tags.html#id_session-tags_adding-getfederationtoken)\.

## CodeCommit IAM roles<a name="security_iam_service-with-iam-roles"></a>

An [IAM role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) is an entity within your AWS account that has specific permissions\.

### Using temporary credentials with CodeCommit<a name="security_iam_service-with-iam-roles-tempcreds"></a>

You can use temporary credentials to sign in with federation, assume an IAM role, or to assume a cross\-account role\. You obtain temporary security credentials by calling AWS STS API operations such as [AssumeRole](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) or [GetFederationToken](https://docs.aws.amazon.com/STS/latest/APIReference/API_GetFederationToken.html)\. 

CodeCommit supports using temporary credentials\. For more information, see [Connecting to AWS CodeCommit repositories with rotating credentials](temporary-access.md)\.

### Service\-linked roles<a name="security_iam_service-with-iam-roles-service-linked"></a>

[Service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role) allow AWS services to access resources in other services to complete an action on your behalf\. Service\-linked roles appear in your IAM account and are owned by the service\. An IAM administrator can view but not edit the permissions for service\-linked roles\.

CodeCommit does not use service\-linked roles\. 

### Service roles<a name="security_iam_service-with-iam-roles-service"></a>

This feature allows a service to assume a [service role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-role) on your behalf\. This role allows the service to access resources in other services to complete an action on your behalf\. Service roles appear in your IAM account and are owned by the account\. This means that an IAM administrator can change the permissions for this role\. However, doing so might break the functionality of the service\.

CodeCommit does not use service roles\. 

## AWS CodeCommit identity\-based policy examples<a name="security_iam_id-based-policy-examples"></a>

By default, IAM users and roles don't have permission to create or modify CodeCommit resources\. They also can't perform tasks using the AWS Management Console, AWS CLI, or AWS API\. An IAM administrator must create IAM policies that grant users and roles permission to perform specific API operations on the specified resources they need\. The administrator must then attach those policies to the IAM users or groups that require those permissions\.

For examples of policies, see the following:
+  [Example 1: Allow a user to perform CodeCommit operations in a single AWS Region](auth-and-access-control-iam-identity-based-access-control.md#identity-based-policies-example-1)
+ [Example 2: Allow a user to use Git for a single repository](auth-and-access-control-iam-identity-based-access-control.md#identity-based-policies-example-2)
+ [Example 3: Allow a user connecting from a specified IP address range access to a repository ](auth-and-access-control-iam-identity-based-access-control.md#identity-based-policies-example-3)
+ [Example 4: Deny or allow actions on branches](auth-and-access-control-iam-identity-based-access-control.md#identity-based-policies-example-4)
+ [Example 5: Deny or allow actions on repositories with tags](auth-and-access-control-iam-identity-based-access-control.md#identity-based-policies-example-5)
+ [Configure cross\-account access to an AWS CodeCommit repository using roles](cross-account.md)

To learn how to create an IAM identity\-based policy using these example JSON policy documents, see [Creating Policies on the JSON Tab](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html#access_policies_create-json-editor) in the *IAM User Guide*\.

**Topics**
+ [Policy best practices](#security_iam_service-with-iam-policy-best-practices)
+ [Using the CodeCommit console](#security_iam_id-based-policy-examples-console)
+ [Allow users to view their own permissions](#security_iam_id-based-policy-examples-view-own-permissions)
+ [Viewing CodeCommit *repositories* based on tags](#security_iam_id-based-policy-examples-view-repositories-tags)

### Policy best practices<a name="security_iam_service-with-iam-policy-best-practices"></a>

Identity\-based policies are very powerful\. They determine whether someone can create, access, or delete CodeCommit resources in your account\. These actions can incur costs for your AWS account\. When you create or edit identity\-based policies, follow these guidelines and recommendations:
+ **Get Started Using AWS Managed Policies** – To start using CodeCommit quickly, use AWS managed policies to give your employees the permissions they need\. These policies are already available in your account and are maintained and updated by AWS\. For more information, see [Get Started Using Permissions With AWS Managed Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#bp-use-aws-defined-policies) in the *IAM User Guide*\.
+ **Grant Least Privilege** – When you create custom policies, grant only the permissions required to perform a task\. Start with a minimum set of permissions and grant additional permissions as necessary\. Doing so is more secure than starting with permissions that are too lenient and then trying to tighten them later\. For more information, see [Grant Least Privilege](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#grant-least-privilege) in the *IAM User Guide*\.
+ **Enable MFA for Sensitive Operations** – For extra security, require IAM users to use multi\-factor authentication \(MFA\) to access sensitive resources or API operations\. For more information, see [Using Multi\-Factor Authentication \(MFA\) in AWS](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa.html) in the *IAM User Guide*\.
+ **Use Policy Conditions for Extra Security** – To the extent that it's practical, define the conditions under which your identity\-based policies allow access to a resource\. For example, you can write conditions to specify a range of allowable IP addresses that a request must come from\. You can also write conditions to allow requests only within a specified date or time range, or to require the use of SSL or MFA\. For more information, see [IAM JSON Policy Elements: Condition](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) in the *IAM User Guide*\.

### Using the CodeCommit console<a name="security_iam_id-based-policy-examples-console"></a>

To access the AWS CodeCommit console, you must have a minimum set of permissions\. These permissions must allow you to list and view details about the CodeCommit resources in your AWS account\. If you create an identity\-based policy that is more restrictive than the minimum required permissions, the console won't function as intended for entities \(IAM users or roles\) with that policy\.

To ensure that those entities can still use the CodeCommit console, also attach the following AWS managed policy to the entities\. For more information, see [Adding Permissions to a User](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_change-permissions.html#users_change_permissions-add-console) in the *IAM User Guide*:

For more information, see [Using identity\-based policies \(IAM Policies\) for CodeCommit](auth-and-access-control-iam-identity-based-access-control.md)\.

You don't need to allow minimum console permissions for users that are making calls only to the AWS CLI or the AWS API\. Instead, allow access to only the actions that match the API operation that you're trying to perform\.

### Allow users to view their own permissions<a name="security_iam_id-based-policy-examples-view-own-permissions"></a>

This example shows how you might create a policy that allows IAM users to view the inline and managed policies that are attached to their user identity\. This policy includes permissions to complete this action on the console or programmatically using the AWS CLI or AWS API\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ViewOwnUserInfo",
            "Effect": "Allow",
            "Action": [
                "iam:GetUserPolicy",
                "iam:ListGroupsForUser",
                "iam:ListAttachedUserPolicies",
                "iam:ListUserPolicies",
                "iam:GetUser"
            ],
            "Resource": ["arn:aws:iam::*:user/${aws:username}"]
        },
        {
            "Sid": "NavigateInConsole",
            "Effect": "Allow",
            "Action": [
                "iam:GetGroupPolicy",
                "iam:GetPolicyVersion",
                "iam:GetPolicy",
                "iam:ListAttachedGroupPolicies",
                "iam:ListGroupPolicies",
                "iam:ListPolicyVersions",
                "iam:ListPolicies",
                "iam:ListUsers"
            ],
            "Resource": "*"
        }
    ]
}
```

### Viewing CodeCommit *repositories* based on tags<a name="security_iam_id-based-policy-examples-view-repositories-tags"></a>

You can use conditions in your identity\-based policy to control access to CodeCommit resources based on tags\. For an example policy that demonstrates how to do this, see [Example 5: Deny or allow actions on repositories with tags](auth-and-access-control-iam-identity-based-access-control.md#identity-based-policies-example-5)\.

For more information, see [IAM JSON Policy Elements: Condition](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) in the *IAM User Guide*\.

## Troubleshooting AWS CodeCommit identity and access<a name="security_iam_troubleshoot"></a>

Use the following information to help you diagnose and fix common issues that you might encounter when working with CodeCommit and IAM\.

**Topics**
+ [I Am not authorized to perform an action in CodeCommit](#security_iam_troubleshoot-no-permissions)
+ [I Am not authorized to perform iam:PassRole](#security_iam_troubleshoot-passrole)
+ [I want to view my access keys](#security_iam_troubleshoot-access-keys)
+ [I'm an administrator and want to allow others to access CodeCommit](#security_iam_troubleshoot-admin-delegate)
+ [I want to allow people outside of my AWS account to access my CodeCommit resources](#security_iam_troubleshoot-cross-account-access)

### I Am not authorized to perform an action in CodeCommit<a name="security_iam_troubleshoot-no-permissions"></a>

If the AWS Management Console tells you that you're not authorized to perform an action, then you must contact your administrator for assistance\. Your administrator is the person that provided you with your user name and password\.

For more information, see [Permissions required to use the CodeCommit console](auth-and-access-control-iam-identity-based-access-control.md#console-permissions)

### I Am not authorized to perform iam:PassRole<a name="security_iam_troubleshoot-passrole"></a>

If you receive an error that you're not authorized to perform the `iam:PassRole` action, then you must contact your administrator for assistance\. Your administrator is the person that provided you with your user name and password\. Ask that person to update your policies to allow you to pass a role to CodeCommit\.

Some AWS services allow you to pass an existing role to that service, instead of creating a new service role or service\-linked role\. To do this, you must have permissions to pass the role to the service\.

The following example error occurs when an IAM user named `marymajor` tries to use the console to perform an action in CodeCommit\. However, the action requires the service to have permissions granted by a service role\. Mary does not have permissions to pass the role to the service\.

```
User: arn:aws:iam::123456789012:user/marymajor is not authorized to perform: iam:PassRole
```

In this case, Mary asks her administrator to update her policies to allow her to perform the `iam:PassRole` action\.

### I want to view my access keys<a name="security_iam_troubleshoot-access-keys"></a>

After you create your IAM user access keys, you can view your access key ID at any time\. However, you can't view your secret access key again\. If you lose your secret key, you must create a new access key pair\. 

Access keys consist of two parts: an access key ID \(for example, `AKIAIOSFODNN7EXAMPLE`\) and a secret access key \(for example, `wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY`\)\. Like a user name and password, you must use both the access key ID and secret access key together to authenticate your requests\. Manage your access keys as securely as you do your user name and password\.

**Important**  
 Do not provide your access keys to a third party, even to help [find your canonical user ID](https://docs.aws.amazon.com/general/latest/gr/acct-identifiers.html#FindingCanonicalId)\. By doing this, you might give someone permanent access to your account\. 

When you create an access key pair, you are prompted to save the access key ID and secret access key in a secure location\. The secret access key is available only at the time you create it\. If you lose your secret access key, you must add new access keys to your IAM user\. You can have a maximum of two access keys\. If you already have two, you must delete one key pair before creating a new one\. To view instructions, see [Managing Access Keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html#Using_CreateAccessKey) in the *IAM User Guide*\.

### I'm an administrator and want to allow others to access CodeCommit<a name="security_iam_troubleshoot-admin-delegate"></a>

To allow others to access CodeCommit, you must create an IAM entity \(user or role\) for the person or application that needs access\. They will use the credentials for that entity to access AWS\. You must then attach a policy to the entity that grants them the correct permissions in CodeCommit\.

To get started right away, see [Creating Your First IAM Delegated User and Group](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-delegated-user.html) in the *IAM User Guide*\.

### I want to allow people outside of my AWS account to access my CodeCommit resources<a name="security_iam_troubleshoot-cross-account-access"></a>

For more information, see [Configure cross\-account access to an AWS CodeCommit repository using roles](cross-account.md)\.