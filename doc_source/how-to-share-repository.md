# Share a AWS CodeCommit repository<a name="how-to-share-repository"></a>

After you have created a CodeCommit repository, you can share it with other users\. First, decide which protocol \(HTTPS or SSH\) to recommend to users when cloning and using a Git client or an IDE to connect to your repository\. Then send the URL and connection information to the users with whom you want to share the repository\. Depending on your security requirements, sharing a repository might also require creating an IAM group, applying managed policies to that group, and editing IAM policies to refine access\. 

**Note**  
After you have granted users console access to the repository, they can add or edit files directly in the console without having to set up a Git client or other connection\. For more information, see [Create or add a file to an AWS CodeCommit repository](how-to-create-file.md) and [Edit the contents of a file in an AWS CodeCommit repository](how-to-edit-file.md)\.

These instructions are written with the assumption that you have already completed the steps in [Setting up ](setting-up.md) and [Create a repository](how-to-create-repository.md)\. 

**Note**  
Depending on your usage, you might be charged for creating or accessing a repository\. For more information, see [Pricing](http://aws.amazon.com/codecommit/pricing) on the CodeCommit product information page\.

**Topics**
+ [Choose the connection protocol to share with your users](#how-to-share-repo-choose)
+ [Create IAM policies for your repository](#how-to-share-repo-create-policy)
+ [Create an IAM group for repository users](#how-to-share-repository-IAMgroup)
+ [Share the connection information with your users](#how-to-share-repository-cli)

## Choose the connection protocol to share with your users<a name="how-to-share-repo-choose"></a>

When you create a repository in CodeCommit, two endpoints are generated: one for HTTPS connections and one for SSH connections\. Both provide secure connections over a network\. Your users can use either protocol\. Both endpoints remain active regardless of which protocol you recommend to your users\.

HTTPS connections require either: 
+ Git credentials, which IAM users can generate for themselves in IAM\. Git credentials are the easiest method for users of your repository to set up and use\. 
+ An AWS access key or role to assume, which your repository users must configure in their credential profile\. You can configure git\-remote\-codecommit \(recommended\) or the credential helper included in the AWS CLI\. These are the only methods available for root account or federated users\. 

SSH connections require your users to:
+ Generate a public\-private key pair\.
+ Store the public key\.
+ Associate the public key with their IAM user\.
+ Configure their known hosts file on their local computer\.
+ Create and maintain a config file on their local computers\.

Because this is a more complex configuration process, we recommend that you choose HTTPS and Git credentials for connections to CodeCommit\.

For more information about HTTPS, SSH, Git, git\-remote\-codecommit, and remote repositories, see [Setting up ](setting-up.md), [Connecting to AWS CodeCommit repositories with rotating credentials](temporary-access.md), or consult your Git documentation\. For a general overview of communication protocols and how each communicates with remote repositories, see [Git on the Server \- The Protocols](http://git-scm.com/book/ch4-1.html)\.

**Note**  
Although Git supports a variety of connection protocols, CodeCommit does not support connections with unsecured protocols, such as the local protocol or generic HTTP\.

## Create IAM policies for your repository<a name="how-to-share-repo-create-policy"></a>

AWS provides three managed policies in IAM for CodeCommit\. These policies cannot be edited and apply to all repositories associated with your AWS account\. However, you can use these policies as templates to create your own custom managed policies that apply only to the repository you want to share\. Your customer managed policy can apply specifically to the repository you want to share\. For more information, see [Managed Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/policies_managed-vs-inline.html#aws-managed-policies) and [IAM Users and Groups](https://docs.aws.amazon.com/IAM/latest/UserGuide/Using_WorkingWithGroupsAndUsers.html)\. 

**Tip**  
For more fine\-grained control over access to your repository, you can create more than one customer managed policy and apply the policies to different IAM users and groups\.

For information about reviewing the contents of managed policies and using policies to create and apply permissions, see [Authentication and access control for AWS CodeCommit](auth-and-access-control.md)\.

**Create a customer managed policy for your repository**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the **Dashboard** navigation area, choose **Policies**, and then choose **Create Policy**\. 

1. On the **Create Policy** page,, choose **Import managed policy**\.

1. On the **Import managed policies** page, in **Filter policies**, enter **AWSCodeCommitPowerUser**\. Choose the button next to the policy name and then choose **Import**\.

1. On the **Create policy** page, choose **JSON**\. Replace the "\*" portion of the `Resource` line for CodeCommit actions with the Amazon Resource Name \(ARN\) of the CodeCommit repository, as shown here:

   ```
   "Resource": [
    "arn:aws:codecommit:us-east-2:111111111111:MyDemoRepo"
    ]
   ```
**Tip**  
To find the ARN for the CodeCommit repository, go to the CodeCommit console and choose the repository name from the list\. For more information, see [View repository details](how-to-view-repository-details.md)\.

   If you want this policy to apply to more than one repository, add each repository as a resource by specifying its ARN\. Include a comma between each resource statement, as shown here:

   ```
   "Resource": [
    "arn:aws:codecommit:us-east-2:111111111111:MyDemoRepo",
    "arn:aws:codecommit:us-east-2:111111111111:MyOtherDemoRepo"
    ]
   ```

   When you are finished editing, choose **Review policy**\.

1. On the **Review Policy** page, in **Name**, enter a new name for the policy \(for example, *AWSCodeCommitPowerUser\-MyDemoRepo*\)\. Optionally provide a description for this policy\.

1. Choose **Create Policy**\.

## Create an IAM group for repository users<a name="how-to-share-repository-IAMgroup"></a>

To manage access to your repository, create an IAM group for its users, add IAM users to that group, and then attach the customer managed policy you created in the previous step\. 

If you use SSH, you must attach another managed policy to the IAMUserSSHKeys group, the IAM managed policy that allows users to upload their SSH public key and associate it with the IAM user they use to connect to CodeCommit\.

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the **Dashboard** navigation area, choose **Groups**, and then choose **Create New Group**\. 

1. On the **Set Group Name** page, in **Group Name**, enter a name for the group \(for example, *MyDemoRepoGroup*\), and then choose **Next Step**\. Consider including the repository name as part of the group name\.
**Note**  
This name must be unique across an AWS account\.

1. Select the box next to the customer managed policy you created in the previous section \(for example, **AWSCodeCommitPowerUser\-MyDemoRepo**\)\. 

1. On the **Review** page, choose **Create Group**\. IAM creates this group with the specified policies already attached\. The group appears in the list of groups associated with your AWS account\.

1. Choose your group from the list\. 

1. On the group summary page, choose the **Users** tab, and then choose **Add Users to Group**\. On the list that shows all users associated with your AWS account, select the boxes next to the users to whom you want to allow access to the CodeCommit repository, and then choose **Add Users**\.
**Tip**  
You can use the Search box to quickly find users by name\.

1. When you have added your users, close the IAM console\.

## Share the connection information with your users<a name="how-to-share-repository-cli"></a>

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. In the region selector, choose the AWS Region where the repository was created\. Repositories are specific to an AWS Region\. For more information, see [Regions and Git connection endpoints](regions.md)\.

1. On the **Repositories** page, choose the repository you want to share\. 

1. In **Clone URL**, choose the protocol that you want your users to use\. This copies the clone URL for the connection protocol\. 

1. Send your users the clone URL along with any other instructions, such as installing the AWS CLI, configuring a profile, or installing Git\. Make sure to include the configuration information for the connection protocol \(for example, HTTPS\)\. 

The following example email provides information for users connecting to the MyDemoRepo repository with the HTTPS connection protocol and Git credentials in the US East \(Ohio\) \(us\-east\-2\) Region\. This email is written with the assumption the user has already installed Git and is familiar with using it\.

```
I've created a CodeCommit repository for us to use while working on our project. 
The name of the repository is MyDemoRepo, and
it is in the  US East (Ohio) (us-east-2) region. 
Here's what you need to do in order to get started using it:

1. Make sure that your version of Git on your local computer is 1.7.9 or later.
2. Generate Git credentials for your IAM user by signing into the IAM console here: [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/). 
Switch to the Security credentials tab for your IAM user and choose the Generate button in HTTPS Git credentials for CodeCommit. 
Make sure to save your credentials in a secure location!
3. Switch to a directory of your choice and clone the CodeCommit repository to your local machine by running the following command:
    git clone https://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo my-demo-repo
4. When prompted for user name and password, use the Git credentials you just saved.          
    
That's it!  If you'd like to learn more about using CodeCommit, you can start with the tutorial here.
```

You can find complete setup instructions in [Setting up ](setting-up.md)\. 