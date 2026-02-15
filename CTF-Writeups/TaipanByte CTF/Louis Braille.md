# Louis Braille
## Challenge
- Cryptography
- TaipanByte CTF
- 10 Points

### Description
>⠠⠠⠞⠃⠸⠣⠼⠁⠨⠤⠙⠼⠚⠝⠞⠨⠤⠎⠼⠉⠉⠨⠤⠠⠁⠝⠽⠨⠤⠏⠗⠼⠚⠰⠃⠇⠼⠉⠍⠎⠸⠜

Challenge URL: https://chart.taipanbyte.ru/

![image2](image2)

### Objective
Decode the Braille-encoded flag to retrieve the plaintext.

## Analysis
The challenge title "Louis Braille" immediately suggests that the encrypted text is written in Braille, a tactile writing system used by people who are blind or visually impaired.

Looking at the description, we can see a series of Braille characters (dots arranged in patterns). The challenge is to decode these Braille characters back into readable text.

## Theory
**Braille System**:
Braille is a tactile writing system invented by Louis Braille in 1824. It consists of patterns of raised dots arranged in cells of up to six dots in a 3×2 configuration. Each pattern represents a letter, number, punctuation mark, or special symbol.

Key characteristics of Braille:
- Each Braille cell consists of 6 dots arranged in 2 columns of 3 dots each
- Different combinations of raised dots represent different characters
- Numbers and special characters use indicator symbols
- Capital letters are preceded by a capital indicator
- The system is standardized across multiple languages

In digital representation, Braille is encoded using Unicode characters in the range U+2800 to U+28FF, which is what we see in this challenge.

## Solution
1. **Identify the encoding**: The title "Louis Braille" and the pattern of dots confirm this is Braille encoding.

2. **Use a Braille decoder**: I navigated to https://abcbraille.com/braille to decode the message.
   - Input: `⠠⠠⠞⠃⠸⠣⠼⠁⠨⠤⠙⠼⠚⠝⠞⠨⠤⠎⠼⠉⠉⠨⠤⠠⠁⠝⠽⠨⠤⠏⠗⠼⠚⠰⠃⠇⠼⠉⠍⠎⠸⠜`
   - The decoder translates each Braille cell into its corresponding character
   - Result: `TB{1_d0nt_s33_Any_pr0bl3ms}`

3. **Verify the flag**: The decoded text follows the TaipanByte CTF flag format `TB{...}` and contains leetspeak (1 for i, 3 for e, 0 for o).

**Flag**: `TB{1_d0nt_s33_Any_pr0bl3ms}`

## How to avoid
When implementing Braille challenges securely:
- Braille is easily identifiable and decoded with online tools
- Consider combining Braille with additional encryption layers
- For more difficulty, use contracted Braille (Grade 2) instead of uncontracted (Grade 1)
- The visual pattern of Braille dots makes it immediately recognizable - obfuscation may be necessary for harder challenges