# SHARED SECRETS PROBLEM GUIDE:

*(Cryptography — Easy, 100pt)*

## HINTS:
Hint 1: Combine a public value with a known private one.

## TOOLS:
Python (`pow`, XOR)

## WALKTHROUGH:
1. `encryption.py` does Diffie-Hellman: it makes a shared secret and XORs the flag with it. `output.txt` leaks `g`, `p`, `A = g^a mod p`, **and `b`** (one side's private exponent).

2. With `b` known, recompute the shared secret directly:
    - `shared = A^b mod p`

3. XOR the ciphertext with the key (`shared % 256`):
```python
shared = pow(A, b, p)
key = shared % 256
enc = bytes.fromhex(enc_hex)
print(bytes(x ^ key for x in enc).decode())
```
    - Answer: `picoCTF{...}` (see note; suffix instance-specific)

## NOTES:
- DH is only secure while **both** private exponents stay secret. Leaking either `a` or `b` collapses the shared key.
- Flag theme is Diffie-Hellman (e.g. `d1ff13_h3llm4n_...`). Run against your own `output.txt` for the exact string.
