# inspecting service footprints

{% code overflow="wrap" %}
```powershell
watch -n 1 "ps -aux | grep pass"sudo tcpdump -i lo -A | grep "pass"
```
{% endcode %}
