# SSH Local port forwarding

## Local port forwarding

{% code overflow="wrap" %}
```sh
ssh -N -L 0.0.0.0:4455:172.16.50.217:445 database_admin@10.4.50.215
```
{% endcode %}

* `-N` - prevent a shell from being opened
* `-L` - local port forwarding argument
* &#x20;`0.0.0.0:4455` - all interfaces on port **4455** on CONFLUENCE01 (**0.0.0.0:4455**)
* `:172.16.50.217:445` forward all packets (through the SSH tunnel to PGDATABASE01) to port **445** on the newly found host (**172.16.50.217:445**).
* `database_admin@10.4.50.215` - target to connect to\
  can pass the **-v** flag to **ssh** in order to receive debug output.

`ss -ntplu` - confirm ssh process

## Dynamic port forwarding

_**can also access any other port on any other host that PGDATABASE01 has access to, through this single port.**_

```sh
ssh -N -D 0.0.0.0:9999 database_admin@10.4.116.215
```

* `-D` - dynamic port forwarding

**Proxychains**

smbclient doesnt provide a way to use a SOCKS proxy so proxychian is needed

* Proxychains is a tool that can force network traffic from third party tools over HTTP or SOCKS proxies. As the name suggests, it can also be configured to push traffic over a chain of concurrent proxies.
* stored by default at `/etc/proxychains4.conf`
* We can simply replace any existing proxy definition in that file with a single line defining the proxy type, IP address, and port of the SOCKS proxy running on CONFLUENCE01 (socks5 192.168.50.63 9999).

Add to proxychains conf on kali machine = `socks5 {192.168.116.63} 9998`

instead of using the IP we are connected to, we write it as if we have direct access to the other machine

_**connect to smb through proxy**_:

{% code overflow="wrap" %}
```sh
proxychains smbclient -L //172.16.50.217/ -U hr_admin - password=Welcome1234
```
{% endcode %}

_**port scan through proxy**_:

```sh
proxychains nmap -vvv -sT --top-ports=20 -Pn 172.16.50.217
```

if nmap returns ports as filtered either run the command with sudo or use nc to confirm port status

```sh
proxychains nc -zv 172.16.116.217 4870-4880
```
