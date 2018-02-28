# Product and Service Integrations with AWS CodeCommit<a name="integrations"></a>

By default, AWS CodeCommit is integrated with a number of AWS services\. You can also use AWS CodeCommit with products and services outside of AWS\. The following information can help you configure AWS CodeCommit to integrate with the products and services you use\.

**Note**  
You can automatically build and deploy commits to an AWS CodeCommit repository by integrating with AWS CodePipeline\. To learn more, follow the steps in the [AWS for DevOps Getting Started Guide](http://docs.aws.amazon.com/devops/latest/gsg/welcome.html)\. 


+ [Integration with Other AWS Services](#integrations-aws)
+ [Integration Examples from the Community](#integrations-community)

## Integration with Other AWS Services<a name="integrations-aws"></a>

AWS CodeCommit is integrated with the following AWS services:


|  |  | 
| --- |--- |
| AWS Cloud9 |  [AWS Cloud9](http://docs.aws.amazon.com/AmazonCloudWatch/latest/events/) contains a collection of tools that you use to code, build, run, test, debug, and release software in the cloud\. This collection of tools is referred to as the AWS Cloud9 integrated development environment, or IDE\.  You access the AWS Cloud9 IDE through a web browser\. The IDE offers a rich code\-editing experience with support for several programming languages and runtime debuggers, as well as a built\-in terminal\.  Learn more: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codecommit/latest/userguide/integrations.html)  | 
| Amazon CloudWatch Events |  [CloudWatch Events](http://docs.aws.amazon.com/AmazonCloudWatch/latest/events/) delivers a near real\-time stream of system events that describe changes in Amazon Web Services \(AWS\) resources\. Using simple rules that you can quickly set up, you can match events and route them to one or more target functions or streams\. CloudWatch Events becomes aware of operational changes as they occur\. CloudWatch Events responds to these operational changes and takes action as necessary, by sending messages to respond to the environment, activating functions, making changes, and capturing state information\.   You can configure CloudWatch Events to monitor AWS CodeCommit repositories and respond to repository events by targeting streams, functions, tasks, or other processes in other AWS services, such as Amazon Simple Queue Service, Amazon Kinesis, AWS Lambda, and many more\. Learn more: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codecommit/latest/userguide/integrations.html)  | 
| AWS CodeStar |  [AWS CodeStar](http://docs.aws.amazon.com/codestar/latest/userguide/welcome.html) is a cloud\-based service for creating, managing, and working with software development projects on AWS\. You can quickly develop, build, and deploy applications on AWS with an AWS CodeStar project\. An AWS CodeStar project creates and integrates AWS services for your project development toolchain, including an AWS CodeCommit repository for the project\. AWS CodeStar also assigns permissions to team members for that project\. These permissions are applied automatically, including permissions for accessing AWS CodeCommit, creating and managing Git credentials, and more\.  You can configure repositories created for AWS CodeStar projects just as you would any other AWS CodeCommit repository by using the AWS CodeCommit console, AWS CodeCommit commands from the AWS CLI, the local Git client, and from the AWS CodeCommit API\.  Learn more: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codecommit/latest/userguide/integrations.html)  | 
| AWS CloudTrail |  [CloudTrail](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/) captures AWS API calls and related events made by or on behalf of an AWS account and delivers log files to an Amazon S3 bucket that you specify\. You can configure CloudTrail to capture API calls from the AWS CodeCommit console, AWS CodeCommit commands from the AWS CLI, the local Git client, and from the AWS CodeCommit API\.  Learn more: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codecommit/latest/userguide/integrations.html)  | 
| AWS CodeBuild |  [AWS CodeBuild](http://docs.aws.amazon.com/codebuild/latest/userguide/welcome.html) is a fully managed build service in the cloud that compiles your source code, runs unit tests, and produces artifacts that are ready to deploy\. You can store the source code to be built and the build specification in an AWS CodeCommit repository\. You can use AWS CodeBuild directly with AWS CodeCommit, or you can incorporate both AWS CodeBuild and AWS CodeCommit in a continuous delivery pipeline with AWS CodePipeline\. Learn more: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codecommit/latest/userguide/integrations.html)  | 
| AWS Elastic Beanstalk |  [Elastic Beanstalk](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/) is a managed service that makes it easy to deploy and manage applications in the AWS cloud without worrying about the infrastructure that runs those applications\. You can use the Elastic Beanstalk command line interface \(EB CLI\) to deploy your application directly from a new or existing AWS CodeCommit repository\. Learn more: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codecommit/latest/userguide/integrations.html)  | 
| AWS CloudFormation |  [AWS CloudFormation](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/) is a service that helps you model and set up your AWS resources so that you can spend less time managing those resources and more time focusing on your applications\. You create a template that describes resources, including an AWS CodeCommit repository, and AWS CloudFormation takes care of provisioning and configuring those resources for you\. Learn more: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codecommit/latest/userguide/integrations.html)  | 
| AWS CodePipeline |  [AWS CodePipeline](http://docs.aws.amazon.com/codepipeline/latest/userguide/) is a continuous delivery service you can use to model, visualize, and automate the steps required to release your software\. You can configure AWS CodePipeline to use an AWS CodeCommit repository as a source action in a pipeline, and automate building, testing, and deploying your changes\. Learn more: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codecommit/latest/userguide/integrations.html)  | 
| AWS Key Management Service |  [AWS KMS](http://docs.aws.amazon.com/kms/latest/developerguide/) is a managed service that makes it easy for you to create and control the encryption keys used to encrypt your data\. By default, AWS CodeCommit uses AWS KMS to encrypt repositories\.  Learn more: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codecommit/latest/userguide/integrations.html)  | 
| AWS Lambda |  [Lambda](http://docs.aws.amazon.com/lambda/latest/dg/) lets you run code without provisioning or managing servers\. You can configure triggers for AWS CodeCommit repositories that will invoke Lambda functions in response to repository events\.  Learn more: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codecommit/latest/userguide/integrations.html)  | 
| Amazon Simple Notification Service |  [Amazon SNS](http://docs.aws.amazon.com/sns/latest/dg/) is a web service that enables applications, end users, and devices to instantly send and receive notifications from the cloud\. You can configure triggers for AWS CodeCommit repositories that will send Amazon SNS notifications in response to repository events\. You can also use Amazon SNS notifications to integrate with other AWS services\. For example, you can use an Amazon SNS notification to send messages to an Amazon Simple Queue Service queue\. Learn more: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codecommit/latest/userguide/integrations.html)  | 

## Integration Examples from the Community<a name="integrations-community"></a>

The following sections provide links to blog posts, articles, and community\-provided examples\.

**Note**  
These links are provided for informational purposes only, and should not be considered either a comprehensive list or an endorsement of the content of the examples\. AWS is not responsible for the content or accuracy of external content\.


+ [Blog Posts](#integrations-community-blogposts)
+ [Code Samples](#integrations-community-code)

### Blog Posts<a name="integrations-community-blogposts"></a>

+ *[Build Serverless AWS CodeCommit Workflows using Amazon CloudWatch Events and JGit](https://aws.amazon.com/blogs/devops/build-serverless-aws-codecommit-workflows-using-amazon-cloudwatch-events-and-jgit/)*

  Learn how to create CloudWatch Events rules that process changes in a repository using AWS CodeCommit repository events and target actions in other AWS services\. Examples include AWS Lambda functions that enforce Git commit message policies on commits, replicate an AWS CodeCommit repository, and backing up an AWS CodeCommit repository to Amazon S3\.

  Published August 3, 2017

+ *[Replicating and Automating Sync\-Ups for a Repository with AWS CodeCommit](https://aws.amazon.com/blogs/devops/replicating-and-automating-sync-ups-for-a-repository-with-aws-codecommit/)*

  Learn how to back up or replicate an AWS CodeCommit repository to another AWS region, and how to back up repositories hosted on other services to AWS CodeCommit\.

  Published March 17, 2017

+ *[Migrating to AWS CodeCommit](https://romikoderbynew.com/2016/09/06/migrating-to-aws-codecommit/)*

  Learn how to push code to two repositories as part of migrating from using another Git repository to AWS CodeCommit when using SourceTree\.

  Published September 6, 2016

+ *[Set Up Continuous Testing with Appium, AWS CodeCommit, Jenkins, and AWS Device Farm](https://aws.amazon.com/blogs/mobile/set-up-continuous-testing-with-appium-aws-codecommit-jenkins-and-aws-device-farm/)*

  Learn how to create a continuous testing process for mobile devices using Appium, AWS CodeCommit, Jenkins, and Device Farm\.

  Published February 2, 2016

+ **[Using AWS CodeCommit with Git Repositories in Multiple AWS Accounts](https://alestic.com/2015/11/aws-codecommit-iam-role/)**

  Learn how to clone your AWS CodeCommit repository and, in one command, configure the credential helper to use a specific IAM role for connections to that repository\.

  Published November 2015

+ **[Integrating AWS OpsWorks and AWS CodeCommit](https://aws.amazon.com/blogs/devops/integrating-aws-opsworks-and-aws-codecommit/)**

  Learn how AWS OpsWorks can automatically fetch Apps and Chef cookbooks from AWS CodeCommit\.

  Published August 25 2015

+ **[Using AWS CodeCommit and GitHub Credential Helpers](http://jameswing.net/aws/using-codecommit-and-git-credentials.html)**

  Learn how to configure your gitconfig file to work with both AWS CodeCommit and GitHub credential helpers\.

  Published September 2015

+ **[Using AWS CodeCommit from Eclipse](https://java.awsblog.com/post/Tx579PWM8RIYV5/Using-AWS-CodeCommit-from-Eclipse)**

  Learn how to use the EGit tools in Eclipse to work with AWS CodeCommit\.

  Published August 2015

+ **[AWS CodeCommit with Amazon EC2 Role Credentials](http://jameswing.net/aws/codecommit-with-ec2-role-credentials.html)**

  Learn how to use an instance profile for Amazon EC2 when configuring automated agent access to an AWS CodeCommit repository\.

  Published July 2015

+ **[Integrating AWS CodeCommit with Jenkins](https://blogs.aws.amazon.com/application-management/post/Tx1C8B98XN0AF2E/Integrating-AWS-CodeCommit-with-Jenkins)**

  Learn how to use AWS CodeCommit and Jenkins to support two simple continuous integration \(CI\) scenarios\.

  Published July 2015

+ **[Integrating AWS CodeCommit with Review Board](https://blogs.aws.amazon.com/application-management/post/Tx35O95VQF5I0AT/Integrating-AWS-CodeCommit-with-Review-Board)**

  Learn how to integrate AWS CodeCommit into a development workflow using the [Review Board](https://www.reviewboard.org/) code review system\.

  Published July 2015

### Code Samples<a name="integrations-community-code"></a>

The following are code samples that might be of interest to AWS CodeCommit users\.

+ **[Mac OS X Script to Periodically Delete Cached Credentials in the OS X Certificate Store](https://github.com/nicc777/macaws-codecommit-pwdel)**

  If you use the credential helper for AWS CodeCommit on Mac OS X, you are likely familiar with the problem with cached credentials\. This script demonstrate one solution\.

  **Author:** Nico Coetzee

  Published February 2016