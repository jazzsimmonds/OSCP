# GPP

Historically, system administrators often changed local workstation passwords through [_Group Policy Preferences_](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn581922\(v=ws.11\)) (GPP).

Even though GPP-stored passwords are encrypted with AES-256, the private key for the encryption has been posted on [_MSDN_](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-gppref/2c15cbf0-f086-4c74-8b70-1f2fa45dd4be?redirectedfrom=MSDN#endNote2).

```sh
gpp-decrypt "+bsY0V3d4/KgX3VJdO/vyepPfAN1zMFTiQDApgR92JE"
```
