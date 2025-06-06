# PowerView.ps1

{% embed url="https://powersploit.readthedocs.io/en/latest/Recon/" %}

[https://gist.github.com/HarmJ0y/184f9822b195c52dd50c379ed3117993](https://gist.github.com/HarmJ0y/184f9822b195c52dd50c379ed3117993)&#x20;

```powershell
Import-Module .\PowerView.ps1
```

### Enumerate Users

List all users:

```powershell
Get-NetUser
```

```powershell
Get-NetUser | select cn
Get-NetUser | select cn,pwdlastset,lastlogon
Get-NetUser "fred"
```

### Enumerate Groups

List groups:

```powershell
Get-NetGroup | select cn
```

List members of a group:

```powershell
Get-NetGroup "Sales Department" | select member
```

Get domain admins:

```powershell
Get-NetGroupMember "Domain Admins"
```

### Enumerate OS

Enumerate the computer objects in the&#x20;domain:

```powershell
 Get-NetComputer 
```

{% code overflow="wrap" %}
```powershell
 Get-NetComputer | select operatingsystem,dnshostname
 Get-NetComputer "web04.corp.com"
```
{% endcode %}

Query operating system and version:

```powershell
Get-NetComputer | select 
dnshostname,operatingsystem,operatingsystemversion
```

### Permissions and Logged On Users

Find machines our user has administrative privs on:

```powershell
Find-LocalAdminAccess
```

* Get IP address of machine: `Resolve-DnsName web04.corp.com` or `nslookup.exe web04.corp.com`&#x20;
* Access the machine: `xfreerdp /u:{username} /p:'{password}' /d:{domain} /v:{machine_ip} +clipboard`

Check logged on users with `Get-NetSession`:

```powershell
Get-NetSession -ComputerName files04 -Verbose
Get-NetSession -ComputerName web04 -Verbose
Get-NetSession -ComputerName client74
```

Display permissions on the DefaultSecurity registry hive (**SrvsvcSessionInfo** registry key):

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

Enumerate a user's access control entries (ACEs):

```powershell
Get-ObjectAcl -Identity stephanie
```

* ObjectSID - The unique Security Identifier (SID) of the object being queried; this identifies the Active Directory object itself (such as a user, group, or computer).
* SecurityIdentifier - The SID of the user, group, or computer that the Access Control Entry (ACE) applies to; this represents who has been granted or denied permissions on the object.

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

* can check for [GPP passwords](../../gpp.md)

Check what shares are accessible for the current user:

```powershell
$shares = Find-DomainShare
```

{% code title="access.ps1" overflow="wrap" %}
```powershell
function Test-ShareAccess {
    param (
        [Parameter(Mandatory=$true)]
        [PSObject[]]$Shares
    )

    foreach ($entry in $Shares) {
        $computer = $entry.ComputerName
        $sharePattern = $entry.Name

        if ($sharePattern -like '*`*') {
            try {
                $matchedShares = Get-NetShare -ComputerName $computer -ErrorAction Stop | Where-Object { $_.Name -like $sharePattern }
            } catch {
                Write-Warning ("Failed to get shares from " + $computer + ": " + $_)
                continue
            }

            foreach ($ms in $matchedShares) {
                $path = "\\$computer\$($ms.Name)"
                try {
                    Get-ChildItem $path -ErrorAction Stop | Out-Null
                    Write-Host "$path : Accessible" -ForegroundColor Green
                } catch [System.UnauthorizedAccessException] {
                    Write-Host "$path : Access Denied" -ForegroundColor Red
                } catch {
                    Write-Host "$path : Other Error - $($_.Exception.Message)" -ForegroundColor Yellow
                }
            }
        } else {
            $path = "\\$computer\$sharePattern"
            try {
                Get-ChildItem $path -ErrorAction Stop | Out-Null
                Write-Host "$path : Accessible" -ForegroundColor Green
            } catch [System.UnauthorizedAccessException] {
                Write-Host "$path : Access Denied" -ForegroundColor Red
            } catch {
                Write-Host "$path : Other Error - $($_.Exception.Message)" -ForegroundColor Yellow
            }
        }
    }
}

```
{% endcode %}

Import script and run it against the enumerated shares

```powershell
Import-Module ./access.ps1
Test-ShareAccess -Shares $shares
```
