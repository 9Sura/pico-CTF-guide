# CRYPTOMAZE PROBLEM GUIDE:

*(Cryptography — Easy, 100pt)*

## HINTS:
Hint 1: Use the LFSR state + taps to generate 128 bits -> 16-byte AES key.
Hint 2: Decrypt with AES-ECB; convert the ciphertext from hex first.

## TOOLS:
Python `pycryptodome` (`Crypto.Cipher.AES`)

## WALKTHROUGH:
1. `output.txt` gives the LFSR initial state (64 bits), taps `[63,61,60,58]`, and the hex ciphertext.

2. Clock the LFSR 128 times to build the AES key: each step outputs the leading bit, XORs the tap positions to make the new bit, appends it. Group the 128 output bits into 16 bytes.

3. AES-ECB decrypt:
```python
from Crypto.Cipher import AES
def keystream(state, taps, n=128):
    s, out = list(state), []
    for _ in range(n):
        out.append(s[0]); nb = 0
        for t in taps: nb ^= s[t]
        s = s[1:] + [nb]
    return bytes(int("".join(map(str,out[i:i+8])),2) for i in range(0,n,8))
key = keystream(initial_state, [63,61,60,58])
print(AES.new(key, AES.MODE_ECB).decrypt(bytes.fromhex(ct_hex)))
```
    - Answer: `picoCTF{...}` (instance-specific)

## NOTES:
- An LFSR is deterministic: given state + taps, its whole output stream is fixed — so it's a poor key source.
- Watch the tap/shift direction; the challenge shifts left and outputs the leading bit.
