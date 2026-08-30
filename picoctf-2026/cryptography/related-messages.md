# RELATED MESSAGES PROBLEM GUIDE:

*(Cryptography — Medium, 200pt)*

## HINTS:
Hint 1: How are the two messages related?
Hint 2: Franklin-Reiter related-message attack.

## TOOLS:
SageMath (polynomial GCD over Z/N)

## WALKTHROUGH:
1. `chall.py`: same RSA key encrypts `Message` and `Message_fixed` (a typo-fix), so `Message - Message_fixed = 3` (given). You also get both ciphertexts, `N`, `e = 0x11`.

2. Two messages differing by a known amount under the same key = **Franklin-Reiter**. Build two polynomials whose common root is the message:
    - `f1(x) = x^e - c1`
    - `f2(x) = (x+3)^e - c2`
    - Their GCD over Z/N is `(x - Message)`

3. Extract the root and add 3 for the corrected message:
```python
P.<x> = PolynomialRing(Zmod(N))
def cgcd(a,b):
    while b: a,b = b, a % b
    return a.monic()
g = cgcd(x^e - c1, (x+3)^e - c2)
M = -g.coefficients()[0]
print(long_to_bytes(int(M+3)).decode())
```
    - Answer: `picoCTF{...}` (instance-specific)

## NOTES:
- Franklin-Reiter breaks RSA when two plaintexts with a small, known relationship share a modulus — no factoring needed.
- Works because `gcd(f1,f2)` over the ring collapses to the linear factor containing the message. Small `e` keeps it tractable.
