# LDAP Enumeration

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

### _<mark style="background-color:blue;">**automate LDAP enumeration**</mark>_

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

#### Adding Search Functionality

{% code title="filter.ps1" overflow="wrap" %}
```powershell
function LDAPSearch {
    param (
        [string]$LDAPQuery
    )

    $PDC = [System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain().PdcRoleOwner.Name
    $DistinguishedName = ([adsi]'').distinguishedName

    $DirectoryEntry = New-Object System.DirectoryServices.DirectoryEntry("LDAP://$PDC/$DistinguishedName")

    $DirectorySearcher = New-Object System.DirectoryServices.DirectorySearcher($DirectoryEntry, $LDAPQuery)

    return $DirectorySearcher.FindAll()

}
```
{% endcode %}

Import to memory:

```powershell
Import-Module .\filter.ps1
```

Call script and use it to search:

```powershell
LDAPSearch -LDAPQuery "(samAccountType=805306368)"
```

```powershell
LDAPSearch -LDAPQuery "(objectclass=group)"
```

{% code overflow="wrap" %}
```powershell
foreach ($group in $(LDAPSearch -LDAPQuery "(objectCategory=group)")) {
$group.properties | select {$_.cn}, {$_.member}
}
```
{% endcode %}

