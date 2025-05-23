# PowerView.ps1

A set of PowerShell functions that can be used to enumerate ActiveDirectory. Part of the larger [**PowerSploit Framework**](https://github.com/PowerShellMafia/PowerSploit)

Transfer [PowerView.ps1](https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Recon/PowerView.ps1) to the compromised target. Requires a PowerShell session. Then, source the file into the current session:

```powershell
. .\PowerView.ps1

Invoke-AllChecks
```
