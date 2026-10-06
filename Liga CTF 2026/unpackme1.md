# unpackme1

* **Category:** Reverse Engineering
* **Flag:** `OWASPKL{Unpackm3_4mat3ur0923257}`

## 1. Challenge Overview

The challenge continued from unpackme0. The objective is to run the binary, capture its unpacked state in memory, and extract the hidden flag.

## 2. Step-by-Step Solution

### Step 1: Detect UPX Header Tampering

1. Running `upx -d unpackme1` returned an error.

2. The challenge author altered the standard UPX header signatures inside the binary to prevent static decompression with default tools.

### Step 2: Run and Freeze with GDB

1. I moved the binary to the `/tmp` directory.

2. I loaded the file into **GDB (GNU Debugger)** and ran it to allow the binary to unpack itself into memory.

3. I paused execution while the payload was fully decompressed in memory and dumped the memory state into a file named `memory.core`.

### Step 3: Extract the Flag from Memory Dump

1. Using the `strings` command piped into `grep`, I searched `memory.core` for the event flag prefix `OWASPKL`:

   ```bash
   strings memory.core | grep OWASPKL
   ```

2. The search returned the flag: `OWASPKL{Unpackm3_4mat3ur0923257}`.

<img width="958" height="145" alt="image" src="https://github.com/user-attachments/assets/850c60ef-2c0a-4750-85d9-5469386ed14e" />

## 3. Terminal Commands

```bash
# Dump memory inside GDB after execution pauses
(gdb) generate-core-file memory.core

# Search for the flag string inside the memory dump
strings memory.core | grep OWASPKL
```
