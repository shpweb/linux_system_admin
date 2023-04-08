# Basic Commands for Linux System Administration

#### How to set timezone

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
