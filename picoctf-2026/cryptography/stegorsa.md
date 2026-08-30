# STEGORSA PROBLEM GUIDE:

*(Cryptography — Easy, 100pt)*

## HINTS:
Hint 1: Metadata can hold more than you expect.
Hint 2: Hex turns back into a key file.

## TOOLS:
`$ exiftool image.jpg`

CyberChef `From Hex`

`$ openssl pkeyutl -decrypt -inkey pkey.pem -in flag.enc`

## WALKTHROUGH:
1. `$ exiftool image.jpg`
    - `Comment: 2d2d2d2d2d424547494e20...` — hex that starts with `-----BEGIN`

2. Unhex it (CyberChef `From Hex`, or `xxd -r -p`). You get a PEM private key:
    - `-----BEGIN PRIVATE KEY-----` ... save as `pkey.pem`

3. `$ openssl pkeyutl -decrypt -inkey pkey.pem -in flag.enc`
    - Answer: `picoCTF{rs4_k3y_1n_1mg_66388eb3}`

## NOTES:
- The RSA private key was hidden in the JPEG's Comment field as hex — once recovered, decryption is a one-liner.
- Combines steganography (metadata) with RSA. `openssl pkeyutl -decrypt` handles the raw RSA decrypt.
