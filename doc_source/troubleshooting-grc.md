# Troubleshooting git\-remote\-codecommit and AWS CodeCommit<a name="troubleshooting-grc"></a>

The following information might help you troubleshoot issues with git\-remote\-codecommit when connecting with AWS CodeCommit repositories\.

**Topics**
+ [I see an error: git: 'remote\-codecommit' is not a git command](#troubleshooting-grc-syn1)
+ [I see an error: fatal: Unable to find remote helper for 'codecommit'](#troubleshooting-grc-syn2)
+ [Cloning error: I cannot clone a CodeCommit repository from an IDE](#troubleshooting-grc-ide1)
+ [Push or pull error: I cannot push or pull commits from an IDE to a CodeCommit repository](#troubleshooting-grc-ide2)

## I see an error: git: 'remote\-codecommit' is not a git command<a name="troubleshooting-grc-syn1"></a>

**Problem:** When you try to use git\-remote\-codecommit, you see an error that git\-remote\-codecommit is not a git command\. See 'git \-\-help'"\.

**Possible fixes:** The most common reason for this error is a syntax error\. This can happen where a hyphen is missing between git and remote\-codecommit, or when an extra git is placed before git\-remote\-codecommit\.

For more information about setting up and using git\-remote\-codecommit, see [Setup steps for HTTPS connections to AWS CodeCommit with git\-remote\-codecommit](setting-up-git-remote-codecommit.md)\.

## I see an error: fatal: Unable to find remote helper for 'codecommit'<a name="troubleshooting-grc-syn2"></a>

**Problem:** When you try to use git\-remote\-codecommit, you see an error stating "fatal: Unable to find remote helper for 'codecommit'"\.

**Possible fixes:** The most common reason for this error is that the setup is not complete for git\-remote\-codecommit, Python is not in your path, or you are using a terminal or command line window that has not been restarted since the installation of git\-remote\-codecommit completed\.

For more information about setting up and using git\-remote\-codecommit, see [Setup steps for HTTPS connections to AWS CodeCommit with git\-remote\-codecommit](setting-up-git-remote-codecommit.md)\.

## Cloning error: I cannot clone a CodeCommit repository from an IDE<a name="troubleshooting-grc-ide1"></a>

**Problem:** When you try to clone a CodeCommit repository in an IDE, you see an error that says the endpoint or URL is not valid\.

**Possible fixes:** Not all IDEs support the URL used by git\-remote\-codecommit during cloning\. Clone the repository locally from the terminal or command line, and then add that local repo to your IDE\. For more information, see [Step 3: Connect to the CodeCommit console and clone the repository](setting-up-git-remote-codecommit.md#setting-up-git-remote-codecommit-connect-console)\.

## Push or pull error: I cannot push or pull commits from an IDE to a CodeCommit repository<a name="troubleshooting-grc-ide2"></a>

**Problem:** When you try to pull or push code from an IDE, you see a connection error\.

**Possible fixes:** The most common reason for this error is that the IDE is not compatible with Git remote helpers such as git\-remote\-codecommit\. Instead of using the IDE functionality to commit, push, and pull code, update the local repo manually from the command line or terminal using Git commands\.

For more information about remote helpers and Git, see the [Git documentation](https://git-scm.com/docs/git-remote-helpers)\.