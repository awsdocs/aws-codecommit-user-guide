# Troubleshooting AWS CodeCommit<a name="troubleshooting"></a>

The following information might help you troubleshoot common issues in AWS CodeCommit\.

**Topics**
+ [Troubleshooting Git credentials and HTTPS connections to AWS CodeCommit](troubleshooting-gc.md)
+ [Troubleshooting git\-remote\-codecommit and AWS CodeCommit](troubleshooting-grc.md)
+ [Troubleshooting SSH connections to AWS CodeCommit](troubleshooting-ssh.md)
+ [Troubleshooting the credential helper and HTTPS connections to AWS CodeCommit](troubleshooting-ch.md)
+ [Troubleshooting Git clients and AWS CodeCommit](troubleshooting-git.md)
+ [Troubleshooting access errors and AWS CodeCommit](troubleshooting-ae.md)
+ [Troubleshooting configuration errors and AWS CodeCommit](troubleshooting-cf.md)
+ [Troubleshooting console errors and AWS CodeCommit](troubleshooting-cs.md)
+ [Troubleshooting triggers and AWS CodeCommit](troubleshooting-ti.md)
+ [Turn on debugging](#troubleshooting-debug)

## Turn on debugging<a name="troubleshooting-debug"></a>

**Problem:** I want to turn on debugging to get more information about my repository and how Git is executing commands\. 

**Possible fixes:** Try the following:

1. At the terminal or command prompt, run the following commands on your local machine before running Git commands:

   On Linux, macOS, or Unix:

   ```
   export GIT_TRACE_PACKET=1
   export GIT_TRACE=1
   export GIT_CURL_VERBOSE=1
   ```

   On Windows:

   ```
   set GIT_TRACE_PACKET=1
   set GIT_TRACE=1
   set GIT_CURL_VERBOSE=1
   ```
**Note**  
Setting `GIT_CURL_VERBOSE` is useful for HTTPS connections only\. SSH does not use the `libcurl` library\.

1. To get more information about your Git repository, create a shell script similar to the following, and then run the script:

   ```
   #!/bin/sh
   
   gc_output=`script -q -c 'git gc' | grep Total`
   object_count=$(echo $gc_output | awk -F ' |\(|\)' '{print $2}')
   delta_count=$(echo $gc_output | awk -F ' |\(|\)' '{print $5}')
   
   verify_pack_output=`git verify-pack -v objects/pack/pack-*.pack .git/objects/pack/pack-*.pack 2>/dev/null`
   largest_object=$(echo "$verify_pack_output" | grep blob | sort -k3nr | head -n 1 | awk '{print $3/1024" KiB"}')
   largest_commit=$(echo "$verify_pack_output" | grep 'tree\|commit\|tag' | sort -k3nr | head -n 1 | awk '{print $3/1024" KiB"}')
   longest_delta_chain=$(echo "$verify_pack_output" | grep chain | tail -n 1 | awk -F ' |:' '{print $4}')
   
   branch_count=`git branch -a | grep remotes/origin | grep -v HEAD | wc -l`
   if [ $branch_count -eq 0 ]; then
       branch_count=`git branch -l | wc -l`
   fi
   
   echo "Size: `git count-objects -v | grep size-pack | awk '{print $2}'` KiB"
   echo "Branches: $branch_count"
   echo "Tags: `git show-ref --tags | wc -l`"
   echo "Commits: `git rev-list --all | wc -l`"
   echo "Objects: $object_count"
   echo "Delta objects: $delta_count"
   echo "Largest blob: $largest_object"
   echo "Largest commit/tag/tree: $largest_commit"
   echo "Longest delta chain: $longest_delta_chain"
   ```

1. If these steps do not provide enough information for you to resolve the issue on your own, ask for help on [the AWS CodeCommit forum](https://forums.aws.amazon.com/forumID=189)\. Be sure to include relevant output from these steps in your post\.