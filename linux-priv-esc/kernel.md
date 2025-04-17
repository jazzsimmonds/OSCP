# kernel

_<mark style="background-color:purple;">**enumerate kernel info**</mark>_

```powershell
cat /etc/issue
uname -r
arch
```

_<mark style="background-color:purple;">**searchploit to find exploits from enumerated kernel info**</mark>_

{% code overflow="wrap" %}
```sh
searchsploit "linux kernel Ubuntu 16 Local Privilege Escalation" | grep "4." | grep -v " < 4.4.0" | grep -v "4.8"
```
{% endcode %}
