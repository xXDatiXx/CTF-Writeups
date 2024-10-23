# Missing Pieces
## Challenge
- Cryptography
- Beginner
- DEADFACE CTF

>We found this code from deephax that looks like it was built to hide a flag using some form of cryptography.
>The program is missing code that will reveal the flag. Try to fix the program and submit the flag.
>
>Submit the flag as flag{flag_text}.

```c++
# include <stdio.h>
# include <string.h>

const char *hex_string = "b52195a4a82bc5ade23e9c9c8725c79cb07d90f0ae";  // The hex string for the flag
const char *key = "d34df4c3";

int hex_char_to_int(char c) {
    if (c >= '0' && c <= '9') return c - '0';
    if (c >= 'a' && c <= 'f') return c - 'a' + 10;
    if (c >= 'A' && c <= 'F') return c - 'A' + 10;
    return -1;
}

void hex_string_to_bytes(const char *hex, unsigned char *bytes, size_t length) {
    for (size_t i = 0; i < length; ++i) {
        bytes[i] = (hex_char_to_int(hex[i * 2]) << 4) | hex_char_to_int(hex[i * 2 + 1]);
    }
}

void xor_bytes(unsigned char *data, size_t data_length, const unsigned char *key, size_t key_length) {
    for (size_t i = 0; i < data_length; ++i) {
        data[i] ^= key[i % key_length];
    }
}

int main() {
    size_t flag_length = strlen(hex_string) / 2;
    unsigned char flag[flag_length];
    unsigned char key_bytes[4];

    // Convert hex string to bytes
    hex_string_to_bytes(hex_string, flag, flag_length);
    hex_string_to_bytes(key, key_bytes, 4);

    xor_bytes(flag, flag_length, key_bytes, 4);

    // Print the result as a string
    printf("Resulting string: ");
    for (size_t i = 0; i < flag_length; ++i) {
        printf("%c");
    }
    printf("\n");

    return 0;
}
```
## Analysis
When you run the code we can see that nothing returns:

![image](https://github.com/user-attachments/assets/521d13b8-7d47-4e9f-b51b-696e550188d6)

Then the error may be when printing the flag.
In the code line `printf ("% c"); `we can see that you need the value to print.

## Solution
Modify the print code line to print the flag: `printf ("% c ", flag [i]);`.

## How to avoid
Do not leave the encryption key within reach of other people.

## Solution
When we execute the code it gives us the flag.

```c++
#include <stdio.h>
#include <string.h>

const char *hex_string = "b52195a4a82bc5ade23e9c9c8725c79cb07d90f0ae";  // The hex string for the flag
const char *key = "d34df4c3";

int hex_char_to_int(char c) {
    if (c >= '0' && c <= '9') return c - '0';
    if (c >= 'a' && c <= 'f') return c - 'a' + 10;
    if (c >= 'A' && c <= 'F') return c - 'A' + 10;
    return -1;
}

void hex_string_to_bytes(const char *hex, unsigned char *bytes, size_t length) {
    for (size_t i = 0; i < length; ++i) {
        bytes[i] = (hex_char_to_int(hex[i * 2]) << 4) | hex_char_to_int(hex[i * 2 + 1]);
    }
}

void xor_bytes(unsigned char *data, size_t data_length, const unsigned char *key, size_t key_length) {
    for (size_t i = 0; i < data_length; ++i) {
        data[i] ^= key[i % key_length];
    }
}

int main() {
    size_t flag_length = strlen(hex_string) / 2;
    unsigned char flag[flag_length];
    unsigned char key_bytes[4];

    // Convert hex string to bytes
    hex_string_to_bytes(hex_string, flag, flag_length);
    hex_string_to_bytes(key, key_bytes, 4);

    // Apply XOR operation
    xor_bytes(flag, flag_length, key_bytes, 4);

    // Print the result as a string
    printf("Resulting string: ");
    for (size_t i = 0; i < flag_length; ++i) {
        printf("%c", flag[i]);  // Corrected line
    }
    printf("\\n");

    return 0;
}
```
![image](https://github.com/user-attachments/assets/6027ab7a-e4a1-4f26-927e-553c55295892)

`flag{f1n1sh_Th3_c0d3}`
