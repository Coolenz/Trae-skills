---
name: "gcc-powershell"
description: "Compiles C/C++ code using GCC in PowerShell. Invoke when user asks to compile C code in PowerShell or needs GCC compiler."
---

# GCC in PowerShell

This skill enables GCC compilation in PowerShell environment on Windows with MSYS2 MinGW.

## GCC Path

```
C:\msys64\mingw64\bin\gcc.exe
```

## How to Invoke GCC in PowerShell

### Method 1: Set PATH temporarily

```powershell
$env:Path = "C:\msys64\mingw64\bin;$env:Path"
gcc --version
```

### Method 2: Use full path directly

```powershell
C:\msys64\mingw64\bin\gcc.exe your_file.c -o output.exe
```

## Compilation Examples

### Basic compilation
```powershell
$env:Path = "C:\msys64\mingw64\bin;$env:Path"
gcc example.c -o example.exe
```

### With include/library paths
```powershell
$env:Path = "C:\msys64\mingw64\bin;$env:Path"
gcc generate_qr.c -o generate_qr.exe -Ic:\msys64\mingw64\include -Lc:\msys64\mingw64\lib -lqrencode -lpng -lz -lwinpthread
```

### Compile and link multiple libraries
```powershell
$env:Path = "C:\msys64\mingw64\bin;$env:Path"
gcc -c source.c -o source.o
gcc source.o -o program.exe -lLibrary1 -lLibrary2
```

## Common Issues

1. **"gcc: command not found"** - Add to PATH: `$env:Path = "C:\msys64\mingw64\bin;$env:Path"`
2. **Linker errors** - Make sure -L path points to correct library directory
3. **Header not found** - Use -I flag to specify include directory

## Libraries Available

- libqrencode
- libpng (png.h)
- zlib (zlib.h)
- winpthread
