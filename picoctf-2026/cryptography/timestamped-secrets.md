# TIMESTAMPED SECRETS PROBLEM GUIDE:

*(Cryptography — Medium, 200pt)*

## HINTS:
Hint 1: The AES key is derived from the current time — and the time is printed.

## TOOLS:
Python `pycryptodome`, `hashlib`

## WALKTHROUGH:
1. `encryption.py`: `key = sha256(str(int(time.time())).encode()).digest()[:16]`, AES-ECB. The output even prints "encryption was done around <timestamp>".

2. Brute force timestamps around the printed value; derive the key, decrypt, check for `picoCTF{`:
```python
import hashlib
from Crypto.Cipher import AES
from Crypto.Util.Padding import unpad
ct = bytes.fromhex(ct_hex); base = 1770242615
for off in range(-1000, 1000):
    key = hashlib.sha256(str(base+off).encode()).digest()[:16]
    dec = AES.new(key, AES.MODE_ECB).decrypt(ct)
    if b"picoCTF{" in dec:
        print(unpad(dec, 16).decode()); break
```
    - Answer: `picoCTF{...}` (instance-specific)

## NOTES:
- A key derived from a low-entropy source (a Unix second) has almost no keyspace — a ±1000s window is 2000 guesses.
- Never seed keys from time or PIDs; use a CSPRNG.
