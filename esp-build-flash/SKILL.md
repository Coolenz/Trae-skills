---
name: esp-build-flash
aliases:
  - esp32idfbuild
description: Build and flash an ESP-IDF project to a specific board. Usage: esp-build-flash <project_path> <board> <port> [idf_path]
user-invocable: true
allowed-tools:
  - Bash
  - Read
  - Write
  - Glob
---

# /esp-build-flash — Build and Flash ESP-IDF Project

Build an ESP-IDF project and flash it to the target board.

## Arguments

`$ARGUMENTS` = `<project_path> <board> <port> [idf_path]`

| Parameter | Description | Example |
|---|---|---|
| `project_path` | Absolute path to the ESP-IDF project | `C:\CC Switch\esp-who\examples\qrcode_recognition` |
| `board` | BSP board name | `esp32_s3_eye`, `esp32_p4_function_ev_board`, `esp32_s3_korvo_2` |
| `port` | Serial COM port | `COM7` |
| `idf_path` | (optional) ESP-IDF path, defaults to `D:\esp\v5.5.4\esp-idf` | `D:\esp\v5.5.4\esp-idf` |

### Supported Boards

- `esp32_s3_eye` → ESP32-S3-EYE (target: esp32s3)
- `esp32_p4_function_ev_board` → ESP32-P4-FUNCTION-EV-BOARD (target: esp32p4)
- `esp32_s3_korvo_2` → ESP32-S3-KORVO-2 (target: esp32s3)

## Pre-flight Checks

1. Validate all 3 required arguments are provided.
2. Verify `project_path` exists (read it or glob for CMakeLists.txt).
3. Verify `idf_path` exists and contains `tools/idf.py`.
4. Verify port is accessible (if not, warn but proceed with flash).

## Environment Setup

Always run via PowerShell with `-NoProfile` to avoid profile interference:

```
powershell.exe -NoProfile -Command "Remove-Item Env:MSYSTEM -ErrorAction SilentlyContinue; ..."
```

### Required Environment Variables

