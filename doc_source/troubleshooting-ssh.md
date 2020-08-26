# Troubleshooting SSH connections to AWS CodeCommit<a name="troubleshooting-ssh"></a>

The following information might help you troubleshoot common issues when using SSH to connect to CodeCommit repositories\.

**Topics**
+ [Access error: Public key is uploaded successfully to IAM but connection fails on Linux, macOS, or Unix systems](#troubleshooting-ae4)
+ [Access error: Public key is uploaded successfully to IAM and SSH tested successfully but connection fails on Windows systems](#troubleshooting-ae5)
+ [Authentication challenge: Authenticity of host can't be established when connecting to a CodeCommit repository](#troubleshooting-ac1)
+ [IAM error: 'Invalid format' when attempting to add a public key to IAM](#troubleshooting-iam1)
+ [I need to access CodeCommit repositories in multiple AWS accounts with SSH credentials](#troubleshooting-ssh-multi)
+ [Git on Windows: Bash emulator or command line freezes when attempting to connect using SSH](#troubleshooting-gw2)

## Access error: Public key is uploaded successfully to IAM but connection fails on Linux, macOS, or Unix systems<a name="troubleshooting-ae4"></a>

**Problem:** When you try to connect to an SSH endpoint to communicate with a CodeCommit repository, either when testing the connection or cloning a repository, the connection fails or is refused\.

**Possible fixes:** The SSH key ID assigned to your public key in IAM might not be associated with your connection attempt\. [You might not have configured a config file](setting-up-ssh-unixes.md#cc-configure-config), you might not have access to the configuration file, another setting might be preventing a successful read of the config file, you might have provided the wrong key ID, or you might have provided the ID of the IAM user instead of the key ID\.

The SSH key ID can be found in the IAM console in the profile for your IAM user:

![\[The SSH Key ID in the IAM console\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-ssh-key-id-iam.png)![\[The SSH Key ID in the IAM console\]](http://docs.aws.amazon.com/codecommit/latest/userguide/)

**Note**  
If you have more than one SSH key IDs uploaded, the keys are listed alphabetically by key ID, not by upload date\. Make sure that you have copied the key ID that is associated with the correct upload date\.

Try testing the connection with the following command:

```
ssh Your-SSH-Key-ID@git-codecommit.us-east-2.amazonaws.com
```

If you see a success message after confirming the connection, your SSH key ID is valid\. Edit your config file to associate your connection attempts with your public key in IAM\. If you do not want to edit your config file, you can preface all connection attempts to your repository with your SSH key ID\. For example, if you wanted to clone a repository named *MyDemoRepo* without modifying your config file to associate your connection attempts, you would run the following command:

```
git clone ssh://Your-SSH-Key-ID@git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo my-demo-repo
```

For more information, see [For SSH connections on Linux, macOS, or Unix](setting-up-ssh-unixes.md)\. 

## Access error: Public key is uploaded successfully to IAM and SSH tested successfully but connection fails on Windows systems<a name="troubleshooting-ae5"></a>

**Problem:** When you try to use an SSH endpoint to clone or communicate with a CodeCommit repository, an error message appears containing the phrase `No supported authentication methods available`\.

**Possible fixes:** The most common reason for this error is that you have a Windows system environment variable set that directs Windows to use another program when you attempt to use SSH\. For example, you might have set a GIT\_SSH variable to point to one of the PuTTY set of tools \(plink\.exe\)\. This might be a legacy configuration, or it might be required for one or more other programs installed on your computer\. If you are sure that this environment variable is not required, you can remove it by opening your system properties\.

To work around this issue, open a Bash emulator and then try your SSH connection again, but include `GIT_SSH_COMMAND="SSH"` as a prefix\. For example, to clone a repository using SSH:

```
GIT_SSH_COMMAND="ssh" git clone ssh://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo my-demo-repo
```

A similar problem might occur if your version of Windows requires that you include the SSH key ID as part of the connection string when connecting through SSH at the Windows command line\. Try your connection again, this time including the SSH key ID copied from IAM as part of the command\. For example:

```
git clone ssh://Your-SSH-Key-ID@git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo my-demo-repo
```

## Authentication challenge: Authenticity of host can't be established when connecting to a CodeCommit repository<a name="troubleshooting-ac1"></a>

**Problem:** When you try to use an SSH endpoint to communicate with a CodeCommit repository, a warning message appears containing the phrase `The authenticity of host 'host-name' can't be established.`

**Possible fixes:** Your credentials might not be set up correctly\. Follow the instructions in [For SSH connections on Linux, macOS, or Unix](setting-up-ssh-unixes.md) or [For SSH connections on Windows](setting-up-ssh-windows.md)\. 

If you have followed those steps and the problem persists, someone might be attempting a man\-in\-the\-middle attack\. When you see the following message, type `no`, and press Enter\.

```
Are you sure you want to continue connecting (yes/no)?
```

Make sure the fingerprint and public key for CodeCommit connections match those documented in the SSH setup topics before you continue with the connection\.


**Public fingerprints for CodeCommit**  

| Server | Cryptographic hash type | Fingerprint | 
| --- | --- | --- | 
| git\-codecommit\.us\-east\-2\.amazonaws\.com | MD5 | a9:6d:03:ed:08:42:21:be:06:e1:e0:2a:d1:75:31:5e | 
| git\-codecommit\.us\-east\-2\.amazonaws\.com | SHA256 | 3lBlW2g5xn/NA2Ck6dyeJIrQOWvn7n8UEs56fG6ZIzQ | 
| git\-codecommit\.us\-east\-1\.amazonaws\.com | MD5 | a6:9c:7d:bc:35:f5:d4:5f:8b:ba:6f:c8:bc:d4:83:84 | 
| git\-codecommit\.us\-east\-1\.amazonaws\.com | SHA256 | eLMY1j0DKA4uvDZcl/KgtIayZANwX6t8\+8isPtotBoY | 
| git\-codecommit\.us\-west\-2\.amazonaws\.com | MD5 | a8:68:53:e3:99:ac:6e:d7:04:7e:f7:92:95:77:a9:77 | 
| git\-codecommit\.us\-west\-2\.amazonaws\.com | SHA256 | 0pJx9SQpkbPUAHwy58UVIq0IHcyo1fwCpOOuVgcAWPo | 
| git\-codecommit\.eu\-west\-1\.amazonaws\.com | MD5 | 93:42:36:ea:22:1f:f1:0f:20:02:4a:79:ff:ea:12:1d | 
| git\-codecommit\.eu\-west\-1\.amazonaws\.com | SHA256 | tKjRkOL8dmJyTmSbeSdN1S8F/f0iql3RlvqgTOP1UyQ | 
| git\-codecommit\.ap\-northeast\-1\.amazonaws\.com | MD5 | 8e:a3:f0:80:98:48:1c:5c:6f:59:db:a7:8f:6e:c6:cb | 
| git\-codecommit\.ap\-northeast\-1\.amazonaws\.com | SHA256 | Xk/WeYD/K/bnBybzhiuu4dWpBJtXPf7E30jHU7se4Ow | 
| git\-codecommit\.ap\-southeast\-1\.amazonaws\.com | MD5 | 65:e5:27:c3:09:68:0d:8e:b7:6d:94:25:80:3e:93:cf | 
| git\-codecommit\.ap\-southeast\-1\.amazonaws\.com | SHA256 | ZIsVa7OVzxrTIf\+Rk4UbhPv6Es22mSB3uTBojfPXIno | 
| git\-codecommit\.ap\-southeast\-2\.amazonaws\.com | MD5 | 7b:d2:c1:24:e6:91:a5:7b:fa:c1:0c:35:95:87:da:a0 | 
| git\-codecommit\.ap\-southeast\-2\.amazonaws\.com | SHA256 | nYp\+gHas80HY3DqbP4yanCDFhqDVjseefVbHEXqH2Ec | 
| git\-codecommit\.eu\-central\-1\.amazonaws\.com | MD5 | 74:5a:e8:02:fc:b2:9c:06:10:b4:78:84:65:94:22:2d | 
| git\-codecommit\.eu\-central\-1\.amazonaws\.com | SHA256 | MwGrkiEki8QkkBtlAgXbYt0hoZYBnZF62VY5RzGJEUY | 
| git\-codecommit\.ap\-northeast\-2\.amazonaws\.com | MD5 | 9f:68:48:9b:5f:fc:96:69:39:45:58:87:95:b3:69:ed | 
| git\-codecommit\.ap\-northeast\-2\.amazonaws\.com | SHA256 | eegAPQrWY9YsYo9ZHIKOmxetfXBHzAZd8Eya53Qcwko | 
| git\-codecommit\.sa\-east\-1\.amazonaws\.com | MD5 | 74:99:9d:ff:2b:ef:63:c6:4b:b4:6a:7f:62:c5:4b:51 | 
| git\-codecommit\.sa\-east\-1\.amazonaws\.com | SHA256 | kW\+VKB0jpRaG/ZbXkgbtMQbKgEDK7JnISV3SVoyCmzU | 
| git\-codecommit\.us\-west\-1\.amazonaws\.com | MD5 | 3b:76:18:83:13:2c:f8:eb:e9:a3:d0:51:10:32:e7:d1 | 
| git\-codecommit\.us\-west\-1\.amazonaws\.com | SHA256 | gzauWTWXDK2u5KuMMi5vbKTmfyerdIwgSbzYBODLpzg | 
| git\-codecommit\.eu\-west\-2\.amazonaws\.com | MD5 | a5:65:a6:b1:84:02:b1:95:43:f9:0e:de:dd:ed:61:d3 | 
| git\-codecommit\.eu\-west\-2\.amazonaws\.com | SHA256 | r0Rwz5k/IHp/QyrRnfiM9j02D5UEqMbtFNTuDG2hNbs | 
| git\-codecommit\.ap\-south\-1\.amazonaws\.com | MD5 | da:41:1e:07:3b:9e:76:a0:c5:1e:64:88:03:69:86:21 | 
| git\-codecommit\.ap\-south\-1\.amazonaws\.com | SHA256 | hUKwnTj7\+Xpx4Kddb6p45j4RazIJ4IhAMD8k29itOfE | 
| git\-codecommit\.ca\-central\-1\.amazonaws\.com | MD5 | 9f:7c:a2:2f:8c:b5:74:fd:ab:b7:e1:fd:af:46:ed:23 | 
| git\-codecommit\.ca\-central\-1\.amazonaws\.com | SHA256 | Qz5puafQdANVprLlj6r0Qyh4lCNsF6ob61dGcPtFS7w | 
| git\-codecommit\.eu\-west\-3\.amazonaws\.com | MD5 | 1b:7f:97:dd:d7:76:8a:32:2c:bd:2c:7b:33:74:6a:76 | 
| git\-codecommit\.eu\-west\-3\.amazonaws\.com | SHA256 | uw7c2FL564jVoFgtc\+ikzILnKBsZz7t9\+CFdSJjKbLI | 
| git\-codecommit\.us\-gov\-west\-1\.amazonaws\.com | MD5 | 9f:6c:19:3b:88:cd:e8:88:1b:9c:98:6a:95:31:8a:69 | 
| git\-codecommit\.us\-gov\-west\-1\.amazonaws\.com | SHA256 | djXQoSIFcg8vHe0KVH1xW/gOF9X37tWTqu4Hkng75x4 | 
| git\-codecommit\.us\-gov\-east\-1\.amazonaws\.com | MD5 | 00:8d:b5:55:6f:05:78:05:ed:ea:cb:3f:e6:f0:62:f2 | 
| git\-codecommit\.us\-gov\-east\-1\.amazonaws\.com | SHA256 | fVb\+R0z7qW7minenW\+rUpAABRCRBTCzmETAJEQrg98 | 
| git\-codecommit\.eu\-north\-1\.amazonaws\.com | MD5 | 8e:53:d8:59:35:88:82:fd:73:4b:60:8a:50:70:38:f4 | 
| git\-codecommit\.eu\-north\-1\.amazonaws\.com | SHA256 | b6KSK7xKq\+V8jl7iuAcjqXsG7zkqoUZZmmhYYFBq1wQ | 
| git\-codecommit\.me\-south\-1\.amazonaws\.com | MD5 | 0e:39:28:56:d5:41:e6:8d:fa:81:45:37:fb:f3:cd:f7 | 
| git\-codecommit\.me\-south\-1\.amazonaws\.com | SHA256 | O\+NToCGgjrHekiBuOl0ad7ROGEsz\+DBLXOd/c9wc0JU | 
| git\-codecommit\.ap\-east\-1\.amazonaws\.com | MD5 | a8:00:3d:24:52:9d:61:0e:f6:e3:88:c8:96:01:1c:fe | 
| git\-codecommit\.ap\-east\-1\.amazonaws\.com | SHA256 | LafadYwUYW8hONoTRpojbjNs9IRnbEwHtezD3aAIBX0 | 
| git\-codecommit\.cn\-north\-1\.amazonaws\.com\.cn | MD5 | 11:7e:2d:74:9e:3b:94:a2:69:14:75:6f:5e:22:3b:b3 | 
| git\-codecommit\.cn\-north\-1\.amazonaws\.com\.cn | SHA256 | IYUXxH2OpTDsyYMLIp\+JY8CTLS4UX\+ZC5JVZXPRaxc8 | 
| git\-codecommit\.cn\-northwest\-1\.amazonaws\.com\.cn | MD5 | 2e:a7:fb:4c:33:ac:6c:f9:aa:f2:bc:fb:0a:7b:1e:b6 | 
| git\-codecommit\.cn\-northwest\-1\.amazonaws\.com\.cn | SHA256 | wqjd6eHd0\+mOBx\+dCNuL0omUoCNjaDtZiEpWj5TmCfQ | 

## IAM error: 'Invalid format' when attempting to add a public key to IAM<a name="troubleshooting-iam1"></a>

**Problem:** In IAM, when attempting to set up to use SSH with CodeCommit, an error message appears containing the phrase `Invalid format` when you attempt to add your public key\.

**Possible fixes:** IAM accepts public keys in the OpenSSH format only and  has additional requirements as specified in [Use SSH Keys with CodeCommit](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_ssh-keys.html#ssh-keys-code-commit) in the *IAM User Guide*\. If you provide your public key in another format, or if the key does not contain the required number of bits, you see this error\. 
+ When you copied the SSH public key, your operating system might have introduced line breaks\. Make sure that there are no line breaks in the public key that you add to IAM\.
+ Some Windows operating systems do not use the OpenSSH format\. To generate a key pair and copy the OpenSSH format required by IAM, see [SSH and Windows: Set up the public and private keys for Git and CodeCommit](setting-up-ssh-windows.md#setting-up-ssh-windows-keys-windows)\.

For more information about the requirements for SSH keys in IAM, see [Use SSH Keys with CodeCommit](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_ssh-keys.html#ssh-keys-code-commit) in the *IAM User Guide*\.

## I need to access CodeCommit repositories in multiple AWS accounts with SSH credentials<a name="troubleshooting-ssh-multi"></a>

**Problem:** I want to set up SSH access to CodeCommit repositories in more than one AWS account\.

**Possible fixes:** You can create unique SSH public/private key pairs for each AWS account and configure IAM with each key\. You can then configure your \~/\.ssh/config file with information about each IAM User ID associated with the public key\. For example:

```
Host codecommit-1
    Hostname git-codecommit.us-east-1.amazonaws.com
    User SSH-KEY-ID-1 # This is the SSH Key ID you copied from IAM in AWS account 1 (for example, APKAEIBAERJR2EXAMPLE1).
    IdentityFile ~/.ssh/codecommit_rsa # This is the path to the associated public key file, such as id_rsa.  We advise creating CodeCommit specific _rsa files.
 
Host codecommit-2
    Hostname git-codecommit.us-east-1.amazonaws.com
    User SSH-KEY-ID-2 # This is the SSH Key ID you copied from IAM in AWS account 2 (for example, APKAEIBAERJR2EXAMPLE2).
    IdentityFile ~/.ssh/codecommit_2_rsa # This is the path to the other associated public key file.  We advise creating CodeCommit specific _rsa files.
```

In this configuration, you will be able to replace 'git\-codecommit\.us\-east\-1\.amazonaws\.com' with 'codecommit\-2'\. For example, to clone a repository in your second AWS account:

```
git clone ssh://codecommit-2/v1/repos/YourRepositoryName
```

To set up a remote for your repository, run git remote add\. For example:

```
git remote add origin ssh://codecommit-2/v1/repos/YourRepositoryName
```

For more examples, see [this forum post](https://forums.aws.amazon.com/thread.jspa?messageID=711158) and [this contribution on GitHub](https://gist.github.com/justinpawela/3a7056cd592d688425e59de2ef6f1da0)\.

## Git on Windows: Bash emulator or command line freezes when attempting to connect using SSH<a name="troubleshooting-gw2"></a>

**Problem:** After you configure SSH access for Windows and confirm connectivity at the command line or terminal, you see a message that the server's host key is not cached in the registry, and the prompt to store the key in the cache is frozen \(does not accept y/n/return input\) when you attempt to use commands such as git pull, git push, or git clone at the command prompt or Bash emulator\.

**Possible fixes:** The most common cause for this error is that your Git environment is configured to use something other than OpenSSH for authentication \(probably PuTTY\)\. This is known to cause problems with the caching of keys in some configurations\. To fix this problem, try one of the following:
+ Open a Bash emulator and add the `GIT_SSH_COMMAND="ssh"` parameter before the Git command\. For example, if you are attempting to push to a repository, instead of typing git push, type: 

  ```
  GIT_SSH_COMMAND="ssh" git push
  ```
+ If you have PuTTY installed, open PuTTY, and in **Host Name \(or IP address\)**, enter the CodeCommit endpoint you want to reach \(for example, git\-codecommit\.us\-east\-2\.amazonaws\.com\)\. Choose **Open**\. When prompted by the PuTTY security alert, choose **Yes** to permanently cache the key\.
+ Rename or delete the `GIT_SSH` environment variable if you are no longer using it\. Then open a new command prompt or Bash emulator session, and try your command again\.

For other solutions, see [Git clone/pull continually freezing at Store key in cache](http://stackoverflow.com/questions/33240137/git-clone-pull-continually-freezing-at-store-key-in-cache) on Stack Overflow\. 