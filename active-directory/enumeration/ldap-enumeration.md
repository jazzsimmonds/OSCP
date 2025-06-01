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

## **Automate LDAP enumeration**

`powershell -ep bypass` - bypass the execution policy, which was designed to keep us from accidentally running PowerShell scripts

{% code title="enumeration.ps1" overflow="wrap" %}
```powershell
$PDC = [System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain().PdcRoleOwner.Name
$DN = ([adsi]'').distinguishedName 
$LDAP = "LDAP://$PDC/$DN"
$LDAP
```
{% endcode %}

### **Receive all objects in the entire domain**

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

### **Enumerate all users in the domain**

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

#### Enumerate what groups a user belongs to&#x20;

{% code overflow="wrap" %}
```powershell
$dirsearcher = New-Object System.DirectoryServices.DirectorySearcher($direntry)
$dirsearcher.filter="name=jeffadmin"
$result = $dirsearcher.FindAll()

Foreach($obj in $result)
{
    Foreach($prop in $obj.Properties)
    {
        $prop.memberof
    }

    Write-Host "-------------------------------"
}
```
{% endcode %}

### Searchable script

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

#### Call script and use it to search:

Search by account type:

```powershell
LDAPSearch -LDAPQuery "(samAccountType=805306368)"
```

Enumerate users:

```powershell
$results = LDAPSearch -LDAPQuery "(samAccountType=805306368)"

foreach ($result in $results) {
    $result.Properties["samaccountname"]
}

```

List all the groups in the domain:

```powershell
LDAPSearch -LDAPQuery "(objectclass=group)"
```

Enumerate every group available in the domain and display the user members:

{% code overflow="wrap" %}
```powershell
foreach ($group in $(LDAPSearch -LDAPQuery "(objectCategory=group)")) {
$group.properties | select {$_.cn}, {$_.member}
}
```
{% endcode %}

Store the search results:

{% code overflow="wrap" %}
```powershell
$sales = LDAPSearch -LDAPQuery "(&(objectCategory=group)(cn=Sales Department))"
```
{% endcode %}

{% code overflow="wrap" %}
```powershell
$group = LDAPSearch -LDAPQuery "(&(objectCategory=group)(cn=Development Department*))"
```
{% endcode %}

Print attribute from results:

```powershell
$sales.properties.member
```

### Check for nested groups

{% code overflow="wrap" %}
```powershell
function Get-NestedGroupMembers {
    param (
        [string]$GroupDN,
        [System.Collections.Hashtable]$VisitedGroups = @{}
    )

    if ($VisitedGroups.ContainsKey($GroupDN)) {
        Write-Host "[!] Already visited group: $GroupDN"
        return
    }
    $VisitedGroups[$GroupDN] = $true

    Write-Host "`n[*] Processing group: $GroupDN"

    $searcher = New-Object DirectoryServices.DirectorySearcher
    $searcher.Filter = "(distinguishedName=$GroupDN)"
    $searcher.PropertiesToLoad.Add("member") | Out-Null
    $result = $searcher.FindOne()

    if ($result -eq $null) {
        Write-Host "[!] Group not found or inaccessible: $GroupDN"
        return
    }

    foreach ($memberDN in $result.Properties["member"]) {
        $memberSearcher = New-Object DirectoryServices.DirectorySearcher
        $memberSearcher.Filter = "(distinguishedName=$memberDN)"
        $memberSearcher.PropertiesToLoad.Add("objectClass") | Out-Null
        $memberSearcher.PropertiesToLoad.Add("cn") | Out-Null
        $memberSearcher.PropertiesToLoad.Add("samaccountname") | Out-Null
        $memberSearcher.PropertiesToLoad.Add("description") | Out-Null
        $memberSearcher.PropertiesToLoad.Add("info") | Out-Null
        $memberSearcher.PropertiesToLoad.Add("userprincipalname") | Out-Null

        $memberResult = $memberSearcher.FindOne()

        if ($memberResult -eq $null) {
            Write-Host "[!] Could not find member: $memberDN"
            continue
        }

        $objectClasses = $memberResult.Properties["objectClass"]

        if ($objectClasses -contains "group") {
            Write-Host "[+] Nested group found: $memberDN"
            # Recursive call
            Get-NestedGroupMembers -GroupDN $memberDN -VisitedGroups $VisitedGroups
        }
        elseif ($objectClasses -contains "user") {
            Write-Host "    - User found: $($memberResult.Properties["samaccountname"]) (CN: $($memberResult.Properties["cn"]))"
            # Optionally, print more user info here if needed
        }
        else {
            Write-Host "[?] Unknown object type for: $memberDN"
        }
    }
}

# Start from your initial group DN here:
$startGroupDN = "CN=Service Personnel,CN=Users,DC=corp,DC=com"

Get-NestedGroupMembers -GroupDN $startGroupDN

```
{% endcode %}

### Get user properties

{% code overflow="wrap" %}
```powershell
$userDN = "CN=michelle,CN=Users,DC=corp,DC=com"

$searcher = New-Object DirectoryServices.DirectorySearcher
$searcher.Filter = "(distinguishedName=$userDN)"
# Load all properties you want to check
$propertiesToLoad = @("cn", "samaccountname", "description", "info", "userprincipalname", "comment", "displayName")
foreach ($prop in $propertiesToLoad) {
    $searcher.PropertiesToLoad.Add($prop) | Out-Null
}

$result = $searcher.FindOne()

if ($result -ne $null) {
    Write-Host "===== User properties ====="
    foreach ($prop in $propertiesToLoad) {
        $val = $result.Properties[$prop]
        if ($val) {
            Write-Host "$prop : $val"
        }
    }
} else {
    Write-Host "User not found or no access"
}
```
{% endcode %}
