--------

 The procedures in this guide support the new console design\. If you choose to use the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\.

--------

# Share an AWS CodeCommit Repository<a name="how-to-share-repository"></a>

After you have created an AWS CodeCommit repository, you can share it with other users\. First, decide which protocol to recommend to users when cloning and using a Git client or an IDE to connect to your repository: HTTPS or SSH\. Then send the URL and connection information to the users with whom you want to share the repository\. Depending on your security requirements, sharing a repository might also require creating an IAM group, applying managed policies to that group, and editing IAM policies to refine access\. This topic walks you through these steps\.

**Note**  
After you have granted users console access to the repository, they can add or edit files directly in the console without having to set up a Git client or other connection\. For more information, see [Create or Add a File to an AWS CodeCommit Repository](how-to-create-file.md) and [Edit the Contents of a File in an AWS CodeCommit Repository](how-to-edit-file.md)\.

These instructions are written with the assumption that you have already completed the steps in [Setting Up ](setting-up.md) and [Create a Repository](how-to-create-repository.md)\. 

**Note**  
Depending on your usage, you might be charged for creating or accessing a repository\. For more information, see [Pricing](http://aws.amazon.com/codecommit/pricing) on the AWS CodeCommit product information page\.

**Topics**
+ [Choose the Connection Protocol to Share with Your Users](#how-to-share-repo-choose)
+ [Create IAM Policies for Your Repository](#how-to-share-repo-create-policy)
+ [Create an IAM Group for Repository Users](#how-to-share-repository-IAMgroup)
+ [Share the Connection Information with Your Users](#how-to-share-repository-cli)

## Choose the Connection Protocol to Share with Your Users<a name="how-to-share-repo-choose"></a>

When you create a repository in AWS CodeCommit, two endpoints are generated: one for HTTPS connections and one for SSH connections\. Both provide secure connections over a network\. Your users can use either protocol\. Both endpoints remain active regardless of which protocol you recommend to your users\.

HTTPS connections require either: 
+ Git credentials, which IAM users can generate for themselves in IAM\. Git credentials are the easiest method for users of your repository to set up and use\. 
+ An AWS access key, which your repository users must configure in the credential helper included in the AWS CLI\. \(This is the only method available for root account or federated users\.\) 

SSH connections require your users to:
+ Generate a public\-private key pair\.
+ Store the public key\.
+ Associate the public key with their IAM user\.
+ Configure their known hosts file on their local computer\.
+ Create and maintain a config file on their local computers\.

Because this is a more complex configuration process, we recommend that you choose HTTPS and Git credentials for connections to AWS CodeCommit\.

For more information about HTTPS, SSH, Git, and remote repositories, see [Setting Up ](setting-up.md) or consult your Git documentation\. For a general overview of communication protocols and how each communicates with remote repositories, see [Git on the Server \- The Protocols](http://git-scm.com/book/ch4-1.html)\.

**Note**  
Although Git supports a variety of connection protocols, AWS CodeCommit does not support connections with unsecured protocols, such as the local protocol or generic HTTP\.

## Create IAM Policies for Your Repository<a name="how-to-share-repo-create-policy"></a>

AWS provides three managed policies in IAM for AWS CodeCommit\. These policies cannot be edited and apply to all repositories associated with your AWS account\. However, you can use these policies as templates to create your own custom managed policies that apply only to the repository you want to share\. Your customer managed policy can apply specifically to the repository you want to share\. For more information about managed policies and IAM users, see [Managed Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/policies_managed-vs-inline.html#aws-managed-policies) and [IAM Users and Groups](https://docs.aws.amazon.com/IAM/latest/UserGuide/Using_WorkingWithGroupsAndUsers.html)\. 

**Tip**  
For more fine\-grained control over access to your repository, you can create more than one customer managed policy and apply the policies to different IAM users and groups\.

To review the contents of the policy and the other managed policies for AWS CodeCommit and learn more about creating and applying permissions by using policies, see [Authentication and Access Control for AWS CodeCommit](auth-and-access-control.md)\.

**Create a customer managed policy for your repository**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the **Dashboard** navigation area, choose **Policies**, and then choose **Create Policy**\. 

1. On the **Create Policy** page, next to **Copy an AWS Managed Policy**, choose **Select**\.

1. On the **Copy an AWS Managed Policy** page, in **Search Policies**, enter **AWSCodeCommitPowerUser**\. Choose **Select** next to the policy name\.

1. On the **Review Policy** page, in **Policy Name**, enter a new name for the policy \(for example, *AWSCodeCommitPowerUser\-MyDemoRepo*\)\.

   In **Policy Document**, replace the "\*" portion of the `Resource` line with the Amazon Resource Name \(ARN\) of the AWS CodeCommit repository, as shown here:

   ```
   "Resource": [
    "arn:aws:codecommit:us-east-2:80398EXAMPLE:MyDemoRepo"
    ]
   ```
**Tip**  
To find the ARN for the AWS CodeCommit repository, go to the AWS CodeCommit console and choose the repository name from the list\. For more information, see [View Repository Details](how-to-view-repository-details.md)\.

   If you want this policy to apply to more than one repository, add each repository as a resource by specifying its ARN\. Include a comma between each resource statement, as shown here:

   ```
   "Resource": [
    "arn:aws:codecommit:us-east-2:80398EXAMPLE:MyDemoRepo",
    "arn:aws:codecommit:us-east-2:80398EXAMPLE:MyOtherDemoRepo"
    ]
   ```

1. Choose **Validate Policy**\. After the policy is validated, choose **Create Policy**\.

## Create an IAM Group for Repository Users<a name="how-to-share-repository-IAMgroup"></a>

To manage access to your repository, create an IAM group for its users, add IAM users to that group, and then attach the customer managed policy you created in the previous step\. 

If you use SSH, you must attach another managed policy to the IAMUserSSHKeys group, the IAM managed policy that allows users to upload their SSH public key and associate it with the IAM user they use to connect to AWS CodeCommit\.

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the **Dashboard** navigation area, choose **Groups**, and then choose **Create New Group**\. 

1. On the **Set Group Name** page, in **Group Name**, enter a name for the group \(for example, *MyDemoRepoGroup*\), and then choose **Next Step**\. Consider including the repository name as part of the group name\.
**Note**  
This name must be unique across an AWS account\.

1. Select the check box next to the customer managed policy you created in the previous section \(for example, **AWSCodeCommitPowerUser\-MyDemoRepo**\)\. 

1. On the **Review** page, choose **Create Group**\. IAM creates this group with the specified policies already attached\. The group appears in the list of groups associated with your AWS account\.

1. Choose your group from the list\. 

1. On the group summary page, choose the **Users** tab, and then choose **Add Users to Group**\. On the list that shows all users associated with your AWS account, select the check boxes next to the users to whom you want to allow access to the AWS CodeCommit repository, and then choose **Add Users**\.
**Tip**  
You can use the Search box to quickly find users by name\.

1. When you have added your users, close the IAM console\.

## Share the Connection Information with Your Users<a name="how-to-share-repository-cli"></a>

1. Open the AWS CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In the region selector, choose the region where the repository was created\. Repositories are specific to an AWS region\. For more information, see [Regions and Git Connection Endpoints](regions.md)\.

1. On the **Repositories** page, find the name of the repository you want to share\. 

1. In **Clone URL**, choose the protocol \(HTTPS or SSH\) that you want your users to use\. This copies the clone URL for the connection protocol\. 

1. Send your users the clone URL along with any other instructions, such as installing the AWS CLI, configuring a profile, or installing Git\. Make sure to include the configuration information for the connection protocol \(for example, for HTTPS, configuring the credential helper for Git\)\. 

The following example email provides information for users connecting to the MyDemoRepo repository with the HTTPS connection protocol and Git credentials in the US East \(Ohio\) \(us\-east\-2\) region\. This email is written with the assumption the user has already installed Git and is familiar with using it\.

```
I've created an AWS CodeCommit repository for us to use while working on our project. 
The name of the repository is MyDemoRepo, and
it is in the  US East (Ohio) (us-east-2) region. 
Here's what you need to do in order to get started using it:

1. Make sure that your version of Git on your local computer is 1.7.9 or later.
2. Generate Git credentials for your IAM user by signing into the IAM console here: [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/). 
Switch to the Security credentials tab for your IAM user and choose the Generate button in HTTPS Git credentials for AWS CodeCommit. 
Make sure to save your credentials in a secure location!
3. Switch to a directory of your choice and clone the AWS CodeCommit repository to your local machine by running the following command:
    git clone https://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo my-demo-repo
4. When prompted for user name and password, use the Git credentials you just saved.          
    
That's it!  If you'd like to learn more about using AWS CodeCommit, you can start with the tutorial [here](getting-started.md#getting-started-create-commit).
```

You can find complete setup instructions in [Setting Up ](setting-up.md)\. 