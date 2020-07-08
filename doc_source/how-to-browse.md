# Browse files in an AWS CodeCommit repository<a name="how-to-browse"></a>

After you connect to a CodeCommit repository, you can clone it to a local repo or use the CodeCommit console to browse its contents\. This topic describes how to use the CodeCommit console to browse the content of a CodeCommit repository\.

**Note**  
For active CodeCommit users, there is no charge for browsing code from the CodeCommit console\. For information about when charges might apply, see [Pricing](http://aws.amazon.com/codecommit/pricing/)\.

![\[A view of the contents of a file in the CodeCommit console\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-code-browse-file.png)

## Browse a CodeCommit repository<a name="how-to-browse-console"></a>

You can use the CodeCommit console to review the files contained in a repository or to quickly read the contents of a file\. 

**To browse the content of a repository**

1. Open the CodeCommit console at [https://console\.aws\.amazon\.com/codesuite/codecommit/home](https://console.aws.amazon.com/codesuite/codecommit/home)\.

1. On the **Repositories** page, from the list of repositories, choose the repository you want to browse\. 

1.  In the **Code** view, browse the contents of the default branch for your repository\. 

   To change the view to a different branch or tag, choose the view selector button\. Either choose a branch or tag name from the drop\-down list, or in the filter box, enter the name of the branch or tag, and then choose it from the list\.

1. Do one of the following:
   + To view the contents of a directory, choose it from the list\. You can choose any of the directories in the navigation list to return to that directory view\. You can also use the up arrow at the top of the directory list\.
   + To view the contents of a file, choose it from the list\. If the file is larger than the commit object limit, it cannot be displayed in the console and must be viewed in a local repo instead\. For more information, see [Quotas](limits.md)\. To exit the file view, from the code navigation bar, choose the directory you want to view\.
**Note**  
If you choose a binary file, a warning message appears, asking you to confirm that you want to display the contents\. To view the file, choose **Show file contents**\. If you do not want to view the file, from the code navigation bar, choose the directory you want to view\.  
If you choose a markdown file \(\.md\), use the **Rendered Markdown** and **Markdown Source** buttons to toggle between the rendered and syntax views\.