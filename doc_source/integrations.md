# Product and service integrations with AWS CodeCommit<a name="integrations"></a>

By default, CodeCommit is integrated with a number of AWS services\. You can also use CodeCommit with products and services outside of AWS\. The following information can help you configure CodeCommit to integrate with the products and services you use\.

**Note**  
You can automatically build and deploy commits to a CodeCommit repository by integrating with CodePipeline\. To learn more, follow the steps in the [AWS for DevOps Getting Started Guide](https://docs.aws.amazon.com/devops/latest/gsg/welcome.html)\. 

**Topics**
+ [Integration with other AWS services](#integrations-aws)
+ [Integration examples from the community](#integrations-community)

## Integration with other AWS services<a name="integrations-aws"></a>

CodeCommit is integrated with the following AWS services:


|  |  | 
| --- |--- |
| AWS Amplify |  [AWS Amplify](https://aws.amazon.com/amplify/) makes it easy to create, configure, and implement scalable mobile applications powered by AWS\. Amplify seamlessly provisions and manages your mobile backend and provides a simple framework to easily integrate your backend with your iOS, Android, Web, and React Native frontends\. Amplify also automates the application release process of both your frontend and backend, which makes it possible for you to deliver features faster\. You can connect your CodeCommit repository in the Amplify console\. After you authorize the Amplify console, Amplify fetches an access token from the repository provider, but it doesn't store the token on the AWS servers\. Amplify accesses your repository using deploy keys installed in a specific repository only\. Learn more: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codecommit/latest/userguide/integrations.html)  | 
| AWS Cloud9 |  [AWS Cloud9](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/) contains a collection of tools that you use to code, build, run, test, debug, and release software in the cloud\. This collection of tools is referred to as the AWS Cloud9 integrated development environment, or IDE\.  You access the AWS Cloud9 IDE through a web browser\. The IDE offers a rich code\-editing experience with support for several programming languages and runtime debuggers, and a built\-in terminal\.  Learn more: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codecommit/latest/userguide/integrations.html)  | 
| AWS CloudFormation |  [AWS CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/) is a service that helps you model and set up your AWS resources so that you can spend less time managing those resources and more time focusing on your applications\. You create a template that describes resources, including a CodeCommit repository, and AWS CloudFormation takes care of provisioning and configuring those resources for you\. Learn more: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codecommit/latest/userguide/integrations.html)  | 
| AWS CloudTrail |  [CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/) captures AWS API calls and related events made by or on behalf of an AWS account and delivers log files to an Amazon S3 bucket that you specify\. You can configure CloudTrail to capture API calls from the AWS CodeCommit console, CodeCommit commands from the AWS CLI, the local Git client, and from the CodeCommit API\.  Learn more: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codecommit/latest/userguide/integrations.html)  | 
| Amazon CloudWatch Events |  [CloudWatch Events](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/) delivers a near real\-time stream of system events that describe changes in AWS resources\. Using simple rules that you can quickly set up, you can match events and route them to one or more target functions or streams\. CloudWatch Events becomes aware of operational changes as they occur\. CloudWatch Events responds to these operational changes and takes action as necessary, by sending messages to respond to the environment, activating functions, making changes, and capturing state information\.   You can configure CloudWatch Events to monitor CodeCommit repositories and respond to repository events by targeting streams, functions, tasks, or other processes in other AWS services, such as Amazon Simple Queue Service, Amazon Kinesis, AWS Lambda, and many more\. Learn more: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codecommit/latest/userguide/integrations.html)  | 
| AWS CodeBuild |  [CodeBuild](http://docs.aws.amazon.com/codebuild/latest/userguide/welcome.html) is a fully managed build service in the cloud that compiles your source code, runs unit tests, and produces artifacts that are ready to deploy\. You can store the source code to be built and the build specification in a CodeCommit repository\. You can use CodeBuild directly with CodeCommit, or you can incorporate both CodeBuild and CodeCommit in a continuous delivery pipeline with CodePipeline\. Learn more: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codecommit/latest/userguide/integrations.html)  | 
| Amazon CodeGuru Reviewer | Amazon CodeGuru Reviewer is an automated code review service that uses program analysis and machine learning to detect common issues and recommend fixes in your Java code\. You can associate repositories in your AWS account with CodeGuru Reviewer\. When you do, CodeGuru Reviewer creates a service\-linked role that allows CodeGuru Reviewer to analyze code in all pull requests created after the association is made\. Learn more:[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codecommit/latest/userguide/integrations.html) | 
| AWS CodePipeline |  [CodePipeline](https://docs.aws.amazon.com/codepipeline/latest/userguide/) is a continuous delivery service you can use to model, visualize, and automate the steps required to release your software\. You can configure CodePipeline to use a CodeCommit repository as a source action in a pipeline, and automate building, testing, and deploying your changes\. Learn more: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codecommit/latest/userguide/integrations.html)  | 
| AWS CodeStar |  [AWS CodeStar](https://docs.aws.amazon.com/codestar/latest/userguide/welcome.html) is a cloud\-based service for creating, managing, and working with software development projects on AWS\. You can quickly develop, build, and deploy applications on AWS with an AWS CodeStar project\. An AWS CodeStar project creates and integrates AWS services for your project development toolchain, including a CodeCommit repository for the project\. AWS CodeStar also assigns permissions to team members for that project\. These permissions are applied automatically, including permissions for accessing CodeCommit, creating and managing Git credentials, and more\.  You can configure repositories created for AWS CodeStar projects just as you would any other CodeCommit repository by using the AWS CodeCommit console, CodeCommit commands from the AWS CLI, the local Git client, and from the CodeCommit API\.  Learn more: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codecommit/latest/userguide/integrations.html)  | 
| AWS Elastic Beanstalk |  [Elastic Beanstalk](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/) is a managed service that makes it easy to deploy and manage applications in the AWS cloud without worrying about the infrastructure that runs those applications\. You can use the Elastic Beanstalk command line interface \(EB CLI\) to deploy your application directly from a new or existing CodeCommit repository\. Learn more: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codecommit/latest/userguide/integrations.html)  | 
| AWS Key Management Service |  [AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/) is a managed service that makes it easy for you to create and control the encryption keys used to encrypt your data\. By default, CodeCommit uses AWS KMS to encrypt repositories\.  Learn more: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codecommit/latest/userguide/integrations.html)  | 
| AWS Lambda |  [Lambda](https://docs.aws.amazon.com/lambda/latest/dg/) lets you run code without provisioning or managing servers\. You can configure triggers for CodeCommit repositories that invoke Lambda functions in response to repository events\.  Learn more: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codecommit/latest/userguide/integrations.html)  | 
| Amazon Simple Notification Service |  [Amazon SNS](https://docs.aws.amazon.com/sns/latest/dg/) is a web service that enables applications, end users, and devices to instantly send and receive notifications from the cloud\. You can configure triggers for CodeCommit repositories that send Amazon SNS notifications in response to repository events\. You can also use Amazon SNS notifications to integrate with other AWS services\. For example, you can use an Amazon SNS notification to send messages to an Amazon Simple Queue Service queue\. Learn more: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codecommit/latest/userguide/integrations.html)  | 

## Integration examples from the community<a name="integrations-community"></a>

The following sections provide links to blog posts, articles, and community\-provided examples\.

**Note**  
These links are provided for informational purposes only, and should not be considered either a comprehensive list or an endorsement of the content of the examples\. AWS is not responsible for the content or accuracy of external content\.

**Topics**
+ [Blog posts](#integrations-community-blogposts)
+ [Code samples](#integrations-community-code)

### Blog posts<a name="integrations-community-blogposts"></a>
+ **[Integrating SonarQube as a Pull Request Approver on AWS CodeCommit](https://aws.amazon.com/blogs/devops/integrating-sonarqube-as-a-pull-request-approver-on-aws-codecommit/)**

  Learn how to create a CodeCommit repository that requires a successful SonarQube quality analysis before pull requests can be merged\. 

  Published December 12, 2019
+ **[Migration to AWS CodeCommit, AWS CodePipeline and AWS CodeBuild From GitLab](https://aws.amazon.com/blogs/devops/migration-to-aws-codecommit-aws-codepipeline-and-aws-codebuild-from-gitlab/)**

  Learn how to migrate multiple repositories to AWS CodeCommitAWS CodeCommit from GitLab and set up a CI/CD pipeline using AWS CodePipeline and AWS CodeBuild\. 

  Published November 22, 2019
+ **[Implementing GitFlow Using AWS CodePipeline, AWS CodeCommit, AWS CodeBuild, and AWS CodeDeploy](https://aws.amazon.com/blogs/devops/implementing-gitflow-using-aws-codepipeline-aws-codecommit-aws-codebuild-and-aws-codedeploy/)**

  Learn how to implement GitFlow using AWS CodePipeline, AWS CodeCommit, AWS CodeBuild, and AWS CodeDeploy\.

  Published February 22, 2019
+ **[Using Git with AWS CodeCommit Across Multiple AWS Accounts](https://aws.amazon.com/blogs/devops/using-git-with-aws-codecommit-across-multiple-aws-accounts/)**

  Learn how to manage your Git configuration across multiple AWS accounts\.

  Published February 12, 2019
+ **[Validating AWS CodeCommit Pull Requests with AWS CodeBuildand AWS Lambda](https://aws.amazon.com/blogs/devops/validating-aws-codecommit-pull-requests-with-aws-codebuild-and-aws-lambda/)**

  Learn how to validate pull requests with AWS CodeCommit, AWS CodeBuild, and AWS Lambda\. By running tests against the proposed changes prior to merging them into the master branch, you can help ensure a high level of quality in pull requests, catch any potential issues, and boost the confidence of the developer in relation to their changes\.

  Published February 11, 2019
+ **[Using Federated Identities with AWS CodeCommit](https://aws.amazon.com/blogs/devops/using-federated-identities-with-aws-codecommit/)**

  Learn how to access repositories in AWS CodeCommit using the identities used in your business\.

  Published October 5, 2018
+ **[Refining Access to Branches in AWS CodeCommit](https://aws.amazon.com/blogs/devops/refining-access-to-branches-in-aws-codecommit/)**

  Learn how to restrict commits to repository branches by creating and applying an IAM policy that uses a context key\.

  Published May 16, 2018
+ **[Replicate AWS CodeCommit Repositories Between Regions Using AWS Fargate](https://aws.amazon.com/blogs/devops/replicate-aws-codecommit-repository-between-regions-using-aws-fargate/)**

  Learn how to set up continuous replication of a CodeCommit repository from one AWS region to another using a serverless architecture\.

  Published April 11, 2018
+ **[Distributing Your AWS OpsWorks for Chef Automate Infrastructure](https://aws.amazon.com/blogs/mt/distributing-your-aws-opsworks-for-chef-automate-infrastructure/)**

  Learn how to use CodePipeline, CodeCommit, CodeBuild, and AWS Lambda to ensure that cookbooks and other configurations are consistently deployed across two or more Chef Servers residing in one or more AWS Regions\.

  Published March 9, 2018
+ **[Peanut Butter and Chocolate: Azure Functions CI/CD Pipeline with AWS CodeCommit](https://get-powershellblog.blogspot.com/2018/02/peanut-butter-and-chocolate-azure.html)**

  Learn how to create a PowerShell\-based Azure Functions CI/CD pipeline where the code is stored in a CodeCommit repository\.

  Published February 19, 2018
+ **[Continuous Deployment to Kubernetes Using AWS CodePipeline, AWS CodeCommit, AWS CodeBuild, Amazon ECR, and AWS Lambda](https://aws.amazon.com/blogs/devops/continuous-deployment-to-kubernetes-using-aws-codepipeline-aws-codecommit-aws-codebuild-amazon-ecr-and-aws-lambda/)**

  Learn how to use Kubernetes and AWS together to create a fully managed, continuous deployment pipeline for container based applications\.

  Published January 11, 2018
+ **[Use AWS CodeCommit Pull Requests to Request Code Reviews and Discuss Code](https://aws.amazon.com/blogs/devops/using-aws-codecommit-pull-requests-to-request-code-reviews-and-discuss-code/)**

  Learn how to use pull requests to review, comment upon, and interactively iterate on code changes in a CodeCommit repository\.

  Published November 20, 2017
+ **[Build Serverless AWS CodeCommit Workflows Using Amazon CloudWatch Events and JGit](https://aws.amazon.com/blogs/devops/build-serverless-aws-codecommit-workflows-using-amazon-cloudwatch-events-and-jgit/)**

  Learn how to create CloudWatch Events rules that process changes in a repository using CodeCommit repository events and target actions in other AWS services\. Examples include AWS Lambda functions that enforce Git commit message policies on commits, replicate a CodeCommit repository, and backing up a CodeCommit repository to Amazon S3\.

  Published August 3, 2017
+ **[Replicating and Automating Sync\-Ups for a Repository with AWS CodeCommit](https://aws.amazon.com/blogs/devops/replicating-and-automating-sync-ups-for-a-repository-with-aws-codecommit/)**

  Learn how to back up or replicate a CodeCommit repository to another AWS region, and how to back up repositories hosted on other services to CodeCommit\.

  Published March 17, 2017
+ **[Migrating to AWS CodeCommit](https://romikoderbynew.com/2016/09/06/migrating-to-aws-codecommit/)**

  Learn how to push code to two repositories as part of migrating from using another Git repository to CodeCommit when using SourceTree\.

  Published September 6, 2016
+ **[Set Up Continuous Testing with Appium, AWS CodeCommit, Jenkins, and AWS Device Farm](https://aws.amazon.com/blogs/mobile/set-up-continuous-testing-with-appium-aws-codecommit-jenkins-and-aws-device-farm/)**

  Learn how to create a continuous testing process for mobile devices using Appium, CodeCommit, Jenkins, and Device Farm\.

  Published February 2, 2016
+ **[Using AWS CodeCommit with Git Repositories in Multiple AWS Accounts](https://alestic.com/2015/11/aws-codecommit-iam-role/)**

  Learn how to clone your CodeCommit repository and, in one command, configure the credential helper to use a specific IAM role for connections to that repository\.

  Published November 2015
+ **[Integrating AWS OpsWorks and AWS CodeCommit](https://aws.amazon.com/blogs/devops/integrating-aws-opsworks-and-aws-codecommit/)**

  Learn how AWS OpsWorks can automatically fetch Apps and Chef cookbooks from CodeCommit\.

  Published August 25, 2015
+ **[Using AWS CodeCommit and GitHub Credential Helpers](http://jameswing.net/aws/using-codecommit-and-git-credentials.html)**

  Learn how to configure your gitconfig file to work with both CodeCommit and GitHub credential helpers\.

  Published September 2015
+ **[Using AWS CodeCommit from Eclipse](https://java.awsblog.com/post/Tx579PWM8RIYV5/Using-AWS-CodeCommit-from-Eclipse)**

  Learn how to use the EGit tools in Eclipse to work with CodeCommit\.

  Published August 2015
+ **[AWS CodeCommit with Amazon EC2 Role Credentials](http://jameswing.net/aws/codecommit-with-ec2-role-credentials.html)**

  Learn how to use an instance profile for Amazon EC2 when configuring automated agent access to a CodeCommit repository\.

  Published July 2015
+ **[Integrating AWS CodeCommit with Jenkins](https://blogs.aws.amazon.com/application-management/post/Tx1C8B98XN0AF2E/Integrating-AWS-CodeCommit-with-Jenkins)**

  Learn how to use CodeCommit and Jenkins to support two simple continuous integration \(CI\) scenarios\.

  Published July 2015
+ **[Integrating AWS CodeCommit with Review Board](https://blogs.aws.amazon.com/application-management/post/Tx35O95VQF5I0AT/Integrating-AWS-CodeCommit-with-Review-Board)**

  Learn how to integrate CodeCommit into a development workflow using the [Review Board](https://www.reviewboard.org/) code review system\.

  Published July 2015

### Code samples<a name="integrations-community-code"></a>

The following are code samples that might be of interest to CodeCommit users\.
+ **[Mac OS X Script to Periodically Delete Cached Credentials in the OS X Certificate Store](https://github.com/nicc777/macaws-codecommit-pwdel)**

  If you use the credential helper for CodeCommit on Mac OS X, you are likely familiar with the problem with cached credentials\. This script demonstrate one solution\.

  **Author:** Nico Coetzee

  Published February 2016