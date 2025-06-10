# 1433 - MSSQL

{% code overflow="wrap" %}
```sh
nmap -n -v -sV -Pn -p 1433 –script ms-sql-info,ms-sql-ntlm-info,ms-sql-empty-password $ip
```
{% endcode %}

Brute force users and passwords with nmap:

{% code overflow="wrap" %}
```sh
nmap -n -v -sV -Pn -p 1433 –script ms-sql-brute –script-args userdb=users.txt,passdb=passwords.txt $ip
```
{% endcode %}

### &#x20; <a href="#rce-with-sql-server" id="rce-with-sql-server"></a>





{% code overflow="wrap" %}
```bash
impacket-mssqlclient oscp.exam/eric.wallows:EricLikesRunning800@10.10.138.148 -windows-auth
```
{% endcode %}

* oscp.exam = domain
* username:password
* -windows-auth: use Windows authentication (NTLM)
