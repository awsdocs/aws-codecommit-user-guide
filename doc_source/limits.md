# Quotas in AWS CodeCommit<a name="limits"></a>

The following table describes quotas in CodeCommit\. For information about quotas that can be changed, see [AWS CodeCommit Endpoints and Quotas](https://docs.aws.amazon.com/general/latest/gr/codecommit.html) and [AWS Service Quotas](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html)\. For information about required versions of Git and other software, see [Compatibility for CodeCommit, Git, and other components](setting-up.md#setting-up-compat)\.


|  |  | 
| --- |--- |
| Approval rule and approval rule template names | Any combination of letters, numbers, periods, spaces, underscores, and dashes between 1 and 100 characters in length\. Names are case sensitive\. Names cannot end in \.git and cannot contain any of the following characters: \! ? @ \# $ % ^ & \* \( \) \+ = \{ \} \[ \] \| \\ / > < \~ ` ' " ; :  | 
| Approval rule content length | 3000 characters | 
| Approval rule template description length | 1000 characters | 
| Approval rule template destination references | 100 | 
| Approval rule templates | 1000 in an AWS Region | 
| Approval rules on a pull request | Up to 30 maximum\. Up to 25 of these can be from approval rule templates\. | 
| Approval rules on a pull request created from an approval rule template | 25 | 
| Approvals on a pull request | 200 | 
| Approvers in an approval pool | 50 | 
| Branch names |  Any combination of allowed characters between 1 and 256 characters in length\. Branch names cannot: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codecommit/latest/userguide/limits.html) Branch names are references\. Many of the limitations on branch names are based on the Git reference standard\. For more information, see [Git Internals](https://git-scm.com/book/en/v2/Git-Internals-Git-References) and [git\-check\-ref\-format](https://git-scm.com/docs/git-check-ref-format)\.  | 
| Custom data for triggers | This is a string field limited to 1,000 characters\. It cannot be used to pass any dynamic parameters\.  | 
| Email addresses in commits made in the console | Any combination of allowed characters between 1 and 256 characters in length\. Email addresses are not validated\. | 
| File paths | Any combination of allowed characters between 1 and 4,096 characters in length\. File paths must be an unambiguous name that specifies the file and the exact location of the file\. File paths cannot exceed 20 directories in depth\. In addition, file paths cannot:[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codecommit/latest/userguide/limits.html) File names and paths must be fully qualified\. The name and path to a file on your local computer must follow the standards for that operating system\. When specifying the path to a file in a CodeCommit repository, use the standards for Amazon Linux\. | 
| File size | Maximum of 6 MB for any individual file when using the CodeCommit console, APIs, or the AWS CLI\. | 
| Git blob size |  Maximum of 2 GB\.  There is no limit on the number or the total size of all files in a single commit, as long as the metadata does not exceed 6 MB and a single blob does not exceed 2 GB\.   | 
| Graph display of branches in the Commit Visualizer | 35 per page\. If there are more than 35 branches on a single page, the graph is not displayed\. | 
| Metadata for a commit  |  Maximum of 20 MB for the combined [metadata for a commit](https://git-scm.com/book/en/v2/Git-Internals-Git-Objects) \(for example, the combination of author information, date, parent commit list, and commit messages\) when using the CodeCommit console, APIs, or the AWS CLI\.   There is no limit on the number or the total size of all files in a single commit, as long as the data does not exceed 20 MB, an individual file does not exceed 6 MB, and a single blob does not exceed 2 GB\.   | 
| Number of references in a single push | Maximum of 4,000, including create, delete, and update\. There is no limit on the overall number of references in the repository\. | 
| Number of repositories |  Maximum of 1,000 per AWS account\. This limit can be changed\. For more information, see [AWS CodeCommit Endpoints and Quotas](codecommit.html) and [AWS Service Quotas](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html)\.   | 
|  Number of triggers in a repository  |  Maximum of 10\.  | 
| Regions |  CodeCommit is available in the following AWS Regions: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codecommit/latest/userguide/limits.html) For more information, see [Regions and Git connection endpoints](regions.md)\.  | 
| Repository descriptions | Any combination of characters between 0 and 1,000 characters in length\. Repository descriptions are optional\. | 
| Repository names |  Any combination of letters, numbers, periods, underscores, and dashes between 1 and 100 characters in length\. Names are case sensitive\. Repository names cannot end in \.git and cannot contain any of the following characters: `! ? @ # $ % ^ & * ( ) + = { } [ ] \| \ / > < ~ ` ' " ; : `  | 
| Repository tag key names |  Any combination of Unicode letters, numbers, spaces, and allowed characters in UTF\-8 between 1 and 128 characters in length\. Allowed characters are `+ - = . _ : / @` Tag key names must be unique, and each key can only have one value\. A tag cannot: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codecommit/latest/userguide/limits.html)  | 
| Repository tag values |  Any combination of Unicode letters, numbers, spaces, and allowed characters in UTF\-8 between 1 and 256 characters in length\. Allowed characters are `+ - = . _ : / @` A key can only have one value, but many keys can have the same value\. A tag cannot: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codecommit/latest/userguide/limits.html)  | 
| Repository tags | Tags are case sensitive\. Maximum of 50 per resource\. | 
| Trigger names | Any combination of letters, numbers, periods, underscores, and dashes between 1 and 100 characters in length\. Trigger names cannot contain spaces or commas\. | 
| User names in commits made in the console | Any combination of allowed characters between 1 and 1,024 characters in length\. | 