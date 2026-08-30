# AUTOREV 1 PROBLEM GUIDE:

*(Reverse Engineering — Medium, 200pt)*

## HINTS:
Hint 1: 20 binaries, 1 second each — automate.

## TOOLS:
pwntools ; `struct` ; objdump knowledge

## WALKTHROUGH:
1. `$ nc mysterious-sea.picoctf.net <port>`: the server sends 20 ELF binaries as hex; each stores a secret in `main` as `v5 = <int>; if (v5 == input) ...`.

2. In every binary that constant is loaded with `mov DWORD PTR [rbp-0x4], imm32` -> opcode bytes `C7 45 FC <imm32 little-endian>`. Find that pattern and read the 4-byte int:
```python
from pwn import *; import struct, binascii
r = remote('mysterious-sea.picoctf.net', <port>)
r.recvuntil(b'Good luck >:)')
for _ in range(20):
    r.recvuntil(b'in bytes:\n')
    elf = binascii.unhexlify(r.recvline().strip())
    i = elf.find(b'\xc7\x45\xfc')
    secret = struct.unpack('<I', elf[i+3:i+7])[0]
    r.recvuntil(b"What's the secret?:")
    r.sendline(str(secret).encode())
r.interactive()
```
    - Flag printed after 20 correct answers:
        - Answer: `picoCTF{...}` (instance-specific)

## NOTES:
- The 1-second-per-binary limit rules out manual decompiling — you must parse the opcode bytes programmatically.
- `C7 45 FC` = `mov [rbp-4], imm32`; the immediate follows, little-endian.
