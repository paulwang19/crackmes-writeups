# StaticAuth

## Analysis

### Main Function

The main function implements the following logic:

1. Displays "System authentication required"
2. Prompts user for authentication code
3. Reads input character by character using `getch()` (masked input with asterisks)
4. Calls `sub_1400015B0()` to generate the expected password
5. Compares user input with the expected password
6. If match: calls `sub_140001AE0()` and displays success
7. If no match: displays "Authentication failed" and allows up to 3 attempts
8. After 3 failed attempts: displays "Maximum attempts reached" and locks the system

### Password Generation Function

The key function `sub_1400015B0()` constructs the password by concatenating three parts:

1. Get prefix "goo" (function `sub_1400012A0`)
    - Creates a string buffer
    - Hardcodes the string **"goo"** using `qmemcpy(Src, "goo", 3)`
    - Returns the first 3 characters: 'g', 'o', 'o'
2. Hardcoded Middle Section
    - The function hardcodes the string **"djob"** using `strcpy((char *)Block, "djob")`
    - Appends each character individually: 'd', 'j', 'o', 'b'
3. Get suffix "123" (function `sub_140001490`)
    - Creates a string buffer
    - Hardcodes the string **"123"** using `strcpy(Str, "123")`
    - Returns the first 3 characters: '1', '2', '3'

### Password Reconstruction

The password is built by concatenating:

```cpp
"goo" + "djob" + "123" = "goodjob123"
```

## Verification

```shell
> .\trytopass.exe
System authentication required.

Enter authentication code: goodjob123

Verification successful.

Secure data accessed:
----------------------
Location: Grid 47-C
Access code: 8891-3324
Status: Active
Timestamp: Verified

Clearing in 15 seconds...
```
