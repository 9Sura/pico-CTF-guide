# BYPASS ME PROBLEM GUIDE:

*(Reverse Engineering — Easy, 100pt)*

## HINTS:
Hint 1: Disassemble it; note the functions.
Hint 2: The password is decoded at runtime.

## TOOLS:
`$ scp` (copy binary out), LLDB/Ghidra/IDA ; Python (XOR)

## WALKTHROUGH:
1. SSH in; `bypassme.bin` asks for a password (3 tries). `scp` it to your box and decompile.

2. `decode_password()` builds the password by XORing an embedded byte array with `0xAA`:
    - little-endian bytes: `F9 DF DA CF D8 F9 CF C9 DF D8 CF`

3. XOR each with `0xAA`:
```python
enc = [0xF9,0xDF,0xDA,0xCF,0xD8,0xF9,0xCF,0xC9,0xDF,0xD8,0xCF]
print("".join(chr(b ^ 0xAA) for b in enc))   # -> SuperSecure
```

4. Run `bypassme.bin` on the server and enter `SuperSecure`:
    - Answer: `picoCTF{...}` (instance-specific — read the server output)

## NOTES:
- Runtime-decoded passwords are still recoverable statically: find the decode routine and replicate its math.
- Mind endianness when reading multi-byte constants out of the disassembly.
