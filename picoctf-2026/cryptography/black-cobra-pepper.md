# BLACK COBRA PEPPER PROBLEM GUIDE:

*(Cryptography — Medium, 200pt)*

## HINTS:
Hint 1: This looks like a well-known cipher.
Hint 2: What does the S-box do — and what if it's missing?

## TOOLS:
Python (the challenge's own AES functions, run in reverse)

## WALKTHROUGH:
1. `chall.py` is AES **with `sub_bytes` gutted** (`def sub_bytes(state): return state`). It gives you a known plaintext's ciphertext and the flag's ciphertext, same key.

2. Without SubBytes, every remaining step (AddRoundKey, ShiftRows, MixColumns) is linear over GF(2), so:
    - `E_K(P) = E_0(P) XOR E_K(0)`
    - i.e. the cipher splits into a key-only part and a plaintext-only part.

3. Recover the flag using the known pair:
    - Compute `E_0(pt1)` with a zero key (linear map, no secret)
    - `E_K(0) = E_K(pt1) XOR E_0(pt1)`  (E_K(pt1) is given)
    - `E_0(flag) = E_K(flag) XOR E_K(0)`  (E_K(flag) is given)
    - Decrypt `E_0(flag)` with the zero key (invert ShiftRows/MixColumns) -> flag
    - Answer: `picoCTF{...}` (instance-specific)

## NOTES:
- SubBytes is AES's **only** non-linear step. Remove it and the whole cipher becomes an affine map you can peel apart with one known plaintext.
- The zero-key trick isolates the key contribution as a constant XOR term.
