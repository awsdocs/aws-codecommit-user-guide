# Customer managed policy examples<a name="customer-managed-policies"></a>

You can create your own custom IAM policies to allow permissions for CodeCommit actions and resources\. You can attach these custom policies to the IAM users or groups that require those permissions\. You can also create your own custom IAM policies for integration between CodeCommit and other AWS services\.

**Topics**
+ [Customer managed identity policy examples](#customer-managed-policies-identity)
+ [Customer managed integration policy examples](#integration-policy-examples)

## Customer managed identity policy examples<a name="customer-managed-policies-identity"></a>

The following example IAM policies grant permissions for various CodeCommit actions\. Use them to limit CodeCommit access for your IAM users and roles\. These policies control the ability to perform actions with the CodeCommit console, API, AWS SDKs, or the AWS CLI\.



**Note**  
All examples use the US West \(Oregon\) Region \(us\-west\-2\) and contain fictitious account IDs\.

 **Examples**
+ [Example 1: Allow a user to perform CodeCommit operations in a single AWS Region](#identity-based-policies-example-1)
+ [Example 2: Allow a user to use Git for a single repository](#identity-based-policies-example-2)
+ [Example 3: Allow a user connecting from a specified IP address range access to a repository ](#identity-based-policies-example-3)
+ [Example 4: Deny or allow actions on branches](#identity-based-policies-example-4)
+ [Example 5: Deny or allow actions on repositories with tags](#identity-based-policies-example-5)

### Example 1: Allow a user to perform CodeCommit operations in a single AWS Region<a name="identity-based-policies-example-1"></a>

The following permissions policy uses a wildcard character \(`"codecommit:*"`\) to allow users to perform all CodeCommit actions in the us\-east\-2 Region and not from other AWS Regions\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "codecommit:*",
            "Resource": "arn:aws:codecommit:us-east-2:111111111111:*",
            "Condition": {
                "StringEquals": {
                    "aws:RequestedRegion": "us-east-2"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": "codecommit:ListRepositories",
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "aws:RequestedRegion": "us-east-2"
                }
            }
        }
    ]
}
```

### Example 2: Allow a user to use Git for a single repository<a name="identity-based-policies-example-2"></a>

In CodeCommit, the `GitPull` IAM policy permissions apply to any Git client command where data is retrieved from CodeCommit, including git fetch, git clone, and so on\. Similarly, the `GitPush` IAM policy permissions apply to any Git client command where data is sent to CodeCommit\. For example, if the `GitPush` IAM policy permission is set to `Allow`, a user can push the deletion of a branch using the Git protocol\. That push is unaffected by any permissions applied to the `DeleteBranch` operation for that IAM user\. The `DeleteBranch` permission applies to actions performed with the console, the AWS CLI, the SDKs, and the API, but not the Git protocol\. 

The following example allows the specified user to pull from, and push to, the CodeCommit repository named `MyDemoRepo`:

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

### Example 3: Allow a user connecting from a specified IP address range access to a repository<a name="identity-based-policies-example-3"></a>

You can create a policy that only allows users to connect to a CodeCommit repository if their IP address is within a certain IP address range\. There are two equally valid approaches to this\. You can create a `Deny` policy that disallows CodeCommit operations if the IP address for the user is not within a specific block, or you can create an `Allow` policy that allows CodeCommit operations if the IP address for the user is within a specific block\.

You can create a `Deny` policy that denies access to all users who are not within a certain IP range\. For example, you could attach the AWSCodeCommitPowerUser managed policy and a customer\-managed policy to all users who require access to your repository\. The following example policy denies all CodeCommit permissions to users whose IP addresses are not within the specified IP address block of 203\.0\.113\.0/16:

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

The following example policy allows the specified user to access a CodeCommit repository named MyDemoRepo with the equivalent permissions of the AWSCodeCommitPowerUser managed policy only if their IP address is within the specified address block of 203\.0\.113\.0/16:

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
            "codecommit:Post*",
            "codecommit:Merge*",
            "codecommit:TagResource",
            "codecommit:Test*",
            "codecommit:UntagResource",
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



### Example 4: Deny or allow actions on branches<a name="identity-based-policies-example-4"></a>

You can create a policy that denies users permissions to actions you specify on one or more branches\. Alternatively, you can create a policy that allows actions on one or more branches that they might not otherwise have in other branches of a repository\. You can use these policies with the appropriate managed \(predefined\) policies\. For more information, see [Limit pushes and merges to branches in AWS CodeCommit](how-to-conditional-branch.md)\.

For example, you can create a `Deny` policy that denies users the ability to make changes to a branch named main, including deleting that branch, in a repository named *MyDemoRepo*\. You can use this policy with the **AWSCodeCommitPowerUser** managed policy\. Users with these two policies applied would be able to create and delete branches, create pull requests, and all other actions as allowed by **AWSCodeCommitPowerUser**, but they would not be able to push changes to the branch named *main*, add or edit a file in the *main* branch in the CodeCommit console, or merge branches or a pull request into the *main* branch\. Because `Deny` is applied to `GitPush`, you must include a `Null` statement in the policy, to allow initial `GitPush` calls to be analyzed for validity when users make pushes from their local repos\.

**Tip**  
If you want to create a policy that applies to all branches named *main* in all repositories in your Amazon Web Services account, for `Resource`, specify an asterisk \( `*` \) instead of a repository ARN\.

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
                "codecommit:Merge*"
            ],
            "Resource": "arn:aws:codecommit:us-east-2:111111111111:MyDemoRepo",
            "Condition": {
                "StringEqualsIfExists": {
                    "codecommit:References": [
                        "refs/heads/main"   
                    ]
                },
                "Null": {
                    "codecommit:References": "false"
                }
            }
        }
    ]
}
```

The following example policy allows a user to make changes to a branch named main in all repositories in an Amazon Web Services account\. It does not allow changes to any other branches\. You might use this policy with the AWSCodeCommitReadOnly managed policy to allow automated pushes to the repository in the main branch\. Because the Effect is `Allow`, this example policy would not work with managed policies such as AWSCodeCommitPowerUser\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "codecommit:GitPush",
                "codecommit:Merge*"
            ],
            "Resource": "*",
            "Condition": {
                "StringEqualsIfExists": {
                    "codecommit:References": [
                        "refs/heads/main"
                    ]
                }
            }
        }
    ]
}
```



### Example 5: Deny or allow actions on repositories with tags<a name="identity-based-policies-example-5"></a>

You can create a policy that allows or denies actions on repositories based on the AWS tags associated with those repositories, and then apply those policies to the IAM groups you configure for managing IAM users\. For example, you can create a policy that denies all CodeCommit actions on any repositories with the AWS tag key *Status* and the key value of *Secret*, and then apply that policy to the IAM group you created for general developers \(*Developers*\)\. You then need to make sure that the developers working on those tagged repositories are not members of that general *Developers* group, but belong instead to a different IAM group that does not have the restrictive policy applied \(*SecretDevelopers*\)\.

The following example denies all CodeCommit actions on repositories tagged with the key *Status* and the key value of *Secret*:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": "codecommit:*",
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "aws:ResourceTag/Status": "Secret"
        }
      }
    }
  ]
}
```

