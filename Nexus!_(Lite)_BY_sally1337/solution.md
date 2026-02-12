# Nexus! (Lite)

## Description

`nexus-lite.exe` is a Windows console crackme that asks for a "Master Key."

## Analysis

### Initial Behavior

Program flow

1. Prints banner

    ```shell
    === NEXUS! (lite) ===
    Created by sally1337( butterfly )


    [*] System initialized...
    [*] Running anti-debug checks...
    [+] All checks passed!

    Time: 00:00
    Enter Master Key:
    ```

2. Repeats

    - asks `Enter Master Key:`
    - prints `[*] Validating...`
    - prints `[!] Access Denied` on failure

### Static Analysis

1. Locate strings in IDA:

    - `NEXUS-MASTER-KEY-2025`
    - `Enter Master Key:`
    - `[!] Access Denied`
    - `===== ACCESS GRANTED! =====`

2. Follow xrefs/decompile

    - `main` function initializes a string buffer with: `"NEXUS-MASTER-KEY-2025"`
    - It then calls validation loop function `sub_7FF6E5361C70`

3. Inspect validation logic in `sub_7FF6E5361C70`:

    - Reads user input
    - Converts input to uppercase via `toupper`
    - Compares input against stored key using length check + `memcmp`
    - On match, calls `sub_7FF6E53620A0` (prints success and exits)
    - On mismatch, prints `[!] Access Denied`

4. Master Key

    - So the master key is: `NEXUS-MASTER-KEY-2025`

## Verification

```shell
=== NEXUS! (lite) ===
Created by sally1337( butterfly )


[*] System initialized...
[*] Running anti-debug checks...
[+] All checks passed!

Time: 00:00
Enter Master Key: NEXUS-MASTER-KEY-2025
[*] Validating...

===== ACCESS GRANTED! =====
Correct Key: NEXUS-MASTER-KEY-2025

[+] Challenge Statistics:
    Time: 00:00
[+] Congratulations! You've cracked Nexus! (lite)

[*] Encrypted form: DgUYFRN6DQETFAUSegsFGXplY2Vo
```
