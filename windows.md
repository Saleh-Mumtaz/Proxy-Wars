Legacy right-click menu<br/><br/>
Open cmd and enter
```
reg.exe add "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32" /f /ve
```
to revert back to new one
```
reg.exe delete "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}" /f
```
after using any of them restart explorer Win+R
```
taskkill /f /im explorer.exe & start explorer.exe
```

---
"Admin CMD" option in right click menu<br/><br/>
open regedit with run and go to
```
HKEY_CLASSES_ROOT\DesktopBackground\Shell\
```
Right-click Shell → New → Key, name it "AdminCMD".<br/><br/>
Inside the new key, create another key named "command".<br/><br/>
Set the (Default) value inside "command" to:
```
cmd.exe /k "cd /d %V"
```
Now, right-click Desktop → "Admin CMD" opens CMD as Admin.<br/><br/>
to set Icon for it<br/><br/>
Right-click on the Admin CMD key → New → String Value.<br/><br/>
Name it Icon (case-sensitive).<br/><br/>
Double-click Icon and set its value to:
```
%SystemRoot%\System32\cmd.exe,0
```
or copy paste following code in notepad and save as AdminCMD.reg then right click and choose merge</br></br>
```
Windows Registry Editor Version 5.00

[HKEY_CLASSES_ROOT\Directory\Background\shell\AdminCMD]
"Icon"="cmd.exe,0"

[HKEY_CLASSES_ROOT\Directory\Background\shell\AdminCMD\command]
@="powershell -windowstyle hidden -Command \"Start-Process cmd.exe -ArgumentList '/k cd /d %V' -Verb RunAs\""
```
Restart File Explorer (Apply Changes)<br/><br/>
Open Task Manager (Ctrl + Shift + Esc).<br/><br/>
Find Windows Explorer → Restart.<br/><br/>
or use run Win+R
```
taskkill /f /im explorer.exe & start explorer.exe
```

---
to solve blurry properties window in windows 10/11, with 14 inch displays.<br/><br/>
cause is recommended scaling by windows, manual High DPI scaling override takes too much time.<br/><br/>
open cmd as administrator and enter:
```
setx __COMPAT_LAYER HighDpiAware /M
```
to revert
```
setx __COMPAT_LAYER "" /M
```
or
```
reg delete "HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\Environment" /v __COMPAT_LAYER /f
```

---

Get MD5 hash of files inside a folder</br></br>
Open terminal in that folder and run:</br></br>
```
Get-ChildItem -Path "C:\downloads" -Recurse -File | Get-FileHash -Algorithm MD5 | Select-Object Hash | Format-Table -HideTableHeaders | Out-File -Append X:\PATH\md5_checksums.txt
```
Do not set the txt output path inside the folder itself.

---

If wifi keeps disconnecting automatically, Win + (shift+x) > (shift+m) > adapters > disable "Alow the computer to turn off this device to save power"

---

Use --silent-debugger-extension-api to hide started debugging this browser banner.</br>
Find shortuts and copen properties. Then in target field add --silent-debugger-extension-api after what was already there.<br/>
```"C:\Program Files\Google\Chrome\Application\chrome.exe" --silent-debugger-extension-api```
Usually, one shortcut on desktop, the other is pinned in taskbar, present in ```C:\ProgramData\Microsoft\Windows\Start Menu\Programs```


---
