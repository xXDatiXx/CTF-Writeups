# ABA ABA?
## Challenge
- Cryptography
- TaipanByte CTF
- 25 Points

### Description
>BAABAAAAAAБ{AAAABAAAAAAABAAВBABABBAА_ABAAAВAААB_ВАABАAAАААBAAABBAABABBA}

Challenge URL: https://chart.taipanbyte.ru/

### Objective
Decode the encrypted flag using cipher identification and decryption techniques.

## Analysis
The flag appears to be encrypted with a substitution cipher. The pattern consists mainly of the letters A and B, along with some Cyrillic characters (Б and В) that look similar to B and V.

First, I used https://www.dcode.fr/cipher-identifier to identify the cipher type. By pasting the encrypted flag, the cipher identifier suggested it was a **Bacon cipher**.

## Theory
**Bacon Cipher**:
The Bacon cipher is a method of steganography and substitution cipher that encodes each letter of the plaintext with a group of 5 binary elements. Traditionally, it uses two distinct letters (commonly A and B) to represent the binary digits.

In this cipher:
- Each letter of the alphabet is encoded as a unique 5-bit sequence
- The pattern of A's and B's represents different letters
- The cipher can be disguised within other text or mixed with look-alike characters

The key characteristic that identified this as a Bacon cipher was the predominant use of two characters (A and B) with occasional look-alike substitutes (Б and В), which is typical of this encryption method.

## Solution
1. **Identify the cipher**: I entered the encrypted text into https://www.dcode.fr/cipher-identifier
   - The tool identified it as a Bacon cipher

2. **Decode the message**: I used https://www.dcode.fr/bacon-cipher to decrypt the flag
   - Input: `BAABAAAAAAБ{AAAABAAAAAAABAAВBABABBAА_ABAAAВAААB_ВАABАAAАААBAAABBAABABBA}`
   - The decoder treated Б and В as B variants
   - Result: `BACONISTASTY`

3. **Format the flag**: Following the TaipanByte CTF flag format `TB{...}`
   - Final flag: `TB{BACON_IS_TASTY}`

**Flag**: `TB{BACON_IS_TASTY}`

## How to avoid
When creating cryptographic challenges:
- Avoid using classical ciphers that are easily identifiable by automated tools
- Don't use obvious patterns like predominantly two-character substitutions
- Consider combining multiple encryption methods
- Avoid using look-alike characters (like Б for B) as they make the cipher more obvious
