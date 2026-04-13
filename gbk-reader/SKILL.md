---
name: "gbk-reader"
description: "Reads GBK/GB2312 encoded files correctly. Use this skill when reading Chinese source code files that fail with UTF-8 errors, especially older MFC projects."
---

# GBK Reader

This skill provides guidance for reading files with GBK (GB2312) encoding, commonly used in older Chinese Windows projects including MFC applications.

## When to Use This Skill

**INVOKE THIS SKILL when:**
- Reading files that fail with "stream did not contain valid UTF-8" error
- Working with MFC/Visual C++ projects from Chinese developers
- Handling legacy Windows source code files
- Any file operation involving GBK/GB2312 encoded source files

## Common File Types Affected

| Project Type | Location | Encoding |
|--------------|----------|----------|
| MFC Projects | `e:\Jichuangfinger\MR_CommTool_0325` | GBK |
| Visual C++ | `.cpp`, `.h`, `.rc` files | GBK/GB2312 |
| Legacy Windows | `.c`, `.h` files | ANSI/GBK |

## Workaround: Use Grep Instead of Read

When Read tool fails with encoding errors:

### 1. Use Grep for Code Search
```bash
# Search for patterns in problematic files
grep -n "pattern" file.cpp
```

### 2. Use Grep to Find Line Numbers
```bash
# Find specific function definitions
grep -n "OnCommunication" SCOMMDlg.cpp
```

### 3. Read Specific Line Ranges with Grep
```bash
# Get content with context
grep -n -A 10 -B 5 "function_name" file.cpp
```

## Tools That Work Better

| Tool | GBK Support | Use Case |
|------|-------------|----------|
| Grep | ? Good | Searching, finding patterns |
| Glob | ? Good | Finding files by pattern |
| LS | ? Good | Listing directories |
| Write | ? Good | Creating/modifying files |
| Edit | ? Good | Making changes |

## Known Issues with Read Tool

The Read tool may fail on:
- `.cpp` files with Chinese comments
- `.h` header files with Chinese includes
- `.rc` resource files (especially Chinese dialog text)
- Any file with non-ASCII Chinese characters

## Alternative: PowerShell File Reading

Use PowerShell to read GBK files:
```powershell
Get-Content -Path "file.cpp" -Encoding Default
```

## Best Practices

1. **Prefer Grep** over Read for searching code
2. **Use Glob** to locate files first
3. **Avoid Read** on known problematic files
4. **Use Line Numbers** from Grep to identify code locations
5. **Write operations** work fine - no encoding issues

## Example Workflow

Instead of:
```
Read file �� Error: Invalid UTF-8
```

Use:
```
Grep "function_name" file.cpp �� Get line number �� Read specific lines
```

Or:
```
Glob "*.cpp" in directory �� Identify file �� Grep for pattern �� Use line info
```

---

*This skill helps handle encoding issues common in legacy Chinese Windows projects.*