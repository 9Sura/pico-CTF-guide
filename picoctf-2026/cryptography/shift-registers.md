# SHIFT REGISTERS PROBLEM GUIDE:

*(Cryptography — Medium, 200pt)*

## HINTS:
Hint 1: An 8-bit LFSR keystream has only 256 possible seeds.

## TOOLS:
Python (brute force 0-255)

## WALKTHROUGH:
1. `chall.py` XORs the plaintext with an 8-bit LFSR keystream (taps at bits 7,5,4,3). The seed is `key & 0xFF` — only 256 possibilities.

2. Brute force every seed; XOR is its own inverse, so re-run the same keystream against the ciphertext:
```python
def step(l):
    fb = ((l>>7)^(l>>5)^(l>>4)^(l>>3)) & 1
    return (fb<<7) | (l>>1)
ct = bytes.fromhex(ct_hex)
for seed in range(256):
    l = seed; pt = bytearray()
    for c in ct:
        l = step(l); pt.append(c ^ l)
    try:
        d = pt.decode('ascii')
        if 'picoCTF{' in d: print(seed, d)
    except: pass
```
    - Answer: `picoCTF{lfsr_1s_n0t_s3cur3}`

## NOTES:
- An 8-bit state = 256 seeds = trivially brute-forceable. LFSRs need large states (and are still weak against Berlekamp-Massey) to resist attack.
- Two adjacent seeds may both print a valid flag — they produce the same stream offset; the flag is identical.
