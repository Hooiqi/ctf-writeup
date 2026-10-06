# Capybara Secret

- **Category:** Steganography
- **Flag:** `KubSTU{W0W_1ncred1ble_capyba6a}`

---

## 1. Challenge Overview
The challenge gives an image of a capybara (`challenge.jpg`). Our goal is to find the hidden text inside the image and get the flag.

---

## 2. Step-by-Step Solution

### Step 1: Scan the Image
1. I uploaded `challenge.jpg` to the online tool **Aperi'Solve**.
2. In the results under **Common password(s)**, I found a hidden text string:
   `XhoFGH{J0J_1aperq1oyr_pnclon6n}`
   
<img width="773" height="312" alt="image" src="https://github.com/user-attachments/assets/feabf11a-b549-4aa3-8203-f4c8ff3f72b2" />

### Step 2: Decode with ROT13
1. The start of the text `XhoFGH` looked like a cipher for the flag prefix `KubSTU`.
2. I used **ROT13** (shift letters by 13) to decode the text:
   - `XhoFGH` becomes `KubSTU`
   - `J0J` becomes `W0W`
   - `1aperq1oyr` becomes `1ncred1ble`
   - `pnclon6n` becomes `capyba6a`
3. Putting them together gave the final flag: `KubSTU{W0W_1ncred1ble_capyba6a}`.

---

## 3. Python Script

```python
import codecs

cipher = "XhoFGH{J0J_1aperq1oyr_pnclon6n}"
flag = codecs.decode(cipher, "rot_13")

print("Flag:", flag)
