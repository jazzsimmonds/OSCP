# net.exe

Print users in the domain:

```powershell
net user /domain
```

Inspect user in the domain:

```powershell
net user jeffadmin /domain
```

Enumerate groups in the domain:

```powershell
net group /domain
```

Inspect group in the domain:

```powershell
net group "Sales Department" /domain
```

Add user to domain group:

{% code overflow="wrap" %}
```powershell
net group "Management Department" stephanie /add /domain
```
{% endcode %}

Delete user from domain group:

{% code overflow="wrap" %}
```powershell
net group "Management Department" stephanie /del /domain
```
{% endcode %}
