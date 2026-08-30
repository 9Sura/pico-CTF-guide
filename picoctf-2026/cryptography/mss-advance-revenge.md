# MSS ADVANCE REVENGE PROBLEM GUIDE:

*(Cryptography — Hard, 400pt)*

## HINTS:
Hint 1: What are lattices?

## TOOLS:
SageMath (LLL lattice reduction); AES-CBC

## WALKTHROUGH:
1. `chall.py`: flag AES-CBC-encrypted (IV all zero) under `MASTER_KEY = sha256(flag)`. You're given 20 pairs `(x, f(x) mod p)` of a degree-29 polynomial whose coefficients are `c0 = MASTER_KEY`, `c_{i+1} = sha256(c_i)`, and the 1024-bit prime `p`.

2. Only 20 equations for 30 coefficients — underdetermined by linear algebra alone. But the `c_i` are ~256 bits while `p` and the pairs are ~1024 bits: the wanted solution is **small** relative to the modulus, so build a lattice and run **LLL** to find the short vector encoding `c0`.

3. Construct the basis (modular relations weighted by a large `W`, identity block for the coefficients), reduce, and scan reduced rows for a ~256-bit value; try it as the AES key:
    - See the reference `solve.sage` for the exact matrix; the recovered 256-bit value is `MASTER_KEY`.
    - Answer: `picoCTF{...}` (instance-specific)

## NOTES:
- LLL finds short vectors in a lattice — the go-to when a system is underdetermined but the true solution is known to be small.
- The chained-hash coefficients mean recovering `c0` alone reconstructs everything, so the lattice only needs to surface one 256-bit number.
