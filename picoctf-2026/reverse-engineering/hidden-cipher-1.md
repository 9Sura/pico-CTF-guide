# HIDDEN CIPHER 1 PROBLEM GUIDE:

*(Reverse Engineering — Easy, 100pt)*

## HINTS:
Hint 1: Figure out the cipher and the key.

## TOOLS:
Ghidra / IDA ; Python (XOR)

## WALKTHROUGH:
1. Decompile `hiddencipher`. It reads `flag.txt` and prints each byte XORed with `secret[i % 6]`.

2. `get_secret()` builds the key from byte constants: `83,51,67,114,51,116` -> ASCII `S3Cr3t`.

3. XOR the printed hex back with `S3Cr3t` (XOR is its own inverse):
```python
cipher = bytes.fromhex(hex_cipher)   # from the server
key = b"S3Cr3t"
print("".join(chr(cipher[i] ^ key[i % 6]) for i in range(len(cipher))))
```
    - Answer: `picoCTF{...}` (instance-specific — decode your own output)

## NOTES:
- A repeating-key XOR with a hardcoded key in the binary is trivially reversible once you read `get_secret`.
- Convert the numeric byte constants to ASCII to recover the key string.
