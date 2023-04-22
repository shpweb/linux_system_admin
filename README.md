# Basic Commands for Linux System Administration


#### disable root login to ssh (with sed command)
#### How to set timezone



================================================================================================
## disable root login to ssh (with sed command)

#### 1. disable the root login into /etc/passwd
```s
sudo sed -i "s|/root:/bin/bash|/root:/sbin/nologin|g" /etc/passwd
```

#### 2. disable the root login in sshd_config file
```s
sudo sed -i "s|PermitRootLogin yes|PermitRootLogin no|g" /etc/ssh/sshd_config
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
