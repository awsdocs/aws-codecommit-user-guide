--------

 The procedures in this guide support the new console design\. If you choose to use the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\.

--------

# Troubleshooting SSH Connections to AWS CodeCommit<a name="troubleshooting-ssh"></a>

The following information might help you troubleshoot common issues when using SSH to connect to AWS CodeCommit repositories\.

**Topics**
+ [Access Error: Public Key Is Uploaded Successfully to IAM but Connection Fails on Linux, macOS, or Unix Systems](#troubleshooting-ae4)
+ [Access Error: Public Key Is Uploaded Successfully to IAM and SSH Tested Successfully but Connection Fails on Windows Systems](#troubleshooting-ae5)
+ [Authentication Challenge: Authenticity of Host Can't Be Established When Connecting to an AWS CodeCommit Repository](#troubleshooting-ac1)
+ [IAM Error: 'Invalid format' when attempting to add a public key to IAM](#troubleshooting-iam1)
+ [Git on Windows: Bash Emulator or Command Line Freezes When Attempting to Connect Using SSH](#troubleshooting-gw2)

## Access Error: Public Key Is Uploaded Successfully to IAM but Connection Fails on Linux, macOS, or Unix Systems<a name="troubleshooting-ae4"></a>

**Problem:** When you try to connect to an SSH endpoint to communicate with an AWS CodeCommit repository, either when testing the connection or cloning a repository, the connection fails or is refused\.

**Possible fixes:** The SSH Key ID assigned to your public key in IAM might not be associated with your connection attempt\. [You might not have configured a config file](setting-up-ssh-unixes.md#cc-configure-config), you might not have access to the configuration file, another setting might be preventing a successful read of the config file, or you might have provided the ID of the IAM user instead of the key ID\.

The SSH Key ID can be found in the IAM console in the profile for your IAM user:

![\[The SSH Key ID in the IAM console\]](http://docs.aws.amazon.com/codecommit/latest/userguide/images/codecommit-ssh-key-id-iam.png)![\[The SSH Key ID in the IAM console\]](http://docs.aws.amazon.com/codecommit/latest/userguide/)

Try testing the connection with the following command:

```
ssh Your-SSH-Key-ID@git-codecommit.us-east-2.amazonaws.com
```

If you see a success message after confirming the connection, your SSH Key ID is valid\. Edit your config file to associate your connection attempts with your public key in IAM\. If you do not want to edit your config file for some reason, you can preface all connection attempts to your repository with your SSH Key ID\. For example, if you wanted to clone a repository named *MyDemoRepo* without modifying your config file to associate your connection attempts, you would type the following command:

```
git clone ssh://Your-SSH-Key-ID@git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo my-demo-repo
```

For more information, see [For SSH Connections on Linux, macOS, or Unix](setting-up-ssh-unixes.md)\. 

## Access Error: Public Key Is Uploaded Successfully to IAM and SSH Tested Successfully but Connection Fails on Windows Systems<a name="troubleshooting-ae5"></a>

**Problem:** When you try to use an SSH endpoint to clone or communicate with an AWS CodeCommit repository, an error message appears containing the phrase `No supported authentication methods available`\.

**Possible fixes:** The most common reason for this error is that you have a Windows system environment variable set that directs Windows to use another program when you attempt to use SSH\. For example, you might have set a GIT\_SSH variable to point to one of the PuTTY set of tools \(plink\.exe\)\. This might be a legacy configuration, or it might be necessary for one or more other programs installed on your computer\. If you are sure that this environment variable is not needed, you can remove it by opening your system properties and deleting the environment variable\.

To work around this issue, open a Bash emulator and then try your SSH connection again, but include `GIT_SSH_COMMAND="SSH"` as a prefix\. For example, to clone a repository using SSH:

```
GIT_SSH_COMMAND="ssh" git clone ssh://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo my-demo-repo
```

A similar problem might occur if your version of Windows requires that you include the SSH Key ID as part of the connection string when connecting using SSH at the Windows command line\. Try your connection again, this time including the SSH Key ID copied from IAM as part of the command\. For example:

```
git clone ssh://Your-SSH-Key-ID@git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo my-demo-repo
```

## Authentication Challenge: Authenticity of Host Can't Be Established When Connecting to an AWS CodeCommit Repository<a name="troubleshooting-ac1"></a>

**Problem:** When you try to use an SSH endpoint to communicate with an AWS CodeCommit repository, a warning message appears containing the phrase `The authenticity of host 'host-name' can't be established.`

**Possible fixes:** Your credentials might not be set up correctly\. Follow the instructions in [For SSH Connections on Linux, macOS, or Unix](setting-up-ssh-unixes.md) or [For SSH Connections on Windows](setting-up-ssh-windows.md)\. 

If you have followed those steps and the problem persists, someone might be attempting a man\-in\-the\-middle attack\. When you see the following message, type `no`, and press Enter\.

```
Are you sure you want to continue connecting (yes/no)?
```

Make sure the fingerprint and public key for AWS CodeCommit connections match those documented in the SSH setup topics before you continue with the connection\.


**Public fingerprints for AWS CodeCommit**  

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

## IAM Error: 'Invalid format' when attempting to add a public key to IAM<a name="troubleshooting-iam1"></a>

**Problem:** In IAM, when attempting to set up to use SSH with AWS CodeCommit, an error message appears containing the phrase `Invalid format` when you attempt to add your public key\.

**Possible fixes:** IAM accepts public keys in the OpenSSH format only\. If you provide your public key in another format, or if the key does not contain the required number of bits, you will see this error\. This problem most commonly occurs when the public/private key pairs are generated on Windows computers\. To generate a key pair and copy the OpenSSH format required by IAM, see [SSH and Windows: Set Up the Public and Private Keys for Git and AWS CodeCommit](setting-up-ssh-windows.md#setting-up-ssh-windows-keys-windows)\.

## Git on Windows: Bash Emulator or Command Line Freezes When Attempting to Connect Using SSH<a name="troubleshooting-gw2"></a>

**Problem:** After you configure SSH access for Windows and confirm connectivity at the command line or terminal, you see a message that the server's host key is not cached in the registry, and the prompt to store the key in the cache is frozen \(does not accept y/n/return input\) when you attempt to use commands such as git pull, git push, or git clone at the command prompt or Bash emulator\.

**Possible fixes:** The most common cause for this error is that your Git environment is configured to use something other than OpenSSH for authentication \(probably PuTTY\)\. This is known to cause problems with the caching of keys in some configurations\. To fix this problem, try one of the following:
+ Open a Bash emulator and add the `GIT_SSH_COMMAND="ssh"` parameter before the Git command\. For example, if you are attempting to push to a repository, instead of typing git push, type: 

  ```
  GIT_SSH_COMMAND="ssh" git push
  ```
+ If you have PuTTY installed, open PuTTY, and in **Host Name \(or IP address\)**, type the AWS CodeCommit endpoint you want to reach \(for example, git\-codecommit\.us\-east\-2\.amazonaws\.com\)\. Choose **Open**\. When prompted by the PuTTY Security Alert, choose **Yes** to permanently cache the key\.
+ Rename or delete the `GIT_SSH` environment variable if you are no longer using it\. Then open a new command prompt or Bash emulator session, and try your command again\.

For other solutions, see [Git clone/pull continually freezing at Store key in cache](http://stackoverflow.com/questions/33240137/git-clone-pull-continually-freezing-at-store-key-in-cache) on Stack Overflow\. 