You can further refine this strategy by specifying specific repositories, rather than all repositories, as resources\. You can also create policies that allow CodeCommit actions on all repositories that are not tagged with specific tags\. For example, the following policy allows the equivalent of AWSCodeCommitPowerUser permissions for all repositories except those tagged with the specified tags:

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
            "codecommit:TagResource",
            "codecommit:Test*",
            "codecommit:UntagResource",
            "codecommit:Update*"
         ],
         "Resource": "*",
         "Condition": {
            "StringNotEquals": {
               "aws:ResourceTag/Status": "Secret",
               "aws:ResourceTag/Team": "Saanvi"
            }
         }
      }
   ]
}
```

## Customer managed integration policy examples<a name="integration-policy-examples"></a>

This section provides example customer\-managed user policies that grant permissions for integrations between CodeCommit and other AWS services\. For specific examples of policies that allow cross\-account access to a CodeCommit repository, see [Configure cross\-account access to an AWS CodeCommit repository using roles](cross-account.md)\.

**Note**  
All examples use the US West \(Oregon\) Region \(us\-west\-2\) when an AWS Region is required, and contain fictitious account IDs\.

 **Examples**
+ [Example 1: Create a policy that enables cross\-account access to an Amazon SNS topic](#access-permissions-sns-int)
+ [Example 2: Create an Amazon Simple Notification Service \(Amazon SNS\) topic policy to allow Amazon CloudWatch Events to publish CodeCommit events to the topic ](#access-permissions-SNS-CWE)
+ [Example 3: Create a policy for AWS Lambda integration with a CodeCommit trigger](#access-permissions-lambda-int)

### Example 1: Create a policy that enables cross\-account access to an Amazon SNS topic<a name="access-permissions-sns-int"></a>

You can configure a CodeCommit repository so that code pushes or other events trigger actions, such as sending a notification from Amazon Simple Notification Service \(Amazon SNS\)\. If you create the Amazon SNS topic with the same account used to create the CodeCommit repository, you do not need to configure additional IAM policies or permissions\. You can create the topic, and then create the trigger for the repository\. For more information, see [Create a trigger for an Amazon SNS topic](how-to-notify-sns.md)\.

However, if you want to configure your trigger to use an Amazon SNS topic in another Amazon Web Services account, you must first configure that topic with a policy that allows CodeCommit to publish to that topic\. From that other account, open the Amazon SNS console, choose the topic from the list, and for **Other topic actions**, choose **Edit topic policy**\. On the **Advanced** tab, modify the policy for the topic to allow CodeCommit to publish to that topic\. For example, if the policy is the default policy, you would modify the policy as follows, changing the items in *red italic text* to match the values for your repository, Amazon SNS topic, and account:

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
        "sns:Subscribe",
        "sns:ListSubscriptionsByTopic",
        "sns:DeleteTopic",
        "sns:GetTopicAttributes",
        "sns:Publish",
        "sns:RemovePermission",
        "sns:AddPermission",
        "sns:Receive",
        "sns:SetTopicAttributes"
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
      "Action": "sns:Publish",
      "Resource": "arn:aws:sns:us-east-2:111111111111:NotMySNSTopic",
      "Condition": {
        "StringEquals": {
          "AWS:SourceArn": "arn:aws:codecommit:us-east-2:111111111111:MyDemoRepo",
          "AWS:SourceAccount": "111111111111"
        }
      }
    }
  ]
}
```

