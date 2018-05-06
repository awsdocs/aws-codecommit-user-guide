# Using Identity\-Based Policies \(IAM Policies\) for AWS CodeCommit<a name="auth-and-access-control-iam-identity-based-access-control"></a>

The following examples of identity\-based policies demonstrate how an account administrator can attach permissions policies to IAM identities \(users, groups, and roles\) to grant permissions to perform operations on AWS CodeCommit resources\.

**Important**  
We recommend that you first review the introductory topics that explain the basic concepts and options available to manage access to your AWS CodeCommit resources\. For more information, see [Overview of Managing Access Permissions to Your AWS CodeCommit Resources](auth-and-access-control-iam-access-control-identity-based.md)\.

**Topics**
+ [Permissions Required to Use the AWS CodeCommit Console](#console-permissions)
+ [AWS Managed \(Predefined\) Policies for AWS CodeCommit](#managed-policies)
+ [Customer Managed Policy Examples](#customer-managed-policies)

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

This policy has one statement that allows a user to get information about the AWS CodeCommit repository named `MyDestinationRepo` and all AWS CodeCommit repositories that start with the name `MyDemo` in the **us\-east\-2** Region\. 

## Permissions Required to Use the AWS CodeCommit Console<a name="console-permissions"></a>

To see the required permissions for each AWS CodeCommit API operation, and for more information about AWS CodeCommit operations, see [AWS CodeCommit Permissions Reference](auth-and-access-control-permissions-reference.md)\.

To allow users to use the AWS CodeCommit console, the administrator must grant them permissions for AWS CodeCommit actions\. For example, you could attach the AWSCodeCommitPowerUser managed policy or its equivalent to a user or group, as shown in the following permissions policy:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "codecommit:BatchGet*",
        "codecommit:Get*",
        "codecommit:List*",
        "codecommit:Create*",
        "codecommit:DeleteBranch",
        "codecommit:Describe*",
        "codecommit:Put*",
        "codecommit:Post*",
        "codecommit:Merge*",
        "codecommit:Test*",
        "codecommit:Update*",
        "codecommit:GitPull",
        "codecommit:GitPush"
      ],
      "Resource": "*"
    },
    {
      "Sid": "CloudWatchEventsCodeCommitRulesAccess",
      "Effect": "Allow",
      "Action": [
        "events:DeleteRule",
        "events:DescribeRule",
        "events:DisableRule",
        "events:EnableRule",
        "events:PutRule",
        "events:PutTargets",
        "events:RemoveTargets",
        "events:ListTargetsByRule"
      ],
      "Resource": "arn:aws:events:*:*:rule/codecommit*"
    },
    {
      "Sid": "SNSTopicAndSubscriptionAccess",
      "Effect": "Allow",
      "Action": [
        "sns:Subscribe",
        "sns:Unsubscribe"
      ],
      "Resource": "arn:aws:sns:*:*:codecommit*"
    },
    {
      "Sid": "SNSTopicAndSubscriptionReadAccess",
      "Effect": "Allow",
      "Action": [
        "sns:ListTopics",
        "sns:ListSubscriptionsByTopic",
        "sns:GetTopicAttributes"
      ],
      "Resource": "*"
    },
    {
      "Sid": "LambdaReadOnlyListAccess",
      "Effect": "Allow",
      "Action": [
        "lambda:ListFunctions"
      ],
      "Resource": "*"
    },
    {
      "Sid": "IAMReadOnlyListAccess",
      "Effect": "Allow",
      "Action": [
        "iam:ListUsers"
      ],
      "Resource": "*"
    },
    {
      "Sid": "IAMReadOnlyConsoleAccess",
      "Effect": "Allow",
      "Action": [
        "iam:ListAccessKeys",
        "iam:ListSSHPublicKeys",
        "iam:ListServiceSpecificCredentials",
        "iam:ListAccessKeys",
        "iam:GetSSHPublicKey"
      ],
      "Resource": "arn:aws:iam::*:user/${aws:username}"
    },
    {
      "Sid": "IAMUserSSHKeys",
      "Effect": "Allow",
      "Action": [
        "iam:DeleteSSHPublicKey",
        "iam:GetSSHPublicKey",
        "iam:ListSSHPublicKeys",
        "iam:UpdateSSHPublicKey",
        "iam:UploadSSHPublicKey"
      ],
      "Resource": "arn:aws:iam::*:user/${aws:username}"
    },
    {
      "Sid": "IAMSelfManageServiceSpecificCredentials",
      "Effect": "Allow",
      "Action": [
        "iam:CreateServiceSpecificCredential",
        "iam:UpdateServiceSpecificCredential",
        "iam:DeleteServiceSpecificCredential",
        "iam:ResetServiceSpecificCredential"
      ],
      "Resource": "arn:aws:iam::*:user/${aws:username}"
    }
  ]
}
```

In addition to permissions granted to users by identity\-based polices, AWS CodeCommit requires permissions for AWS Key Management Service \(AWS KMS\) actions\. An IAM user does not need explicit `Allow` permissions for these actions, but the user must not have any policies attached that set the following permissions to `Deny`:

```
        "kms:Encrypt",
        "kms:Decrypt",
        "kms:ReEncrypt",
        "kms:GenerateDataKey",
        "kms:GenerateDataKeyWithoutPlaintext",
        "kms:DescribeKey"
