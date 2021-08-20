# Creating CodeCommit resources with AWS CloudFormation<a name="creating-resources-with-cloudformation"></a>

AWS CodeCommit is integrated with AWS CloudFormation, a service that helps you to model and set up your AWS resources so that you can spend less time creating and managing your resources and infrastructure\. You create a template that describes all the AWS resources that you want \(such as repositories\), and AWS CloudFormation provisions and configures those resources for you\. 

When you use AWS CloudFormation, you can reuse your template to set up your CodeCommit resources consistently and repeatedly\. Describe your resources once, and then provision the same resources over and over in multiple AWS accounts and Regions\. 

## CodeCommit and AWS CloudFormation templates<a name="working-with-templates"></a>

To provision and configure resources for CodeCommit and related services, you must understand [AWS CloudFormation templates](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-guide.html)\. Templates are formatted text files in JSON or YAML\. These templates describe the resources that you want to provision in your AWS CloudFormation stacks\. If you're unfamiliar with JSON or YAML, you can use AWS CloudFormation Designer to help you get started with AWS CloudFormation templates\. For more information, see [What is AWS CloudFormation Designer?](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/working-with-templates-cfn-designer.html) in the *AWS CloudFormation User Guide*\.

CodeCommit supports creating repositories in AWS CloudFormation Unlike creating repositories from the console or command line, you can use AWS CloudFormation to create repositories and automatically commit code to the newly created repository from a specified \.zip file in an Amazon S3 bucket\. For more information, including examples of JSON and YAML templates for repositories, see [AWS::CodeCommit::Repository](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-codecommit-repository.html)\.

When you create a CodeCommit repository using AWS CloudFormation, you have the option to commit code to that repository as part of the creation process by configuring properties in [AWS:CodeCommit::Repository Code](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-codecommit-repository-code.html)\. You can specify the Amazon S3 bucket where the code is stored, and optionally use the [BranchName property](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-codecommit-repository-code.html) to specify the name of the default branch that will be created in the initial commit of that code\. These properties are only used in initial repository creation, and are ignored on stack updates\. You cannot use these properties to make additional commits to a repository, or to change the name of the default branch after the initial commit is made\.

**Note**  
On January 19, 2021, AWS changed the name of the default branch in CodeCommit from *master* to *main*\. This name change affects the default behavior of CodeCommit when creating the initial commit for repositories using the CodeCommit console, the CodeCommit APIs, the AWS SDKs, and the AWS CLI\. Repositories created with AWS CloudFormation or the AWS CDK with an initial commit of code as part of creation align with this change beginning March 4, 2021\. This change does not affect existing repositories or branches\. Customers who use local Git clients to create their initial commits have a default branch name that follows the configuration of those Git clients\. For more information, see [Working with branches](https://docs.aws.amazon.com/codecommit/latest/userguide/branches.html), [Create a commit](https://docs.aws.amazon.com/codecommit/latest/userguide/how-to-create-commit.html), and [Change branch settings](https://docs.aws.amazon.com/codecommit/latest/userguide/how-to-change-branch.html)\.

You can also create templates that create related resources, such as [notification rules](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-codestarnotifications-notificationrule.html) for repositories, [AWS CodeBuild build projects](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-codebuild-project.html), [AWS CodeDeploy applications](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-codedeploy-application.html), and [AWS CodePipeline pipelines](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-codepipeline-pipeline.html)\.

## Template examples<a name="cloudformation-codecommit-example"></a>

The following examples create a CodeCommit repository named *MyDemoRepo*\. The newly created repository is populated with code stored in an Amazon S3 bucket named *MySourceCodeBucket* and placed in a branch named *development*, which is the default branch for the repository\. 

**Note**  
The name of the Amazon S3 bucket that contains the ZIP file with the content that will be committed to the new repository can be specified using an ARN or the name of the bucket in the Amazon Web Services account\. The Amazon S3 object key is as defined in the [Amazon S3 Developer Guide](https://docs.aws.amazon.com/AmazonS3/latest/dev/Introduction.html#BasicsKeys)\.

 **JSON**:

```
{
    "MyRepo": {
        "Type": "AWS::CodeCommit::Repository",
        "Properties": {
            "RepositoryName": "MyDemoRepo",
            "RepositoryDescription": "This is a repository for my project with code from MySourceCodeBucket.",
            "Code": {
                "BranchName": "development",
                "S3": {
                    "Bucket": "MySourceCodeBucket",
                    "Key": "MyKey",
                    "ObjectVersion": "1"
                }
            }
        }
    }
}
```

**YAML**:

```
MyRepo:
  Type: AWS::CodeCommit::Repository
  Properties:
    RepositoryName: MyDemoRepo
    RepositoryDescription: This is a repository for my project with code from MySourceCodeBucket.
    Code:
      BranchName: development
      S3: 
        Bucket: MySourceCodeBucket,
        Key: MyKey,
        ObjectVersion: 1
```

For more examples, see [AWS::CodeCommit::Repository](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-codecommit-repository.html)\.

## AWS CloudFormation, CodeCommit, and the AWS Cloud Development Kit \(CDK\)<a name="cloudformation-codecommit-cdk"></a>

Repositories created using the AWS CDK use AWS CloudFormation functionality in their creation\. Understanding how AWS CloudFormation templates work with CodeCommit resources can help you create and manage your AWS CDK code\. For more information about the AWS CDK, see the [AWS Cloud Development Kit \(CDK\) Developer Guide](https://docs.aws.amazon.com/cdk/latest/guide/home.html) and the [AWS CDK API Reference\.](https://docs.aws.amazon.com/cdk/api/latest/docs/aws-codecommit-readme.html)

The following AWS CDK Typescript example creates a CodeCommit repository named *MyDemoRepo*\. The newly created repository is populated with code stored in an Amazon S3 bucket named *MySourceCodeBucket* and placed in a branch named *development*, which is the default branch for the repository\. 

```
import * as cdk from '@aws-cdk/core';
import codecommit = require('@aws-cdk/aws-codecommit');
export class CdkCodecommitStack extends cdk.Stack {
  constructor(scope: cdk.Construct, id: string, props?: cdk.StackProps) {
    super(scope, id, props);
    // The code creates a CodeCommit repository with a default branch name development
      new codecommit.CfnRepository(this, 'MyRepoResource', {
            repositoryName: "MyDemoRepo",
            code: {
              "branchName": "development",
              "s3": {
                "bucket": "MySourceCodeBucket",
                "key": "MyKey"
              }
            },
        }
     );
  }
}
```

## Learn more about AWS CloudFormation<a name="learn-more-cloudformation"></a>

To learn more about AWS CloudFormation, see the following resources:
+ [AWS CloudFormation](https://aws.amazon.com/cloudformation/)
+ [AWS CloudFormation User Guide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)
+ [AWS CloudFormation Command Line Interface User Guide](https://docs.aws.amazon.com/cloudformation-cli/latest/userguide/what-is-cloudformation-cli.html)