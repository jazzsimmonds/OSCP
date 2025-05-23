# PowerView.ps1

### Enumerate OS

Enumerate the computer objects in the&#x20;domain:

```powershell
 Get-NetComputer 
```

```powershell
 Get-NetComputer | select operatingsystem,dnshostname
```

Query operating system and version:

```powershell
Get-NetComputer | select 
dnshostname,operatingsystem,operatingsystemversion
```

### Permissions and Logged On Users

Scan domain to find local administrative privileges for our user:

```powershell
Find-LocalAdminAccess
```

Check logged on users with `Get-NetSession`:

```powershell
Get-NetSession -ComputerName files04 -Verbose
Get-NetSession -ComputerName web04 -Verbose
Get-NetSession -ComputerName client74
```

Display permissions on the DefaultSecurity registry hive:

{% code overflow="wrap" %}
```powershell
Get-Acl -Path 
HKLM:SYSTEM\CurrentControlSet\Services\LanmanServer\DefaultSecurity\ | fl 
```
{% endcode %}

* `FullControl`and `ReadKey` means can all read the **SrvsvcSessionInfo** key itself.

### SPNs

Enumerate SPNs

{% code overflow="wrap" %}
```powershell
Get-NetUser -SPN | select samaccountname,serviceprincipalname
```
{% endcode %}

### Object Permissions

Enumerate ACEs:

```powershell
Get-ObjectAcl -Identity stephanie
```

Convert SID to a domain object name:

{% code overflow="wrap" %}
```powershell
Convert-SidToName S-1-5-21-1987370270-658905905-1781884369-1104

#multiple at once
"S-1-5-21-1987370270-658905905-1781884369-512","S-1-5-21-1987370270
658905905-1781884369-1104","S-1-5-32-548","S-1-5-18","S-1-5-21-1987370270-658905905
1781884369-519" | Convert-SidToName 
```
{% endcode %}

Enumerate users in the specified group who have `GenericAll` (full control) permissions.

```powershell
Get-ObjectAcl -Identity "Management Department" | ? 
{$_.ActiveDirectoryRights -eq "GenericAll"} | select 
SecurityIdentifier,ActiveDirectoryRights
```

### Domain Shares

Find the shares in the domain:

```powershell
 Find-DomainShare
```

* `-CheckShareAccess` flag to display shares only available to us

List contents of share:

```
ls \dc1.corp.com\sysvol\corp.com\
```



