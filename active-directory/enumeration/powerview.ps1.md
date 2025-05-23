# PowerView.ps1

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

Enumerate SPNs

{% code overflow="wrap" %}
```powershell
Get-NetUser -SPN | select samaccountname,serviceprincipalname
```
{% endcode %}

Find the shares in the domain:

```powershell
 Find-DomainShare
```

* `-CheckShareAccess` flag to display shares only available to us
