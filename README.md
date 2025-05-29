# 21 - FTP



Check if anonymous login is enabled:

```sh
nmap -p21 --script=ftp-anon $TARGET_IP
ftp $TARGET_IP
```

