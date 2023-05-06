# Basic Commands for Linux System Administration


[Disable root login to ssh (with sed command)](https://github.com/shpweb/linux_system_admin#disable-root-login-to-ssh-with-sed-command-1)
[How to set timezone]()
#### find specific patterns of files at one directory and copy those to another directory
#### How to setup SSH Password-less login to remote linux system



================================================================================
## disable root login to ssh (with sed command)

#### 1. disable the root login in sshd_config file
Disable the root login
```s
sudo sed -i "s|PermitRootLogin yes|PermitRootLogin no|g" /etc/ssh/sshd_config
```

Disable the root login evenif commented the string
```s
sudo sed -i "s|#PermitRootLogin yes|PermitRootLogin no|g" /etc/ssh/sshd_config
```

Restart the service to take effect the change
```s
sudo systemctl restart sshd
```

Validate if RootLogin disabled
```s
sudo cat /etc/ssh/sshd_config | grep RootLogin
```


#### 2. disable the root login into /etc/passwd
```s
sudo sed -i "s|/root:/bin/bash|/root:/sbin/nologin|g" /etc/passwd
```


## How to set timezone

### 1. See which timezone is currently set in System

```s
date
```

```s
timedatectl
```

### 2. List the timezone so the required timezone selected correctly to set in system

```s
timedatectl list-timezones
```

Above command will list the files in /usr/share/zoneinfo/ and so alternatevily, you can list the files in that path to know the timezone

```s
ls /usr/share/zoneinfo/America/
```
```s
ls /usr/share/zoneinfo/Europe/
```

```s
ls /usr/share/zoneinfo/Asia/
```

### 3. Set the Timezone for the givne system
##### Option-1: timedatectl command
```s
timedatectl set-timezone <timezone to set>
```
##### Option-2: symlink to set the timezone
```s
rm -rf /etc/localtime
```
```s
ln -s /usr/share/zoneinfo/<timezone to set> /etc/localtime
```


## find specific patterns of files at one directory and copy those to another directory

Find the files having .mp3 extensions and copy to target directory with preserveing the parent directory path
```s
find /home/userdata -name '*.mp3' -exec cp --parents \{\} /home/target \;
```

Find the files in a directory with owner "testuser" and copy those file to target directory with preserveing the parent directory path and ownership
```s
find /home/userdata -type f -user "testuser" -exec cp --parents \{\} -p /home/target \;
```

In above commands:

- find – command to find files and folders in Unix-like systems.
- /home/userdata - source directory where find command will run and copy data from
- -name ‘*.mp3’ – search for files matching with extension .mp3.
- -exec cp – execute the ‘cp’ command to copy files from source to destination directory.
- --parents - create the intermediate parent directories if needed to preserve the parent directory structure.
- -p preserve the ownership and permissions
- \{\} – is automatically replaced with the file name of the files found by ‘find’ command. And the braces are escaped to protect them from expansion by the shell in some "find" command versions. You can also use {} without escape characters.
- /home/target – target directory to save the matching files.
\; – indicates it that the commands to be executed are now complete, and to carry out the command again on the next match.



## How to setup SSH Password-less login to remote linux system

**Step 1: Generate a key pair**
Use ssh-keygen to generate a key pair consisting of a public key and a private key on the client computer:
```s
ssh-key-gen -t rsa
```
The -t rsa option specifies that the type of the key should be RSA. Other choices include DSA, ECDSA, and ED25519. Select the protocol your SSH connection will use.

When prompted, enter a filename for the key.
```s
Enter file in which to save the key.
(\home\<current-User>\.ssh\id_rsa):
```

The default (id_rsa in the .ssh directory under the user’s home directory) works perfectly in most cases (and if this is to be your primary key, or only key, it is often the best option). **Hit enter to accept the default**.  
Then enter the passphrase when prompted.

```s
Enter Passphrase (empty for no passphrase):
```

Adding a passphrase is an important step for securing the local key, which otherwise will be usable by anyone who acquires the key itself. Choose a passphrase with the same rigor that you would use to create any secure passphrase. Some clients can be configured to save passphrases for a true “passwordless” access experience, while others may require it to be entered with each use.

With the initial step to set up SSH passwordless login using ssh keygen completed, you now have two files:

- id_rsa contains the private key.
- id_rsa.pub contains the public key.

**Step 2: Upload public key to remote server**

Uploading your public key with a Linux or macOS client - 
On a macOS or Linux client, use ssh-copy-id to propagate the public key to the remote server, like this:
```s
ssh-copy-id <remote-user>@<remote-server>
```
Make sure to replace user with a valid username and remote-server with the valid IP or domain of the server.

It may ask the password of remote-user to connect remote-server

**Step 3: Test connection**

Now, when you reopen it and try to connect to the remote server again from the client where you have your private key saved, you will not recieve a request to enter password (or if you have givne the passphrase in above step-1, then it should receive a request to enter the passphrase instead of your username and password). Test it out:
```s
ssh <remote-user>@<remote-server>
```
Done!!

**Note:**
- In above **step-2, ssh-copy-id** command should create a .ssh directory in remote server and copy the public inside ~/.ssh/authorized_keys directory. If that didn't happen due to one or other reason and your password-less authentication getting failed then create the .ssh directory manually and set the required permission as below: 

Check .ssh directory in remote-server and create if not available
```s
ls .ssh
```
run above command from the user you want to connect with from client (i.e. /home/<remote-user>)
```s
mkdir -p .ssh
```
  
- After creating .ssh directory manually in remote-server, run above step-2 for testing the password-less ssh and if it failed then you should consider check and Change the permissions of .ssh directory created manually on remote-server as per above step. 
```s
chmod 700 ~/.ssh
```
```s
chmod 600 ~/.ssh/authorized_keys
```
