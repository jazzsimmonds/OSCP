# inspecting user trails

`env` - check for any unusual entries

usual environment variable entries :

```sh
joe@debian-privesc:~$ env 
... 
XDG_SESSION_CLASS=user 
TERM=xterm-256color 
USER=joe 
LC_TERMINAL_VERSION=3.4.16
SHLVL=1 
XDG_SESSION_ID=35 
LC_CTYPE=UTF-8 
XDG_RUNTIME_DIR=/run/user/1000 
SSH_CLIENT=192.168.118.2 59808 22 PATH=/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games DBUS_SESSION_BUS_ADDRESS=unix:path=/run/user/1000/bus MAIL=/var/mail/joe SSH_TTY=/dev/pts/1 OLDPWD=/home/joe/.cache _=/usr/bin/env
```

`cat .bashrc` - check bash config file to check any unusual values are permanent values

