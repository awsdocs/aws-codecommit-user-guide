# Infrastructure security in AWS CodeCommit<a name="infrastructure-security"></a>

As a managed service, AWS CodeCommit is protected by the AWS global network security procedures that are described in the [Amazon Web Services: Overview of Security Processes](https://d0.awsstatic.com/whitepapers/Security/AWS_Security_Whitepaper.pdf) whitepaper\.

You use AWS published API calls to access CodeCommit through the network\. Clients must support Transport Layer Security \(TLS\) 1\.0 or later\. We recommend TLS 1\.2 or later\. Clients must also support cipher suites with perfect forward secrecy \(PFS\) such as Ephemeral Diffie\-Hellman \(DHE\) or Elliptic Curve Ephemeral Diffie\-Hellman \(ECDHE\)\. Most modern systems such as Java 7 and later support these modes\.

Requests must be signed by using an access key ID and a secret access key that is associated with an IAM principal\. Or you can use the [AWS Security Token Service](https://docs.aws.amazon.com/STS/latest/APIReference/Welcome.html) \(AWS STS\) to generate temporary security credentials to sign requests\.

You can call these API operations from any network location, but CodeCommit does support restrictions based on the source IP address\. You can also use CodeCommit policies to control access from specific Amazon Virtual Private Cloud \(Amazon VPC\) endpoints or specific VPCs\. Effectively, this isolates network access to a given CodeCommit resource from only the specific VPC in the AWS network\.

For more information, see the following:
+ [Example 1: Allow a user to perform CodeCommit operations in a single AWS Region](auth-and-access-control-iam-identity-based-access-control.md#identity-based-policies-example-1)
+ [Example 3: Allow a user connecting from a specified IP address range access to a repository ](auth-and-access-control-iam-identity-based-access-control.md#identity-based-policies-example-3)
+ [Using AWS CodeCommit with interface VPC endpoints](codecommit-and-interface-VPC.md)