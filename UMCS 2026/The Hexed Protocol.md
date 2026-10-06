# The Hexed Protocol

- **Category:** Cryptography
- - **Flag:** `UMCS{m4sk_4tt4cks_b34t_brut3_f0rc3}`

---

## 1. Challenge Overview
The challenge provides a raw hexadecimal data dump instead of a standard downloadable file. Our objective is to identify the file format, recover the file, and unlock its contents to retrieve the hidden flag.

---

## 2. Step-by-Step Solution

### Step 1: Reconstructing the File from Hex
1. I inspected the raw text file containing the long string of hexadecimal data.
2. I copied the hex dump into **CyberChef** and applied the **From Hex** recipe.
3. The converted binary output matched a KeePass password manager database format, so I saved it as `vault.kdbx`.

### Step 2: Analyzing Clues in `scraps.md`
1. Attempting to open `vault.kdbx` directly prompted for a master password.
2. Reviewing the remaining challenge files revealed a file named `scraps.md`.
3. The file explicitly described the structure of the master password using a specific format:
   $$\text{Password} = \text{[Core Value]} + \text{[4-Digit PIN]} + \text{[One Special Character]}$$

### Step 3: Automating the Attack with Python
1. I wrote a Python script utilizing the `pykeepass` library to automate the brute-force process.
2. The script iterated through every 4-digit combination (`0000` to `9999`) combined with the core value and common special characters until successful authentication occurred.

### Step 4: Extracting the Flag
Once I got the correct password, I searched the database entries and found the flag: UMCS{m4sk_4tt4cks_b34t_brut3_f0rc3}

<img width="1919" height="1147" alt="image" src="https://github.com/user-attachments/assets/f19dc3aa-fa9f-42e8-8993-f6c7113735da" />


---

## 3. Python Script

```python
from pykeepass import PyKeePass, exceptions

kdbx_file = 'vault.kdbx'
core_value = 'CoreValueHere' # Replace with the actual core value found in the challenge
special_chars = ['!', '@', '#', '$', '%']

found = False

for pin in range(10000):
    pin_str = f"{pin:04d}"
    
    for char in special_chars:
        password = f"{core_value}{pin_str}{char}"
        
        try:
            # Try to open the database with the generated password
            kp = PyKeePass(kdbx_file, password=password)
            print(f"[+] SUCCESS! Password found: {password}")
            
            # Search entries for the flag
            for entry in kp.entries:
                print(f"Title: {entry.title} | Notes: {entry.notes}")
                
            found = True
            break
        except exceptions.CredentialsError:
            # Wrong password, keep trying
            continue
            
    if found:
        break

if not found:
    print("[-] Password not found.")
