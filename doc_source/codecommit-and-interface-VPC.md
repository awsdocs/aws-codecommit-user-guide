# Using AWS CodeCommit with interface VPC endpoints<a name="codecommit-and-interface-VPC"></a>

If you use Amazon Virtual Private Cloud \(Amazon VPC\) to host your AWS resources, you can establish a private connection between your VPC and CodeCommit\. You can use this connection to enable CodeCommit to communicate with your resources on your VPC without going through the public internet\.

Amazon VPC is an AWS service that you can use to launch AWS resources in a virtual network that you define\. With a VPC, you have control over your network settings, such the IP address range, subnets, route tables, and network gateways\. With VPC endpoints, the routing between the VPC and AWS services is handled by the AWS network, and you can use IAM policies to control access to service resources\.

To connect your VPC to CodeCommit, you define an *interface VPC endpoint* for CodeCommit\. An interface endpoint is an elastic network interface with a private IP address that serves as an entry point for traffic destined to a supported AWS service\. The endpoint provides reliable, scalable connectivity to CodeCommit without requiring an internet gateway, network address translation \(NAT\) instance, or VPN connection\. For more information, see [What Is Amazon VPC](https://docs.aws.amazon.com/vpc/latest/userguide/) in the *Amazon VPC User Guide*\.

**Note**  
Other AWS services that provide VPC support and integrate with CodeCommit, such as AWS CodePipeline, might not support using Amazon VPC endpoints for that integration\. For example, traffic between CodePipeline and CodeCommit cannot be restricted to the VPC subnet range\. 

 Interface VPC endpoints are powered by AWS PrivateLink, an AWS technology that enables private communication between AWS services using an elastic network interface with private IP addresses\. For more information, see [AWS PrivateLink](https://aws.amazon.com/privatelink/)\.

The following steps are for users of Amazon VPC\. For more information, see [Getting Started](https://docs.aws.amazon.com/vpc/latest/userguide/GetStarted.html) in the *Amazon VPC User Guide*\.

## Availability<a name="codecommit-interface-VPC-availability"></a>

CodeCommit currently supports VPC endpoints in the following AWS Regions:
+ US East \(Ohio\) 
+ US East \(N\. Virginia\)
+ US West \(N\. California\)
+ US West \(Oregon\)
+ Europe \(Ireland\)
+ Europe \(London\)
+ Europe \(Paris\)
+ Europe \(Frankfurt\)
+ Europe \(Stockholm\)
+ Asia Pacific \(Tokyo\)
+ Asia Pacific \(Singapore\)
+ Asia Pacific \(Sydney\)
+ Asia Pacific \(Seoul\)
+ Asia Pacific \(Mumbai\)
+ Asia Pacific \(Hong Kong\)
+ South America \(São Paulo\)
+ Middle East \(Bahrain\)
+ Canada \(Central\)
+ China \(Beijing\)
+ China \(Ningxia\)
+ AWS GovCloud \(US\-West\)
+ AWS GovCloud \(US\-East\)

## Create VPC endpoints for CodeCommit<a name="create-vpc-endpoint-for-codecommit"></a>

To start using CodeCommit with your VPC, create an interface VPC endpoint for CodeCommit\. CodeCommit requires separate endpoints for Git operations and for CodeCommit API operations\. Depending on your business needs, you might need to create more than one VPC endpoint\. When you create a VPC endpoint for CodeCommit, choose **AWS Services**, and in **Service Name**, choose from the following options:
+ **com\.amazonaws\.*region*\.git\-codecommit**: Choose this option if you want to create a VPC endpoint for Git operations with CodeCommit repositories\. For example, choose this option if your users use a Git client and commands such as `git pull`, `git commit`, and `git push` when they interact with CodeCommit repositories\.
+ **com\.amazonaws\.*region*\.git\-codecommit\-fips**: Choose this option if you want to create a VPC endpoint for Git operations with CodeCommit repositories that complies with the Federal Information Processing Standard \(FIPS\) Publication 140\-2 US government standard\.
+ **com\.amazonaws\.*region*\.codecommit**: Choose this option if you want to create a VPC endpoint for CodeCommit API operations\. For example, choose this option if your users use the AWS CLI, the CodeCommit API, or the AWS SDKs to interact with CodeCommit for operations such as `CreateRepository`, `ListRepositories`, and `PutFile`\.
+ **com\.amazonaws\.*region*\.codecommit\-fips**: Choose this option if you want to create a VPC endpoint for CodeCommit API operations that complies with the Federal Information Processing Standard \(FIPS\) Publication 140\-2 US government standard\.

## Create a VPC endpoint policy for CodeCommit<a name="create-vpc-endpoint-policy-for-codecommit"></a>

You can create a policy for Amazon VPC endpoints for CodeCommit in which you can specify:
+ The principal that can perform actions\.
+ The actions that can be performed\.
+ The resources that can have actions performed on them\.

For example, a company might want to restrict access to repositories to the network address range for a VPC\. You can view an example of this kind of policy here: [Example 3: Allow a user connecting from a specified IP address range access to a repository ](auth-and-access-control-iam-identity-based-access-control.md#identity-based-policies-example-3)\. The company configured two Git VPC endpoints for the US East \(Ohio\) region: `com.amazonaws.us-east-2.codecommit` and `com-amazonaws.us-east-2.git-codecommit-fips`\. They want to allow code pushes to a CodeCommit repository named *MyDemoRepo* only on the FIPS\-compliant endpoint only\. To enforce this, they would configure a policy similar to the following on the `com.amazonaws.us-east-2.codecommit` endpoint that specifically denies Git push actions:

```
{
    "Statement": [
        {
            "Action": "*",
            "Effect": "Allow",
            "Resource": "*",
            "Principal": "*"
        },
        {
            "Action": "codecommit:GitPush",
            "Effect": "Deny",
            "Resource": "arn:aws:codecommit:us-west-2:123456789012:MyDemoRepo",
            "Principal": "*"
        }
    ]
}
```

For more information, see [Creating an Interface Endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#create-interface-endpoint.html) in the *Amazon VPC User Guide*\.