# 3306 - MYSQL

root:root

MySQL Login:

```sh
mysql -h <IP> -u <username> -p <pass> -P <port>
```

nmap vuln scan:

{% code overflow="wrap" %}
```sh
nmap -sV -p 3306 --script mysql-audit,mysql-databases,mysql-dump-hashes,mysql-empty-password,mysql-enum,mysql-info,mysql-query,mysql-users,mysql-variables,mysql-vuln-cve2012-2122 <IP>
```
{% endcode %}

&#x20;If `mysql` client returns a `TLS/SSL error`, add `--ssl-mode=DISABLED`



