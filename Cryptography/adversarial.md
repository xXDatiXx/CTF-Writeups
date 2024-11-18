# Adversarial
Adversarial: crypto
The goal of this analysis is to decrypt a series of encrypted messages using AES in CTR mode with a static initialization vector (IV).
Link: [Google Drive Link](https://drive.google.com/file/d/1vS5hLQ6h7gqjEx7_VcljRe8hsuu8sV1j/view?usp=drive_link)
# Description
You have a new mission from HQ to hunt down some rogue flags. Check the details in the assignment. Find something good, and you'll be rewarded with one better.
**HINT:** flags you discover are not in flag format.
`nc {box} {port}`
# Tools Used
- **Python**
- **Dockers**
# Solution Process
- AES-CTR-128
    - Mode of encryption, in counter mode
        - In counter mode, AES encrypts a counter for each block of text.
    - IV.encode("Hex") should be random, not static
        - The IV, or Initialization Vector, is a unique, non-secret value used to add randomness to the encryption process, preventing predictable patterns in the encrypted text.
    - It essentially works like XOR.
### XOR
The truth table for XOR between two bits `A` and `B` is as follows:
| `A` | `B` | `A XOR B` |
|-----|-----|-----------|
|  0  |  0  |     0     |
|  0  |  1  |     1     |
|  1  |  0  |     1     |
|  1  |  1  |     0     |
## Step 1: Base64 Decoding
The messages are encoded in base64. The first step is to convert these base64-encoded data back to their original binary form. Identify the encryption key and obtain the plaintext messages.
### Strategy
I use a frequency analysis approach to estimate each byte of the key:
- Iterate over each byte position in the ciphertexts.
- For each position, try all possible single-byte keys (0-255).
- Apply the key to all bytes at that specific position across all ciphertexts.
- Evaluate the result based on the frequency of printable ASCII characters, especially common characters in English.
Once the probable key is identified, proceed to decrypt the ciphertexts using this key.
# Source Code
### solucion.py
```python
import base64
from collections import Counter
import string
# Ciphertexts in base64
ciphertexts_base64 = [
    # base64 strings...
]
# Decode from Base64
ciphertexts = [base64.b64decode(ciphertext) for ciphertext in ciphertexts_base64]
# Function to decrypt using frequency analysis
def decrypt_with_frequency_analysis(ciphertexts):
    # Assume all ciphertexts are the same length, take the smallest one
    key_length = min(len(ciphertext) for ciphertext in ciphertexts)
    key = []
    # Test each byte of the key
    for i in range(key_length):
        # Collect the i-th byte from each ciphertext
        ith_bytes = []
        for ciphertext in ciphertexts:
            if len(ciphertext) > i:  # Check the length, just in case
                ith_bytes.append(ciphertext[i])
        # Test all possible single-byte keys
        frequencies = Counter()
        for possible_key in range(256):
            decoded_bytes = [b ^ possible_key for b in ith_bytes]
            decoded_text = ''.join(chr(byte) for byte in decoded_bytes if chr(byte) in string.printable)
            score = sum(decoded_text.count(c) for c in 'etaoin shrdlu')  # common English letters
            frequencies[possible_key] = score
        best_key = max(frequencies, key=frequencies.get)
        key.append(best_key)
    return bytes(key)
# Find the key
key = decrypt_with_frequency_analysis(ciphertexts)
print("Probable key found:", key)  # Print the probable key in hexadecimal
# Decrypt messages for verification
for ciphertext in ciphertexts:
    plaintext = bytes([c ^ k for c, k in zip(ciphertext, key)])
    print("Decrypted text:", plaintext)
```
# Results
### Running solucion.py
```bash
dati@fedora ~/D/U/C/adversarial> python3 solucion.py
Probable key found: b'\xb6\x83...]'
Decrypted text: b"lhat is real? How do you define r$al? If you're talki/g abouu what you *an feel,!what you can smell, what you eanataste and see, then "
Decrypted text: b"ueo, sooner or later you're goingato realize, just asaI did,!that therens a diffdrence between knowing the patn,  nd walking the path."
Decrypted text: b'Ohe flag is: 4fb81eac0729a -- theaflag is: 4fb81eac07s9a -- uhe flag iss 4fb81eab0729a -- the flag is: 4fb81eae07s9a -- the flag is: 4'
# More decrypted texts...
```
### Server Response
```bash
dati@fedora ~> nc 127.0.0.1 8000
Hello Morpheus. Back from the mission so quickly? I see.
Well, what flags have you discovered? See, if I like what you have, I'll be willing to trade with you...
> 4fb81eac0729a
flag{m1ss1on_acc00mpl11shheedd!!}
```
