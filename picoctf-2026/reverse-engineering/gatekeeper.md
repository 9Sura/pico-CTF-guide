# GATEKEEPER PROBLEM GUIDE:

*(Reverse Engineering — Easy, 100pt)*

## HINTS:
Hint 1: Ghidra/IDA/radare2 reveal the input check.
Hint 2: The flag output is reversed and padded with junk — clean it, then reverse.

## TOOLS:
Ghidra / IDA / radare2 ; `$ nc <host> <port>` ; `sed`, `rev`

## WALKTHROUGH:
1. Decompile. The gate: value must be `> 999`, `<= 9999`, **and** the input string length `== 3`. Input is parsed as decimal if all digits, else as hex.
    - A 3-char decimal can't exceed 999, so use **hex**: 3 hex chars with a non-decimal letter, e.g. `abc` (= 2748), passes all three checks.

2. `$ nc green-hill.picoctf.net <port>` -> enter `abc`:
    - `Access granted: }847ftc_oc_ip...` — a reversed string with `ftc_oc_ip` junk repeated

3. Strip the junk and reverse:
    - `$ echo "<output>" | sed -e 's/ftc_oc_ip//g' | rev`
    - Answer: `picoCTF{...}` (instance-specific — clean your own output)

## NOTES:
- The `len == 3` plus `> 999` combo forces hexadecimal input — the intended "aha."
- `sed 's/pattern//g'` deletes every occurrence of the padding; `rev` flips the line.
