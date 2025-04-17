# SSH - sshuttle

set up a port forward in a shell on CONFLUENCE01, listening on port 2222 on the WAN interface and forwarding to port 22 on PGDATABASE01:

```sh
socat TCP-LISTEN:2222,fork TCP:10.4.50.215:22
```

Run **sshuttle**, specifying the SSH connection string we want to use, as well as the subnets that we want to tunnel through this connection (**10.4.50.0/24** and **172.16.50.0/24**):

{% code overflow="wrap" %}
```sh
sshuttle -r database_admin@192.168.50.63:2222 10.4.50.0/24 172.16.50.0/24
```
{% endcode %}