| Variable | Value |
|---|---|
| `IDF_PATH` | The `idf_path` argument (or default) |
| `IDF_PYTHON_ENV_PATH` | `D:\Espressif\tools\python\v5.5.4\venv` |
| `IDF_TOOLS_PATH` | `D:\esp` |
| `IDF_EXTRA_ACTIONS_PATH` | `<project_path>\..\tools` (parent's tools dir, for BSP extensions) |
| `PATH` additions | cmake bin, ninja, xtensa-esp-elf bin, riscv32-esp-elf bin |

### PATH Additions (prepend)

```
D:\Espressif\tools\cmake\4.0.3\bin
D:\Espressif\tools\ninja\1.12.1
D:\Espressif\tools\xtensa-esp-elf\esp-14.2.0_20260121\xtensa-esp-elf\bin
D:\Espressif\tools\riscv32-esp-elf\esp-14.2.0_20260121\riscv32-esp-elf\bin
```

## Step 1: Set Environment Variable

Before any idf.py commands, set `IDF_EXTRA_ACTIONS_PATH` to the esp-who tools directory:

```powershell
$env:IDF_EXTRA_ACTIONS_PATH="<project_path>\..\tools\"
```

For esp-who projects, the tools directory is at `esp-who/tools/`.

## Step 2: Set Target SOC

Set the target chip and copy BSP config files:

```powershell
idf.py -DSDKCONFIG_DEFAULTS="sdkconfig.bsp.<board>" set-target "<chip>"
```

For PowerShell, use quotes:
```powershell
idf.py -DSDKCONFIG_DEFAULTS="sdkconfig.bsp.<board>" set-target "<chip>"
```

This will:
1. Copy `sdkconfig.bsp.<board>` → `sdkconfig`
2. Copy `dependencies.lock.<board>` → `dependencies.lock`
3. Set the target chip

Chip mapping:
- `esp32_s3_eye` → `esp32s3`
- `esp32_p4_function_ev_board` → `esp32p4`
- `esp32_s3_korvo_2` → `esp32s3`

## Step 3: Build

Run via PowerShell with all env vars set:

```powershell
powershell.exe -NoProfile -Command "
  Remove-Item Env:MSYSTEM -ErrorAction SilentlyContinue;
  \$env:IDF_PATH='<idf_path>';
  \$env:IDF_PYTHON_ENV_PATH='D:\Espressif\tools\python\v5.5.4\venv';
  \$env:IDF_TOOLS_PATH='D:\esp';
  \$env:IDF_EXTRA_ACTIONS_PATH='<extra_actions_path>';
  \$env:PATH='D:\Espressif\tools\cmake\4.0.3\bin;D:\Espressif\tools\ninja\1.12.1;D:\Espressif\tools\xtensa-esp-elf\esp-14.2.0_20260121\xtensa-esp-elf\bin;D:\Espressif\tools\riscv32-esp-elf\esp-14.2.0_20260121\riscv32-esp-elf\bin;' + \$env:PATH;
  D:\Espressif\tools\python\v5.5.4\venv\Scripts\python.exe <idf_path>\tools\idf.py -C '<project_path>' build
"
```

**Timeout**: 600000ms (10 minutes).

**Success indicator**: Look for `Project build complete` in output.

## Step 4: Flash and Monitor

Flash and monitor in one command:

```powershell
idf.py -p <port> flash monitor
```

Or via PowerShell with full env setup:

```powershell
powershell.exe -NoProfile -Command "
  Remove-Item Env:MSYSTEM -ErrorAction SilentlyContinue;
  \$env:IDF_PATH='<idf_path>';
  \$env:IDF_PYTHON_ENV_PATH='D:\Espressif\tools\python\v5.5.4\venv';
  \$env:IDF_TOOLS_PATH='D:\esp';
  \$env:IDF_EXTRA_ACTIONS_PATH='<extra_actions_path>';
  \$env:PATH='D:\Espressif\tools\cmake\4.0.3\bin;D:\Espressif\tools\ninja\1.12.1;D:\Espressif\tools\xtensa-esp-elf\esp-14.2.0_20260121\xtensa-esp-elf\bin;D:\Espressif\tools\riscv32-esp-elf\esp-14.2.0_20260121\riscv32-esp-elf\bin;' + \$env:PATH;
  D:\Espressif\tools\python\v5.5.4\venv\Scripts\python.exe <idf_path>\tools\idf.py -C '<project_path>' -p <port> flash monitor
"
```

**Timeout**: 300000ms (5 minutes) for flash, monitor runs continuously.

**Success indicator**: Look for `Hard resetting via RTS pin...` and `Done`.

## Error Handling

- If `MSYSTEM` warning appears → already handled by `Remove-Item Env:MSYSTEM`
- If `IDF_PYTHON_ENV_PATH` warning about mismatch → safe to ignore, still works
- If `C:\esp\python_env\...` not found → means `IDF_TOOLS_PATH` not set correctly, set to `D:\esp`
- If port access denied → check no other monitor/flash process is using it, ask user to unplug/replug
- If build fails → report the cmake or compilation error line
- If "BSP is not defined" error → ensure `IDF_EXTRA_ACTIONS_PATH` is set to esp-who/tools directory

## Quick Reference (from ESP-WHO README)

Based on [ESP-WHO README](../../E:/Esp32_s3_eye/esp-who-master/README.md):

1. **Set environment**: `IDF_EXTRA_ACTIONS_PATH=/path_to_esp-who/tools/`
2. **Set target**: `idf.py -DSDKCONFIG_DEFAULTS=sdkconfig.bsp.<board> set-target <chip>`
3. **Build flash monitor**: `idf.py [-p port] flash monitor`

### Board to Chip Mapping

| Board Name | BSP File | Chip |
|---|---|---|
| ESP32-S3-EYE | `sdkconfig.bsp.esp32_s3_eye` | esp32s3 |
| ESP32-P4 Function EV Board | `sdkconfig.bsp.esp32_p4_function_ev_board` | esp32p4 |
| ESP32-S3-Korvo-2 | `sdkconfig.bsp.esp32_s3_korvo_2` | esp32s3 |

## Output to User

After successful flash, report:
- Project name and board
- Flashed binary sizes (bootloader, app, partition)
- Confirmation that board should be running
