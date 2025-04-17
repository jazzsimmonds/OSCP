# 3306 - MYSQL

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
