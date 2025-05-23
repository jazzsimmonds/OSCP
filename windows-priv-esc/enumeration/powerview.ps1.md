# PowerView.ps1

A set of PowerShell functions that can be used to enumerate ActiveDirectory. Part of the larger [**PowerSploit Framework**](https://github.com/PowerShellMafia/PowerSploit)

Transfer [PowerView.ps1](https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Recon/PowerView.ps1) to the compromised target. Requires a PowerShell session. Then, source the file into the current session:

{% code overflow="wrap" %}
```powershell
. .\PowerView.ps1

Invoke-AllChecks

Get-NetDomain

Get-NetUser

Get-NetComputer
Get-NetComputer | select 
dnshostname,operatingsystem,operatingsystemversion

Find-LocalAdminAccess 

Get-NetSession -ComputerName files04 -Verbose 

Get-Acl -Path 
HKLM:SYSTEM\CurrentControlSet\Services\LanmanServer\DefaultSecurity\ | fl 


```
{% endcode %}
