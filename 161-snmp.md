# 161 - SNMP

Windows SNMP MIB values:

<table><thead><tr><th>OID</th><th>Attribute</th><th data-hidden></th><th data-hidden></th></tr></thead><tbody><tr><td>1.3.6.1.2.1.25.1.6.0</td><td>System Processes</td><td></td><td></td></tr><tr><td>1.3.6.1.2.1.25.4.2.1.2</td><td>Running Programs</td><td></td><td></td></tr><tr><td>1.3.6.1.2.1.25.4.2.1.4</td><td>Processes Path</td><td></td><td></td></tr><tr><td>1.3.6.1.2.1.25.2.3.1.4</td><td>Storage Units</td><td></td><td></td></tr><tr><td>1.3.6.1.2.1.25.6.3.1.2</td><td>Software Name</td><td></td><td></td></tr><tr><td>1.3.6.1.4.1.77.1.2.25</td><td>User Accounts</td><td></td><td></td></tr><tr><td>1.3.6.1.2.1.6.13.1.3</td><td>TCP Local Ports</td><td></td><td></td></tr></tbody></table>

\


{% code overflow="wrap" %}
```sh
sudo nmap -sU --open -p 161 192.168.50.1-254 -oG open-snmp.txt
```
{% endcode %}

