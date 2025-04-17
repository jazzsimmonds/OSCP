# Enumeration



```sh
id
cat /etc/passwd
hostname
```

_**information about the operating system**_

```sh
cat /etc/issue
cat /etc/os-release
uname -a
```

_**list system processes:**_

```sh
ps aux
```

_**list TCP/IP configuration**_

```sh
ifconfig
ip a
```

_**display network routing tables:**_

```sh
routel
route
```

_**list all network connections:**_

```sh
ss -anp
netstat
```

_**files created by iptables-save command**_

```sh
cat /etc/iptables/rules.v4
```

_**list schedules cron jobs**_

```sh
ls -lah /etc/cron*
```

view current user's scheduled jobs:

```shell
crontab -l
```

```sh
sudo crontab -l
```

_**list applications installed by dpkg**_

```sh
dpkg -l
```

_**find writeable directories**_

```sh
find / -writeable -type d 2>/dev/null
```

_**find writeable files**_

```sh
find / -writeable 2>/dev/null
```

_**mounted filesystems and drives**_

```sh
cat /etc/fstab
mount
```

_**view available disks**_

```shell
lsblk
```

_**get loaded kernel modules**_

```shell
lsmod
```

_**get more info about a module**_

```sh
/sbin/modinfo {module}
```

_**search for SUID binaries**_

```sh
find / -perm -u=s -type f 2>/dev/null
```
