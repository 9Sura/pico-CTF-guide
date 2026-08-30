# SMALL TROUBLE PROBLEM GUIDE:

*(Cryptography — Medium, 200pt)*

## HINTS:
Hint 1: Might be a job for Boneh-Durfee (a small-`d` attack).

## TOOLS:
Python or SageMath — Wiener's attack

## WALKTHROUGH:
1. `encryption.py`: `d = getPrime(256)` while `n` is ~2096 bits, and `e = inverse(d, phi)`. A small private exponent `d` relative to `n` -> **Wiener's attack** (Boneh-Durfee also works).

2. Wiener: expand `e/n` as a continued fraction; each convergent `k/d` is a candidate. Test it — if `(e*d - 1) % k == 0` and the derived `phi` yields integer primes, you've found `d`:
```python
def wiener(e, n):
    cf = []; a,b = e,n
    while b: cf.append(a//b); a,b = b, a%b
    n2,d2,n1,d1 = 0,1,1,0
    for q in cf:
        k = q*n1+n2; d = q*d1+d2
        if k and d%2 and (e*d-1)%k==0:
            phi=(e*d-1)//k; s=n-phi+1; disc=s*s-4*n
            if disc>=0:
                t=isqrt(disc)
                if t*t==disc and (s+t)%2==0: return d
        n2,d2,n1,d1 = n1,d1,k,d
d = wiener(e, n); print(long_to_bytes(pow(c, d, n)))
```
    - Answer: `picoCTF{...}` (instance-specific)

## NOTES:
- Wiener's attack works when `d < n^0.25`. A 256-bit `d` against a 2096-bit `n` is well inside that bound.
- Small private exponents are tempting for fast decryption but fatal — pick `d` large and derive `e`, never the reverse.
