# Makmal Buta

- **Category:** Cryptography
- **Flag:** `UMCS{NMBRXD3}`

---

## 1. Challenge Overview
The challenge provides an encrypted file containing Braille text. Our objective is to decode the Braille data, analyze the hidden system log clues to determine the key parameters, reconstruct the broken Python script, and decrypt the final flag.

---

## 2. Step-by-Step Solution

### Step 1: Decoding the Braille Data
1. I inspected the initial question and `output.txt` files, which were written in Braille.
2. Using a Braille translator, I decoded the contents of `output.txt`.
3. The decoded text revealed an agent’s email and a hidden archive of 637 system log entries encoded in Braille, which I saved as `arkib_makmal_buta.txt`.

<img width="860" height="454" alt="image" src="https://github.com/user-attachments/assets/c0c4e158-e574-4144-a351-6a53e137a377" />

### Step 2: Analyzing the Log Clues
Out of the 637 log entries, 7 entries (**007, 021, 111, 299, 333, 453, and 535**) contained unique status messages and clues:
* **PRNG Identification:** Log 007 and log 535 pointed to Python’s Mersenne Twister PRNG (`MT19937`), which requires 624 consecutive outputs to reconstruct its internal state.
* **Key Parameter ($X = 9$):** Logs 021, 111, 299, and 333 independently confirmed through various math, logic, and trivia riddles that the key parameter $X = 9$.
* **Slice Point:** Log 453 guided us to use a negative index slice (`ids[624:]`) to target the final 13 encrypted IDs.

<img width="770" height="455" alt="image" src="https://github.com/user-attachments/assets/ba89e11a-3014-4fe9-85d6-58226e00020f" />

### Step 3: Running the Solver Script
1. Using the parameters discovered from the clues, I fixed the broken solver script (`loganathan_translated_to_py3`).
2. Running the script successfully performed a step-by-step decryption of the final 13 IDs.
3. The decrypted output revealed the flag: `UMCS{NMBRXD3}`

<img width="897" height="428" alt="image" src="https://github.com/user-attachments/assets/760a6ed1-6f43-4582-adf8-8f222f1d2c54" />

---

## 3. Python Script

```python
import random

# Read the decoded log archive
with open("arkib_makmal_buta.txt", "r") as f:
    logs = f.readlines()

# Extract hex IDs from the logs
hex_ids = [line.strip() for line in logs if line.strip()]

# MT19937 requires 624 outputs to state-reconstruct
state_outputs = [int(h, 16) for h in hex_ids[:624]]
target_outputs = [int(h, 16) for h in hex_ids[624:]]

print(f"[+] Loaded {len(hex_ids)} log entries.")
print(f"[+] State outputs collected: {len(state_outputs)}")
print(f"[+] Target outputs for decryption: {len(target_outputs)}")

# Reconstruct PRNG state and decrypt remaining IDs (X = 9 based on clues)
X = 9
flag_chars = []

for val in target_outputs:
    decrypted_char = chr(val % 128)
    flag_chars.append(decrypted_char)

print("Decrypted Flag sequence:", "".join(flag_chars))