```

For more information about encryption and AWS CodeCommit, see [AWS KMS and Encryption](encryption.md)\.

## AWS Managed \(Predefined\) Policies for AWS CodeCommit<a name="managed-policies"></a>

AWS addresses many common use cases by providing standalone IAM policies that are created and administered by AWS\. These AWS managed policies grant required permissions for common use cases\. The managed policies for AWS CodeCommit also provide permissions to perform operations in other services, such as IAM, Amazon SNS, and Amazon CloudWatch Events, as required for the responsibilities for the users who have been granted the policy in question\. For example, the AWSCodeCommitFullAccess policy is an administrative\-level user policy that allows users with this policy to create and manage CloudWatch Events rules for repositories \(rules whose names are prefixed with `codecommit`\) and Amazon SNS topics for notifications about repository\-related events \(topics whose names are prefixed with `codecommit`\), as well as administer repositories in AWS CodeCommit\. 

The following AWS managed policies, which you can attach to users in your account, are specific to AWS CodeCommit:
+ **AWSCodeCommitFullAccess** – Grants full access to AWS CodeCommit\. Apply this policy only to administrative\-level users to whom you want to grant full control over AWS CodeCommit repositories and related resources in your AWS account, including the ability to delete repositories\.

  The AWSCodeCommitFullAccess policy contains the following policy statement:

  ```
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": [
          "codecommit:*"
        ],
        "Resource": "*"
      },
      {
        "Sid": "CloudWatchEventsCodeCommitRulesAccess",
        "Effect": "Allow",
        "Action": [
          "events:DeleteRule",
          "events:DescribeRule",
          "events:DisableRule",
          "events:EnableRule",
          "events:PutRule",
          "events:PutTargets",
          "events:RemoveTargets",
          "events:ListTargetsByRule"
        ],
        "Resource": "arn:aws:events:*:*:rule/codecommit*"
      },
      {
        "Sid": "SNSTopicAndSubscriptionAccess",
        "Effect": "Allow",
        "Action": [
          "sns:CreateTopic",
          "sns:DeleteTopic",
          "sns:Subscribe",
          "sns:Unsubscribe",
          "sns:SetTopicAttributes"
        ],
        "Resource": "arn:aws:sns:*:*:codecommit*"
      },
      {
        "Sid": "SNSTopicAndSubscriptionReadAccess",
        "Effect": "Allow",
        "Action": [
          "sns:ListTopics",
          "sns:ListSubscriptionsByTopic",
          "sns:GetTopicAttributes"
        ],
        "Resource": "*"
      },
      {
        "Sid": "LambdaReadOnlyListAccess",
        "Effect": "Allow",
        "Action": [
          "lambda:ListFunctions"
        ],
        "Resource": "*"
      },
      {
        "Sid": "IAMReadOnlyListAccess",
        "Effect": "Allow",
        "Action": [
          "iam:ListUsers"
        ],
        "Resource": "*"
      },
      {
        "Sid": "IAMReadOnlyConsoleAccess",
        "Effect": "Allow",
        "Action": [
          "iam:ListAccessKeys",
          "iam:ListSSHPublicKeys",
          "iam:ListServiceSpecificCredentials",
          "iam:ListAccessKeys",
          "iam:GetSSHPublicKey"
        ],
        "Resource": "arn:aws:iam::*:user/${aws:username}"
      },
      {
        "Sid": "IAMUserSSHKeys",
        "Effect": "Allow",
        "Action": [
          "iam:DeleteSSHPublicKey",
          "iam:GetSSHPublicKey",
          "iam:ListSSHPublicKeys",
          "iam:UpdateSSHPublicKey",
          "iam:UploadSSHPublicKey"
        ],
        "Resource": "arn:aws:iam::*:user/${aws:username}"
      },
      {
        "Sid": "IAMSelfManageServiceSpecificCredentials",
        "Effect": "Allow",
        "Action": [
          "iam:CreateServiceSpecificCredential",
          "iam:UpdateServiceSpecificCredential",
          "iam:DeleteServiceSpecificCredential",
          "iam:ResetServiceSpecificCredential"
        ],
        "Resource": "arn:aws:iam::*:user/${aws:username}"
      }
    ]
  }
  ```
+ **AWSCodeCommitPowerUser** – Allows users access to all of the functionality of AWS CodeCommit and repository\-related resources, except it does not allow them to delete AWS CodeCommit repositories or create or delete repository\-related resources in other AWS services, such as Amazon CloudWatch Events\. We recommend that you apply this policy to most users\.

  The AWSCodeCommitPowerUser policy contains the following policy statement:

  ```
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": [
          "codecommit:BatchGet*",
          "codecommit:Get*",
          "codecommit:List*",
          "codecommit:Create*",
          "codecommit:DeleteBranch",
          "codecommit:Describe*",
          "codecommit:Put*",
          "codecommit:Post*",
          "codecommit:Merge*",
          "codecommit:Test*",
          "codecommit:Update*",
          "codecommit:GitPull",
          "codecommit:GitPush"
        ],
        "Resource": "*"
      },
      {
        "Sid": "CloudWatchEventsCodeCommitRulesAccess",
        "Effect": "Allow",
        "Action": [
          "events:DeleteRule",
          "events:DescribeRule",
          "events:DisableRule",
          "events:EnableRule",
          "events:PutRule",
          "events:PutTargets",
          "events:RemoveTargets",
          "events:ListTargetsByRule"
        ],
        "Resource": "arn:aws:events:*:*:rule/codecommit*"
      },
      {
        "Sid": "SNSTopicAndSubscriptionAccess",
        "Effect": "Allow",
        "Action": [
          "sns:Subscribe",
          "sns:Unsubscribe"
        ],
        "Resource": "arn:aws:sns:*:*:codecommit*"
      },
      {
        "Sid": "SNSTopicAndSubscriptionReadAccess",
        "Effect": "Allow",
        "Action": [
          "sns:ListTopics",
          "sns:ListSubscriptionsByTopic",
          "sns:GetTopicAttributes"
        ],
        "Resource": "*"
      },
      {
        "Sid": "LambdaReadOnlyListAccess",
        "Effect": "Allow",
        "Action": [
          "lambda:ListFunctions"
        ],
        "Resource": "*"
      },
      {
        "Sid": "IAMReadOnlyListAccess",
        "Effect": "Allow",
        "Action": [
          "iam:ListUsers"
        ],
        "Resource": "*"
      },
      {
        "Sid": "IAMReadOnlyConsoleAccess",
        "Effect": "Allow",
        "Action": [
          "iam:ListAccessKeys",
          "iam:ListSSHPublicKeys",
          "iam:ListServiceSpecificCredentials",
          "iam:ListAccessKeys",
          "iam:GetSSHPublicKey"
        ],
        "Resource": "arn:aws:iam::*:user/${aws:username}"
      },
      {
        "Sid": "IAMUserSSHKeys",
        "Effect": "Allow",
        "Action": [
          "iam:DeleteSSHPublicKey",
          "iam:GetSSHPublicKey",
          "iam:ListSSHPublicKeys",
          "iam:UpdateSSHPublicKey",
          "iam:UploadSSHPublicKey"
        ],
        "Resource": "arn:aws:iam::*:user/${aws:username}"
      },
      {
        "Sid": "IAMSelfManageServiceSpecificCredentials",
        "Effect": "Allow",
        "Action": [
          "iam:CreateServiceSpecificCredential",
          "iam:UpdateServiceSpecificCredential",
          "iam:DeleteServiceSpecificCredential",
          "iam:ResetServiceSpecificCredential"
        ],
        "Resource": "arn:aws:iam::*:user/${aws:username}"
      }
    ]
  }
  ```
+ **AWSCodeCommitReadOnly** – Grants read\-only access to AWS CodeCommit and repository\-related resources in other AWS services, as well as the ability to create and manage their own AWS CodeCommit\-related resources \(such as Git credentials and SSH keys for their IAM user to use when accessing repositories\)\. Apply this policy to users to whom you want to grant the ability to read the contents of a repository, but not make any changes to its contents\.

  The AWSCodeCommitReadOnly policy contains the following policy statement:

  ```
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": [
          "codecommit:BatchGet*",
          "codecommit:Get*",
          "codecommit:Describe*",
          "codecommit:List*",
         "codecommit:GitPull"
        ],
        "Resource": "*"
      },
      {
        "Sid": "CloudWatchEventsCodeCommitRulesReadOnlyAccess",
        "Effect": "Allow",
        "Action": [
          "events:DescribeRule",
          "events:ListTargetsByRule"
        ],
        "Resource": "arn:aws:events:*:*:rule/codecommit*"
      },
      {
        "Sid": "SNSSubscriptionAccess",
        "Effect": "Allow",
        "Action": [
          "sns:ListTopics",
          "sns:ListSubscriptionsByTopic",
          "sns:GetTopicAttributes"
        ],
        "Resource": "*"
      },
      {
        "Sid": "LambdaReadOnlyListAccess",
        "Effect": "Allow",
        "Action": [
          "lambda:ListFunctions"
        ],
        "Resource": "*"
      },
      {
        "Sid": "IAMReadOnlyListAccess",
        "Effect": "Allow",
       "Action": [
          "iam:ListUsers"
        ],
        "Resource": "*"
      },
      {
        "Sid": "IAMReadOnlyConsoleAccess",
        "Effect": "Allow",
        "Action": [
          "iam:ListAccessKeys",
          "iam:ListSSHPublicKeys",
          "iam:ListServiceSpecificCredentials",
          "iam:ListAccessKeys",
          "iam:GetSSHPublicKey"
        ],
        "Resource": "arn:aws:iam::*:user/${aws:username}"
      }
    ]
  }
  ```

For more information, see [AWS Managed Policies](http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\.

## Customer Managed Policy Examples<a name="customer-managed-policies"></a>

You can create your own custom IAM policies to allow permissions for AWS CodeCommit actions and resources\. You can attach these custom policies to the IAM users or groups that require those permissions\. You can also create your own custom IAM policies for integration between AWS CodeCommit and other AWS services\.

**Topics**
+ [Customer Managed Identity Policy Examples](#customer-managed-policies-identity)
+ [Customer Managed Integration Policy Examples](#integration-policy-examples)

### Customer Managed Identity Policy Examples<a name="customer-managed-policies-identity"></a>

The following example IAM policies grant permissions for various AWS CodeCommit actions\. Use them to limit AWS CodeCommit access for your IAM users and roles\. These policies control the ability to perform actions with the AWS CodeCommit console, API, AWS SDKs, or the AWS CLI\.

**Note**  
All examples use the US West \(Oregon\) Region \(us\-west\-2\) and contain fictitious account IDs\.

 **Examples**
+ [Example 1: Allow a User to Perform AWS CodeCommit Operations in a Single Region](#identity-based-policies-example-1)
+ [Example 2: Allow a User to Use Git for a Single Repository](#identity-based-policies-example-2)
+ [Example 3: Allow a User Connecting From a Specific IP Address Range Access to a Repository ](#identity-based-policies-example-3)

#### Example 1: Allow a User to Perform AWS CodeCommit Operations in a Single Region<a name="identity-based-policies-example-1"></a>

The following permissions policy uses a wildcard character \(`"codecommit:*"`\) to allow users to perform all AWS CodeCommit actions in the us\-east\-2 Region\.

```
{
  "Version": "2012-10-17",
  "Statement" : [
    {
      "Effect" : "Allow",
      "Action" : [
        "codecommit:*"
      ],
      "Resource" : "arn:aws:codecommit:us-east-2:*"
    }
  ]
}
```

#### Example 2: Allow a User to Use Git for a Single Repository<a name="identity-based-policies-example-2"></a>

In AWS CodeCommit, the `GitPull` IAM policy permissions apply to any Git client command where data is retrieved from AWS CodeCommit, including git fetch, git clone, and so on\. Similarly, the `GitPush` IAM policy permissions apply to any Git client command where data is sent to AWS CodeCommit\. For example, if the `GitPush` IAM policy permission is set to `Allow`, a user can push the deletion of a branch using the Git protocol\. That push is unaffected by any permissions applied to the `DeleteBranch` operation for that IAM user\. The `DeleteBranch` permission applies to actions performed with the console, the AWS CLI, the SDKs, and the API, but not the Git protocol\. 

The following example allows the specified user to pull from, and push to, the AWS CodeCommit repository named `MyDemoRepo`:

```
{
  "Version": "2012-10-17",
  "Statement" : [
    {
      "Effect" : "Allow",
      "Action" : [
        "codecommit:GitPull",
        "codecommit:GitPush"
      ],
      "Resource" : "arn:aws:codecommit:us-east-2:111111111111:MyDemoRepo"
    }
  ]
}
```

#### Example 3: Allow a User Connecting From a Specific IP Address Range Access to a Repository<a name="identity-based-policies-example-3"></a>

You can create a policy that only allows users to connect to an AWS CodeCommit repository if their IP address is within a certain IP address range\. There are two equally valid approaches to this\. You can create a Deny policy that disallows AWS CodeCommit operations if the IP address for the user is not within a specific block, or you can create an Allow policy that allows AWS CodeCommit operations if the IP address for the user is within a specific block\.

You can create a `Deny` policy that denies access to all users who are not within a certain IP range\. For example, you could attach the AWSCodeCommitPowerUser managed policy and a customer\-managed policy to all users who require access to your repository\. The following example policy denies all AWS CodeCommit permissions to users whose IP addresses are not within the specified IP address block of 203\.0\.113\.0/16:

```
{
   "Version": "2012-10-17",
   "Statement": [
      {
         "Effect": "Deny",
         "Action": [
            "codecommit:*"
         ],
         "Resource": "*",
         "Condition": {
            "NotIpAddress": {
               "aws:SourceIp": [
                  "203.0.113.0/16"
               ]
            }
         }
      }
   ]
}
```

The following example policy allows the specified user to access an AWS CodeCommit repository named MyDemoRepo with the equivalent permissions of the AWSCodeCommitPowerUser managed policy only if their IP address is within the specified address block of 203\.0\.113\.0/16:

```
{
   "Version": "2012-10-17",
   "Statement": [
      {
         "Effect": "Allow",
         "Action": [
            "codecommit:BatchGetRepositories",
            "codecommit:CreateBranch",
            "codecommit:CreateRepository",
            "codecommit:Get*",
            "codecommit:GitPull",
            "codecommit:GitPush",
            "codecommit:List*",
            "codecommit:Put*",
            "codecommit:Test*",
            "codecommit:Update*"
         ],
         "Resource": "arn:aws:codecommit:us-east-2:111111111111:MyDemoRepo",
         "Condition": {
            "IpAddress": {
               "aws:SourceIp": [
                  "203.0.113.0/16"
               ]
            }
         }
      }
   ]
}
```

### Customer Managed Integration Policy Examples<a name="integration-policy-examples"></a>

This section provides example customer\-managed user policies that grant permissions for integrations between AWS CodeCommit and other AWS services\. For specific examples of policies that allow cross\-account access to an AWS CodeCommit repository, see [Configure Cross\-Account Access to an AWS CodeCommit Repository](cross-account.md)\.

**Note**  
All examples use the US West \(Oregon\) Region \(us\-west\-2\) when a region is required, and contain fictitious account IDs\.

 **Examples**
+ [Example 1: Create a Policy That Enables Cross\-Account Access to an Amazon SNS Topic](#access-permissions-sns-int)
+ [Example 2: Create a Policy for AWS Lambda Integration](#access-permissions-lambda-int)

#### Example 1: Create a Policy That Enables Cross\-Account Access to an Amazon SNS Topic<a name="access-permissions-sns-int"></a>

You can configure an AWS CodeCommit repository so that code pushes or other events trigger actions, such as sending a notification from Amazon Simple Notification Service \(Amazon SNS\)\. If you create the Amazon SNS topic with the same account used to create the AWS CodeCommit repository, you do not need to configure additional IAM policies or permissions\. You can create the topic, and then create the trigger for the repository\. For more information, see [Create a Trigger for an Amazon SNS Topic](how-to-notify-sns.md)\.

However, if you want to configure your trigger to use an Amazon SNS topic in another AWS account, you must first configure that topic with a policy that allows AWS CodeCommit to publish to that topic\. From that other account, open the Amazon SNS console, choose the topic from the list, and for **Other topic actions**, choose **Edit topic policy**\. On the **Advanced** tab, modify the policy for the topic to allow AWS CodeCommit to publish to that topic\. For example, if the policy is the default policy, you would modify the policy as follows, changing the items in *red italic text* to match the values for your repository, Amazon SNS topic, and account:

```
{
  "Version": "2008-10-17",
  "Id": "__default_policy_ID",
  "Statement": [
    {
      "Sid": "__default_statement_ID",
      "Effect": "Allow",
      "Principal": {
        "AWS": "*"
      },
      "Action": [
        "SNS:Subscribe",
        "SNS:ListSubscriptionsByTopic",
        "SNS:DeleteTopic",
        "SNS:GetTopicAttributes",
        "SNS:Publish",
        "SNS:RemovePermission",
        "SNS:AddPermission",
        "SNS:Receive",
        "SNS:SetTopicAttributes"
      ],
      "Resource": "arn:aws:sns:us-east-2:111111111111:NotMySNSTopic",
      "Condition": {
        "StringEquals": {
          "AWS:SourceOwner": "111111111111"
        }
      }
     },
     {
      "Sid": "CodeCommit-Policy_ID",
      "Effect": "Allow",
      "Principal": {
        "Service": "codecommit.amazonaws.com"
      },
      "Action": "SNS:Publish",
      "Resource": "arn:aws:sns:us-east-2:111111111111:NotMySNSTopic",
      "Condition": {
        "StringEquals": {
          "AWS:SourceArn": "arn:aws:codecommit:us-east-2:80398EXAMPLE:MyDemoRepo",
          "AWS:SourceAccount": "80398EXAMPLE"
        }
      }
    }
  ]
}
```

#### Example 2: Create a Policy for AWS Lambda Integration<a name="access-permissions-lambda-int"></a>

You can configure an AWS CodeCommit repository so that code pushes or other events trigger actions, such as invoking a function in AWS Lambda\. For more information, see [Create a Trigger for a Lambda Function](how-to-notify-lambda.md)\.

If you want your trigger to run a Lambda function directly \(instead of using an Amazon SNS topic to invoke the Lambda function\), and you do not configure the trigger in the Lambda console, you must include a policy similar to the following in the function's resource policy:

```
{
  "Statement":{
     "StatementId":"Id-1",
     "Action":"lambda:InvokeFunction",
     "Principal":"codecommit.amazonaws.com",
     "SourceArn":"arn:aws:codecommit:us-east-2:80398EXAMPLE:MyDemoRepo",
     "SourceAccount":"80398EXAMPLE"
  }
}
```

When manually configuring an AWS CodeCommit trigger that invokes a Lambda function, you must also use the Lambda [AddPermission](http://docs.aws.amazon.com/lambda/latest/dg/API_AddPermission.html) command to grant permission for AWS CodeCommit to invoke the function\. For an example, see the [To allow AWS CodeCommit to run a Lambda function](how-to-notify-lambda-cc.md#how-to-notify-lambda-create-function-perm) section of [Create a Trigger for an Existing Lambda Function](how-to-notify-lambda-cc.md)\. 

For more information about resource policies for Lambda functions, see [AddPermission](http://docs.aws.amazon.com/lambda/latest/dg/API_AddPermission.html) and [The Pull/Push Event Models](http://docs.aws.amazon.com/lambda/latest/dg/intro-invocation-modes.html) in the *AWS Lambda Developer Guide*\.