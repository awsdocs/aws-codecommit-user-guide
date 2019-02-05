--------

 The procedures in this guide support the new console design\. If you choose to use the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\.

--------

# Limits in AWS CodeCommit<a name="limits"></a>

The following table describes limits in AWS CodeCommit\. For information about limits that can be changed, see [AWS Service Limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_codecommit)\.


|  |  | 
| --- |--- |
| Number of repositories |  Maximum of 1,000 per AWS account\. This limit can be changed\. For more information, see [AWS Service Limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html)\.  | 
| Regions |  AWS CodeCommit is available in the following regions: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codecommit/latest/userguide/limits.html) For more information, see [Regions and Git Connection Endpoints](regions.md)\.  | 
| Number of references in a single push | Maximum of 4,000, including create, delete, and update\. There is no limit on the overall number of references in the repository\. | 
|  Number of triggers in a repository  |  Maximum of 10\.  | 
| Repository names |  Any combination of letters, numbers, periods, underscores, and dashes between 1 and 100 characters in length\. Repository names cannot end in \.git and cannot contain any of the following characters: ****\! ? @ \# $ % ^ & \* \( \) \+ = \{ \} \[ \] \| \\ / > < \~ ` ‘ “ ; :****   | 
| Branch names |  Any combination of allowed characters between 1 and 256 characters in length\. Branch names cannot: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codecommit/latest/userguide/limits.html) Branch names are references\. Many of the limitations on branch names are based on the Git reference standard\. For more information, see [Git Internals](https://git-scm.com/book/en/v2/Git-Internals-Git-References) and [git\-check\-ref\-format](https://git-scm.com/docs/git-check-ref-format)\.  | 
| Trigger names | Any combination of letters, numbers, periods, underscores, and dashes between 1 and 100 characters in length\. Trigger names cannot contain spaces or commas\. | 
| User names in commits made in the console | Any combination of allowed characters between 1 and 1,024 characters in length\. | 
| Email addresses in commits made in the console | Any combination of allowed characters between 1 and 256 characters in length\. Email addresses are not validated\. | 
| Repository descriptions | Any combination of characters between 0 and 1,000 characters in length\. Repository descriptions are optional\. | 
| Metadata for a commit  |  Maximum of 20 MB for the combined [metadata for a commit](https://git-scm.com/book/en/v2/Git-Internals-Git-Objects) \(for example, the combination of author information, date, parent commit list, and commit messages\)\.   There is no limit on the number or the total size of all files in a single commit, as long as the metadata does not exceed 20 MB and a single blob does not exceed 2 GB\.   | 
| File size display limit in the AWS CodeCommit console | Maximum of 6 MB\. | 
| File paths | Any combination of allowed characters between 1 and 4,096 characters in length\. File paths must be an unambiguous name that specifies the file and the exact location of the file\. File paths cannot exceed 20 directories in depth\. In addition, file paths cannot:[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codecommit/latest/userguide/limits.html) File names and paths must be fully qualified\. The name and path to a file on your local computer must follow the standards for that operating system\. When specifying the path to a file in an AWS CodeCommit repository, use the standards for Amazon Linux\. | 
| Git blob size |  Maximum of 2 GB\.  There is no limit on the number or the total size of all files in a single commit, as long as the metadata does not exceed 6 MB and a single blob does not exceed 2 GB\.   | 
| Custom data for triggers | This is a string field limited to 1,000 characters\. It cannot be used to pass any dynamic parameters\.  | 
| Graph display of branches in the Commit Visualizer | 35 per page\. If there are more than 35 branches on a single page, the graph is not displayed\. | 