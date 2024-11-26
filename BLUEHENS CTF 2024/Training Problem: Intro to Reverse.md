# Training Problem: Intro to Reverse
## Challenge
- Reverse
- Bluehens CTF 2024
- Easy
### Description
Just a classic flagchecker
(Try using dogbolt.org)
## Analysis
Uploding the code to `dogbolt`
![image](https://github.com/user-attachments/assets/0b00e6e3-5141-4017-b68a-1631a21bd525)
In the section of `BinaryNinja` I found the next main function:
```C
int __fastcall main(int argc, const char **argv, const char **envp)
{
  int i; // [rsp+Ch] [rbp-44h]
  char v5[32]; // [rsp+10h] [rbp-40h] BYREF
  char s[24]; // [rsp+30h] [rbp-20h] BYREF
  unsigned __int64 v7; // [rsp+48h] [rbp-8h]

  v7 = __readfsqword(0x28u);
  strcpy(v5, "ucaqbvl,n*d\\'R#!!l");
  v5[19] = 0;
  fgets(s, 19, _bss_start);
  for ( i = 0; i <= 17; ++i )
  {
    if ( s[i] - i != v5[i] )
    {
      puts("wrong");
      return 1;
    }
  }
  puts("You got it!");
  return 0;
}
// 1189: using guessed type char s[24];
```

### Code Analysis

1. **Variables and Definitions**:
    - `v5` is a character string of size 32 that contains a specific character string (key): `" ucaqbvl, n * d \\\ 'R # !!l "`.
    -`s` is a 24 size character array that will be used to store user input.
    - `fgets (s, 19, _bss_start);` reads up to 18 characters from the input and stores them in`s`.
2. **Condition for the Right Message**:
The `for` loop performs a check for each character in the`s` entry. The condition that is verified is:
    
    ```c
    if (s [i] - i!= v5 [i])
    ```
    
    For each index `i`, `s [i] - i` is expected to be equal to the value` v5 [i]`.
    
3. **Deciphering the Right Entry**:
For the program to print `" You got it! "`` s [i] `must meet the condition:
    
    ```c
    s [i] = v5 [i] + i;
    ```
    This means that for each character in `v5`, we must add the corresponding value of `i` to get the input character `s [i]`.

## Theory
**String Manipulation**:
The condition `s[i] - i == v5[i]` implies a specific relationship between the characters of `s` (user input) and `v5` (hardcoded key). Understanding this relationship is critical to solving the challenge.

**Loop Behavior**:
The `for` loop iterates over indices from 0 to 17, comparing characters at each index. If any mismatch occurs, the program immediately outputs "wrong" and exits.

**Constraint Satisfaction**:
To pass the check, every character of `s` must exactly satisfy the condition `s[i] = v5[i] + i`. This makes the problem a straightforward case of reconstructing the required input.

## Solution
Python solution that calculates the correct input string `s` based on the condition `s[i] = v5[i] + i`.
```python
# Given v5 string
v5 = "ucaqbvl,n*d\\'R#!!l"

# Initialize an empty string for the correct input
correct_input = ""

# Loop through each character in v5 and calculate the corresponding character in s
for i in range(len(v5)):
    # Calculate the character for s[i]
    correct_input += chr(ord(v5[i]) + i)

# Print the correct input string
print("Correct Input for s:", correct_input)
```
**Steps**:
   - Loop through each index `i` in the `v5` string.
   - Convert `v5[i]` to its ASCII value using `ord(v5[i])`.
   - Add the index `i` to the ASCII value.
   - Convert the resulting ASCII value back to a character using `chr()`.
   - Append the character to the result string `correct_input`.

### Output 
```
Correct Input for s: udctf{r3v3ng3_101}
```
## How to avoid
**Avoid Hardcoding Keys**:

Hardcoding sensitive data (like v5 here) makes it easily retrievable using disassemblers or debuggers.
Use dynamically generated or encrypted keys stored securely.

**Use Stronger Algorithms**:

Replace simple constraints like `s[i] - i == v5[i]` with cryptographically secure checks (e.g., hash-based verification).
