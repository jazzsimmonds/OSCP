# Enumeration



* [enumerate users](./#enumerate-users)
* [enumerate groups](./#enumerate-groups)
* [LDAP enumeration](./#ldap-enumeration)
* [enumerate OS](./#enumerate-operating-systems)
* [enumerate permissions and logged on users](./#permissions-and-logged-on-users)
* [enumerate service principle names](./#service-principal-names)
* [enumerate object permissions](./#enumerate-object-permissions)

### <mark style="background-color:purple;">**enumerate users**</mark>

&#x20;print out the users in the domain :

```powershell
net user /domain
```

<mark style="background-color:green;">Administrators often tend to add prefixes or suffixes to usernames that identify accounts by their function.</mark>

```powershell
net user jeffadmin /domain 
```

### <mark style="background-color:purple;">**enumerate groups**</mark>

enumerate groups in the domain with net group:

```
net group /domain
```

focusing on the sales department group:

```
net group "Sales Department" /domain
```

### <mark style="background-color:purple;">LDAP Enumeration</mark>

`LDAP://HostName[:PortNumber][/DistinguishedName]`&#x20;

#### _**getting the hostname**_

{% code overflow="wrap" %}
```powershell
[System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()
```
{% endcode %}

* _PdcRoleOwner_ - hostname which can be used in LDAP pat

#### _**getting distinguished name**_

```powershell
([adsi]'').distinguishedName
```

returns the DN in the proper format for the LDAP path

_<mark style="background-color:blue;">**automate LDAP enumeration**</mark>_

`powershell -ep bypass` - bypass the execution policy, which was designed to keep us from accidentally running PowerShell scripts

{% code title="enumeration.ps1" overflow="wrap" %}
```powershell
$PDC = [System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain().PdcRoleOwner.Name
$DN = ([adsi]'').distinguishedName 
$LDAP = "LDAP://$PDC/$DN"
$LDAP
```
{% endcode %}

#### _**adding search functionality to the script**_

{% code title="enumeration_search.ps1" overflow="wrap" %}
```powershell
$PDC = [System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain().PdcRoleOwner.Name
$DN = ([adsi]'').distinguishedName 
$LDAP = "LDAP://$PDC/$DN"

$direntry = New-Object System.DirectoryServices.DirectoryEntry($LDAP)

$dirsearcher = New-Object System.DirectoryServices.DirectorySearcher($direntry)
$dirsearcher.FindAll()
```
{% endcode %}

* &#x20;receives all objects in the entire domain
* `$direntry = New-Object System.DirectoryServices.DirectoryEntry($LDAP)` - encapsulating the obtained LDAP path
* `$dirsearcher = New-Object System.DirectoryServices.DirectorySearcher($direntry)` - uses $direntry as the _SearchRoot_, pointing to the top of the hierarchy where _DirectorySearcher_ will run the _FindAll()_ method.

#### _**filtering search functionality**_

{% code title="enumeration_filtered.ps1" overflow="wrap" %}
```powershell
$PDC = [System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain().PdcRoleOwner.Name
$DN = ([adsi]'').distinguishedName 
$LDAP = "LDAP://$PDC/$DN"

$direntry = New-Object System.DirectoryServices.DirectoryEntry($LDAP)

$dirsearcher = New-Object System.DirectoryServices.DirectorySearcher($direntry)
$dirsearcher.filter="samAccountType=805306368"
$dirsearcher.FindAll()
```
{% endcode %}

* `$dirsearcher.filter="samAccountType=805306368"` - _samAccountType_ value of 0x30000000 (decimal 805306368), which will enumerate all users in the domain.
* `$dirsearcher.filter="name=jeffadmin"` - filter by user

<mark style="background-color:blue;">print each property on its own line:</mark>

{% code title="enumeration.ps1" overflow="wrap" %}
```powershell
$domainObj = [System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()
$PDC = $domainObj.PdcRoleOwner.Name
$DN = ([adsi]'').distinguishedName 
$LDAP = "LDAP://$PDC/$DN"

$direntry = New-Object System.DirectoryServices.DirectoryEntry($LDAP)

$dirsearcher = New-Object System.DirectoryServices.DirectorySearcher($direntry)
$dirsearcher.filter="samAccountType=805306368"
$result = $dirsearcher.FindAll()

Foreach($obj in $result)
{
    Foreach($prop in $obj.Properties)
    {
        $prop
    }

    Write-Host "-------------------------------"
}
```
{% endcode %}

* `$prop.memberof` - print what groups they are a member of



### <mark style="background-color:purple;">enumerate operating systems</mark>

enumerate the computer objects in the\
domain:

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

### <mark style="background-color:purple;">enumerate permissions and logged on users</mark>

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

### <mark style="background-color:purple;">enumerate Service Principal Names (SPN)</mark>

Enumerate SPNs in the domain with setspn.exe

```powershell
setspn -L iis_service 
```

Enumerate SPNs

{% code overflow="wrap" %}
```powershell
Get-NetUser -SPN | select samaccountname,serviceprincipalname
```
{% endcode %}

Resolve domain:

```powershell
nslookup.exe web04.corp.com 
```

### <mark style="background-color:purple;">enumerate object permissions</mark>

enumerate ACEs with powerview:

```powershell
Get-ObjectAcl -Identity stephanie 
```

* `SecurityIdentifier` -  use PowerView’s Convert-SidToName command to convert it to an actual domain object  \
  name

convert the ObjectSID into name:

{% code overflow="wrap" %}
```powershell
Convert-SidToName S-1-5-21-1987370270-658905905-1781884369-1104
```
{% endcode %}

Enumerate ACLs for a specifc group:

{% code overflow="wrap" %}
```powershell
Get-ObjectAcl -Identity "Management Department" | ? 
{$_.ActiveDirectoryRights -eq "GenericAll"} | select 
SecurityIdentifier,ActiveDirectoryRights 
```
{% endcode %}

* `-eq`- filter the  \
  ActiveDirectoryRights property, only displaying the values that equal GenericAll.

## <mark style="background-color:purple;">enumerate domain shares</mark>

find the shares in the domain:

```powershell
 Find-DomainShare
```

* `-CheckShareAccess` flag to display shares only available to us

Decrypt GPP passwords

```sh
gpp-decrypt "+bsY0V3d4/KgX3VJdO/vyepPfAN1zMFTiQDApgR92JE"
```
