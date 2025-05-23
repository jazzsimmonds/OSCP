# BloodHound

### Gather data

Perform LDAP enumeration against an Active Directory (AD) environment and gather data compatible with **BloodHound:**

{% code overflow="wrap" %}
```sh
nxc ldap DOMAINNAME -u 'USER' -p 'PASSWORD' --bloodhound -c all --dns-server IPOFDOMAINCONTROLLER
```
{% endcode %}

### Sharphound

Import script to memory

```powershell
Import-Module .\Sharphound.ps1
```

Collect and export data:

{% code overflow="wrap" %}
```powershell
Invoke-BloodHound -CollectionMethod All -OutputDirectory 
C:\Users\stephanie\Desktop\ -OutputPrefix "corp audit"
```
{% endcode %}

### Set up BloodHound

Start neo4j:

```
sudo neo4j start 
```

Login at [http://localhost:7474/](http://localhost:7474/)

Start BloodHound:

```
bloodhound
```

login and upload files
