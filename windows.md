to solve blurry properties window in windows 10/11, with 14 inch displays.</br>
cause is recommended scaling by window, High DPI scaling override takes too much time.</br>
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
