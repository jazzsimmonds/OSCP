# 22 - SSH

* id\_rsa - if password is require use ssh2john to crack it

```sh
chmod 600 id_rsa
```

```
ssh2john id_rsa > ssh.hash
```

```
john --wordlist=/usr/share/wordlists/rockyou.txt ssh.hash
```



{% code overflow="wrap" %}
```sh
sudo sh -c 'cat /home/kali/passwordattacks/ssh.rule >> /etc/john/john.conf'
```
{% endcode %}

```sh
john --wordlist=ssh.passwords --rules=sshRules ssh.hash
```

```
ssh -i id_rsa daniela@192.168.50.244
```

