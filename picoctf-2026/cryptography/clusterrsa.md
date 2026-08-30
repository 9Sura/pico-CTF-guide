# CLUSTERRSA PROBLEM GUIDE:

*(Cryptography — Hard, 400pt)*

## HINTS:
Hint 1: RSA usually means two primes... what if someone got greedy?
Hint 2: Prime factor decomposition.

## TOOLS:
factordb.com ; Python `pycryptodome`

## WALKTHROUGH:
1. `message.txt`: `n`, `e = 65537`, `ct`. The modulus is a multi-prime RSA — `n = p1*p2*p3*p4` instead of `p*q`.

2. `n` is small enough to factor. Look it up on factordb.com — it splits into 4 primes.

3. Multi-prime RSA decrypts normally once you have all primes: `phi = product of (pi - 1)`, `d = inverse(e, phi)`:
```python
from Crypto.Util.number import long_to_bytes, inverse
primes = [p1, p2, p3, p4]     # from factordb
phi = 1
for p in primes: phi *= (p-1)
d = inverse(e, phi)
print(long_to_bytes(pow(ct, d, n)).decode())
```
    - Answer: `picoCTF{...}` (instance-specific)

## NOTES:
- Multi-prime RSA is real and valid, but more, smaller primes make `n` easier to factor — the opposite of the intended security gain here.
- Always try factordb first for CTF RSA; small or "known" moduli are often already listed.
