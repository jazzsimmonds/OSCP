# BloodHound

### Gather data

Perform LDAP enumeration against an Active Directory (AD) environment and gather data compatible with **BloodHound:**

{% code overflow="wrap" %}
```sh
nxc ldap DOMAINNAME -u 'USER' -p 'PASSWORD' --bloodhound -c all --dns-server IPOFDOMAINCONTROLLER
```
{% endcode %}







### Set up BloodHound

