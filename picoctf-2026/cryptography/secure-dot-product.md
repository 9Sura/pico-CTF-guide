# SECURE DOT PRODUCT PROBLEM GUIDE:

*(Cryptography — Medium, 300pt)*

## HINTS:
Hint 1: A system is only as secure as what it trusts.
Hint 2: Automate with pwntools; it may not always be solvable (reconnect).

## TOOLS:
pwntools; SHA-512 length-extension; numpy (linear solve)

## WALKTHROUGH:
1. `remote.py`: encrypts the flag with a random 32-byte key (AES-CBC, IV+ct printed). It will compute the dot product of its **key vector** with any vector you send — if your vector's salted SHA-512 hash matches. Salt is secret (256 bytes), so you can't hash arbitrary vectors... normally.

2. SHA-512 is vulnerable to **length extension**: given `H(salt || v)` and `len`, you can compute `H(salt || v || pad || extra)` for chosen `extra` without knowing the salt. The server hands you 5 trusted (vector, hash) pairs to extend from.

3. Extend a trusted vector with 32 chosen coefficients, send it 32 times to get 32 dot products, then solve the linear system for the 32-byte key. Decrypt AES-CBC:
    - Full solve uses a from-scratch `sha512_extend()` + `numpy.linalg.solve` (see the reference solve script). Loop until a non-singular matrix/clean padding is drawn.
    - Answer: `picoCTF{...}` (instance-specific)

## NOTES:
- Two bugs chained: (1) length extension forges the integrity check the server "trusts"; (2) 32 linearly independent dot products fully determine the 32-byte key.
- Use HMAC (immune to length extension), not `H(secret || message)`.