### Example 2: Create an Amazon Simple Notification Service \(Amazon SNS\) topic policy to allow Amazon CloudWatch Events to publish CodeCommit events to the topic<a name="access-permissions-SNS-CWE"></a>

You can configure CloudWatch Events to publish to an Amazon SNS topic when events occur, including CodeCommit events\. To do so, you must make sure that CloudWatch Events has permission to publish events to your Amazon SNS topic by creating a policy for the topic or modifying an existing policy for the topic similar to the following:

```
{
  Version":"2012-10-17",
  "Id":"__default_policy_ID",
  "Statement":[
    {
      "Sid":"__default_statement_ID",
      "Effect":"Allow",
      "Principal":"{"AWS":"*"},
      "Action":
        "sns:Publish"
      ]
      "Resource":"arn:aws:sns:us-east-2:123456789012:MyTopic",
      "Condition":{
        "StringEquals":{"AWS:SourceOwner":123456789012"}
      }
    },                    
    {
      "Sid":"Allow_Publish_Events",
      "Effect":"Allow",
      "Principal":{"Service":"events.amazonaws.com"},
      "Action":"sns:Publish",
      "Resource":"arn:aws:sns:us-east-2:123456789012:MyTopic"
    }
  ]
}
```

For more information about CodeCommit and CloudWatch Events, see [CloudWatch Events Event Examples From Supported Services](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/EventTypes.html#codecommit_event_type)\. For more information about IAM and policy language, see [Grammar of the IAM JSON Policy Language](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_grammar.html)\.

### Example 3: Create a policy for AWS Lambda integration with a CodeCommit trigger<a name="access-permissions-lambda-int"></a>

You can configure a CodeCommit repository so that code pushes or other events trigger actions, such as invoking a function in AWS Lambda\. For more information, see [Create a trigger for a Lambda function](how-to-notify-lambda.md)\. This information is specific to triggers, and not CloudWatch Events\.

If you want your trigger to run a Lambda function directly \(instead of using an Amazon SNS topic to invoke the Lambda function\), and you do not configure the trigger in the Lambda console, you must include a policy similar to the following in the function's resource policy:

```
{
  "Statement":{
     "StatementId":"Id-1",
     "Action":"lambda:InvokeFunction",
     "Principal":"codecommit.amazonaws.com",
     "SourceArn":"arn:aws:codecommit:us-east-2:111111111111:MyDemoRepo",
     "SourceAccount":"111111111111"
  }
}
```

When manually configuring a CodeCommit trigger that invokes a Lambda function, you must also use the Lambda [AddPermission](https://docs.aws.amazon.com/lambda/latest/dg/API_AddPermission.html) command to grant permission for CodeCommit to invoke the function\. For an example, see the [To allow CodeCommit to run a Lambda function](how-to-notify-lambda-cc.md#how-to-notify-lambda-create-function-perm) section of [Create a trigger for an existing Lambda function](how-to-notify-lambda-cc.md)\. 

For more information about resource policies for Lambda functions, see [AddPermission](https://docs.aws.amazon.com/lambda/latest/dg/API_AddPermission.html) and [The Pull/Push Event Models](https://docs.aws.amazon.com/lambda/latest/dg/intro-invocation-modes.html) in the *AWS Lambda Developer Guide*\.