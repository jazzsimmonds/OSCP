# Enumeration

**key pieces of information we should always obtain:**

{% stepper %}
{% step %}
[Username and hostname](./#username-and-hostname)
{% endstep %}

{% step %}
[Group memberships of the current user](./#group-memberships-of-the-current-user)
{% endstep %}

{% step %}
[Existing users and groups](./#existing-users-and-groups)
{% endstep %}

{% step %}
[Operating system, version and architecture](./#operating-system-version-and-architecture)
{% endstep %}

{% step %}
[Network information](./#network-information)
{% endstep %}

{% step %}
[Installed applications](./#installed-applications)
{% endstep %}

{% step %}
[Running processes](./#running-processes)
{% endstep %}
{% endstepper %}

#### <mark style="background-color:purple;">**Username and hostname:**</mark>

```
whoami
```

#### <mark style="background-color:purple;">Group memberships of the current user</mark>

```
whoami /groups
```

#### <mark style="background-color:purple;">Existing users and groups</mark>

obtain a list of all local users:

```powershell
net user
Get-LocalUser
```

```powershell
net user steve
```

enumerate the existing groups:

```powershell
net group
Get-LocalGroup
```

get members of the group adminteam:

```powershell
Get-LocalGroupMember adminteam
```

```powershell
Get-LocalGroupMember -Group "Remote Management Users"
```

there are several built-in groups we should analyse, such as :

* _**Administrators**_,
* _**Backup Operators**_: can backup and restore all files on a computer, even those files they don’t have permissions for.
* _**Remote Desktop Users**_: can access the system with RDP
* _**Remote Management Users**_: can access the system with WinRM

#### <mark style="background-color:purple;">Operating system, version and architecture</mark>

```powershell
systeminfo
```

#### <mark style="background-color:purple;">Network information</mark>

list all network interfaces:

```powershell
ipconfig /all
```

display routing table (contains all routes of the system):

```powershell
route print
```

list all active network connections:

```powershell
netstat -ano
```

#### <mark style="background-color:purple;">Installed applications</mark>

check all 32bit installed applications:

{% code overflow="wrap" %}
```powershell
Get-ItemProperty "HKLM:\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*" | select displayname
```
{% endcode %}

check all 64bit installed applications:

{% code overflow="wrap" %}
```powershell
Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\*" | select displayname
```
{% endcode %}

review C:\ and Downloads directory to find more potential programs

_search for public exploits for the identified applications after finishing situational awareness_

#### <mark style="background-color:purple;">Running processes</mark>

review running processes of the system:

```powershell
Get-Process
```
