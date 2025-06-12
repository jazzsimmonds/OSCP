# Enumerate Creds

Use Winpeas to check for saved creds:

```shell
.\winPEASany.exe quiet cmd windowscreds
```

List saved credentials in Windows Credential Manager:

```
cmdkey /list
```



### VNC



```
reg query "HKCU\Software\ORL\WinVNC3\Password"
```

### Windows Autologin



{% code overflow="wrap" %}
```
reg query "HKLM\SOFTWARE\Microsoft\Windows NT\Currentversion\Winlogon"
```
{% endcode %}

### SNMP



```
reg query "HKLM\SYSTEM\Current\ControlSet\Services\SNMP"
```

### Registry

Queries to enumerate for credentials in the Registry. Upon execution, any registry key containing the word "password" will be displayed.

```powershell
reg query HKLM /f password /t REG_SZ /s
reg query HKCU /f password /t REG_SZ /s
```

### PuTTY

Queries to enumerate for PuTTY credentials in the Registry. PuTTY must be installed for this test to work. If any registry entries are found, they will be displayed.

Check PuTTY is installed: `dir C:\"Program Files"`

{% code overflow="wrap" %}
```powershell
reg query HKCU\Software\SimonTatham\PuTTY\Sessions /t REG_SZ /s
```
{% endcode %}
