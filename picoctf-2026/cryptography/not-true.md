# NOT TRUE PROBLEM GUIDE:

*(Cryptography — Hard, 400pt)*

## HINTS:
Hint 1: A lattice-based cryptosystem — is there a lattice attack to recover the private key from the public info?

## TOOLS:
SageMath (LLL); NTRU math

## WALKTHROUGH:
1. This is **NTRU**. Public: `N=48`, `p=3`, `q=509`, public key polynomial `h`, and ciphertext polynomials. The private key `f` has coefficients in {-1, 0, 1} (short).

2. Build the NTRU lattice `M = [[I, H], [0, qI]]` where `H` is the circulant matrix of `h`. LLL finds a short vector; the private `f` is the row whose first `N` entries are all in {-1,0,1}:
```python
M = Matrix(ZZ, 2*N, 2*N)
for i in range(N):
    M[i,i]=1
    for j in range(N): M[i,N+j]=h_list[(j-i)%N]
    M[N+i,N+i]=q
for row in M.LLL():
    if all(abs(c)<=1 for c in row[:N]): f = R(list(row[:N])); break
```

3. Decrypt each ciphertext: `a = f*ct mod q` (center coefficients to (-q/2, q/2]), then `m = a * f_p_inv mod p`; the coefficient bits reassemble to the flag string.
    - Answer: `picoCTF{...}` (instance-specific)

## NOTES:
- NTRU's private key is a short polynomial; for small parameters LLL reduction recovers it directly from the public key.
- Coefficient centering (mapping to the symmetric range around 0) is the step people most often get wrong.
