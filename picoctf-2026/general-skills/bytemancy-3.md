# BYTEMANCY 3 PROBLEM GUIDE:

*(General Skills — Hard, 400pt)*

## HINTS:
Hint 1: "Send the raw 4-byte addresses of four named functions in little-endian."
Hint 2: `nm` gives you the addresses; pwntools `p32()` does the byte order.

## TOOLS:
`$ nm spellbook | grep -E "<funcs>"`

pwntools `p32()`, `remote()`

## WALKTHROUGH:
1. `app.py` asks, each round, for the little-endian raw address of one of:
    - `ember_sigil`, `glyph_conflux`, `astral_spark`, `binding_word`

2. `$ nm spellbook | grep -E "ember_sigil|glyph_conflux|astral_spark|binding_word"`
    - `080491c1 T astral_spark` / `080491e3 T binding_word` / `08049176 T ember_sigil` / `0804919a T glyph_conflux`

3. Little-endian means bytes reversed: `0x080491c1` -> `\xc1\x91\x04\x08`. Automate reading the prompt and replying with `p32()`:
```python
from pwn import *
io = remote('green-hill.picoctf.net', <port>)
addrs = {"astral_spark":p32(0x080491c1),"binding_word":p32(0x080491e3),
         "ember_sigil":p32(0x08049176),"glyph_conflux":p32(0x0804919a)}
for _ in range(3):
    io.recvuntil(b"procedure '")
    name = io.recvuntil(b"'").decode().strip("'")
    io.send(addrs[name])
print(io.recvall().decode())
```
    - Answer: `picoCTF{0bjdump_m4g1c_9ee35d3a}`

## NOTES:
- `nm` lists symbol addresses; `objdump -t` does the same. `p32()` packs a 32-bit int as little-endian raw bytes in one call.
- Final `bytemancy` ([[bytemancy-0]]..[[bytemancy-2]]): from typing chars, to raw bytes, to raw addresses.
