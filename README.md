# Learning-ctypes-package

## Loading dynamic link libraries
ctype exports the cdll, and on windows windll and oledll objectes, for loading dynamic link libraries.
Libraries are loaded by accessing them as attributes of these objectes. cdll loads libraries which export functions using  the standard cdecl calling convention,
while windll libraries call functions using the stdcalling convention. oledll also uses the stdcall calling convention, and assumes the function returns a Windwos HRESULT error code.
The error code is used to automatically raise an OSError exception when the function call fails.

```Python
from ctypes import *
print("Loading library\n")
DllHandle = windll.kernel32
print("DllHandle:\t", DLLHandle)
```

## Accessing functions from loaded dlls

```Python
AddressOfFunc = DLLHandle.GetModuleHandleA
```
*NOTE:* Win32 system dlls like kernel32 and user32 often export ANSI as well as UNICODE versions of a function. The UNICODE version is exported with and W appended to the name, 
while the ANSI version is exported with an A appended to the name.

Sometimes , dlls export functions woth names which aren't vsild python identifiers, like "??2@YAPAXI@Z". In this case you have to use *getattr()* to retrieve the function:
```Python
getattr(cdll.msvcrt, "??2@YAPAXI@Z")
```

On Windows, some Dlls export functions not by name but by ordinal. These functions can be asscessd by indexing the dll ovjects with the ordinal number:
```Python
windll.kernel32[23]
```

## Calling function
These functions can be called like any other python function. This example uses the time() fiunction, which returns system time in seconds since the Unix epoch,
and GetModuleHandleA() functions , which returns a win32 module handle.
None should be used a NULL pointer.
```Python
cdll.libc.time(None)
windll.kernel32.GetModuleHandleA("user32.dll")
```
