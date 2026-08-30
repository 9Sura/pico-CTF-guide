# THE ADD-ON TRAP PROBLEM GUIDE:

*(Reverse Engineering — Medium, 200pt)*

## HINTS:
Hint 1: What kind of file ends in `.xpi`?
Hint 2: Which modern Python scheme uses url-safe base64 32-byte keys? (Fernet)

## TOOLS:
`$ unzip` ; Python `cryptography` (Fernet)

## WALKTHROUGH:
1. `.xpi` is a Firefox extension = a zip. `$ unzip` it (password `picoctf`) and read `background/main.js`.

2. It hardcodes a Fernet `key` (base64) and an encrypted `webhookUrl` (`gAAAAAB...` = Fernet token):
    - `key = "cGljb0NURnt5b3UncmUgb24gdGhlIHJpZ2h0IHRyYX0="`

3. Decrypt the token with the key:
```python
from cryptography.fernet import Fernet
key = b'cGljb0NURnt5b3UncmUgb24gdGhlIHJpZ2h0IHRyYX0='
token = b'gAAAAABmfRjw...'
print(Fernet(key).decrypt(token).decode())
```
    - Answer: `picoCTF{...}` (the decrypted webhook value; run against your own token)

## NOTES:
- Fernet keys are url-safe base64 of 32 bytes and tokens start with `gAAAAA` — the hint's fingerprint.
- The base64 key itself decodes to a red-herring partial string (`you're on the right tra`); the real flag comes from decrypting the token.
