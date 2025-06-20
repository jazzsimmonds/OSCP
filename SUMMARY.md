# Table of contents

## Services

* [21 - FTP](README.md)
* [22 - SSH](services/22-ssh.md)

***

* [25 - SMTP](25-smtp.md)
* [80,443 - HTTP/S](80-443-http-s.md)
* [88 - Kerberos](88-kerberos.md)
* [111 -NFS](111-nfs.md)
* [161 - SNMP](161-snmp.md)
* [135,593 - RPC](135-593-rpc.md)
* [139,445 - SMB](139-445-smb.md)
* [389,636,3268 - LDAP](389-636-3268-ldap.md)
* [3389 - RDP](3389-rdp.md)
* [3306 - MYSQL](3306-mysql.md)
* [1433 - MSSQL](1433-mssql.md)
* [,5437 - Postgres](5437-postgres.md)
* [5985,5986 - WinRM](5985-5986-winrm.md)

## Password Attacks

* [Password Cracking](password-attacks/password-cracking.md)
* [Password Guessing](password-attacks/page-1.md)

***

* [NLTM](nltm/README.md)
  * [NTLM Relay](nltm/ntlm-relay.md)
* [Net-NTLMv2](net-ntlmv2.md)
* [GPP](gpp.md)

## Web App Exploits

* [webapps](web-app-exploits/webapps/README.md)
  * [WebDav](web-app-exploits/webapps/webdav.md)
  * [git](web-app-exploits/webapps/git.md)
  * [Wordpress](web-app-exploits/webapps/wordpress.md)
* [Password Attacks](web-app-exploits/password-attacks.md)
* [Directory traversal](web-app-exploits/directory-traversal.md)
* [File inclusion](web-app-exploits/file-inclusion.md)
* [Command Injection](web-app-exploits/command-injection.md)
* [File Upload](web-app-exploits/file-upload.md)
* [SQL Injection](web-app-exploits/sql-injection/README.md)
  * [MySQL](web-app-exploits/sql-injection/mysql.md)
  * [PostgreSQL](web-app-exploits/sql-injection/postgresql.md)
  * [MSSQL](web-app-exploits/sql-injection/mssql.md)

## Client Side Attacks

* [Phishing](client-side-attacks/phishing.md)

## Pivoting

* [SSH Local port forwarding](pivoting/ssh-local-port-forwarding.md)
* [SSH Remote port forwarding](pivoting/ssh-remote-port-forwarding.md)
* [SSH - sshuttle](pivoting/ssh-sshuttle.md)
* [Port forwarding on Windows](pivoting/port-forwarding-on-windows.md)
* [HTTP tunneling](pivoting/http-tunneling.md)

***

* [DNS tunneling](dns-tunneling.md)
* [Chisel](chisel.md)
* [Ligolo](ligolo.md)

## Windows Priv Esc

* [Enumeration](windows-priv-esc/enumeration/README.md)
  * [PowerView.ps1](windows-priv-esc/enumeration/powerview.ps1.md)
* [Files](windows-priv-esc/files.md)
* [Anti-Virus Evasion](windows-priv-esc/anti-virus-evasion.md)
* [Service Binary Hijacking](windows-priv-esc/service-binary-hijacking.md)
* [DLL Hijacking](windows-priv-esc/dll-hijacking.md)
* [scheduled tasks](windows-priv-esc/scheduled-tasks.md)
* [Unquoted service paths](windows-priv-esc/unquoted-service-paths.md)
* [Enumerate Creds](windows-priv-esc/enumerate-creds.md)
* [SeBackupPrivilege](windows-priv-esc/sebackupprivilege.md)

***

* [SeImpersonatePrivilege](seimpersonateprivilege.md)
* [FullPowers.exe](fullpowers.exe.md)
* [PowerUp.ps1](powerup.ps1.md)
* [UAC Bypass](uac-bypass.md)
* [exploits](exploits.md)

## Linux Priv Esc

* [Enumeration](linux-priv-esc/enumeration.md)
* [SUID](linux-priv-esc/suid.md)
* [sudo](linux-priv-esc/sudo.md)
* [Cron jobs](linux-priv-esc/cron-jobs.md)
* [inspecting service footprints](linux-priv-esc/inspecting-service-footprints.md)
* [inspecting user trails](linux-priv-esc/inspecting-user-trails.md)
* [/etc/passwd](linux-priv-esc/etc-passwd.md)
* [kernel](linux-priv-esc/kernel.md)
* [Disk group](linux-priv-esc/disk-group.md)

## Active Directory

* [Connect](active-directory/connect.md)
* [Enumeration](active-directory/enumeration/README.md)
  * [net.exe](active-directory/enumeration/net.exe.md)
  * [LDAP Enumeration](active-directory/enumeration/ldap-enumeration.md)
  * [PowerView.ps1](active-directory/enumeration/powerview.ps1.md)
  * [PsLoggedOn.exe](active-directory/enumeration/psloggedon.exe.md)
  * [setspn.exe](active-directory/enumeration/setspn.exe.md)
* [BloodHound](active-directory/bloodhound.md)
* [Mimikatz](active-directory/mimikatz.md)
* [Password Attacks](active-directory/password-attacks.md)
* [AS-REP Roasting](active-directory/as-rep-roasting.md)
* [Kerberoasting](active-directory/kerberoasting.md)
* [Silver Tickets](active-directory/silver-tickets.md)
* [Domain Controller Synchronization](active-directory/domain-controller-synchronization.md)
* [Lateral Movement](active-directory/lateral-movement/README.md)
  * [WMI, WinRS, and WinRM](active-directory/lateral-movement/wmi-winrs-and-winrm.md)
  * [PsExec](active-directory/lateral-movement/psexec.md)
  * [Pass The Hash](active-directory/lateral-movement/pass-the-hash.md)
  * [Overpass the Hash](active-directory/lateral-movement/overpass-the-hash.md)
  * [Pass the Ticket](active-directory/lateral-movement/pass-the-ticket.md)
  * [DCOM](active-directory/lateral-movement/dcom.md)

***

* [Golden Ticket](golden-ticket.md)
* [Shadow Copies](shadow-copies.md)
* [Permissions](permissions/README.md)
  * [GenericWrite](permissions/genericwrite.md)
  * [GenericAll](permissions/genericall.md)
  * [WriteDACL](permissions/writedacl.md)
  * [ReadLAPSPassword](permissions/readlapspassword.md)
  * [AllowedToDelegate](permissions/allowedtodelegate.md)
  * [ForceChangePassword](permissions/forcechangepassword.md)

## AWS Cloud

* [Page 2](aws-cloud/page-2.md)
