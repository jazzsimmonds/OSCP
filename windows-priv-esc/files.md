# Files

**RDCMan.settings** - plaintext RDP creds

**sitemanager.xml, recentservers.xml** - FTP creds

**sysprep.xml, Unattend.xml** -&#x20;

web.config -&#x20;

Groups.xml - Encrypted passwords but can be decrypted

```
c:\sysprep.inf
c:\sysprep\sysprep.xml
c:\unattend.xml
%WINDIR%\Panther\Unattend\Unattended.xml
%WINDIR%\Panther\Unattended.xml
```

\
Scripts
-------

search for files:

{% code overflow="wrap" %}
```powershell
$sourcePath = "C:\"  # Search entire C drive

$importantNames = @("SAM", "SYSTEM", "NTDS.dit", "SECURITY", "DEFAULT", "DRIVERS")
$importantExtensions = @(".ini", ".config", ".xml", ".json", ".pwd", ".txt", ".settings")

Get-ChildItem -Path $sourcePath -Recurse -Force -ErrorAction SilentlyContinue |
    Where-Object {
        ($importantNames -contains $_.Name) -or
        ($importantExtensions -contains $_.Extension.ToLower())
    } |
    ForEach-Object {
        Write-Output $_.FullName
    }

```
{% endcode %}



oneliner to dump one of the powershell history's

{% code overflow="wrap" %}
```
ls "C:\Users\" -dir | ForEach-Object { $dir=$_.FullName; $username=$dir.Split('\')[-1]; Write-Output "--$username--"; $historyFile=Join-Path -Path $dir -ChildPath "AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt"; if(Test-Path $historyFile){cat $historyFile}else{Write-Output "History file not found for $username."}; Write-Output "-----------------------"}
```
{% endcode %}

{% code overflow="wrap" %}
```


oneliner to dump one of the powershell history's including hidden DIRS
Get-ChildItem -Force "C:\Users\" | ForEach-Object { $dir=$_.FullName; $username=$dir.Split('\')[-1]; Write-Output "--$username--"; $historyFile=Join-Path -Path $dir -ChildPath "AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt"; try { if(Test-Path $historyFile){ Get-Content $historyFile } else { Write-Output "History file not found for $username." } } catch { Write-Output "Unable to access history file for $username. Error: $_" } Write-Output "-----------------------" }


Looking for user files: PS oneliner run from anywhere
Just users visible on dir
ls "C:\Users\" -dir | % { $dir=$_.FullName; Write-Output "--$($dir.Split('\')[-1])--"; ls $dir -file -r | ? { $_.FullName -notmatch [regex]::Escape("$dir\AppData"), [regex]::Escape("$dir\Local Settings"), [regex]::Escape("$dir\Start Menu"), [regex]::Escape("$dir\SendTo"), [regex]::Escape("$dir\Application Data") -and $_.Extension -notin ('.url', '.lnk') -and $_.Name -ne 'desktop.ini' } | select -exp FullName; "-----------------------" }

All Users, including hidden dirs
get-childitem -force "C:\Users\" | % { $dir=$_.FullName; Write-Output "--$($dir.Split('\')[-1])--"; ls $dir -file -r | ? { $_.FullName -notmatch [regex]::Escape("$dir\AppData"), [regex]::Escape("$dir\Local Settings"), [regex]::Escape("$dir\Start Menu"), [regex]::Escape("$dir\SendTo"), [regex]::Escape("$dir\Application Data") -and $_.Extension -notin ('.url', '.lnk') -and $_.Name -ne 'desktop.ini' } | select -exp FullName; "-----------------------" }

All Users, including hidden dirs., but not C:\Users\All Users\VMware, C:\Users\All Users\USOShared, C:\Users\All Users\Microsoft
get-childitem -force "C:\Users\" | % { $dir=$_.FullName; $excludeDirs=@("C:\Users\All Users\VMware", "C:\Users\All Users\USOShared", "C:\Users\All Users\Microsoft"); if ($excludeDirs -notmatch ($dir -replace "\\","\\")) { Write-Output "--$($dir.Split('\')[-1])--"; ls $dir -file -r -ErrorAction SilentlyContinue | ? { $_.FullName -notmatch [regex]::Escape("$dir\AppData"), [regex]::Escape("$dir\Local Settings"), [regex]::Escape("$dir\Start Menu"), [regex]::Escape("$dir\SendTo"), [regex]::Escape("$dir\Application Data") -and $_.Extension -notin ('.url', '.lnk') -and $_.Name -ne 'desktop.ini' } | select -exp FullName; "-----------------------" } }



Looking for user files: CMD oneliner - run inside dir and amend to say users name
•     dir *.* /s /b /a:-d | findstr /v /i /c:"C:\Users\offsec\AppData" | findstr /v /i /c:"C:\Users\offsec\Local Settings" | findstr /v /i /c:"C:\Users\offsec\Start Menu" | findstr /v /i /c:"C:\Users\offsec\SendTo" | findstr /v /i /c:"C:\Users\offsec\Application Data" | findstr /v /i /c:"desktop.ini"

Looking for user files: manual look with Tree to look for files
•     tree /a /f




Find interesting words in a file
First choice - looking for a string in a file type over C:
      Get-ChildItem -Path "C:\" -Filter "*.bak" -Recurse -ErrorAction SilentlyContinue | Select-String -Pattern "Password" -CaseSensitive:$false
Other choices
      Don’t run from root, run from user dirs., temp, interesting directories
      for /R C:\ %f in (*.txt) do @findstr /m /i "password" "%f"                                (hit and miss)
      Select-String -Path "C:\Users\steve\Contacts\logins.txt" -Pattern "steve" -CaseSensitive:$false             (reliable)
      dir /s *pass* == *.config                                                     (pass in the name and config)
      findstr /si password *.xml *.ini *.txt                                              (find password in these files)
Look for the following files:
•     bak
•     db
•     vbs
•     doc
•     docx
•     xls
•     xlsx
•     kdbx
•     config
•     zip
•     ini
•     xml
•     txt
finding a file
      where /R C:\ local.txt
      Get-ChildItem -Path "C:\" -Recurse -Filter "local.txt"
      dir /s /b local.txt
      dir /s /b proof.txt
      Get-ChildItem -Path C:\ -Recurse -Filter "local.txt"
      Get-ChildItem -Path C:\ -Recurse -Filter "proof.txt"

Find a type of file
      where /R C:\ *.txt
      Get-ChildItem -Path "C:\" -Recurse -Filter "*.db" -Force -ErrorAction SilentlyContinue
Finding types of file (can be really messy, limit the number of extensions!)
      @for %e in (*.bak *.db *.txt *.doc *.docx *.xls *.xlsx *.kdbx) do @dir C:\%e /S /B 2>nul
      Get-ChildItem -Path "C:\" -Recurse -Include "*.bak", "*.db", "*.doc", "*.docx", "*.xls", "*.xlsx", "*.kdbx", "*.zip", "*.vbs" -ErrorAction SilentlyContinue -Force
```
{% endcode %}



