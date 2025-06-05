Legacy right-click menu<br/><br/>

---
"Admin CMD" option in right click menu<br/><br/>
open regedit with run and go to
```
HKEY_CLASSES_ROOT\DesktopBackground\Shell\
```
Right-click Shell → New → Key, name it "Admin CMD".<br/><br/>
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
 Restart File Explorer (Apply Changes)<br/><br/>
Open Task Manager (Ctrl + Shift + Esc).<br/><br/>
Find Windows Explorer → Restart.<br/><br/>

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
