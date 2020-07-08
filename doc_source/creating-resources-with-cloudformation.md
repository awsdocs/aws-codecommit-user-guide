# Creating CodeCommit resources with AWS CloudFormation<a name="creating-resources-with-cloudformation"></a>

CodeCommit is integrated with AWS CloudFormation, a service that helps you model and set up your AWS resources so that you can spend less time creating and managing your resources and infrastructure\. You create a template that describes all the AWS resources that you want \(like repositories\), and AWS CloudFormation takes care of provisioning and configuring those resources for you\.

When you use AWS CloudFormation, you can reuse your template to set up your CodeCommit resources consistently and repeatedly\. Just describe your resources once and then provision the same resources over and over in multiple accounts and AWS Regions\. 

## CodeCommit and AWS CloudFormation templates<a name="working-with-templates"></a>

To provision and configure resources for CodeCommit and related services, you must understand [AWS CloudFormation templates](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-guide.html)\. Templates are formatted text files in JSON or YAML\. These templates describe the resources that you want to provision in your AWS CloudFormation stacks\. If you're unfamiliar with JSON or YAML, you can use AWS CloudFormation Designer to help you get started with AWS CloudFormation templates\. For more information, see [What is AWS CloudFormation Designer?](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/working-with-templates-cfn-designer.html) in the *AWS CloudFormation User Guide*\.

CodeCommit supports creating repositories in AWS CloudFormation\. Unlike creating repositories from the console or command line, you can use AWS CloudFormation to create repositories and automatically commit code to the newly created repository from a specified \.zip file in an Amazon S3 bucket\. For more information, including examples of JSON and YAML templates for repositories, see [AWS::CodeCommit::Repository](https://docs.aws.amazon.com/aws-resource-codecommit-repository.html)\.

You can also create templates that create related resources, such as [notification rules](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-codestarnotifications-notificationrule.html) for repositories, [AWS CodeBuild build projects](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-codebuild-project.html), [AWS CodeDeploy applications](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-codedeploy-application.html), and [AWS CodePipeline pipelines](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-codepipeline-pipeline.html)\.

## Learn more about AWS CloudFormation<a name="learn-more-cloudformation"></a>

To learn more about AWS CloudFormation, see the following resources:
+ [AWS CloudFormation](https://aws.amazon.com/cloudformation/)
+ [AWS CloudFormation User Guide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)
+ [AWS CloudFormation Command Line Interface User Guide](https://docs.aws.amazon.com/cloudformation-cli/latest/userguide/what-is-cloudformation-cli.html)