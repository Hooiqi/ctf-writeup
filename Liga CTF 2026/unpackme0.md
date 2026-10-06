# unpackme0

- **Category:** Reverse Engineering
- **Flag:** `OWASPKL{1cc6a3b62cac36ab18e0c4685a7f4bdf}`

---

## 1. Challenge Overview
The challenge provides a packed executable file named `unpackme0`. The goal is to identify the packer, unpack the binary, and calculate its MD5 hash to obtain the flag.

---

## 2. Step-by-Step Solution

### Step 1: Check the Packer
1. I checked the binary for signatures of common executable packers.
2. The file contained UPX signatures, confirming that it was packed using **UPX (Ultimate Packer for eXecutables)**.

### Step 2: Decompress the File
1. In the Linux terminal, I used the `upx` utility with the `-d` (decompress) option:
   ```bash
   upx -d -o unpacked_binary unpackme0
   ```
2. The file decompressed successfully into `unpacked_binary`.

### Step 3: Compute the MD5 Hash
1. Based on the challenge instructions, the flag is derived from the MD5 hash of the unpacked binary.
2. I ran `md5sum` on the output file:
   ```bash
   md5sum unpacked_binary
   ```
3. The command returned the hash `1cc6a3b62cac36ab18e0c4685a7f4bdf`.
4. Wrapping the hash in the competition flag format yields: `OWASPKL{1cc6a3b62cac36ab18e0c4685a7f4bdf}`.

---

## 3. Terminal Commands

```bash
# Unpack the binary
upx -d -o unpacked_binary unpackme0

# Compute the MD5 hash
md5sum unpacked_binary
```

<img width="743" height="237" alt="image" src="https://github.com/user-attachments/assets/a63b7b46-9da9-411d-993e-aa7971c57679" />
