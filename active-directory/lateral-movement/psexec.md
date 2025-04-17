# PsExec

Three conditions must be met:

1. The user that authenticates to the target machine needs to be part of the Administrators local group.&#x20;
2. The _ADMIN$_ share must be available
3. File and Printer Sharing has to be turned on.&#x20;





```powershell
.\PsExec64.exe -i  \\FILES04 -u corp\jen -p Nexus123! cmd
```

