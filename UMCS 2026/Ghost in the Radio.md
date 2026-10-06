# Ghost in the Radio

- **Category:** Cryptography
- **Flag:** `UMCS{XOR_M4G1C_BYT3S}`

---

## 1. Challenge Overview
The challenge provides a corrupted binary file (`corrupted.bin`). Our objective is to analyze the file header, calculate the encryption key, and reverse a rolling XOR encryption scheme to restore the hidden PNG image containing the flag.

---

## 2. Step-by-Step Solution

### Step 1: Inspecting the Corrupted Header
1. I opened `corrupted.bin` using **HxD** (Hex Editor) to inspect the raw bytes.
2. The first 8 bytes of the file were: `CB 13 0A 02 4B 4D 52 43`.

<img width="601" height="185" alt="image" src="https://github.com/user-attachments/assets/23cc566a-e585-43e7-9bf7-6e7bf92085db" />

### Step 2: Deriving the Initial XOR Key
1. The challenge provided a hint to XOR the first 4 bytes of the file with standard PNG magic bytes (`89 50 4E 47`).
2. By comparing the encrypted bytes with the expected PNG bytes, I derived the key used at each position:
   - `0xCB ^ 0x89 = 0x42`
   - `0x13 ^ 0x50 = 0x43`
   - `0x0A ^ 0x4E = 0x44`
   - `0x02 ^ 0x47 = 0x45`

### Step 3: Automating Rolling XOR Decryption with Python
1. The encryption used a rolling key that drifted over time following the formula: $(0x42 + i) \pmod{256}$, where $i$ is the byte index.
2. I prepared a Python script to loop through the file byte-by-byte, applying this dynamic mask to synchronize with the encryption's drift.
3. This process cancelled the mathematical interference, repaired the file structure, and restored the hidden PNG file.
4. Opening the restored `signal_recovered.png` file revealed the flag: `UMCS{XOR_M4G1C_BYT3S}`.

<img width="1837" height="758" alt="image" src="https://github.com/user-attachments/assets/a76e6582-c691-49fd-8786-274da45ad4c6" />

---

## 3. Python Script

```python
# Open the corrupted binary file
with open("corrupted.bin", "rb") as f:
    encrypted_data = f.read()

decrypted_bytes = bytearray()

# Loop through each byte and apply the rolling XOR key: (0x42 + i) % 256
for i, byte in enumerate(encrypted_data):
    key = (0x42 + i) % 256
    decrypted_byte = byte ^ key
    decrypted_bytes.append(decrypted_byte)

# Save the restored PNG image
with open("signal_recovered.png", "wb") as f:
    f.write(decrypted_bytes)

print("[+] Successfully restored signal_recovered.png!")
```
