# Basic Commands for Linux System Administration


#### disable root login to ssh (with sed command)
#### How to set timezone
#### find specific patterns of files at one directory and copy those to another directory



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
